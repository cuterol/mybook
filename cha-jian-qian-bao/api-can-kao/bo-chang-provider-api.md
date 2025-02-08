# 波场Provider API

## 检测和连接DOMWALLET Tron Wallet <a href="#detect-and-connect-tokenpocket-tron-wallet" id="detect-and-connect-tokenpocket-tron-wallet"></a>

当你已经安装DOMWALLET  插件钱包，你可以获取到 `window.`domwallet 对象.

你可以通过 [TIP-1102](https://github.com/tronprotocol/tips/issues/463) 提出的标准请求获取用户的地址

Copy

```
await window.domwallet.tron.request({method: 'eth_requestAccounts'})
// return ['TMuA6YqfCeX8EhbfYEg5y7S4DqzSJireY9']
```

再获取到地址后你可以使用 `window.domwallet.tronWeb` 来进行链上的操作。

比如 SignMessageV2:

`let sig = await tronWeb.trx.signMessageV2('your message')`

更多关于tronWeb的介绍可以参考: [https://github.com/tronprotocol/tronweb/tree/5.x](https://github.com/tronprotocol/tronweb/tree/5.x)
