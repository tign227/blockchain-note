* 合约结构
  * 合约的基本结构
  * 构造函数



#### Solidity编程基础

##### 合约的基本结构

通过以下代码演示合约的结构，包含了编译器版本、合同声明、状态变量、函数、事件以及修饰器。

```solidity
// SPDX-License-Identifier: MIT // 合同的许可证或者授权信息
pragma solidity ^0.8.0; // 编译器版本

contract SolidityExample { // 合同声明
    address public owner;	// 状态变量
    uint256 public data;

    event DataUpdated(address indexed user, uint256 newValue);	// 事件

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {	// 修饰器
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function updateData(uint256 newValue) public onlyOwner {	// 函数
        data = newValue;
        emit DataUpdated(msg.sender, newValue);
    }
}

```



###### 1，编译器版本

该声明确保合同与特定编译器版本兼容，控制警告、错误、新特性和安全性，维护合同稳定性。

###### 2，合同声明

Solidity合约以`contract`关键字声明，并指定合同的名称，如`contract SolidityExample`.

###### 3，状态变量

状态变量用于在智能合约中永久存储数据，允许合约跟踪和维护状态，实现数据的持久性存储和共享。代码中`owner`和`data`分别记录合约的部署者以及存储的数据。

###### 4，函数

函数定义了智能合约的行为和功能，允许合约与外部互动，执行计算，修改状态，实现合约的业务逻辑和交互。代码中的`updateDate`函数表示更新链上的状态变量，并发送`DataUpdated`事件。

###### 5，补充操作

* 事件：是合约中用于发布通知和记录关键事件的机制，允许外部应用程序和合约监听合约的活动。事件`DataUpdated`以用户地址和更新的数据为参数，`indexed`关键字用于指示在事件中对参数进行索引，提高查询效率和过滤性能。
* 修饰器：修饰器用于修改和增强函数的行为，通常用于控制函数的可见性、权限和输入验证。无参修饰器`onlyOwner`通过`require`语句进行权限控制，表示只有合约的部署者才能更新链上的数据。



##### 构造函数

在Solidity中，构造函数是合约部署时自动执行的特殊函数。构造函数的作用是在合约部署时进行初始化工作。它在整个合约的生命周期中只能执行一次。

构造函数的定义方式如下：

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    // 构造函数
    constructor() {
        // 初始化逻辑
    }

    // 其他合约逻辑...
}
```

构造函数使用 `constructor` 关键字声明，没有返回类型，与合约同名。在构造函数内，你可以执行一些初始化的操作，例如设置初始状态、分配初始值等。当你通过交易将合约部署到区块链上时，构造函数会被自动调用。

一个简单的例子：

```solidity
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 public data;

    // 构造函数设置初始值
    constructor() {
        data = 42;
    }

    // 其他合约逻辑...
}
```

在上述例子中，合约 `SimpleStorage` 的构造函数在部署时将 `data` 初始化为 42。构造函数是 Solidity 中非常重要的一个概念，它使得在合约创建时能够执行必要的初始化步骤。

