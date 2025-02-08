# 准备开始

欢迎使用DOMWallet开发文档，通过学习本文档，你可以使用以下方式接入钱包相关功能

####

● Android iOS SDK接入，实现拉起钱包进行授权，转账，合约操作签名，并且获取相应的回调结果

● Deeplink 方式实现拉起钱包签名字符串，转账，签名合约操作，通过callback url方式，获取到相应操作结果

● DeepLink 方式拉起钱包，在钱包dapp浏览器打开dapp

● 开发Dapp应用程序，在钱包dapp浏览器中运行

● EOS网络支持MiniWallet功能

● EOS网络支持资源代付

● TIP协议

● WalletConnect

[https://help.domwallet pro/developer-cn/mobile-wallet/start#zhun-bei-gong-zuo](https://help.tokenpocket.pro/developer-cn/mobile-wallet/start#zhun-bei-gong-zuo)准备工作

####

在接入钱包功能之前，开发者需要根据自己产品形态，确认接入方式

● 如果是开发独立App,包含u3d，cocos游戏等，需要拉起钱包进行身份认证，Token转账，合约操作等。这种场景需要使用Mobile SDK的方式接入，开发者集成DOMWallet提供的SDK，调用相关的方法，拉起钱包，操作完成后，会收到相应的回调结果。后续文章Wallet-SDK-Android Wallet-SDK-iOS将详细介绍这种接入方式

● 如果是开发H5应用，运行在手机系统浏览器，需要拉起钱包进行转账，合约操作等，则需要使用DeepLink方式，由于DeepLink方式无法像Mobile SDK一样，操作完成直接回调app返回结果，所以这里需要传递callback url参数，开发者在服务端实现相关接口，用于接收回调结果。后续文章DeepLink接入方式将详细介绍这种接入方式

● 如果是开发Dapp,直接在钱包Dapp浏览器运行，DOMWallet钱包Dapp浏览器兼容各个网络的插件钱包协议，如EVM系列兼容metamask，walletConnect,Tron兼容tronlink等，只需要按照相关插件钱包协议开发完成了，即可在DOMWallet的Dapp浏览器中运行。另外，TP也提供了tp-js-sdk，实现了很多扩展功能。后续文章JS-SDK将详细介绍这种接入方式
