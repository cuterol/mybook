# 签署数据

#### 使用 DOMWALLETExtension 签署数据 <a href="#signing-data-with-metamask" id="signing-data-with-metamask"></a>

如果您想跳转到一些工作签名示例，[可以访问此存储库](https://github.com/danfinlay/js-eth-personal-sign-examples).

如果你想阅读这些方法的 JavaScript 实现，它们都可以在 npm 包[eth-sig-util 中找到](https://github.com/MetaMask/eth-sig-util).

#### 介绍 <a href="#jie-shao" id="jie-shao"></a>

DOMWALLETExtension 目前有六种签名方法，我们目前的五种方法是：

* `eth_sign`
* `personal_sign`
* `signTypedData`（目前与 相同`signTypedData_v1`）
* `signTypedData_v1`
* `signTypedData_v3`
* `signTypedData_v4`

`eth_sign`是一种开放式签名方法，允许对任意散列进行签名，这意味着它可用于对交易或任何其他数据进行签名，使其具有危险的网络钓鱼风险。

出于这个原因，我们通常不鼓励在生产中使用这种方法。但是，一些应用程序出于易用性的考虑而使用此方法，因此我们支持它，以免破坏活动项目的工作流程。

最终，`personal_sign` [规范](https://github.com/ethereum/go-ethereum/pull/2940)被提议，它为数据添加了一个前缀，因此它不能模拟交易。我们还使此方法能够在 UTF-8 编码时显示人类可读的文本，使其成为站点登录的流行选择。

#### 签署类型的数据 v1 <a href="#sign-typed-data-v1" id="sign-typed-data-v1"></a>

这个早期版本的规范缺乏一些后来的安全改进，通常应该被忽略以支持[signTypedData\_v3](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/signing-aata#qian-shu-lei-xing-de-shu-ju-v3)。

也称为`signTypedData，`这种方法是原始的以状态通道为中心的签名方法。

该`signTypedData`系列有几个主要的设计考虑：

* 在链上验证便宜
* 还是有点可读性
* 难以钓鱼签名

如果链上可验证性成本对您来说是一个高优先级，您可以考虑使用它。

#### 签署类型的数据 v3 <a href="#qian-shu-lei-xing-de-shu-ju-v3" id="qian-shu-lei-xing-de-shu-ju-v3"></a>

该方法`signTypedData_v3`当前代表[EIP-712 规范的最新版本](https://eips.ethereum.org/EIPS/eip-712)，使其成为我们迄今为止签署成本低廉的链上数据的最安全方法。

#### 签署类型的数据 v4 <a href="#sign-typed-data-v4" id="sign-typed-data-v4"></a>

该方法`signTypedData_v4`当前代表[EIP-712 规范的最新版本](https://eips.ethereum.org/EIPS/eip-712)，增加了对数组的支持，并对结构的编码方式进行了重大修复。

**符号类型数据消息参数**

`domain`：域或域签名很重要，因为它：

* 仅适用于特定网站/合约。
* 确保签名仅在预期有效的地方有效。
* 允许您拥有验证地址的唯一合约。
* 这是一组限制签名有效位置的信息。
* 这是有效性域。可以是合约、网址等。
* DApp 告诉你的具体需要放在这里。
* 确保您的签名不会与其他签名发生冲突。

`chainId`：chainId 告诉你你在哪个链上，这很重要，因为：

* 它确保在 Rinkeby 上的签名在另一条链上无效，例如以太坊主网。

`name`：这主要是出于 UX（用户体验）目的。

* 例如，作为用户，您正在使用 Ether Mail 应用程序并出现一个用于加密猫交换的对话框，由于签名上的名称，这会引起怀疑。

`verifyingContract`: 这是额外的保证。即使两个开发人员最终创建了一个同名的应用程序，他们也永远不会拥有相同的合约地址。

* 如果您不确定名称，这将显示负责消息验证的合约。
* 该字段也将采用 url。

`version`: 这告诉你当前版本的域对象。

`message`: 对你想要的结构完全开放。每个字段都是可选的。

下面是使用 DOMWALLETExtension 签署类型数据的示例。参考[这里](https://github.com/danfinlay/js-eth-personal-sign-examples)

**例子**

HTMLJavaScriptCopy

```
<div>
  <h3>Sign Typed Data V4</h3>
  <button type="button" id="signTypedDataV4Button">sign typed data v4</button>
</div>
```
