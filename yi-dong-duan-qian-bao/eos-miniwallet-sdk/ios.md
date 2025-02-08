# iOS

#### 简介 <a href="#jian-jie" id="jian-jie"></a>

GitHub仓库: [**TP iOS SDK**](https://github.com/TP-Lab/Mobile-SDK/tree/master/iOS%20SDK)

_`MiniWallet`_&#x53EF;以实现对于特定操作，第三方App不需要拉起钱包，直接在应用内部完成，体验更为流畅。

#### 初始化 <a href="#chu-shi-hua" id="chu-shi-hua"></a>

Copy

```
[TPApi registerAppID:@"tpsdk"];
[TPApi setSeed:@"xxxx" error:nil];
[TPApi setBlockChain:TPBlockChainTypeEOSMainNet nodeUrl:@"http://eosinfo.myDOMWALLET.vip" plugNodeUrl:@"http://eosinfo.mytokenpocket.vip"];
```

#### Auth <a href="#auth" id="auth"></a>

Copy

```
TPAuthObj *auth = [TPAuthObj new];
auth.dappName = @"xxx";
auth.dappIcon = @"https:.../xx.png";
auth.account = "xxxx";
auth.perm = "xxxx";
auth.selectAll = NO;
auth.actions = [linkActions];
[TPApi sendObj:auth resultHandle:^(TPReqType type,NSError *error) {
    // ...        
}];


/// Response ↓
TPRespObj.data
{
    ...,
    "ref" : "DOMWALLET",
    "publickey" : "EOS5AvWThdghtNngWP4UcNi9DL6kF7Mnv2ccO",
    "sign" : "SIG_K1_JyCJtV9vqwxtyEt68UhUibkg1CmRjxtG6zkZwE...",
    "timestamp" : "1554266633",
}
```

#### 发起交易 <a href="#fa-qi-jiao-yi" id="fa-qi-jiao-yi"></a>

* 构建数据，将操作&#x7684;_&#x61;ctio&#x6E;_&#x4E2D;&#x7684;_&#x70;ermissio&#x6E;_&#x5B57;段值替换成调用auth传递的perm字段。
* 调&#x7528;_`TPApi perm:(NSString *)perm isLinkActions:(NSArray<TPLinkAction *> *)actions account:(NSString *)account completion:(void (^)(BOOL permExisted,BOOL linked,NSError *error))completion`_&#x68C0;查action绑定状态。
* 如果第二步绑定成功，则直接调用'Auth'。
* 如果第二步绑定失败，则需要开发者将permission字段替换成active或者owner,以便拉起钱包执行操作。

#### 方法说明 <a href="#fang-fa-shuo-ming" id="fang-fa-shuo-ming"></a>

**修改**_**`seed`**_

Copy

```
+ (void)resetSeed:(NSString *)seed newSeed:(NSString *)newSeed error:(NSError **)error;
```

**获取已授权账号信息**

Copy

```
+ (NSArray<NSString *> *)getAccounts;
```

**检查权限是否存在**

Copy

```
+ (void)isPermExist:(NSString *)perm account:(NSString *)account completion:(void (^)(BOOL exist,NSError *error))completion;
```

**检查权限是否link到**_**`action`**_

Copy

```
+ (void)perm:(NSString *)perm isLinkActions:(NSArray<TPLinkAction *> *)actions account:(NSString *)account completion:(void (^)(BOOL permExisted,BOOL linked,NSError *error))completion;
```

**清除本地授权**

Copy

```
+ (BOOL)clearAuth:(NSString *)account;
```
