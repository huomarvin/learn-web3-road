![Image](./img/173656651-a46df615-8ec3-43fd-9619-98647a6d2bd2.png)

Solidity是一种面向对象的高级语言，用于实现智能合约。它被设计为针对以太坊虚拟机（EVM）。
它是静态类型的，支持继承、库和复杂的用户定义类型以及其他功能。

## 使用Solidity构建智能合约

### 初始化智能合约

```solidity
// 定义你使用的编译器版本
pragma solidity ^0.8.10;

// 创建一个名为HelloWorld的智能合约
contract HelloWorld {

}
```

### 变量和类型

在`solidity`中有三种类型的变量

- 本地变量
  - 在函数中声明，不存储在区块链上
- 状态变量
  - 在一个函数之外声明，以维护智能合约的状态。
  - 存储在区块链上
- 全局变量
  - 提供有关区块链的信息。它们是由以太坊虚拟机在运行时注入的。
  - 包括诸如交易发送者、区块时间戳、区块哈希等内容。
  - [Examples of global variables](https://docs.soliditylang.org/en/v0.8.17/units-and-global-variables.html)

变量的范围是由它们被声明的地方定义的，而不是它们的值。将一个局部变量的值设置为全局变量并不能使其成为全局变量，因为它仍然只能在其范围内被访问。

```solidity
// 定义你使用的编译器版本
pragma solidity ^0.8.10;

// 创建一个名为Variables的合约
contract Variables {
    /*
        ******** 状态变量 **********
    */
    /*
    uint代表着无符号整型，代表着没有负数， 下面代表着不同的区间
        - uint8   ranges from 0 to 2 ** 8 - 1
        - uint256 ranges from 0 to 2 ** 256 - 1
    `public` 意味着既可以在合约内部访问，也可以被外部访问
    */
    uint8 public u8 = 10;
    uint public u256 = 600;
    uint public u = 1230; // uint 是 uint256的别名

    /*
    int类型可以包含负数 Eg
    - int256 ranges from -2 ** 255 to 2 ** 255 - 1
    */
    int public i = -123; // int和int256是相同的

    // address代表着以太坊的地址
    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    // 布尔变量
    bool public defaultBoo1 = false;

    // 默认值
    // 在solidity中没有被初始化的变量是有默认值的
    bool public defaultBoo2; // false
    uint public defaultUint; // 0
    int public defaultInt; // 0
    address public defaultAddr; // 0x0000000000000000000000000000000000000000

    function doSomething() public {
        /*
        ******** 本地变量 **********
        */
        uint ui = 456;

        /*
        ******** 全局变量 **********
        */

        /*
            block.timestamp代表着当前区块的产生时间
            msg.sender代表着调用当前函数的地址
        */
        uint timestamp = block.timestamp; // Current block timestamp
        address sender = msg.sender; // address of the caller
    }
}
```

### 函数、循环和If/Else

```solidity
// 定义当前使用的编译器版本
pragma solidity ^0.8.10;

// 定义名为Conditions的合约
contract Conditions {
    // 存储number类型的状态变量
    uint public num;

    /*
        函数名为set
        接受一个uint类型的参数，并赋值给状态变量num
        声明为public意味着在合约内外部均可被调用
    */
    function set(uint _num) public {
        num = _num;
    }

    /*
        函数名为get，返回num的值
        被声明为view意味着这个函数不能更改任意变量的类型
        被view声明的函数是不需要gas fee的
    */
    function get() public view returns (uint) {
        return num;
    }

    /*
    		函数名为foo，接受一个uint类型的变量x并且返回uint类型的值
    		使用if else来比较x的值
    */
    function foo(uint x) public returns (uint) {
        if (x < 10) {
            return 0;
        } else if (x < 20) {
            return 1;
        } else {
            return 2;
        }
    }

    /*
        函数名为loop，会运行10次循环
    */
    function loop() public {
        // for循环
        for (uint i = 0; i < 10; i++) {
            if (i == 3) {
                // 跳到下一次循环
                continue;
            }
            if (i == 5) {
                // break语句退出循环
                break;
            }
        }
    }
}
```

### 数组和字符串

数组有定长和非定长两种

```solidity
pragma solidity ^0.8.10;

contract Array {
    // 声明一个public类型的字符串变量
    string public greet = "Hello World!";
    // 初始化数组的几种方式
    // 数组在这里初始化被认为是状态变量，值会被存储在链上
    // 它们被称为 `storage variables`
    uint[] public arr;
    uint[] public arr2 = [1, 2, 3];
    // 定长数组，所有元素都被初始化为0
    uint[10] public myFixedSizeArr;
    /*
        函数名为get,获取存储在array索引上的value值
    */
    function get(uint i) public view returns (uint) {
        return arr[i];
    }
		/*
		 Solidity可以返回整个数组
		 这个函数被调用，并且返回一个`uint[] memory`类型的数组
		 memory - 这个值仅仅存储在内存中而不是区块链上，只在函数被执行区间存在
		 Memory variables 和 Storage variables 可以理解成内存和硬盘。
     Memory variables 只有在函数执行的时候临时存在，然而Storage variables在合约存在期间是一直存在的。
     这里的数组仅在执行期间存在，所以被声明为memory
    */
    function getArr(uint[] memory _arr) public view returns (uint[] memory) {
        return _arr;
    }


    /*
     		这个函数的返回值是 string memory.这样声明的原因是返回值仅在函数调用期间存在，同上面
    */
    function foo() public returns (string memory) {
        return "C";
    }


    function doStuff(uint i) public {
        // 追加到数组中
        // 数组长度+1
        arr.push(i);
        // 删除数组的最后一个元素
        // 数组长度-1
        arr.pop();
        // 获取数组长度
        uint length = arr.length;
        // delete操作并不能更改数组的长度，只是将其值更改为默认值
        // 下面的操作将arr2下标为1的元素设置为0
        uint index = 1;
        delete arr2[index];
        // 在内存中创建一个长度为5的定长数组
        uint[] memory a = new uint[](5);
        // 在内存中创建一个字符串
        string memory hi = "hi";
    }
 }
```

## 参考

[Solidity by Example](https://solidity-by-example.org/)

## 额外的学习资源

- [Cryptozombies](https://cryptozombies.io/)
- [Solidity by Example](https://solidity-by-example.org/)
- [Solidity docs](https://docs.soliditylang.org/en/v0.8.17/)