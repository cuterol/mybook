# 访问账户

访问用户的账户是请求用户签名或签署交易的必要条件，以下`wallet methods`方法都需要发送账户作为函数的参数。

* `eth_sendTransaction`
* `eth_sign`（不安全且不建议使用）
* `eth_personalSign`
* `eth_signTypedData`

[连接到用户](https://help.tokenpocket.pro/developer-cn/extension-wallet/guide/start)后，您始终可以通过`ethereum.selectedAddress`来重新检查当前帐户。

如果您想在地址更改时收到通知，我们有一个您可以订阅的活动：

Copy

```
ethereum.on('accountsChanged', function (accounts) {
  // Time to reload your interface with accounts[0]!
});
```

返回的数组中的第一个账户默认为用户选定的账户，因为将来返回的数组中可能会包含多个账户，但是现在这个功能暂不可用，如果数组中的第一个账户不是您所希望的账户，您应该做相应的操作，例如提示用户账号地址不对
