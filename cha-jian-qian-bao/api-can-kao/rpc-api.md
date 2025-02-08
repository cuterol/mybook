# RPC API

DOMWALLET Extension 使用`ethereum.request(args)`方法包装一个 RPC API。

该 API 基于所有以太坊客户端公开的接口，以及其他钱包可能支持或不支持的越来越多的方法。

提示

所有 RPC 方法请求都可能返回错误。确保处理每次调用的错误`ethereum.request(args)`。

#### 以太坊 JSON-RPC 方法 <a href="#ethereum-json-rpc-methods" id="ethereum-json-rpc-methods"></a>

对于 Ethereum JSON-RPC API，请参阅[Ethereum wiki](https://eth.wiki/json-rpc/API#json-rpc-methods).

此 API 的重要方法包括：

* [`eth_accounts`](https://eth.wiki/json-rpc/API#eth_accounts)
* [`eth_call`](https://eth.wiki/json-rpc/API#eth_call)
* [`eth_getBalance`](https://eth.wiki/json-rpc/API#eth_getbalance)
* [`eth_sendTransaction`](https://eth.wiki/json-rpc/API#eth_sendtransaction)
* [`eth_sign`](https://eth.wiki/json-rpc/API#eth_sign)

#### 受限方法 <a href="#restricted-methods" id="restricted-methods"></a>

DOMWALLETExtension 通过[EIP-2255引入 Web3 钱包权限](https://eips.ethereum.org/EIPS/eip-2255). 在这个权限系统中，每个 RPC 方法要么&#x662F;_&#x53D7;限&#x5236;_&#x7684;，要么&#x662F;_&#x4E0D;受限制的_。如果一个方法被限制，调用者必须有相应的权限才能调用它。同时，不受限制的方法没有相应的权限。其中一些仍然依赖于权限才能成功（例如，签名方法要求您拥有`eth_accounts`签名者帐户的权限），而另一些则需要用户确认（例如`wallet_addEthereumChain`）。

在底层，权限是简单的、与 JSON 兼容的对象，其中包含一些主要由 DOMWALLETExtension 内部使用的字段。以下代码列出了用户可能感兴趣的字段：

Copy

```
interface Web3WalletPermission {
  // The name of the method corresponding to the permission
  parentCapability: string;

  // The date the permission was granted, in UNIX epoch time
  date?: number;
}
```

如果您有兴趣了解更多关于这种&#x53D7;_&#x80FD;&#x529B;_&#x542F;发的权限系统背后的理论，我们鼓励您查看[EIP-2255](https://eips.ethereum.org/EIPS/eip-2255).

**`eth_requestAccounts`**

EIP-1102

此方法由[EIP-1102指定](https://eips.ethereum.org/EIPS/eip-1102). 它等效于已弃用的`ethereum.enable()`提供程序 API 方法。

在引擎盖下，它需要[`wallet_requestPermissions`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/rpc-api#wallet-requestpermissions)许可`eth_accounts`。由于`eth_accounts`目前是唯一的权限，因此您现在只需要此方法。

**Returns**

`string[]`- 单个十六进制以太坊地址字符串的数组。

**描述**

请求用户提供一个以太坊地址来识别。返回一个解析为单个以太坊地址字符串数组的 Promise。如果用户拒绝该请求，Promise 将拒绝并出现`4001`错误。

这个请求会弹出 DOMWALLETExtension 的窗口，通常用按钮来触发，在该请求未有响应时，应禁止用户点击按钮。如果您没有获取到用户的账户信息，您应该提示用户点击按钮等操作来发起请求。

**例子**

Copy

```
document.getElementById('connectButton', connect);

function connect() {
  ethereum
    .request({ method: 'eth_requestAccounts' })
    .then(handleAccountsChanged)
    .catch((error) => {
      if (error.code === 4001) {
        // EIP-1193 userRejectedRequest error
        console.log('Please connect to DOMWALLET Extension.');
      } else {
        console.error(error);
      }
    });
}
```

**`wallet_getPermissions`**

平台可用性

此 RPC 方法在 DOMWALLET Extension Mobile 中尚不可用。

**Returns**

`Web3WalletPermission[]`- 调用者权限的数组。

**描述**

获取调用者的当前权限。返回解析为对象数组的 Promise `Web3WalletPermission`。如果调用者没有权限，则数组将为空。

**`wallet_requestPermissions`**

平台可用性

此 RPC 方法在 DOMWALLET Extension Mobile 中尚不可用。

**参数**

* `Array`
  1. `RequestedPermissions`- 请求的权限。

Copy

```
interface RequestedPermissions {
  [methodName: string]: {}; // an empty object, for future extensibility
}
```

**Returns**

`Web3WalletPermission[]`- 调用者权限的数组。

**描述**

向用户请求给定的权限。返回一个 Promise，它解析为一个非空的`Web3WalletPermission`对象数组，对应于调用者的当前权限。如果用户拒绝该请求，Promise 将拒绝并出现`4001`错误。

该请求会导致 DOMWALLET Extension 弹出窗口。您应该只请求权限以响应用户操作，例如单击按钮。

**例子**

Copy

```
document.getElementById('requestPermissionsButton', requestPermissions);

function requestPermissions() {
  ethereum
    .request({
      method: 'wallet_requestPermissions',
      params: [{ eth_accounts: {} }],
    })
    .then((permissions) => {
      const accountsPermission = permissions.find(
        (permission) => permission.parentCapability === 'eth_accounts'
      );
      if (accountsPermission) {
        console.log('eth_accounts permission successfully requested!');
      }
    })
    .catch((error) => {
      if (error.code === 4001) {
        // EIP-1193 userRejectedRequest error
        console.log('Permissions needed to continue.');
      } else {
        console.error(error);
      }
    });
}
```

#### 不受限制的方法 <a href="#unrestricted-methods" id="unrestricted-methods"></a>

**`eth_decrypt`**

平台可用性

此 RPC 方法在 DOMWALLET Extension Mobile 中尚不可用。

**参数**

* `Array`
  1. `string`- 加密消息。
  2. `string`- 可以解密消息的以太坊账户地址。

**Returns**

`string`- 解密的消息。

**描述**

请求 DOMWALLET Extension 解密给定的加密消息。消息必须已使用给定以太坊地址的公共加密密钥加密。返回解析为解密消息的 Promise，如果解密尝试失败则拒绝。

有关[`eth_getEncryptionPublicKey`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/rpc-api#eth-getencryptionpublickey)更多信息，请参阅。

**例子**

Copy

```
ethereum
  .request({
    method: 'eth_decrypt',
    params: [encryptedMessage, accounts[0]],
  })
  .then((decryptedMessage) =>
    console.log('The decrypted message is:', decryptedMessage)
  )
  .catch((error) => console.log(error.message));
```

**`eth_getEncryptionPublicKey`**

平台可用性

此 RPC 方法在 DOMWALLET Extension Mobile 中尚不可用。

**参数**

* `Array`
  1. `string`- 应检索其加密密钥的以太坊帐户的地址。

**Returns**

`string`- 指定以太坊账户的公共加密密钥。

**描述**

请求用户共享他们的公共加密密钥。返回解析为公共加密密钥的 Promise，如果用户拒绝请求，则拒绝。

公钥是根据与指定用户帐户关联的熵计算的，使用[`nacl`](https://github.com/dchest/tweetnacl-js)算法的`X25519_XSalsa20_Poly1305`实现。

**例子**

Copy

```
let encryptionPublicKey;

ethereum
  .request({
    method: 'eth_getEncryptionPublicKey',
    params: [accounts[0]], // you must have access to the specified account
  })
  .then((result) => {
    encryptionPublicKey = result;
  })
  .catch((error) => {
    if (error.code === 4001) {
      // EIP-1193 userRejectedRequest error
      console.log("We can't encrypt anything without the key.");
    } else {
      console.error(error);
    }
  });
```

**加密**

Copy

```
const ethUtil = require('ethereumjs-util');
const sigUtil = require('@metamask/eth-sig-util');

const encryptedMessage = ethUtil.bufferToHex(
  Buffer.from(
    JSON.stringify(
      sigUtil.encrypt({
        publicKey: encryptionPublicKey,
        data: 'hello world!',
        version: 'x25519-xsalsa20-poly1305',
      })
    ),
    'utf8'
  )
);
```

**`wallet_addEthereumChain`**

EIP-3085

此方法由[EIP-3085指定](https://eips.ethereum.org/EIPS/eip-3085).

**参数**

* `Array`
  1. `AddEthereumChainParameter`- 有关将添加到 DOMWALLET Extension 的链的元数据。

对于`rpcUrls`and`blockExplorerUrls`数组，至少需要一个元素，并且只会使用第一个元素。

Copy

```
interface AddEthereumChainParameter {
  chainId: string; // A 0x-prefixed hexadecimal string
  chainName: string;
  nativeCurrency: {
    name: string;
    symbol: string; // 2-6 characters long
    decimals: 18;
  };
  rpcUrls: string[];
  blockExplorerUrls?: string[];
  iconUrls?: string[]; // Currently ignored.
}
```

**Returns**

`null`- 如果请求成功，则该方法返回`null`，否则返回错误。

**描述**

创建一个确认，要求用户将指定的链添加到 DOMWALLET Extension。添加后，用户可以选择切换到链。

与任何导致确认出现的方法一样，`wallet_addEthereumChain` 只能作为直接用户操作**的**结果调用，例如单击按钮。

DOMWALLET Extension 严格验证该方法的参数，如果任何参数格式不正确，将拒绝请求。此外，DOMWALLET Extension 会在以下情况下自动拒绝该请求：

* 如果 RPC 端点不响应 RPC 调用。
* 如果`eth_chainId`调用时 RPC 端点返回不同的 chain ID。
* 如果 chain ID 对应于任何默认 DOMWALLET Extension 链。

DOMWALLET Extension 尚不支持使用没有 18 位小数的本地货币的链，但将来可能会支持。

**用法与`wallet_switchEthereumChain`**

我们建议将此方法用于[`wallet_addEthereumChain`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/rpc-api#wallet-addethereumchain)：

Copy

```
try {
  await ethereum.request({
    method: 'wallet_switchEthereumChain',
    params: [{ chainId: '0xf00' }],
  });
} catch (switchError) {
  // This error code indicates that the chain has not been added to DOMWALLET Extension.
  if (switchError.code === 4902) {
    try {
      await ethereum.request({
        method: 'wallet_addEthereumChain',
        params: [
          {
            chainId: '0xf00',
            chainName: '...',
            rpcUrls: ['https://...'] /* ... */,
          },
        ],
      });
    } catch (addError) {
      // handle "add" error
    }
  }
  // handle other "switch" errors
}
```

**`wallet_switchEthereumChain`**

EIP-3326

此方法由[EIP-3326指定](https://ethereum-magicians.org/t/eip-3326-wallet-switchethereumchain).

**参数**

* `Array`
  1. `SwitchEthereumChainParameter`- 有关 DOMWALLET Extension 将切换到的链的元数据。

Copy

```
interface SwitchEthereumChainParameter {
  chainId: string; // A 0x-prefixed hexadecimal string
}
```

**`Returns`**

`null`- 如果请求成功，则该方法返回`null`，否则返回错误。

如果错误码（`error.code`）是`4902`，说明请求的链没有被 DOMWALLET Extension 添加，需要通过 请求添加[`wallet_addEthereumChain`](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/rpc-api#wallet-addethereumchain)。

**描述**

提示

有关如何将此方法[与](https://help.tokenpocket.pro/developer-cn/extension-wallet/api-reference/rpc-api#wallet-switchethereumchain)`wallet_addEthereumChain`.

创建一个确认，要求用户切换到指定的链`chainId`。

与任何导致确认出现的方法一样，`wallet_switchEthereumChain` 只能作为直接用户操作**的**结果调用，例如单击按钮。

DOMWALLET Extension 会在以下情况下自动拒绝请求：

* 如果 chain ID 格式错误
* 如果指定 chain ID 的链尚未添加到 DOMWALLET Extension

**`wallet_registerOnboarding`**

提示

作为 API 使用者，您不太可能自己调用此方法。

**Returns**

`boolean`如果请求成功返回`true`，否则返回`false`。

**描述**

使用 DOMWALLET Extension 注册请求站点作为登录的发起人。

此方法旨在在安装 DOMWALLET Extension 之后，但在 DOMWALLET Extension 载入完成之前调用。您可以使用此方法通知 DOMWALLET Extension 您是建议安装 DOMWALLET Extension 的人。这允许 DOMWALLET Extension 在用户引导完成后将用户重定向回您的站点。

**`wallet_watchAsset`**

EIP-747

此方法由[EIP-747指定](https://eips.ethereum.org/EIPS/eip-747).

**参数**

* `WatchAssetParams`- 要观看的资产的元数据。

Copy

```
interface WatchAssetParams {
  type: 'ERC20'; // In the future, other standards will be supported
  options: {
    address: string; // The address of the token contract
    'symbol': string; // A ticker symbol or shorthand, up to 5 characters
    decimals: number; // The number of token decimals
    image: string; // A string url of the token logo
  };
}
```

**Returns**

`boolean`如果添加了 token 返回`true`，否则`false`。

**描述**

请求用户在 DOMWALLET Extension 中跟踪 token。返回一个`boolean`指示是否成功添加 token。

大多数以太坊钱包都支持一组代币，通常来自集中管理的代币注册表。 `wallet_watchAsset`使 web3 应用程序开发人员能够在运行时要求他们的用户跟踪他们钱包中的代币。添加后，token 与通过传统方法（例如集中式注册表）添加的 token 无法区分。

**例子**

Copy

```
ethereum
  .request({
    method: 'wallet_watchAsset',
    params: {
      type: 'ERC20',
      options: {
        address: '0xb60e8dd61c5d32be8058bb8eb970870f07233155',
        symbol: 'FOO',
        decimals: 18,
        image: 'https://foo.io/token-image.svg',
      },
    },
  })
  .then((success) => {
    if (success) {
      console.log('FOO successfully added to wallet!');
    } else {
      throw new Error('Something went wrong.');
    }
  })
  .catch(console.error);
```
