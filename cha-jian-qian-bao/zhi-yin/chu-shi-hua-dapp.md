# 初始化Dapp

[设置好](https://help.tokenpocket.pro/developer-cn/extension-wallet/guide/start)基本的开发环境后，您就可以开始与一些智能合约进行交互了。在与智能合约通信时，下面几个是你必须要要详细阅读的重要信息：

#### 合约网络 <a href="#the-contract-network" id="the-contract-network"></a>

首先应该确保您已经连接到了正确的网络，否则您不可能向您的合约发送任何交易！另外您最好把合约先部署到测试网，以避免您在主网开发的和测试的时候出现不必要的费用。

无论您的 dapp 最终部署在哪个网络上，您的用户都必须能够访问它。DOMWALLET Extension 提供了切换链 wallet\_switchEthereumChain，添加链 wallet\_addEthereumChain 的方法，它会自动提示用户添加您建议的链并且添加之后还会提示用户切换到添加的链。

#### 合约地址 <a href="#the-contract-address" id="the-contract-address"></a>

以太坊中的每个账户都有一个地址，无论是外部密钥对账户还是智能合约。为了让任何智能合约库与您的合约进行通信，他们需要知道其确切地址。

#### 合约 ABI <a href="#the-contract-abi" id="the-contract-abi"></a>

在以太坊中，[ABI 规范](https://docs.soliditylang.org/en/develop/abi-spec.html) 是一种以用户界面可理解的方式对智能合约界面进行编码的方法。它是一组描述方法的对象，当你将这个和地址输入合约抽象库时，`ABI`会告诉这些库要提供哪些方法，以及如何组合事务来调用这些方法。

示例库包括：

* [ethers](https://www.npmjs.com/package/ethers)
* [web3.js](https://www.npmjs.com/package/web3)
* [Embark](https://framework.embarklabs.io/)
* [ethjs](https://www.npmjs.com/package/ethjs)
* [truffle](https://trufflesuite.com/)

#### 合约 Bytecode <a href="#the-contract-bytecode" id="the-contract-bytecode"></a>

如果您的 Web 应用程序要发布一个预编译的新智能合约，它可能需要包含一些`bytecode`. 在这种情况下，您无法提前知道合约地址，而是必须发布、观察要处理的交易，然后从完成的交易中提取最终合约的地址。

如果从`bytecode`发布合约，您仍然需要一个`ABI`来与之交互，`bytecode`没有描述如何与最终合约进行交互。

#### 合约源代码 <a href="#the-contract-source-code" id="the-contract-source-code"></a>

如果您的网站将允许用户编辑智能合约源代码并进行编译，例如[Remix](http://remix.ethereum.org/)，您可以导入整个编译器。在这种情况下，您将从该源代码中派生出您的`bytecode`和 ABI，最终您将从已完成的交易中获得合约的地址，即发布该`bytecode`的地方。
