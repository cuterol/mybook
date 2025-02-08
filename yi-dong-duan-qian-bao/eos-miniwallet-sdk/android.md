# Android

#### 介绍 <a href="#jie-shao" id="jie-shao"></a>

MiniWallet可以实现EOS网络，针对部分需要签名的操作，不用拉起TP钱包，直接在Dapp内部完成，从而使Dapp拥有更顺滑的用户体验。

#### 准备工作 <a href="#zhun-bei-gong-zuo" id="zhun-bei-gong-zuo"></a>

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
-dontwarn com.DOMWALLET.opensdk.**
-keep class com.DOMWALLET.opensdk.**{*;}
-keep interface com.DOMWALLET.opensdk.**{*;}
```

#### 使用过程 <a href="#shi-yong-guo-cheng" id="shi-yong-guo-cheng"></a>

* 初始化SDK

Copy

```
        TPManager.getInstance().initSDK(this);
        //设置主网和节点
        TPManager.getInstance().setBlockChain(this, NetTypeEnum.EOS_MAINNET, "http://openapi.eos.ren");
        //设置miniwallet插件地址
        TPManager.getInstance().setAppPluginNode(this, "http://eosinfo.mytokenpocket.vip");
        //设置一个密码，用来保护miniwallet数据，这里只是为了展示需要设置为12345
        TPManager.getInstance().setSeed(this, "123456");
```

* 授权 这一步操作会自定义权限组，并且将需要使用miniwallet的操作链接到权限组

Copy

```
        AuthorizePerm authorizePerm = new AuthorizePerm();
        //添加权限组的账号
        authorizePerm.setAccount("ljxlzdh54321");
        authorizePerm.setPermExisted(false);
        //要添加的权限组的名字
        authorizePerm.setPerm("testtransfer");
        //和权限组关联的操作，只有把操作关联到添加的权限组上面了，才能利用自定义的权限进行相应的操作
        LinkAction linkAction = new LinkAction();
        //这里定义EOS上TPT的转账操作关联到自定义的testtransfer权限组
        linkAction.setContract("eosiotptoken");
        linkAction.setAction("transfer");
        linkActions.add(linkAction);
        authorizePerm.setActions(linkActions);

        authorizePerm.setDappIcon("https://newdex.io/static/logoicon.png");
        authorizePerm.setDappName("Test");
        authorizePerm.setSelectAll(true);

        TPManager.getInstance().auth(AuthActivity.this, authorizePerm, new TPListener() {
                    @Override
                    public void onSuccess(String data) {
                        //操作成功后，会返回本次操作的hash
                        Toast.makeText(AuthActivity.this, data, Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onError(String data) {
                        Toast.makeText(AuthActivity.this, data, Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onCancel(String data) {
                        Toast.makeText(AuthActivity.this, data, Toast.LENGTH_SHORT).show();
                    }
                });
```

授权成功后，就可以使用自定义权限组对应的key签名相关操作

* 构造交易数据

Copy

```
    String actions = "[\n" +
            "        {\n" +
            "          \"account\": \"eosiotptoken\",\n" +
            "          \"name\": \"transfer\",\n" +
            "          \"authorization\": [\n" +
            "            {\n" +
            "              \"actor\": \"ljxlzdh54321\",\n" +
            "              \"permission\": \"testtransfer\"\n" +
            "            }\n" +
            "          ],\n" +
            "          \"data\": {\n" +
            "            \"from\": \"ljxlzdh54321\",\n" +
            "            \"memo\": \"ddd\",\n" +
            "            \"quantity\": \"0.0100 TPT\",\n" +
            "            \"to\": \"clementtes51\"\n" +
            "          }\n" +
            "        }\n" +
            "      ]";
```

这一步操作要注意:authorization的permission要填入定义权限组名字

* 发送交易

发送交易前，首先检查交易数据中的权限是否已经link，如果没有link，则需要将上一步构造的交易数据中的authorization中permission替换成active，以便拉起钱包授权操作。如果已经link成功，则无需拉起钱包，直接在dapp中完成操作。

Copy

```
 private void pushTx() {
        final Transaction transaction = getTxData();
        //首先通过接口判断，本次执行的操作需要的权限，是否已经在账号中，这个链接的过程可以参考账号授权
        TPManager.getInstance().isPermLinkAction(this, transaction, new TPListener() {
            @Override
            public void onSuccess(String data) {
                //如果本次操作已经通过miniwallet授权，则将操作的参数修改为开发者自己定义的权限组，在demo实例中，
                //我们自定义的权限组是testtransfer,详情可参加授权AuthActivity.java
                replacePerm(transaction, transaction.getPerm());
            }

            @Override
            public void onError(String data) {
                //如果当前操作的权限组没有link到账号，则miniwallet无法完成当前操作，仍然需要拉起钱包授权
                replacePerm(transaction, "active");
            }

            @Override
            public void onCancel(String data) {

            }
        });
    }

    /**
     *
     * @param transaction
     * @param permission
     */
    private void replacePerm(Transaction transaction, String permission) {
        Json actions = new Json(transaction.getActions());
        for (int i = 0; i < actions.getLength(); i++) {
            Json authorization = actions.getObject(i, "{}").getArray("authorization", "[]")
                    .getObject(0, "{}");
            authorization.putString("permission", permission);
        }
        transaction.setActions(actions.toString());
        //将操作的permission替换成自定义的权限组后，就可以直接在miniwallet中完成操作，而不需要拉起tp 钱包授权
        realPushTx(transaction);
    }

    private void realPushTx(Transaction transaction) {
        TPManager.getInstance().pushTransaction(PushTxActivity.this, transaction, new TPListener() {
            @Override
            public void onSuccess(String data) {
                Toast.makeText(PushTxActivity.this, data, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onError(String data) {
                Toast.makeText(PushTxActivity.this, data, Toast.LENGTH_LONG).show();

            }

            @Override
            public void onCancel(String data) {
                Toast.makeText(PushTxActivity.this, data, Toast.LENGTH_LONG).show();

            }
        });
    }
```

#### 方法说明 <a href="#fang-fa-shuo-ming" id="fang-fa-shuo-ming"></a>

**initSDK() 初始化SDK**

**isPermLinkAction() 检查权限是否链接**

**getAccounts 获取授权账号**

**clearAuth 清除授权**

**auth 授权**

#### 总结 <a href="#zong-jie" id="zong-jie"></a>

使用Demo请查看:[https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK/SDK\_DEMO/app/src/main/java/domwllet/pro/sdk\_demo/minwallet](https://github.com/TP-Lab/Mobile-SDK/tree/master/Android%20SDK/SDK_DEMO/app/src/main/java/tokenpocket/pro/sdk_demo/minwallet)

#### 错误排查 <a href="#cuo-wu-pai-cha" id="cuo-wu-pai-cha"></a>

* 权限链接失败，请查看账号是否有足够的资源
* 链接成功后，仍然需要拉起钱包操作。先检查操作参数是否配置正确。另外，对于一些涉及到用户资产安全的操作，即使配置正确，仍然需要拉起钱包确认。
