# 以太坊Provider API

推荐阅读

我们建议所有 web3 站点开发人员阅读[基本用法](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#basic-usage)部分。

DOMWALLET Extension 将一个全局 API 注入到其用户访问的网站中`window.ethereum`。该 API 允许网站请求用户的以太坊账户，从用户连接的区块链读取数据，并建议用户签署消息和交易。Provider 对象的存在表示以太坊用户。

以太坊 JavaScript 提供程序 API 由[EIP-1193指定](https://eips.ethereum.org/EIPS/eip-1193).

#### 基本用法 <a href="#basic-usage" id="basic-usage"></a>

对于任何重要的以太坊网络应用程序——也就是 dapp、web3 站点等运行，你必须：

* 检测以太坊 Provider ( `window.ethereum`或 `window.domwallet.ethereum`)
* 检测用户连接到哪个以太坊网络
* 获取用户的以太坊账户
* 同时我们也支持 [EIP-6963](https://eips.ethereum.org/EIPS/eip-6963)

此页面顶部的代码段足以检测提供程序。Provider API 是您创建功能齐全的 web3 应用程序所需的一切。

您也可以使用一些使用起来更简单的库，而不是直接使用 Provider API，比如 [ethers](https://www.npmjs.com/package/ethers)。

#### Chain ID <a href="#chain-ids" id="chain-ids"></a>

这些是 DOMWALLET Extension 默认支持的以太坊链的 ID。咨询[chainid.network](https://chainlist.tokenpocket.pro/?from=old)更多。

十六进制

十进制

网络

0x1

1

以太坊主网（Mainnet）

0x38

56

币安智能链

0x80

128

火币生态链

0x42

66

OKExChain

0x89

137

Polygon(Matic)

0xa86a

43114

Avalanche C-Chain

0x141

321

KCC Mainnet

0x63564c40

1666600000

Harmony

0xfa

250

Fantom

0x2019

8217

Klaytn

0xa4b1

42161

Arbitrum

0x4e454152

1313161554

Aurora

0xa

10

Optimistic Ethereum

0x46

70

虎符智能链

0x406

1030

Conflux eSpace

0x504

1284

Moonbeam

0x64

100

Gnosis Chain (xDai)

#### Properties <a href="#properties" id="properties"></a>

**ethereum.isTokenPocket**

如果用户的浏览器安装了 DOMWALLET Extension 则返回`true`，否则返回`false`。

#### Methods <a href="#methods" id="methods"></a>

**ethereum.isConnected()**

提示

请注意，此方法与用户的帐户无关。

您可能经常会遇到“已连接”这个词，指的是 web3 站点是否可以访问用户的帐户。然而，在 Provider 接口中，“已连接”和“已断开”是指 Provider 是否可以向当前链发出 RPC 请求。

Copy

```
ethereum.isConnected(): boolean;
```

如果 Provider 连接到当前链，则返回 `true`，否则返回`false`。

如果 Provider 未连接，则必须重新加载页面才能重新建立连接。请参阅[`connect`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#connect)和[`disconnect`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#duan-kai)事件了解更多信息。

**ethereum.request(args)**

Copy

```
interface RequestArguments {
  method: string;
  params?: unknown[] | object;
}

ethereum.request(args: RequestArguments): Promise<unknown>
```

使用`request`通过 DOMWALLET Extension 向以太坊提交 RPC 请求。它返回`Promise`解析为 RPC 方法的调用结果。

`params`和返回值将因 RPC 方法而异。在实践中，如果一个方法有几个 `params`，它们几乎总是 `Array<any>`类型。

如果请求因任何原因失败，Promise 将拒绝并[返回 Ethereum RPC Error](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#errors)。

DOMWALLET Extension 支持大多数标准化的以太坊 RPC 方法，此外还有一些其他钱包可能不支持的方法。有关详细信息，请参阅 DOMWALLET Extension RPC API 文档。

**例子**

Copy

```
params: [
  {
    from: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',
    to: '0xd46e8dd67c5d32be8058bb8eb970870f07244567',
    gas: '0x76c0', // 30400
    gasPrice: '0x9184e72a000', // 10000000000000
    value: '0x9184e72a', // 2441406250
    data:
      '0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675',
  },
];

ethereum
  .request({
    method: 'eth_sendTransaction',
    params,
  })
  .then((result) => {
    // The result varies by RPC method.
    // For example, this method will return a transaction hash hexadecimal string on success.
  })
  .catch((error) => {
    // If the request fails, the Promise will reject with an error.
  });
```

#### Events <a href="#events" id="events"></a>

DOMWALLET Extension Provider 实现了[Node.js`EventEmitter`](https://nodejs.org/api/events.html)API。本节详细介绍了通过该 API 发出的事件，您可以监听这样的事件：

Copy

```
ethereum.on('accountsChanged', (accounts) => {
  // Handle the new accounts, or lack thereof.
  // "accounts" will always be an array, but it can be empty.
});

ethereum.on('chainChanged', (chainId) => {
  // Handle the new chain.
  // Correctly handling chain changes can be complicated.
  // We recommend reloading the page unless you have good reason not to.
  window.location.reload();
});
```

另外，一旦你开启监听器，不要忘记删除它们（例如在 React 或者 Vue 中卸载组件时）：

Copy

```
function handleAccountsChanged(accounts) {
  // ...
}

ethereum.on('accountsChanged', handleAccountsChanged);

// Later

ethereum.removeListener('accountsChanged', handleAccountsChanged);
```

`ethereum.removeListener`第一个参数是事件名称，第二个参数是对已传递给`ethereum.on`第一个参数中提到的事件名称的同一函数的引用。

**连接**

Copy

```
interface ConnectInfo {
  chainId: string;
}
ethereum.on('connect', handler: (connectInfo: ConnectInfo) => void);
```

当 DOMWALLET Extension Provider 第一次能够向链提交 RPC 请求时，它会发出此事件。我们建议使用`connect`事件处理程序和[`ethereum.isConnected()`方法](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-isconnected)来确定何时/是否连接了提供程序。

**断开**

Copy

```
ethereum.on('disconnect', handler: (error: ProviderRpcError) => void);
```

如果因为网络连接问题或者是未知的错误导致了 DOMWALLET Extension Provider 无法向任何链发送 RPC 请求的时候就会发出这个事件。

并且一旦disconnect这个事件发出之后 Provider 将不能再接受任何新的请求，除非重新加载页面并且建立连接，我们还提供了 [`ethereum.isConnected()`方法](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-isconnected)来确定提供程序是否已断开连接。

**帐户更改**

Copy

```
ethereum.on('accountsChanged', handler: (accounts: Array<string>) => void);
```

每当 eth\_accounts RPC 方法的返回值发送变化的时候 DOMWALLET Extension 提供程序都会发出这个事件。返回值是一个空数组或者值为十六进制账户地址的数组，

这就是说您可以监听`accountsChanged`来得到用户每次切换的地址。

**链改变**

提示

有关DOMWALLET Extension 的默认链及其 chain ID，请参阅[Chain ID 部分](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#chain-ids)。

Copy

```
ethereum.on('chainChanged', handler: (chainId: string) => void);
```

**信息**

Copy

```
interface ProviderMessage {
  type: string;
  data: unknown;
}

ethereum.on('message', handler: (message: ProviderMessage) => void);
```

DOMWALLET Extension Provider 在收到一些应该通知用户的消息时发出此事件。消息的种类由字符串标识。

RPC 订阅更新是该 `message` 事件的常见用例。例如，如果使用 `eth_subscribe` 创建订阅，则每个订阅更新都将作为一个消息事件发出，该消息事件的类型为 `eth_subscribe`。

#### Errors <a href="#errors" id="errors"></a>

DOMWALLET Extension 提供程序抛出或返回的所有错误都遵循此接口：

Copy

```
interface ProviderRpcError extends Error {
  message: string;
  code: number;
  data?: unknown;
}
```

该[`ethereum.request(args)`方法](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args)抛出错误。您通常可以使用 error`code`属性来确定请求失败的原因。常见代码及其含义包括：

* `4001`
  * 请求被用户拒绝
* `-32602`
  * 参数无效
* `-32603`
  * 内部错误

有关错误的完整列表，请参阅[EIP-1193](https://eips.ethereum.org/EIPS/eip-1193#provider-errors)和[EIP-1474](https://eips.ethereum.org/EIPS/eip-1474#error-codes).

提示

这[`eth-rpc-errors` ](https://npmjs.com/package/eth-rpc-errors)包实现了 DOMWALLET Extension Provider 抛出的所有 RPC 错误，并且可以帮助您识别它们的含义。

#### 旧版 API <a href="#legacy-api" id="legacy-api"></a>

警告

在实践中，您**永远**不应依赖这些方法、属性或事件中的任何一个。

本节记录了我们的旧提供程序 API。DOMWALLET Extension 仅在 Provider API 通过[EIP-1193标准化之前支持此 API](https://eips.ethereum.org/EIPS/eip-1193)2020 年。因此，您可能会发现使用此 API 的 web3 站点或其他实现它的 providers。

#### 旧版属性 <a href="#legacy-properties" id="legacy-properties"></a>

**ethereum.chainId（已弃用）**

警告

此属性是非标准的，因此已弃用。

如果您需要检索当前 chain ID，请使用[`ethereum.request({ method: 'eth_chainId' })`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args). [`chainChanged`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#chainchanged)有关如何处理 chain ID 的更多信息，另请参阅事件。

此属性的值可以随时更改。

表示当前链 ID 的十六进制字符串。

**ethereum.networkVersion（已弃用）**

警告

您应该使用 chain ID 而不是 network ID。

如果您必须获取 network ID，请使用[`ethereum.request({ method: 'net_version' })`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args).

此属性的值可以随时更改。

表示当前区块链 network ID 的十进制字符串。

**ethereum.selectedAddress（已弃用）**

警告

改为使用[`ethereum.request({ method: 'eth_accounts' })`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args)。

此属性的值可以随时更改。

返回代表用户“当前选择”地址的十六进制字符串。

“当前选择的”地址是返回的数组中的第一项`eth_accounts`。

#### 传统方法 <a href="#legacy-methods" id="legacy-methods"></a>

**ethereum.enable()（已弃用）**

警告

改为使用[`ethereum.request({ method: 'eth_requestAccounts' })`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args)。

**ethereum.sendAsync()（已弃用）**

警告

改为使用[`ethereum.request()`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-request-args)。

Copy

```
interface JsonRpcRequest {
  id: string | undefined;
  jsonrpc: '2.0';
  method: string;
  params?: Array<any>;
}

interface JsonRpcResponse {
  id: string | undefined;
  jsonrpc: '2.0';
  method: string;
  result?: unknown;
  error?: Error;
}

type JsonRpcCallback = (error: Error, response: JsonRpcResponse) => unknown;

ethereum.sendAsync(payload: JsonRpcRequest, callback: JsonRpcCallback): void;
```

这是`ethereum.request` 以前的方法。 它仅适用于 JSON-RPC 方法，并将 JSON-RPC 请求负载对象和错误优先回调函数作为其参数。

查看[以太坊 JSON-RPC API](https://eips.ethereum.org/EIPS/eip-1474)详情。

**ethereum.send()（已弃用）**

警告

改为使用`ethereum.request()`。

Copy

```
ethereum.send(
  methodOrPayload: string | JsonRpcRequest,
  paramsOrCallback: Array<unknown> | JsonRpcCallback,
): Promise<JsonRpcResponse> | void;
```

这种方法的行为不可预测，请避免使用它。它本质上是[`ethereum.sendAsync()`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/ethereum-provider-api#ethereum-sendasync-deprecated).

`ethereum.send()`可以通过三种不同的方式调用：

Copy

```
// 1.
ethereum.send(payload: JsonRpcRequest, callback: JsonRpcCallback): void;

// 2.
ethereum.send(method: string, params?: Array<unknown>): Promise<JsonRpcResponse>;

// 3.
ethereum.send(payload: JsonRpcRequest): unknown;
```

您可以将这些签名视为如下：

1. 这个签名一模一样`ethereum.sendAsync()`
2. 这个签名就像一个`ethereum.sendAsync()`带有`method`和`params`作为参数的异步，而不是 JSON-RPC 有效负载和回调
3. 此签名使您能够同步调用以下 RPC 方法：
   * `eth_accounts`
   * `eth_coinbase`
   * `eth_uninstallFilter`
   * `net_version`
