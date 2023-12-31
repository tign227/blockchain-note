* 合约结构
  * 合约的基本结构
  * 构造函数
  * 访问控制修饰符
* Solidity语法
  * 数据类型，变量和常量
  * 函数和事件
  * 控制结构
* 数据持久化
  * 存储数据和状态变量
  * 内存和栈数据的存储





### Solidity编程基础

#### 合约的基本结构

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



#### 构造函数

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



#### 访问控制修饰符

访问控制修饰符的主要作用是定义合约中函数的访问权限，以确保合约中的各个函数被正确地调用和使用。通过使用这些修饰符，开发者可以控制哪些部分的合约对外是可见的、可调用的，哪些是仅限内部或外部调用的。以下是一些常见的访问控制修饰符：

1. **`public`：** 默认的状态，所有函数都是公共的，可以被外部调用。

   ```solidity
   function myFunction() public {
       // 可被外部调用
   }
   ```

2. **`external`：** 表示函数只能从合约外部调用，不能被合约内部的其他函数调用。

   ```solidity
   function myFunction() external {
       // 只能被外部调用
   }
   ```

3. **`internal`：** 表示函数只能在当前合约内部或继承合约中调用，不能被合约的外部调用。

   ```solidity
   function myFunction() internal {
       // 只能在内部调用
   }
   ```

4. **`private`：** 表示函数只能在当前合约内部调用，不能被继承合约中的函数调用。

   ```solidity
   function myFunction() private {
       // 只能在内部调用
   }
   ```

5. **`internal` 和 `external` 联合使用：** 在接口中使用 `external`，在实现中使用 `internal`。

   ```solidity
   // 接口
   interface MyInterface {
       function myFunction() external;
   }
   
   // 实现
   contract MyContract is MyInterface {
       function myFunction() internal override {
           // 实现接口中的函数
       }
   }
   ```



### Solidity语法

#### 数据类型，变量和常量

在Solidity中，有各种数据类型、变量和常量。以下是一些常见的数据类型、变量和常量：

##### 数据类型

###### 1，整数

- **`uint` (无符号整数):** 用于存储非负整数，包括零。例如，`uint256` 表示一个256位的无符号整数。

- **`int` (有符号整数):** 用于存储可以为正、负或零的整数。例如，`int32` 表示一个32位的有符号整数。

```solidity
uint256 positiveNumber = 42;
int32 signedNumber = -10;
```

###### 2，字节数组

- **`bytes`:** 用于存储动态长度的字节数组。可以通过索引访问单个字节。

- **`string`:** 用于存储字符串，以UTF-8编码。字符串长度可变。

```solidity
bytes dynamicBytes = "Hello, World!";
string greeting = "Hello, Solidity!";
```

###### 3，布尔值

- **`bool`:** 用于存储真（`true`）或假（`false`）值。

```solidity
bool isTrue = true;
bool isFalse = false;
```

###### 4，地址

- **`address`:** 用于存储以太坊地址。可以与其他地址进行交互，例如发送以太币或调用合约。

```solidity
address userAddress = 0x123...;
```

###### 5，定点数

- **`fixed` 和 `ufixed`:** 用于存储定点数，即具有固定小数点位置的数值。

```solidity
fixed fNum = 3.14;
ufixed ufNum = 2.718;
```

###### 6，枚举

- **`enum`:** 用于创建一组有限的可能性，如状态或选项。

```solidity
enum Status {
    Pending,
    Approved,
    Rejected
}

Status currentStatus = Status.Pending;
```

###### 7，Mapping 

- **`mapping`:** 用于建立键值对映射，类似于字典或哈希表。

```solidity
mapping(address => uint256) balances;
balances[0xabc...] = 100;
```

##### 变量

变量是用于存储和操作数据的标识符。

```solidity
uint256 number;  // 无符号整数变量
address owner;   // 地址变量
string name;     // 字符串变量
```

##### 常量

常量是在编译时固定的值，不能被修改。

```solidity
uint256 constant MAX_COUNT = 100;  // 常量
address constant CREATOR = 0x123...;  // 地址常量
string constant GREETING = "Hello";  // 字符串常量
```

#### 函数和事件

在Solidity合约中，函数和事件是两个核心的概念，用于定义合约的行为和与外部环境的交互。

##### 函数

1. **定义：** 函数是Solidity合约中执行特定任务的代码块。

2. **结构：** 函数由函数名、参数列表、可见性修饰符、返回类型和函数体组成。

   ```
   function add(uint256 a, uint256 b) public pure returns (uint256) {
       return a + b;
   }
   ```

