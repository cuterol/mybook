# 发送交易

本章将详细描述发送交易的方法和参数。交易是区块链上的正式行动，通过调用 `eth_sendTransaction` 方法在 DOMWALLET Extension 中启动。它们涉及到的简单的以太币发送、代币发送、创建新的智能合约来改变区块链上的状态。通过外部账户的签名或者秘钥对启动。

在 DOMWALLET Extension 中，可以直接使用 `ethereum.request`方法，发送交易将涉及组合一个选项对象，如下所示：

Copy

```
const transactionParameters = {
  nonce: '0x00', // ignored by DOMWALLET Extension 
  gasPrice: '0x09184e72a000', // customizable by user during DOMWALLET Extension confirmation.
  gas: '0x2710', // customizable by user during DOMWALLET Extension confirmation.
  to: '0x0000000000000000000000000000000000000000', // Required except during contract publications.
  from: ethereum.selectedAddress, // must match user's active address.
  value: '0x00', // Only required to send ether to the recipient from the initiating external account.
  data:
    '0x7f7465737432000000000000000000000000000000000000000000000000000000600057', // Optional, but used for defining smart contract creation and interaction.
  chainId: '0x3', // Used to prevent transaction reuse across blockchains. Auto-filled by TokenPocket Extension.
};

// txHash is a hex string
// As with any RPC call, it may throw an error
const txHash = await ethereum.request({
  method: 'eth_sendTransaction',
  params: [transactionParameters],
});
```

例子

HTMLJavaScriptCopy

```
<button class="enableEthereumButton btn">Enable Ethereum</button>
<button class="sendEthButton btn">Send Eth</button>
```

#### 交易参数 <a href="#transaction-parameters" id="transaction-parameters"></a>

DOMWALLET Extension 会为您处理许多交易参数，但最好了解所有参数的作用。

**Nonce \[忽略]**

DOMWALLET Extension 忽略此字段。

在以太坊中，每笔交易都有一个随机数。这样一来，每笔交易只能由区块链处理一次。此外，要成为有效交易，nonce 必须是`0`，或者之前编号的交易必须已经被处理。

这意味着事务总是按给定帐户的顺序处理，因此增加随机数是一个非常敏感的问题，很容易搞砸，尤其是当用户使用同一个帐户与多个应用程序进行交互时，可能会跨越多个设备。

由于这些原因，DOMWALLET Extension 目前不向应用程序开发人员提供任何方式来自定义其建议的交易的 nonce。

**Gas Price \[可选]**

可选参数 - 最好用于私有区块链。

在以太坊中，每笔交易都指定了它所消耗的 gas price。为了最大化他们的利润，区块生产者将在创建下一个区块时首先选择具有更高 gas price 的待处理交易。这意味着较高的 gas price 通常会使您的交易处理得更快，但代价是更高的交易费用。请注意，这可能不适用于例如第 2 层网络，它可能具有恒定的 gas price或根本没有 gas price。

换句话说，虽然您可以在 DOMWALLET Extension 的默认网络上忽略此参数，但您可能希望在您的应用程序中包含它。在我们的默认网络上，DOMWALLET Extension 允许用户在“慢”、“中”和“快”选项之间选择他们的 gas price。

**Gas Limit \[可选]**

可选参数。对 Dapp 开发人员很少有用。

gas limit 是一个高度可选的参数，我们会自动为它计算一个合理的价格。 如果出于某种原因您自定义了 gas limit，你的智能合约可能会受益于此，获得更少的gas消耗。

**To \[半可选]**

十六进制编码的以太坊地址。与接收方的交易（除合约创建之外的所有交易）是必需的。

**Value \[可选]**

要发送的网络本机货币的十六进制编码值。在以太坊主网络上，这是[以太 ](https://ethereum.org/en/eth/)，&#x4EE5;_&#x77;e&#x69;_&#x8BA1;价，即`1e-18`以太币。

请注意，以太坊中经常使用的这些数字的精度远高于原生 JavaScript 数字，如果没有预料到，可能会导致不可预测的行为。因此，我们强烈推荐使用 [BN.js](https://github.com/indutny/bn.js/) 在操作用于区块链的值时。

**Data \[半可选]**

创建智能合约所必需的。

该字段还用于指定合约方法及其参数。您可以了解有关如何[在solidity ABI 规范中对数据进行编码的更多信息](https://solidity.readthedocs.io/en/develop/abi-spec.html).

**Chain ID \[当前忽略]**

Chain ID 当前由用户当前选择的网络在`ethereum.networkVersion`. 将来我们可能会允许一种方式同时连接到多个网络，此时此参数将变得很重要，因此养成现在包含的习惯可能会很有用。
