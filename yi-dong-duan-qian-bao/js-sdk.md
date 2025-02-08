# JS-SDK

#### 插件协议支持 <a href="#cha-jian-xie-yi-zhi-chi" id="cha-jian-xie-yi-zhi-chi"></a>

DOMWALLET移动端App已经兼容了Metamask(ETH)、Unisat(BTC)、 TronLink(TRON)、Phantom(Solana)、Scatter(EOS)、IWallet(IOST)、TonConnect(TON) 协议。

如果你的DApp已经支持了MetaMask, Unisat, TronLink, Phantom, Scatter, iWallet, TonConnect,这类插件钱包协议，则无需任何修改直接在DOMWALLET App的DApp浏览器中运行。

更多细节信息请查看: [https://github.com/TP-Lab/tp-js-sdk](https://github.com/TP-Lab/tp-js-sdk)

**从Android 2.1.8 、 iOS 2.3.8 开始，DOMWALLET支持TonConnect(TON)协议。**

如何在DOMWALLET中使用Nostr相关功能，可以参考:

[![Logo](https://659607907-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MMF2k4MCaxErpZyah2d%2Ficon%2F8FIvhACj72GmT8skuG39%2Fic_launcher.png?alt=media\&token=8ef812d4-b6eb-43f4-a731-c6efedf020d0)关于Nostr协议DOMWALLET（Chinese）](https://help.tokenpocket.pro/cn/wallet-operation/protocol/nostr)

另外，我们也提供了tp-js-sdk。该sdk只在DOMWALLET移动端Dapp浏览器中生效，提供了全屏，保存图片，旋转屏幕等接口

在TP钱包中调试Dapp，可以打开开发者模式。打开方式：打开钱包->我的->关于我们->快速点击顶部TP图标多次->底部点击开发者模式->开发者模式

#### tp-js-sdk详细文档请参考:[https://github.com/TP-Lab/tp-js-sdk](https://github.com/TP-Lab/tp-js-sdk) <a href="#tpjssdk-xiang-xi-wen-dang-qing-can-kao-httpsgithub.comtplabtpjssdk" id="tpjssdk-xiang-xi-wen-dang-qing-can-kao-httpsgithub.comtplabtpjssdk"></a>

#### 常用的tp-js-sdk接口 <a href="#chang-yong-de-tpjssdk-jie-kou" id="chang-yong-de-tpjssdk-jie-kou"></a>

获取app信息

Copy

```
tp.getAppInfo()
```

扫码

Copy

```
tp.invokeQRScanner().then(console.log)

"abcdefg"
```

设置dapp全屏

Copy

```
tp.fullScreen({
   fullScreen: 0
})
```

更多接口和实用方式，请查看详细文档:https://github.com/TP-Lab/tp-js-sdk