3. **可见性修饰符：** 控制谁可以调用函数，包括 `public`、`external`、`internal`、`private` 等。

4. **状态变更：** 在Solidity中，状态变更（State Mutability）是指函数对合约状态的影响。Solidity有四种状态变更类型，分别是：

   **`pure`：** 表示函数不会读取或修改合约状态，也不会调用其他不带有 `view` 或 `pure` 标识的函数。

   ```solidity
   function calculate(uint256 a, uint256 b) public pure returns (uint256) {
       return a + b;
   }
   ```

   **`view`：** 表示函数不会修改合约状态，但可以读取合约中的数据。在调用其他 `view` 或 `pure` 函数时，不会消耗任何Gas。

   ```solidity
   function getBalance(address account) public view returns (uint256) {
       return balances[account];
   }
   ```

   **`payable`：** 表示函数可以接受以太币，并可能修改合约状态。通常用于接收以太币的函数。

   ```solidity
   function deposit() public payable {
       balances[msg.sender] += msg.value;
   }
   ```

   **无标识符（不带有 `pure`、`view` 或 `payable`）：** 表示函数可以修改合约状态，但不能接受以太币。

   ```solidity
   function updateData() public {
       data = newData;
   }
   ```

   这些状态变更类型有助于开发者清晰地了解函数的行为和对合约状态的影响。选择正确的状态变更类型对于确保合约的正确性和安全性至关重要。

5. **返回值：** 可以定义函数的返回类型，并使用 `returns` 关键字指定。


在Solidity合约中，函数和事件是两个核心的概念，用于定义合约的行为和与外部环境的交互。

##### 事件

1. **定义：** 事件是合约中用于记录和通知外部应用程序的重要状态变更的机制。

2. **结构：** 事件由事件名、参数列表组成，参数可以是任何合法的数据类型。

   ```
   event Transfer(address indexed from, address indexed to, uint256 value);
   ```

3. **日志记录：** 通过 `emit` 关键字触发事件，将重要信息记录在区块链的日志中。

   ```
   function transfer(address to, uint256 value) public {
       // 逻辑处理
       emit Transfer(msg.sender, to, value);
   }
   ```

4. **监听和查询：** 外部应用程序可以监听合约的事件，以便在特定条件发生时执行相应的操作。

   ```
   myContract.events.Transfer()
       .on('data', (event) => {
           console.log('Transfer event:', event.returnValues);
       })
       .on('error', console.error);
   
   ```

函数和事件是Solidity合约中的关键组件，通过定义函数，合约可以执行各种任务，而通过使用事件，合约可以在状态变更时向外部通知重要信息。这两者共同支持合约的交互和通信。



#### 控制结构

Solidity支持常见的控制结构，包括条件语句和循环语句。以下是一些主要的控制结构及其示例：

##### 1. 条件语句：

###### `if` 语句：

```solidity
uint256 x = 10;

if (x > 5) {
    // 如果 x 大于 5，则执行这里的代码
    // ...
} else {
    // 如果 x 不大于 5，则执行这里的代码
    // ...
}
```

###### `else if` 语句：

```solidity
uint256 x = 10;

if (x > 15) {
    // 如果 x 大于 15，则执行这里的代码
    // ...
} else if (x > 5) {
    // 如果 x 大于 5 且小于等于 15，则执行这里的代码
    // ...
} else {
    // 如果 x 不大于 5，则执行这里的代码
    // ...
}
```

##### 2. 循环语句：

###### `for` 循环：

```solidity
uint256 sum = 0;

for (uint256 i = 1; i <= 5; i++) {
    sum += i;
}

// 现在 sum 的值为 15
```

###### `while` 循环：

```solidity
uint256 i = 1;
uint256 sum = 0;

while (i <= 5) {
    sum += i;
    i++;
}

// 现在 sum 的值为 15
```

###### `do-while` 循环：

```solidity
uint256 i = 1;
uint256 sum = 0;

do {
    sum += i;
    i++;
} while (i <= 5);

// 现在 sum 的值为 15
```

##### 3. 中断语句：

###### `break` 语句：

```solidity
for (uint256 i = 0; i < 10; i++) {
    if (i == 5) {
        break; // 中断循环
    }
    // ...
}
```

###### `continue` 语句：

```solidity
for (uint256 i = 0; i < 10; i++) {
    if (i == 5) {
        continue; // 跳过当前迭代，继续下一次迭代
    }
    // ...
}
```

