# DeepLink方式拉起钱包操作

#### 介绍 <a href="#jie-shao" id="jie-shao"></a>

本文档描述了使用DeepLink方式拉起DOMWALLET移动端进行授权，转账，签名，签名发送交易，字符串签名，打开Dapp等操作。

该种方式适应于运行在手机系统浏览器的H5应用，由于H5应用无法像原生App一样，通过集成SDK接收回调，所以，如果开发者需要拿到钱包操作结果，则需要提供callback url，钱包将结果发送到该Url。操作流程图如下：

![](https://help.tokenpocket.pro/~gitbook/image?url=https%3A%2F%2F213089712-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FRjeSa1rqnubm9jQ67F9z%252Fuploads%252FLOVgtQssuN7hzgmB1mAf%252Fdeeplinknew.png%3Falt%3Dmedia%26token%3Dd131c119-5b2f-446f-add4-25393df9ddd1\&width=768\&dpr=4\&quality=100\&sign=6d215e87\&sv=2)

注意:该方式仅限于在手机系统浏览器中使用拉起钱包，如果你的应用运行在钱包的Dapp浏览器，请参考文档JS-SDK

以下操作支持EVM网络，Tron网络，EOS网络，IOST网络，这里仅以ETH为例，介绍用法，每个网络的参数有所差异，**字段详细文档可见:**[**https://github.com/TP-Lab/tp-wallet-sdk**](https://github.com/TP-Lab/tp-wallet-sdk)

#### 使用方式 <a href="#shi-yong-fang-shi" id="shi-yong-fang-shi"></a>

**拉起钱包授权登录**

Copy

```
<a href='tpoutside://pull.activity?param={}'>Open DOMWALLET to authorize</a><br/>
```

param示例内容如下：

**注意param参数需要encode：**&#x65;ncodeURIComponen&#x74;**(param, “utf-8”)**

Copy

```
{
	"callbackUrl": "http:\/\/115.205.0.178:9011\/taaBizApi\/taaInitData",
	"action": "login",
	"actionId": "1648522106711",
	"blockchains": [{
		"chainId": "1",
		"network": "ethereum"
	}],
	"dappIcon": "https:\/\/eosknights.io\/img\/icon.png",
	"dappName": "zs",
	"protocol": "DOMWALLET",
	"version": "2.0"
}
```

**拉起钱包转账**

Copy

```
<a href='tpoutside://pull.activity?param={}'>Open DOMWALLET to transfer</a><br/>
```

param示例内容如下

Copy

```
{
	"amount": 0.1,
	"contract": "0x1161725d019690a3e0de50f6be67b07a86a9fae1",
	"decimal": 18,
	"desc": "",
	"from": "0x12F4900A1fB41f751b8F616832643224606B75B4",
	"memo": "0xe595a6",
	"precision": 0,
	"symbol": "SPT",
	"to": "0x34018569ee4d68a275909cc2538ff67a742f41c8",
	"action": "transfer",
	"actionId": "web-db4c5466-1a03-438c-90c9-2172e8becea5",
	"blockchains": [{
		"chainId": "1",
		"network": "ethereum"
	}],
	"dappIcon": "https:\/\/eosknights.io\/img\/icon.png",
	"dappName": "Test demo",
	"protocol": "DOMWALLET",
	"callbackUrl": "http:\/\/115.205.0.178:9011\/taaBizApi\/taaInitData",
	"version": "2.0"
}
```

**拉起钱包签名**

Copy

```
<a href='tpoutside://pull.activity?param={}'>Open DOMWALLET to pushT   ransaction</a><br/>
```

param示例内容如下

Copy

```
{
	"txData": "{\"from\":\"0x12F4900A1fB41f751b8F616832643224606B75B4\",\"gasPrice\":\"0x6c088e200\",\"gas\":\"0xea60\",\"chainId\":\"1\",\"to\":\"0x1d1e7fb353be75669c53c18ded2abcb8c4793d80\",\"data\":\"0xa9059cbb000000000000000000000000171a0b081493722a5f22ebe6f0c4adf5fde49bd8000000000000000000000000000000000000000000000000000000000012c4b0\"}",
	"action": "pushTransaction",
	"actionId": "web-db4c5466-1a03-438c-90c9-2172e8becea5",
	"blockchains": [{
		"chainId": "1",
		"network": "ethereum"
	}],
	"callbackUrl": "http:\/\/115.205.0.178:9011\/taaBizApi\/taaInitData",
	"dappIcon": "https://eosknights.io/img/icon.png",
	"dappName": "Test demo",
	"protocol": "DOMWALLET",
	"version": "2.0"
}
```

TxData字段取值请参考[示例](https://github.com/TP-Lab/tp-wallet-sdk/blob/master/TxData%20Example.md)

**拉起钱包签名字符串**

Copy

```
<a href='tpoutside://pull.activity?param={}'>Open DOMWALLET to sign message</a><br/>
```

param示例内容如下

Copy

```
{
	"hash": false,
	"memo": "demo",
	"message": "0xc05dfb5d7d33ef21dacffc010ff0a45204a3dd5e0cf6f9a970f07339d7a7770e",
	"signType": "ethSign",
	"action": "sign",
	"actionId": "web-db4c5466-1a03-438c-90c9-2172e8becea5",
	"blockchains": [{
		"chainId": "1",
		"network": "ethereum"
	}],
	"callbackUrl": "http:\/\/115.205.0.178:9011\/taaBizApi\/taaInitData",
	"dappIcon": "https:\/\/eosknights.io\/img\/icon.png",
	"dappName": "Test demo",
	"protocol": "DOMWALLET",
	"version": "2.0"
}
```

#### 如何从TP钱包返回到H5应用 <a href="#ru-he-cong-tp-qian-bao-fan-hui-dao-h5-ying-yong" id="ru-he-cong-tp-qian-bao-fan-hui-dao-h5-ying-yong"></a>

使用**callbackSchema**，完成操作后，TP钱包会通过**callbackSchema**拉起H5应用，并回传结果

Copy

```
 "callbackSchema": "mySchema://myHost"
```

**拉起钱包用Dapp浏览器打开链接**

Copy

```
<a href='tpdapp://open?params={}'>Open DOMWALLET to open url</a><br/>
```

params示例内容如下

Copy

```
{
	"url": "https://dapp.myDOMWALLET.vip/referendum/index.html#/",
	"chain": "EOS",
	"source": "xxx" 
}
```

Copy

```
字段说明
chain：dapp支持的网络
source： 来源，开发者自定义
```

**注意param参数需要encode：URLEncoder.encode(param, “utf-8”)**
