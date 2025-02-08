# Android

#### 准备工作 <a href="#id-1" id="id-1"></a>

* 下载wallet-sdk-release.aar [https://github.com/TP-Lab/Mobile-SDK/blob/master/Android%20SDK/wallet-sdk-release.aar](https://github.com/TP-Lab/Mobile-SDK/blob/master/Android%20SDK/wallet-sdk-release.aar)
* 将aar文件放到你项目工程下app模块下的libs目录，如果没有该文件夹，则创建
* 添加依赖，在app目录下的build.gradle中添加以下代码

Copy

```
dependencies {
    compile(name:'wallet-sdk-release', ext:'aar')
}
```

* 反混淆

Copy

```
# DOMWALLET sdk
-dontwarn com.tokenpocket.opensdk.**
-keep class com.tokenpocket.opensdk.**{*;}
-keep interface com.tokenpocket.opensdk.**{*;}
```

注意，所有SDK相关方法都需要在UI线程调用

以下SDK方法支持EVM系列网络，Tron网络，EOS网络，IOST网络，下面以ETH网络为例，介绍调用方法，每个网络调用的参数有差异，详情可以参照github Demo：[https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK/SDK\_DEMO](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK/SDK_DEMO)

#### SDK主要接口 <a href="#id-2" id="id-2"></a>

**登录授权**

通过调用该方法，开发者可以获取真实钱包地址，用于后续业务操作

Copy

```
	Authorize authorize = new Authorize();
        List<Blockchain> blockchains = new ArrayList<>();
        blockchains.add(new Blockchain("ethereum", "1"));
        authorize.setBlockchains(blockchains);

        authorize.setDappName("Test demo");
        authorize.setDappIcon("https://eosknights.io/img/icon.png");
        //开发者自己定义的业务id
        authorize.setActionId("web-db4c5466-1a03-438c-90c9-2172e8becea5");
        TPManager.getInstance().authorize(this, authorize, new TPListener() {
            @Override
            public void onSuccess(String s) {
                Toast.makeText(EthDemoActivity.this, s, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onError(String s) {
                Toast.makeText(EthDemoActivity.this, s, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onCancel(String s) {
                Toast.makeText(EthDemoActivity.this, s, Toast.LENGTH_LONG).show();

            }
        });
```

认证成功后，会返回签名后的信息，认证钱包地址，开发者可以利用这些信息校验正确性。具体返回内容可以参照github文档:[https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#1%E6%8E%88%E6%9D%83%E7%99%BB%E9%99%86--authorize](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#1%E6%8E%88%E6%9D%83%E7%99%BB%E9%99%86--authorize)

**转账**

Copy

```
        Transfer transfer = new Transfer();
        //标识链
        List<Blockchain> blockchains = new ArrayList<>();
        //evm系列，第一个参数是ethereum,第二个参数是网络的id，这里1是eth网络链上id
        blockchains.add(new Blockchain("ethereum", "1"));
        transfer.setBlockchains(blockchains);

        transfer.setProtocol("DOMWALLET");
        transfer.setVersion("1.0");
        transfer.setDappName("Test demo");
        transfer.setDappIcon("https://eosknights.io/img/icon.png");
        //开发者自己定义的业务Id,用来标识这次操作
        transfer.setActionId("web-db4c5466-1a03-438c-90c9-2172e8becea5");
        //data，如果是转原生代币，可以添加上链数据
        transfer.setAction("transfer");
        //发送者
        transfer.setFrom("0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5");
        //接受者
        transfer.setTo("0x32ff06198da462f1c519d30f4d328b3fef295d19");
        //代币合约地址，如果是转ETH，可以不设置这个参数
        transfer.setContract("0xdAC17F958D2ee523a2206206994597C13D831ec7");
        //转账数量，比如这里demo是转0.01个USDT，就是传入0.01
        transfer.setAmount(0.01);
        //必须设置
        transfer.setDecimal(18);
        transfer.setSymbol("USDT");
        transfer.setDesc("仅供ui展示，不上链");
        //开发者服务端提供的接受调用登录结果的接口，如果设置该参数，钱包操作完成后，会将结果通过post application json方式将结果回调给callbackurl
        transfer.setCallbackUrl("http://115.205.0.178:9011/taaBizApi/taaInitData");
        TPManager.getInstance().transfer(this, transfer, new TPListener() {
            @Override
            public void onSuccess(String s) {
                //转账操作结果，注意，这里只是将交易发送后的hash返回，并不保证交易一定成功，需要开发者根据hash自行确认最终链上结果
                Toast.makeText(EthTransferActivity.this, s, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onError(String s) {
                Toast.makeText(EthTransferActivity.this, s, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onCancel(String s) {
                Toast.makeText(EthTransferActivity.this, s, Toast.LENGTH_LONG).show();

            }
        });
```

每个网络的转账参数可能会有差异，这里只展示EVM系列的方式，详情可见：

[https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#2%E8%BD%AC%E8%B4%A6-token-transfer](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#2%E8%BD%AC%E8%B4%A6-token-transfer)

转账完成后，会将tx hash返回到调用App，注意这里，完成，只是将操作签名push出去，并不能保证本次操作一定成功，开发者需要自己根据tx hash确认最终的状态

**合约操作**

Copy

```
        Transaction transaction = new Transaction();
        //标识链
        List<Blockchain> blockchains = new ArrayList<>();
        blockchains.add(new Blockchain("ethereum", "1");
        transaction.setBlockchains(blockchains);

        transaction.setDappName("Test demo");
        transaction.setDappIcon("https://eosknights.io/img/icon.png");
        transaction.setActionId("web-db4c5466-1a03-438c-90c9-2172e8becea5");
        transaction.setAction("pushTransaction");
        transaction.setLinkActions(new ArrayList<LinkAction>());
        transaction.setTxData("{\"from\":\"0x25F490a1fB41f751b8F61F832643224606B75B4\",\"gasPrice\":\"0x6c088e200\",\"gas\":\"0xea60\",\"chainId\":\"1\",\"to\":\"0x7d1e7fb353be75669c53c18ded2abcb8c4793d80\",\"data\":\"0xa9059cbb000000000000000000000000171a0b081493722a5fb8ebe6f0c4adf5fde49bd8000000000000000000000000000000000000000000000000000000000012c4b0\"}");
        TPManager.getInstance().pushTransaction(this, transaction, new TPListener() {
            @Override
            public void onSuccess(String s) {
                Toast.makeText(EthPushTxActivity.this, s, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onError(String s) {
                Toast.makeText(EthPushTxActivity.this, s, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onCancel(String s) {
                Toast.makeText(EthPushTxActivity.this, s, Toast.LENGTH_LONG).show();

            }
        });
```

转账完成后，会将tx hash返回到调用App，注意这里，完成，只是将操作签名push出去，并不能保证本次操作一定成功，开发者需要自己根据tx hash确认最终的状态

详情可见: [https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#3pushtransaction](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#3pushtransaction)

**签名操作**

该操作支持EVM，EOS网络，Tron网络，IOST网络

#### Android 1.6.8以前版本EVM网络支持ethPersonalSign签名类型，从1.6.8开始，支持ethPersonalSign ethSignTypedDataLegacy ethSignTypedData ethSignTypedData\_v4四种签名类型 <a href="#android-1.6.8-yi-qian-ban-ben-evm-wang-luo-zhi-chi-ethpersonalsign-qian-ming-lei-xing-cong-1.6.8-kai" id="android-1.6.8-yi-qian-ban-ben-evm-wang-luo-zhi-chi-ethpersonalsign-qian-ming-lei-xing-cong-1.6.8-kai"></a>

Copy

```
        Signature signature = new Signature();
        //标识链
        List<Blockchain> blockchains = new ArrayList<>();
        blockchains.add(new Blockchain("ethereum", "1"));
        signature.setBlockchains(blockchains);

        signature.setDappName("Test demo");
        signature.setDappIcon("https://eosknights.io/img/icon.png");
        //开发者自己定义的业务ID，用于标识操作，在授权登录中，需要设置该字段
        signature.setActionId("web-db4c5466-1a03-438c-90c9-2172e8becea5");
        //签名类型 从Android 1.6.8版本开始，EVM网络支持 ethPersonalSign ethSignTypedDataLegacy ethSignTypedData
        //ethSignTypedData_v4 四种签名类型
        signature.setSignType("ethPersonalSign");
        //ethSign类型，签名的数据是16进制字符串
        signature.setMessage("0xc05dfb5d7d33ef21dacffc010ff0a45204a3dd5e0cf6f9a970f07339d7a7770e");
        //开发者服务端提供的接受调用登录结果的接口，如果设置该参数，钱包操作完成后，会将结果通过post application json方式将结果回调给callbackurl
        signature.setCallbackUrl("http://115.205.0.178:9011/taaBizApi/taaInitData");
        TPManager.getInstance().signature(this, signature, new TPListener() {
            @Override
            public void onSuccess(String s) {
                Toast.makeText(EthSignActivity.this, s, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onError(String s) {
                Toast.makeText(EthSignActivity.this, s, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onCancel(String s) {
                Toast.makeText(EthSignActivity.this, s, Toast.LENGTH_LONG).show();

            }
        });
```

其中**signType**的枚举类型有以下几种：

**ethPersonalSign**，**ethSignTypedData**，**ethSignTypedData\_v4**，**ethSignTypedDataLegacy**，**ethSign**(不再建议使用)

该方法对字符串进行签名，返回签名的钱包信息和签名结果。详情可见: [https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#4%E7%AD%BE%E5%90%8D-sign](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK#4%E7%AD%BE%E5%90%8D-sign)

### 如何支持火币生态链，币安智能链等ETH Fork链 <a href="#id-6" id="id-6"></a>

在构建new Blockchain("ethereum", "xx\_chainId")对象时，传入对应的chainId

Copy

```
//ETH主网
new Blockchain("ethereum", "1")

//币安智能链
new Blockchain("ethereum", "56")

//火币生态链
new Blockchain("ethereum", "128")

//OKExChain
new Blockchain("ethereum", "66")
```

#### 错误排查 <a href="#id-7" id="id-7"></a>

**拉起钱包操作后，无法返回到App？**

请确认是否在UI线程调用相关方法

**参数错误?**

请仔细检查参数，特别是网络类型，每个操作都需要指定网络类型，否则钱包无法知道使用什么钱包进行操作

**U3D和COCOS如何开发？**

可以参照相关的游戏开发文档，引入aar文件