##### 4，常见错误

在Solidity中，控制语句最常见的错误之一是由于对Gas消耗的不正确估计而导致的交易失败。以下是一些可能导致问题的情况：

1. **Gas超限：** 如果循环次数过多或者在循环中执行的操作太昂贵，可能导致Gas消耗超过区块的Gas限制，从而使交易失败。

   ```solidity
   for (uint256 i = 0; i < 1e9; i++) {
       // 执行昂贵的操作
   }
   ```

2. **死循环：** 在循环中没有正确更新循环变量，导致循环永远无法退出。

   ```solidity
   while (true) {
       // 无法退出的循环
   }
   ```

3. **条件错误：** 在条件语句中使用了不正确的逻辑条件，导致代码执行不符合预期。

   ```solidity
   if (x = 5) {
       // 使用了赋值运算符而非相等运算符
   }
   ```

4. **缺少中断语句：** 在循环中缺少`break`或`return`语句，导致循环无法在预期条件下中断。

   ```solidity
   for (uint256 i = 0; i < 10; i++) {
       if (i == 5) {
           // 缺少 break 语句，循环将继续执行
       }
   }
   ```

5. **不正确的逻辑操作：** 在条件语句中使用了不正确的逻辑操作符，导致条件不符合预期。

   ```solidity
   if (x & y) {
       // 使用了位与运算符而非逻辑与运算符
   }
   ```

在编写Solidity代码时，特别要注意循环和条件语句的效率和正确性，以确保合约的安全性和预期行为。在循环中执行昂贵的操作时，可以考虑分批处理数据，以防止Gas消耗过大。同时，对条件语句的逻辑进行仔细检查，确保其符合预期。

#### 数据持久化

##### 存储数据结构

Solidity合约存储数据时一般采用以下数据结构：

1. ###### 状态变量：

   这是最基本的数据存储结构，声明在合约中，存储在区块链上，并在整个合约生命周期内存在。可以是简单的数据类型，也可以是数组、映射等复杂结构。

   ```solidity
   uint256 public counter; // 状态变量，存储计数器的值
   ```

2. ###### 数组：

    用于存储相同类型的数据集合。可以是动态数组（`uint256[]`）或静态数组（`uint256[5]`）。

   ```solidity
   uint256[] public dataList; // 数组，存储整数数据
   ```

3. ###### 映射：

   用于存储键值对的数据结构，类似于字典或哈希表。

   ```solidity
   mapping(address => uint256) public balances; // 映射，存储地址对应的余额
   ```

4. ###### 结构体： 

   用于定义自定义的数据结构，将多个数据字段组合在一起。

   ```solidity
   struct Person {
       string name;
       uint256 age;
   }
   
   Person public myPerson; // 结构体，存储个人信息
   ```

##### 数据存储位置

在Solidity中，数据的存储位置分为三种：`storage`、`memory` 和 `stack`。这些存储位置决定了数据的生命周期、访问成本以及可见性。

1. ###### **存储（Storage）：**
   
   - **位置：** 存储在区块链上，成为合约永久的一部分。
   - **生命周期：** 持久存在，不受函数调用的限制。
   - **适用场景：** 用于存储合约状态、永久数据。
   - **访问成本：** 修改存储数据是Gas消耗最高的操作。
   
   ```solidity
   contract StorageExample {
       uint256 public storedData; // 存储数据，永久保存在区块链上
   }
   ```
   
2. ###### **内存（Memory）：**
   
   - **位置：** 存储在函数执行期间的临时数据区域。
   - **生命周期：** 仅在函数执行期间存在，函数结束后清除。
   - **适用场景：** 用于临时计算、复制数据。
   - **访问成本：** 读取和写入内存数据的Gas消耗较低。
   
   ```solidity
   function addNumbers(uint256 a, uint256 b) public pure returns (uint256) {
       uint256 result; // 内存中的临时变量
       result = a + b;
       return result;
   }
   ```
   
3. ###### **栈（Stack）：**
   
   - **位置：** 存储在调用栈上，用于存储局部变量和函数调用信息。
   - **生命周期：** 仅在函数调用期间存在，函数结束后清除。
   - **适用场景：** 用于存储短期、局部的数据。
   - **访问成本：** 读取和写入栈数据的Gas消耗较低。
   
   ```solidity
   function multiplyNumbers(uint256 a, uint256 b) public pure returns (uint256) {
       uint256 result; // 栈中的局部变量
       result = a * b;
       return result;
   }
   ```
