这是一个关于如何创建一个前端、部署一个Solidity智能合约并将它们连接在一起的分步教程。在本例中我们将使用Metamask、Remix IDE和Ethers.js。

在本教程结束时，你将能够创建一个简单的HTML页面，其中的按钮可以与智能合约功能互动。

本教程分3个阶段进行

- 创建一个基本的HTML网页
- 创建一个基本的 Solidity 智能合约
- 使用Ethers.js将网页与智能合约连接起来。

### 准备

- 下载并安装MetaMask

  - 从未使用过Metamask？（观看这个[视频](https://youtu.be/wlm4QcA8c4Q?t=66)）

​	点击顶部的Ethereum Mainnet。改为Goerli Testnet，并在你的Metamask钱包上获得一份该账户的公共地址。

- 从水龙头获取一些测试以太币
  - [Faucet link to request funds](https://goerlifaucet.com/)
  - [Blog explaining a faucet and how to use one](https://blog.b9lab.com/when-we-first-built-our-faucet-we-deployed-it-on-the-morden-testnet-70bfbf4e317e)

- 安装一个代码编辑器。
  - 你可以使用任何你想要的编辑器，但我们建议安装Visual Studio Code，因为它有一些有用的扩展和特性。这些扩展是可选的，但我们推荐以下这些。
    - [Solidity](https://marketplace.visualstudio.com/items?itemName=NomicFoundation.hardhat-solidity) -Solidity语法高亮
    - [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) - 允许你运行一个本地的server去测试 HTML/CSS/JS文件
    - [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) - 代码格式化工具
    - [npm IntelliSense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense) - 自动完成导入语句的npm模块
    - [IntelliSense for CSS class names in HTML](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) - 自动提示class
    - [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) - 查看每行代码开发人员的工具

- 安装一个HTTP服务器。使用任何你喜欢的，推荐使用`lite-server`。

  - 安装Node.js ([Download and Instructions](https://nodejs.org/en/download/) )
  - 安装 lite-server

  ```bash
  # 这个命令将会全局安装lite-server
  npm install -g lite-server
  ```

### 创建和提供一个简单的网页

第一步是创建一个简单的HTML页面

1. 用`mkdir <目录名>`在你的终端创建一个新的文件夹（目录）。

2. 在一个代码编辑器（如Visual Studio Code）中，打开该文件夹

3. 创建一个名为index.html的新文件

4. 打开index.html

5. 创建HTML模板

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta http-equiv="X-UA-Compatible" content="IE=edge" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <title>My First dApp</title>
     </head>
     <body></body>
   </html>
   ```

   我们将创建一个应用程序，简单地读取和写入一个值到区块链上。

6. 在body标签内，添加一些文本，一个标签和输入。

   ```html
   <body>
     <div>
       <h1>This is my dApp!</h1>
       <p>Here we can set or get the mood:</p>
       <label for="mood">Input Mood:</label> <br />
       <input type="text" id="mood" />
     </div>
   </body>
   ```

7. 在DIV标签旁边添加一些按钮和一个标签来显示心情。

   ```html
   <button onclick="getMood()">Get Mood</button>
   <button onclick="setMood()">Set Mood</button>
   <p id="showMood"></p>
   ```

   在<head>标签内，添加一些样式，使其看起来更漂亮。

   ```html
   <style>
     body {
       text-align: center;
       font-family: Arial, Helvetica, sans-serif;
     }
   
   
     div {
       width: 20%;
       margin: 0 auto;
       display: flex;
       flex-direction: column;
     }
   
   
     button {
       width: 100%;
       margin: 10px 0px 5px 0px;
     }
   </style>
   ```

8. 在项目根目录下运行`lite-server`开启静态资源服务

9. 在你的浏览器中进入http://127.0.0.1:3000/，查看你的页面!

   > 端口有可能会有波动，`lite-server`会监测3000端口是否会监听，如果已经被监听过，会自动+1，即3001

10. 你的前端现在已经完成了!

### 创建一个基础的合约

首先创建一个智能合约

1. 此次使用在线IDE [Remix](https://remix.ethereum.org/)

2. 进入[Remix](https://remix.ethereum.org/)

3. 查看 "Solidity Compiler "和 "Deploy and Run Transactions "标签。如果它们不存在，在插件管理器中启用它们

4. 在remix中创建一个新的solidity文件，命名为mood.sol

5. 写一个智能合约

   1. 指定solidity版本并添加许可证

      ```solidity
      // SPDX-License-Identifier: MIT
      pragma solidity ^0.8.1;
      ```

   2. 定义合约

      ```solidity
      contract MoodDiary{
        // 这里完成合约的内容
      }
      ```

   3. 在合同中，创建一个名为 `mood`的变量

      ```solidity
      string mood;
      ```

   4. 创建读写函数

      ```solidity
      // 创建一个函数将mood的值存储到状态变量上
      function setMood(string memory _mood) public{
          mood = _mood;
      }
      
      // 创建一个函数从链上读取状态变量mood
      function getMood() public view returns(string memory){
          return mood;
      }
      ```

   5. 现在你的代码看起来像[这样]()

6. 在Goerli Testnet上部署合同。

   1. 确保你的Metamask已经连接到Goerli Testnet上。
   2. 确保你选择了正确的编译器版本来匹配 solidity 合同。(在编译标签中)
   3. 使用 "Solidity Compiler "标签编译代码。请注意，加载编译器可能需要一些时间
   4. 在 "部署和运行交易 "标签下部署合约
   5. 在 "部署的合约 "部分，您可以在 "Remix Run "选项卡上测试您的功能，以确保您的合约按预期运行

   请确保在`Injected Provider - MetaMask`环境下通过Remix在Goerli上进行部署，并在Metamask中确认部署事务。

制作一个新的临时文件来保存。

- 部署的合同的地址
  - 通过remix的 "运行 "标签中部署的合同下拉菜单旁边的复制按钮来复制它。
- 合同的ABI(什么是[ABI]([what is that?](https://solidity.readthedocs.io/en/develop/abi-spec.html)))
  - 通过remix的编译标签中合同下面的复制按钮来复制它。

### 将你的网页连接到你的智能合约

回到你的本地文本编辑器中的index.html，在你的html页面中添加以下代码。

1. 将Ethers.js源代码导入你的index.html页面，在一组新的脚本标签内。

2. 在脚本标签内，导入合同ABI（[那是什么？](https://solidity.readthedocs.io/en/develop/abi-spec.html)），并指定我们供应商的区块链上的合同地址。

   ```js
   const MoodContractAddress = "<contract address>";
   const MoodContractABI = <contract ABI>
   let MoodContract;
   let signer;
   ```

   对于合同ABI，我们要特别导航到[JSON部分](https://docs.soliditylang.org/en/develop/abi-spec.html#json)。我们需要用JSON格式来描述我们的智能合约。

   由于我们有两个方法，这应该以一个数组开始，有两个对象。

   ```js
   const MoodContractABI = [{}, {}]
   ```

   从上述页面来看，每个对象应该有以下字段 :`constant`, `inputs`, `name`, `outputs`, `payable`, `stateMutability` and `type`.

   - `setMood` 函数:

     - name: `setMood`

     - type: `function`

     - outputs: 因为没有返回任何东西，所以是一个空数组`[]`

     - stateMutability: 这是 `nonpayable` 的，因为这个函数没有接受`ether`

     - inputs: 这是一个函数输入的数组，数组中的每个对象都应该有 `internalType`, `name`, `type`,  它们分别是`string`, `_mood` , `string` 

   - `getMood`函数

     - name: `getMood`

     - type: `function`

     - outputs: 这里和`setMood`的`input`具有同样的类型。分别为 `internalType`, `name` , `type`, 值为`string`, `""`和`string`

     - stateMutability: 这是`view` 因为这是一个`view`函数

     - inputs: 此函数没有参数所以入参为空数组 `[]`

   结果如下

   ```js
   const MoodContractABI = [
   	{
   		"inputs": [],
   		"name": "getMood",
   		"outputs": [
   			{
   				"internalType": "string",
   				"name": "",
   				"type": "string"
   			}
   		],
   		"stateMutability": "view",
   		"type": "function"
   	},
   	{
   		"inputs": [
   			{
   				"internalType": "string",
   				"name": "_mood",
   				"type": "string"
   			}
   		],
   		"name": "setMood",
   		"outputs": [],
   		"stateMutability": "nonpayable",
   		"type": "function"
   	}
   ]
   ```

   

3. 下一步，定义一个以太坊供应商。在我们的例子中，它是Goerli。

   ```js
   const provider = new ethers.providers.Web3Provider(window.ethereum, "goerli");
   ```

4. 请求访问用户的钱包，使用`metamask`连接你的账户（我们使用[0]作为默认），并使用你的合同地址、ABI和签名人定义合同对象。

   ```JS
   provider.send("eth_requestAccounts", []).then(() => {
     provider.listAccounts().then((accounts) => {
       signer = provider.getSigner(accounts[0]);
       MoodContract = new ethers.Contract(
         MoodContractAddress,
         MoodContractABI,
         signer
       );
     });
   });
   ```

5. 创建异步函数调用你的智能合约函数

   ```js
   async function getMood() {
     const getMoodPromise = MoodContract.getMood();
     const Mood = await getMoodPromise;
     document.getElementById("showMood").innerText = `Your Mood: ${Mood}`;
     console.log(Mood);
   }
   
   async function setMood() {
     const mood = document.getElementById("mood").value;
     const setMoodPromise = MoodContract.setMood(mood);
     await setMoodPromise;
   }
   ```

6. 将函数和html代码进行绑定

   ```html
   <button onclick="getMood()">Get Mood</button>
   <button onclick="setMood()">Set Mood</button>
   ```

### 测试你的工作!

1. 你的网络服务器建立起来了吗？在你的浏览器中进入http://127.0.0.1:3000/，看看你的页面!
2. 测试你的功能并根据需要通过Metamask批准交易。注意区块时间是~15秒......所以要等一下才能读取区块链的状态。
3. 通过https://goerli.etherscan.io/ 查看你的合约和交易信息
4. 在浏览器中打开一个控制台（Ctrl + Shift + i），当你按下这些按钮时，可以看到魔法的发生

### DONE!

如果你在学习教程时遇到困难，你可以尝试一下所提供的示例应用程序。

```bash
git clone https://github.com/uuuhds/BasicFrontEndTutorial.git
cd BasicFrontEndTutorial
lite-server
```

尝试使用以下信息与我们在Sepolia testnet上发布的现有合同进行互动（你的将在Goerli上）。

- 我们在[这个交易](https://sepolia.etherscan.io/tx/0x59285315297233a17996a7f4cfb281f2fcc365d7eaeba93a67edd0ff2f9e8d8b)中创建了一个`MoodDiary`合同实例。
- 这里是合同（[在etherscan上](https://sepolia.etherscan.io/address/0x5cb10f2de3805bc3e0a8035148a1d998e4509fc7)）。
  - 我们也验证了我们的源代码到sepolia.etherscan.io，作为一个额外的措施，让你验证合同到底是什么，同时ABI也是向世界提供的!
- ABI也在这个[文件](https://github.com/uuuhds/BasicFrontEndTutorial/blob/main/Mood_ABI.json)中。

这说明了一个重要的问题：你也可以建立一个DApp，而不需要自己编写Ethereum合约！如果你想使用一个已经写好的并且已经在Ethereum上的合约也是可以的。