---
title: Unity 01 - ECS概念
date: 2019-04-04 17:05:24
categories: Unity
---
# ECS概念

<!--more-->

## 传统OOP缺陷

传统OOP下的MonoBehaviour/GameObject模式, 可以非常方便的为创作游戏编写代码, 但是往往在后期会使得代码难以阅读, 维护, 优化, 游戏开销大而性能低, 这是由一系列因素导致的:

1. OOP模型
2. Mono编译的非最优机器吗
3. GC
4. 单线程

## ECS模型

![ecs_overview](https://res.cloudinary.com/dpe4i978o/image/upload/v1554371224/unity/ecs_overview.png)

ESC(Entity-Component-System)是unity中DOTS(Data-Oriented Tech Stack)的核心(还有Burst Compile和Job System), 分为三个主要部分:

- Entity: the entities, or things, that populate your game or program
- Component: the data associated with your entities, but organized by the data itself rather than by entity. (This difference in organization is one of the key differences between an object-oriented and a data-oriented design.)
- System: the logic that transforms the component data from its current state to its next state— for example, a system might update the positions of all moving entities by their velocity times the time interval since the previous frame.

作为取代GameObject/Component的模式, 其模式遵循组合优于继承原则, 游戏内的每一个基本单元都是一个Entity, 每个Entity又是由一个或者多个Component构成, 每个Component仅仅包含代表其特性的数据(即Component中没有任何方法). System是来处理具有一个或多个Component组件的Entity集合的工具, 只拥有行为(即在System中没有任何数据).

Entity和Component是一对多的关系, Entity拥有怎样的能力, 完全取决于有哪些Component, 通过动态添加或者删除Component, 可以在运行时改变Entity的行为.

## Rotation Example

### Legacy

```cs
class Rotator : MonoBehaviour {
    public float RadiansPerSecond;

    public override void Update() {
        transform.rotation *= Quatern.AngleAxix(Time.deltaTime * RadiansPerSecond, Vector3.up);
    }
}
```

继承自MonoBehabiour的GameObject即包含了数据(数据域), 又包含了行为(Update).

### ECS



```cs
// RotationSpeed.cs

using System;
using Unity.Entities;

[Serializable]
public struct RotationSpeed : IComponentData {
    public float RadiansPerSecond;
}
```

```cs
// RotationSpeedProxy.cs

using Unity.Entities;
using Unity.Mathematics;
using UnityEngine;

[RequiresEntityConversion]
public class RotationSpeedProxy : MonoBehaviour, IConvertGameObjectToEntity {
    public float DegreesPerSecond;
    public void Convert(Entity entity, EntityManager dstManager, GameObjectConversionSystem conversionSystem) {
        var data = new RotationSpeed {
            RadiansPerSecond = math.radians(DegreesPerSecond)
        };
        dstManager.AddComponentData(entity, data);
    }
}
```

```cs
// RotationSpeedSystem.cs

using Unity.Burst;
using Unity.Collections;
using Unity.Entities;
using Unity.Jobs;
using Unity.Mathematics;
using Unity.Transforms;
using UnityEngine;

public class RotationSpeedSystem : JobComponentSystem {
    private ComponentGroup _componentGroup;

    protected override void OnCreateManager() {
        _componentGroup = GetComponentGroup(typeof(Rotation), ComponentType.ReadOnly<RotationSpeed>());
    }

    [BurstCompile]
    private struct RotationSpeedJob : IJobChunk {
        public float DeltaTime;
        public ArchetypeChunkComponentType<Rotation> RotationType;
        [ReadOnly] public ArchetypeChunkComponentType<RotationSpeed> RotationSpeedType;
        
        public void Execute(ArchetypeChunk chunk, int chunkIndex, int firstEntityIndex) {
            var chunkRotations = chunk.GetNativeArray(RotationType);
            var chunkRotationSpeeds = chunk.GetNativeArray(RotationSpeedType);
            for (var i = 0; i < chunk.Count; i++) {
                var rotation = chunkRotations[i];
                var rotationSpeed = chunkRotationSpeeds[i];

                chunkRotations[i] = new Rotation {
                    Value = math.mul(math.normalize(rotation.Value),
                        quaternion.AxisAngle(math.up(), rotationSpeed.RadiansPerSecond * DeltaTime))
                };
            }
        }
    }

    protected override JobHandle OnUpdate(JobHandle inputDeps) {
        var rotationType = GetArchetypeChunkComponentType<Rotation>();
        var rotationSpeedType = GetArchetypeChunkComponentType<RotationSpeed>();

        var job = new RotationSpeedJob {
            DeltaTime = Time.deltaTime,
            RotationType = rotationType,
            RotationSpeedType = rotationSpeedType
        };

        return job.Schedule(_componentGroup, inputDeps);
    }
}
```

我们可以看到ECS的工作模式:

- ECS的行为(System)和数据(Component)分别实现
- Entity中存储多种数据(Component)
- 如果存储在Entity中的Component满足本组的数据列表, 则由System执行行为

## ECS优势

1. Component是sturct而不是class, 这意味着我们在存储数据是的时候不是通过new到heap中, 离散到存储, 而是在内存中连续对其存储. 值得注意的是, NativeArray将native内存直接暴露到managed code中, 从而使得managed和native之间数据共享.
2. 基于Burst Compile, 可以生成优于MonoBehabiour的机器码, 以增加预编译时间为代价, 提高运行时效率.
3. 基于Job System, System在调度jobs的时候会把任务放到队列中, 由worker threads多线程完成, 并通过细粒度话数据的读写权限, 加速执行, 提高CPU的利用效率.