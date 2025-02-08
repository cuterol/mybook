# IOS

### SDK接入指南 <a href="#id-1" id="id-1"></a>

#### 引入SDK <a href="#yin-ru-sdk" id="yin-ru-sdk"></a>

GitHub仓库: [**TP iOS SDK**](https://github.com/TP-Lab/Mobile-SDK/tree/master/iOS%20SDK)

下载仓库&#x4E2D;_`TPSDK.zip`_, 解压后，添加到工程目录。

1. 设&#x7F6E;_`URL scheme`_: _`Project`_->_`TARGETS`_->_`info`_->_`URL Types`_->添&#x52A0;_`URL scheme`_;
2. &#x5728;_`info.plist`_&#x4E2D;_`LSApplicationQueriesSchemes`_&#x4E0B;`添加`一项，值&#x4E3A;_`tpoutside`_ ;

#### 初始化 <a href="#id-2" id="id-2"></a>

* **在** _`AppDelegate.m`_ **中添加头文件**

Copy

```
#import <TPSDK/TPSDK.h>
```

* **在** _`application:didFinishLaunchingWithOptions:`_ **方法中注册**_`scheme`_

Copy

```
[TPApi registerAppID:@"demoapp"];
```

* **在** _`application:openURL:`_ **方法中添加监听回调方法**

Copy

```
[TPApi handleURL:url options:options result:^(TPRespObj *respObj) {
    respObj.result;     // TPRespResultCanceled = 0,TPRespResultSuccess, TPRespResultFailure,
    respObj.message;    // Result message
    respObj.data;       // Json details
    /* Json details:
    {
        "result" : 1,
        "action" : "sign",
        "version" : "1.0",
        "protocol" : "TPProtocol",
        "ref" : "DOMWALLET",
        "wallet" : "xxx...xxx",       // 成功时返回
        "publickey" : "xxx...xxx",    // 成功时返回
        "permissions" : [             // 成功时返回;  eosio/iost网络返回该字段
            "active",
            "owner"
        ],
        ...,
    }
    */
}];
```

#### 支持操作 <a href="#id-3" id="id-3"></a>

[授权登录](https://help.tokenpocket.pro/developer-cn/mobile-wallet/mobile-sdk/ios#shou-quan-deng-lu)

[签名](https://help.tokenpocket.pro/developer-cn/mobile-wallet/mobile-sdk/ios#qian-ming)

[Push Transaction](https://help.tokenpocket.pro/developer-cn/mobile-wallet/mobile-sdk/ios#push-transaction)

#### 代码示例 <a href="#id-4" id="id-4"></a>

**授权登录**

Copy

```
TPLoginObj *login = [TPLoginObj new];
login.dappName = @"xxx";
login.dappIcon = @"https:.../xx.png";
login.blockchains = @[
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"1"],
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"56"], /** BSC */
];
[TPApi sendObj:login];


/// 回调数据 ↓
TPRespObj.data
{
    ...,
    "ref" : "DOMWALLET",
    "wallet" : "0x...",
    "sign" : "...",
    "network" : "ethereum",
    "chainId" : "56",
    "timestamp" : "1554266633",
    "sign" : "{\"r\":\"0x3a5...bd5\",\"message\":\"1554266633{account}TokenPocket\",\"messageHash\":\"0xcdf...f29\",\"s\":\"0x6c1...f55\",\"signature\":\"0x3a5...51b\",\"v\":\"0x1b\"}"
}
```

**签名**

Copy

```
TPSignObj *sign = [TPSignObj new];
sign.dappName = @"xxx";
sign.dappIcon = @"https:.../xx.png";
sign.message = @"sign data...";
sign.blockchains = @[
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"56"], /** 如果选择 BSC */
];
[TPApi sendObj:sign];


/// 回调数据 ↓
TPRespObj.data
{
    ...,
    "sign" : "signature...",
}
```

**Push Transaction**

Copy

```
TPPushTransactionObj *transaction = [TPPushTransactionObj new];
transaction.dappName = @"xxx";
transaction.dappIcon = @"https:.../xx.png";
transaction.blockchains = @[
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"56"], /** 如果选择 BSC */
];
transaction.actions = @{
    @"from": @"0x...",
    @"to": @"0x...",
    @"data": @"0x095ea7b30000000000000000000000004184d9a175d13e568f3466ea93c02b6f8eb9f8c10000000000000000000000000000000000000000000000000000000000000000",
    @"value": @"0x0",
    @"gasPrice": @"0x16b969d000",
    @"gas": @"0x186a0",
    @"nonce": @"0x01"
};
[TPApi sendObj:transaction];


/// 回调数据 ↓
TPRespObj.data
{
    ...,
    "txId" : "abc...123",
    "processed" : {
        ...
    },
}
```

**EthGetEncryptionPublicKey**

Copy

```
TPEthGetEncryptionPublicKeyObj *ethGetEncryptionPublicKeyObj = [TPEthGetEncryptionPublicKeyObj new];
ethGetEncryptionPublicKeyObj.dappName = @"xxx";
ethGetEncryptionPublicKeyObj.dappIcon = @"https:.../xx.png";
ethGetEncryptionPublicKeyObj.blockchains = @[
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"56"], /** 如果选择 BSC */
];
TPEthGetEncryptionPublicKeyObjData *data = TPEthGetEncryptionPublicKeyObjData.new;
data.address = @"xxx";
ethGetEncryptionPublicKeyObj.data = data;
[TPApi sendObj:ethGetEncryptionPublicKeyObj];


/// 回调数据 ↓
TPRespObj.data
{
    ...,
    "txId" : "abc...123",
    "data" : {
        "address":"xxx",
        "encryptitonPublicKey":"xxx"
    },
}
```

**EthDecrypt**

Copy

```
TPEthDecryptObj *ethDecryptObj = [TPEthDecryptObj new];
ethDecryptObj.dappName = @"xxx";
ethDecryptObj.dappIcon = @"https:.../xx.png";
ethDecryptObj.blockchains = @[
    [TPChainObj objWithNetwork:@"ethereum" chainId:@"56"], /** 如果选择 BSC */
];
TPEthDecryptObjData *data = TPEthDecryptObjData.new;
data.address = @"xxx";
data.message = @"xxxxxxx";
ethDecryptObj.data = data;
[TPApi sendObj:ethDecryptObj];


/// 回调数据 ↓
TPRespObj.data
{
    ...,
    "txId" : "abc...123",
    "data" : {
        "address":"xxx",
        "decryptedData":"xxx"
    },
}
```
