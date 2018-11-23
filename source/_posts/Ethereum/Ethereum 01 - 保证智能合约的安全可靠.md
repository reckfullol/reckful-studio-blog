---
title: Ethereum 01 - 保证智能合约的安全可靠
date: 2018-05-11 10:09:14
categories: Ethereum
---
# 保证智能合约的安全可靠

<!--more-->

首先要注意, 即使智能合约代码中没有任何Bug, 编译器和运行平台本身也可能有Bug.

详见[List of Know Bugs of Solidity](http://solidity.readthedocs.io/en/develop/bugs.html).

## 常见的安全陷阱

1. 私有状态

    在Solidity中, 可以通过private定义私有变量或者私有函数. 但是开发者要意识到, 在区块链上智能合约的所有信息都是公开可见的, 即使是被private修饰的变量(事实上, private变量仅仅是不能被其他智能合约在执行时"直接"访问到而已). 只是因为每个全节点都可以拿到智能合约创建和调用的字节码, 他们都会将智能合约执行后的状态保存在本地以供验证, 而所有的变量都可以通过eth_getStorageAt()这样的API探测到. 

2. 随机

    在智能合约中使用随机数是一件十分微妙的事情, 因为所谓的随机是由创建当前区块的"矿机"决定的, "矿机"虽然不能篡改执行结果, 但是有可能预先得知随机数(the casino with a public RNG seed), 所以自己做轮子写随机数生成器是很危险的.

3. 重入

    在计算机程序中, 重入(Re-Entrancy)是指一段程序在执行过程中被打断, 并且在上一次调用还未完全结束之前再次被重新调用的现象.

    任何从合约A到合约B的转账过程中, 将控制权移交给合约B的行为都有可能造成合约B在转账完成之前再次调用合约A.

    ```solidity
    // bug contain
    contract Fund {
        // mapping of ether shares of the contract
        mapping(address => uint) shares;
        // withdraw your share
        function withdraw() {
            if(this.balance < shares[msg.sender]) {
                throw;
            }
            if(msg.sender.call.value(shares[msg.sender])()) {
                shares[msg.sender] = 0;
            }
        }
    }
    ```

    在这段智能合约中, 有一个mapping类型的变量shares, 和一个withdraw函数. shares记录着每个用户在Fund中拥有的"股权", 假定一份Fund的股权等价于一个以太币. 当用户想将在Fund中的"股权"换回以太币时, 可以通过调用withdraw()函数进行撤回操作.

    在这段代码中, Fund先将以太币返还给用户, 再将shares里记录的相应"股权"清零. 当一个普通的用户账户调用执行退款时, 另一个智能合约同时来调用, 就会有严重的隐患.

    由于Gas的限制的限制, 我们不需要担心死循环的问题. 但是以太币的转账总是会触发代码的执行, 如果接收方是一个智能合约, 即msg.send是一个智能合约, 那么他将能够在接收过程中再次调用withdraw()函数. 具体做法是, 接收方智能合约中自己定义一个匿名函数, 在这个匿名函数中再次调用withdraw()函数. 由于在执行msg.sender.call时, 接收方合约(msg.sender)的匿名函数是会自动执行的, 这将导致接收方合约的匿名函数和Fund合约的匿名函数之间循环调用, 使得Fund合约一直执行不到shares[msg.sender] = 0这句话, 而重复地执行msg.sender.call.value(shares[msg.sender])(). 这样一直执行下去, Fund被反复提款, 要么调用栈到达最大深度, 要么Fund合约中的余额不足, 才会使得程序的执行被终止.

    为了避免重入问题, 可以像下面代码一样进行检测.

    ```solidity
    contract Fund {
        // mapping of ther shares of the contract
        mapping(address => uint) shares;
        // withdraw your share
        function withdraw() {
            uint share = shares[msg.sender];
            if(this.balance < share) {
                throw;
            }
            shares[msg.sender] = 0;
            msg.sender.transfer(share);
        }
    }
    ```

    要注意的是, 不仅是以太币转账会带来重入问题, 其他任何对其他合约的访问都会有一样的问题. 此外, 在使用多重组合的合约时, 被调用的合约也可能修改调用合约所依赖的另一个合约的状态.
    
4. Gas限制和循环

    在以太坊智能合约中, 每一步操作是要求用户以Gas的形式付出相应的代价. 对于非固定次数的循环(如依赖于某一个存储的值)的使用, 一定要格外的注意.

    由于每个区块的Gas限制(Gas Limit), Transaction最多能消耗的Gas是有限的. 固定次数的循环可以准确的计算出消耗的Gas, 从而可以避免执行智能合约消耗的Gas超出限制.

    因此设计者必须考虑到这一点, 可以通过限定最大循环次数方式, 来避免发生对智能合约的某次调用不能在Gas限制之内执行完毕的情况.

5. tx.origin和msg.sender

    Solidity提供两两个方式来获取调用者的身份: tx.origin和msg.sender. 不过这两者有明显的区别. tx.orgin是用来获取发起Transaction的账户地址, 而msg.sender只能获取上一级调用者的地址. 比如智能合约A调用了智能合约B, 有一个用户P给智能合约A发Transaction, 调用A调用了B. 那么对于智能合约A来说, msg.sender和tx.origin都是P的地址, 而对于智能合约B来说, msg.sender是A的地址, tx.origin是P的地址.

    按照Solidity官方的建议, 不推荐使用tx.orgin进行权限控制.

    ```solidity
    // bug contain
    contract TxUserWaller {
        address owner;

        function TxUserWallet() {
            owner = msg.sender;
        }
        function transferTo(address dest, uint amount) {
            require(tx.origin == owner);
            dest.transfer(amount);
        }
    }
    ```

    黑客可以定义一个这样的智能合约:

    ```solidity
    interface TxUserWallet {
        function transferTo(address dest, uint amount);
    }

    contract TxAttackWallet {
        address hacker;

        function TxAttackWallet() {
            hacker = msg.sender;
        }

        function() {
            TxUserWallet(msg.sender).transferTo(hacker, msg.sender.balance);
        }
    }
    ```

    在TxAttackWallet中定义了一个匿名函数, 在这个函数中会调用TxUserWallet的transferTo函数. 黑客会欺骗他人给TxAttackWallet这个合约地址转钱, 甚至只是让他发生一个交易, 这都会触发TxAttackWallet的匿名函数. 通过这个匿名函数判断tx.origin确实是TxUserWallet合约的owner, 虽然owner没有直接调用transferTo函数, 但是owner在TxUserWallet中的钱都转到了hack账户.

    因此msg.sender和tx.origin的使用应该结合具体场景, 开发者应该在充分理解二者的区别的基础上考虑合约的安全性.

6. 其他细节
    1. 溢出
        
        在Solidity中, uint是256位, 最大值为256^2 - 1, 最小是0. 那么当整形算数结果超过这个范围时, 会出现上溢或者下溢, 这是计算结果将于实际期望结果出现偏差. 因此在做四则运算的时候, 推荐使用[SafeMath](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol).

    1. var
        
        ```solidity
        for(var i = 0; i < length; i++) {}
        ```

        这里i的编译器设置为uint8, 因为uint8是能表示9值的最小类型, i最大只能为255, 那么如果length大于255, for将不会停止, 直到Gas耗尽.

## 智能合约开发建议

1. 使用Checks-Effects-Interactions模式
    1. Check
    
        在执行之前, 先进行权限及安全性检查. 检查的内容包括: 判断function的调用者身份, 判断是否有相关操作权限, 调用的参数是否符合要求, 调用function的Transaction是否附上了指定的以太币数量等.

    1. Effect

        当必要的检查都通过了之后, 再对当前合同中的状态变量进行更改.
    
    1. Interactions

        在状态变量的更改生效之后, 再进行和其他合约的交互.

1. 充分的容错机制
    1. 使用Fail-Safe模式

        所谓Fail-Safe, 就是在智能合约出现异常情况下, 能尽可能保障合约中数据的安全. 首先, 开发者需要在智能合约中添加一个自检查的函数, 在这个函数中对合约的状态进行检查, 特别是和数字资产相关的内容一定要格外注意. 一旦自检查函数执行出现异常, 那么要能自动的触发Fail-Safe模式, 这是可以将交易相关的函数禁用, 只允许指定合约的创始人或一个可信的第三方控制
    
    1. 限制合约中数字资产的数量

        最好不要在智能合约中存储大量的数字资产, 毕竟以太坊社区本身还在不断发展, 很多新的特性被不断的引入, 很难保证每次升级都是稳定的.

    1. 让代码轻巧且模式化

        为了保证代码的安全性, 清晰的代码结构, 易于理解的代码逻辑都是很必要的. 尽可能不要在一个函数里写太复杂的逻辑, 可以将工具性的逻辑抽离出来, 将一个复杂的函数拆成多函数. 另外和其他软件开发类似, 开发者最好有一份清晰详细的文档来介绍合约内容, 以及各个函数的功能和逻辑.
    
    1. 充分的测试

        在以太坊主网络上正式发布智能合约前, 一定要做好充分的测试, 任何的漏洞都有可能让你损失惨重. 也可以用下面的工具进行检查:

        - [Oyente](https://github.com/melonproject/oyente): 一个Python语言编写的工具, 判断代码中有没有常见的安全漏洞, 也会提示出可能有安全隐患的地方.

        - [solidity-coverage](https://github.com/sc-forks/solidity-coverage): 一个Node.js编写的Solidity代码覆盖率测试工具, 需要结合测试网络一起使用.

        - [Solgraph](https://github.com/raineorshine/solgraph): 一个Node.js工具, 可以将一个智能合约作为输入, 输出一个DOT图文件, 能将智能合约的功能控制流程画成一个流程图, 也可以标注出潜在的安全漏洞.