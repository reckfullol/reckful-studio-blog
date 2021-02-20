---
title: Unreal 01 - LoadMap
date: 2021-02-20 16:09:58
categories: Unreal
---
# LoadMap

<!--more-->

## 跟踪分析

1. `TRACE_LOADTIME_REQUEST_GROUP_SCOPE`  LoadTime
	2. `LLM_SCOPE` [低级内存跟踪器 | Unreal Engine Documentation](https://docs.unrealengine.com/zh-CN/ProductionPipelines/DevelopmentSetup/Tools/LowLevelMemoryTracker/index.html)
	3. `NETWORK_PROFILER` [网络分析器（Network Profiler） | Unreal Engine Documentation](https://docs.unrealengine.com/zh-CN/InteractiveExperiences/Networking/NetworkProfiler/index.html)
	4. `MALLOC_PROFILER`

## 卸载资源
	
1. 检查`level streaming isn’t frozen`
2. `FCoreUObjectDelegates::PreLoadMap.Broadcast(URL.Map);` 回调事件通知
3. 注册析构函数PostLoadMap().
4. 取消等待中的纹理流送请求`UTexture2D::CancelPendingTextureStreaming();` [纹理流送概述 | Unreal Engine Documentation](https://docs.unrealengine.com/zh-CN/RenderingAndGraphics/Textures/Streaming/Overview/index.html)
5. 卸载package[UEngine::CleanupPackagesToFullyLoad | Unreal Engine Documentation](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/UEngine/CleanupPackagesToFullyLoad/index.html)
```cpp 
if (WorldContext.World() && WorldContext.World()->PersistentLevel)
    {
        CleanupPackagesToFullyLoad(WorldContext, FULLYLOAD_Map, WorldContext.World()->PersistentLevel->GetOutermost()->GetName());
    }
CleanupPackagesToFullyLoad(WorldContext, FULLYLOAD_Game_PreLoadClass, TEXT(""));
CleanupPackagesToFullyLoad(WorldContext, FULLYLOAD_Game_PostLoadClass, TEXT(""));
CleanupPackagesToFullyLoad(WorldContext, FULLYLOAD_Mutator, TEXT(""));
```
6. 强制同步阻塞处理异步加载, 确保没有异步加载还在进行`FlushAsyncLoading();`
7.  取消所有等待中的地图异步更改`CancelPendingMapChange(WorldContext);`, 需要保证`FlushAsyncLoading();`在这之前执行
8. 卸载当前world
   1. 关闭网络`ShutdownWorldNetDriver(WorldContext.World());`
   2.  同步阻塞处理level streaming`FlushLevelStreaming`, [UWorld::FlushLevelStreaming | Unreal Engine Documentation](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/UWorld/FlushLevelStreaming/index.html)
   3.  向所有level广播卸载事件`FWorldDelegates::LevelRemovedFromWorld.Broadcast(nullptr, WorldContext.World());`
   4.  将玩家与玩家控制器断开
```cpp
WorldContext.World()->DestroyActor(Player->PlayerController->GetPawn(), true);
WorldContext.World()->DestroyActor(Player->PlayerController, true);
Player->PlayerController = nullptr;
```
   5.  清除玩家的view state, 防止world不能彻底clean`Player->CleanupViewState();`  [ULocalPlayer::CleanupViewState | Unreal Engine Documentation](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/ULocalPlayer/CleanupViewState/index.html) 
   6.  销毁world中的Actor`ActorIt->RouteEndPlay(EEndPlayReason::LevelTransition);`
   7.  执行CleanupWorld, `WorldContext.World()->CleanupWorld();`, 需在销毁pawns/playercontrollers后, 防止生成新的事物(如丢掉的武器)
   8.  将world从root set移除`WorldContext.World()->RemoveFromRoot();`
   9.  标记object删除`CastChecked<UWorld>(Level->GetOuter())->MarkObjectsPendingKill();`
   10. 标记level删除`CastChecked<UWorld>(LevelStreaming->GetLoadedLevel()->GetOuter())->MarkObjectsPendingKill();`
   11. 移除audio`	AudioDevice->Flush(WorldContext.World());`
   12. 设置当前world为空`WorldContext.SetCurrentWorld(nullptr);`
9.  同步阻塞进行全量GC `TrimMemory();`.
10. 移除强制驻留的资源`IStreamingManager::Get().CancelForcedResources();`
11. 检查world是否清理完毕`VerifyLoadMapWorldCleanup();`

## 加载资源

1.  调用注册函数预加载资源`CurrentWorld->GetGameInstance()->PreloadContentForURL(PendingTravelURL);`
2. 检查如果是PIE instance, 就修改PIERemapPrefix来加载这份world拷贝, 而不是直接加载PIE world.
```cpp
NewWorld = UWorld::DuplicateWorldForPIE(SourceWorldPackage, nullptr);
NewWorld->StreamingLevelsPrefix = UWorld::BuildPIEPackagePrefix(WorldContext.PIEInstance);
```
3. 如果是minimal net rpc world则创建`UGameInstance::CreateMinimalNetRPCWorld(*URL.Map, WorldPackage, NewWorld);`
4. 常规地图加载
   1. 设置world type`UWorld::WorldTypePreLoadMap.FindOrAdd( URLMapFName ) = WorldContext.WorldType;` [WorldTypePreLoadMap | Unreal Engine Documentation](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/UWorld/WorldTypePreLoadMap/index.html)
   2. 检查level是否已经在内存中`WorldPackage = FindPackage(nullptr, *URL.Map);`
   3. 如果level不在地图中, 则从磁盘进行加载`WorldPackage = LoadPackage(nullptr, *URL.Map, (WorldContext.WorldType == EWorldType::PIE ? LOAD_PackageForPIE : LOAD_None));`
   4. PostLoad执行完毕后, 清理world type list.`UWorld::WorldTypePreLoadMap.Remove( URLMapFName );`
   5. 拿到加载完的world`NewWorld = UWorld::FindWorldInPackage(WorldPackage);`
   6.  persistent level初始化`NewWorld->PersistentLevel->HandleLegacyMapBuildData();`
   7.  如果是world type是PIE, 尝试复制一份PIE world`NewWorld = CreatePIEWorldByDuplication(WorldContext, NewWorld, URL.Map);`. `NewWorld = CreatePIEWorldByLoadingFromPackage(WorldContext, URL.Map, WorldPackage);`
   8.  设置world的context
```cpp
NewWorld->SetGameInstance(WorldContext.OwningGameInstance);
GWorld = NewWorld;
WorldContext.SetCurrentWorld(NewWorld);
WorldContext.World()->WorldType = WorldContext.WorldType;
```
   9. 如果不是PIE, 将world加入root节点`WorldContext.World()->AddToRoot();`, 并进行world初始化`WorldContext.World()->InitWorld();`
   10.  将PendingNetGame中的NetDriver对象赋值给新的world的NetDriver`MovePendingLevel(WorldContext);`
   11. 设置game mode`WorldContext.World()->SetGameMode(URL);` [Game Mode 和 Game State | Unreal Engine Documentation](https://docs.unrealengine.com/zh-CN/InteractiveExperiences/Framework/GameMode/index.html)
   12. audio device 初始化`AudioDevice->SetDefaultBaseSoundMix(WorldContext.World()->GetWorldSettings()->DefaultBaseSoundMix);`
   13. 监听客户端`WorldContext.World()->Listen(URL)`
   14. 处理异步完成的shader map, 并将其分配到对应的材质上`GShaderCompilingManager->ProcessAsyncResults(false, true);`
   15. 加载需要fully load的package`LoadPackagesFully(WorldContext.World(), FULLYLOAD_Map, WorldContext.World()->PersistentLevel->GetOutermost()->GetName());`
   16. 确保”always loaded”子关卡已经全部加载完毕`WorldContext.World()->FlushLevelStreaming(EFlushLevelStreamingType::Visibility);`
   17. 在source level 完成创建后复制动态level`WorldContext.World()->DuplicateRequestedLevels(FName(*URL.Map));`
   18. 初始化AI system`WorldContext.World()->CreateAISystem();`
   19. 初始化level中的gameplay`WorldContext.World()->InitializeActorsForPlay(URL, true, &Context);`
   20. 初始化navigation system`FNavigationSystem::AddNavigationSystemToWorld(*WorldContext.World(), FNavigationSystemRunMode::GameMode);`
   21. 为所有的local players创建player actor`(*It)->SpawnPlayActor(URL.ToString(1),Error2,WorldContext.World())`
   22. 通知stream manager开始texture streaming`IStreamingManager::Get().NotifyLevelChange();`
   23. XRsystem初始化`GEngine->XRSystem->OnBeginPlay(WorldContext);`  [XRSystem | Unreal Engine Documentation](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/UEngine/XRSystem/index.html)

1.  开始game play`WorldContext.World()->BeginPlay();`
2.  发送回调信息`PostLoadMapCaller.Broadcast(WorldContext.World());`
3.  更新stream`RedrawViewports(false);`
4.  从streaming manager中移除所有streaming views `IStreamingManager::Get().RemoveStreamingViews( RemoveStreamingViews_All );`
