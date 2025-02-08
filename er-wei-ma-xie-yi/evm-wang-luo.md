# EVM网络

该文档描述了DOMWALLET钱包在EVM网络下的相关二维码协议。DOMWALLETAndroid版本从1.6.7开始支持本协议，EVM网络观察钱包冷钱包交互使用该协议，新版本的应用兼容老版本协议。

#### 签名交易 <a href="#qian-ming-jiao-yi" id="qian-ming-jiao-yi"></a>

Copy

```
//
ethereum:signTransaction-version=1.0&protocol=DOMWALLET&network=ethereum
&chain_id=1&data=
    {
    	"from": "0x8894E0a0c962CB723c1976a4421c95949bE2D4E3",
    	"gas": "0x5208",
    	"chainId": 1,
    	"to": "0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42",
    	"value": "0x38d7ea4c68000",
    	"type": "0x2",
    	"maxFeePerGas": "0x11853fe080",
    	"maxPriorityFeePerGas": "0x4a817c80",
    	"nonce": "0x0"
    }


ethereum:signTransaction //标识操作类型
network //该操作支持的网络
chain_id //网络id
data //交易数据

//签名交易结果
ethereum:signTransactionSignature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"rawTransaction":"xxxx"
 }
 
```

#### 签名并发送交易 <a href="#qian-ming-bing-fa-song-jiao-yi" id="qian-ming-bing-fa-song-jiao-yi"></a>

Copy

```
//
ethereum:signAndSendTransaction-version=1.0&protocol=DOMWALLET&network=ethereum
&chain_id=1&data=
    {
    	"from": "0x8894E0a0c962CB723c1976a4421c95949bE2D4E3",
    	"gas": "0x5208",
    	"chainId": 1,
    	"to": "0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42",
    	"value": "0x38d7ea4c68000",
    	"type": "0x2",
    	"maxFeePerGas": "0x11853fe080",
    	"maxPriorityFeePerGas": "0x4a817c80",
    	"nonce": "0x0"
    }
```

#### ethPersonalSign签名 <a href="#ethpersonalsign-qian-ming" id="ethpersonalsign-qian-ming"></a>

Copy

```
//
 ethereum:personalSign-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"message":"abcdeea",
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //可选，如果填写了地址，钱包会去找指定地址签名
 }
 ethereum:personalSignSignature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"signature":"xxx", //签名结果
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //签名地址
 }
```

#### ethSignTypedDataLegacy签名 <a href="#ethsigntypeddatalegacy-qian-ming" id="ethsigntypeddatalegacy-qian-ming"></a>

Copy

```
//
 ethereum:signTypedDataLegacy-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"message": [{
		"type": "string",
		"name": "Message",
		"value": "Hi, Alice!"
	}, {
		"type": "uint32",
		"name": "A number",
		"value": "1337"
	}],
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //可选，如果填写了地址，钱包会去找指定地址签名
 }
 ethereum:signTypedDataLegacySignature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"signature":"xxx", //签名结果
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //签名地址
 }
```

#### ethSignTypedData <a href="#ethsigntypeddata" id="ethsigntypeddata"></a>

Copy

```
//
ethereum:signTypeData-version=1&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
          "message":{"types": {
		"EIP712Domain": [{
			"name": "name",
			"type": "string"
		}, {
			"name": "version",
			"type": "string"
		}, {
			"name": "chainId",
			"type": "uint256"
		}, {
			"name": "verifyingContract",
			"type": "address"
		}],
		"Person": [{
			"name": "name",
			"type": "string"
		}, {
			"name": "wallet",
			"type": "address"
		}],
		"Mail": [{
			"name": "from",
			"type": "Person"
		}, {
			"name": "to",
			"type": "Person"
		}, {
			"name": "contents",
			"type": "string"
		}]
	},
	"primaryType": "Mail",
	"domain": {
		"name": "Ether Mail",
		"version": "1",
		"chainId": 1,
		"verifyingContract": "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC"
	},
	"message": {
		"sender": {
			"name": "Cow",
			"wallet": "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826"
		},
		"recipient": {
			"name": "Bob",
			"wallet": "0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB"
		},
		"contents": "Hello, Bob!"
	}
	},
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42"
}

//签名结果
 ethereum:signTypeDataSignature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"signature":"xxx", //签名结果
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //签名地址
 }
```

#### ethSignTypedData\_V4 <a href="#ethsigntypeddata_v4" id="ethsigntypeddata_v4"></a>

Copy

```
// 
 ethereum:signTypeDataV4-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"message":{
	  "domain": {
		"chainId": 1,
		"name": "EtherMail",
		"verifyingContract": "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC",
		"version": "1"
	 },
	  "message": {
		"contents": "Hello,Bob!",
		"from": {
			"name": "Cow",
			"wallets": ["0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826", "0xDeaDbeefdEAdbeefdEadbEEFdeadbeEFdEaDbeeF"]
		},
		"to": [{
			"name": "Bob",
			"wallets": ["0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB", "0xB0BdaBea57B0BDABeA57b0bdABEA57b0BDabEa57", "0xB0B0b0b0b0b0B000000000000000000000000000"]
		}]
	},
	"primaryType": "Mail",
	"types": {
		"EIP712Domain": [{
			"name": "name",
			"type": "string"
		}, {
			"name": "version",
			"type": "string"
		}, {
			"name": "chainId",
			"type": "uint256"
		}, {
			"name": "verifyingContract",
			"type": "address"
		}],
		"Group": [{
			"name": "name",
			"type": "string"
		}, {
			"name": "members",
			"type": "Person[]"
		}],
		"Mail": [{
			"name": "from",
			"type": "Person"
		}, {
			"name": "to",
			"type": "Person[]"
		}, {
			"name": "contents",
			"type": "string"
		}],
		"Person": [{
			"name": "name",
			"type": "string"
		}, {
			"name": "wallets",
			"type": "address[]"
		}]
		}
	},
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //可选，如果填写了地址，钱包会去找指定地址签名
 }
 ethereum:signTypeDataV4Signature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data={
	"signature":"xxx", //签名结果
	"address":"0xdA5535939971B55c0Ecf4dAc961a8854D0aFad42" //签名地址
 }
```

#### 参考文档 <a href="#can-kao-wen-dang" id="can-kao-wen-dang"></a>

[https://github.com/ethereum/EIPs/blob/9e393a79d9937f579acbdcb234a67869259d5a96/EIPS/eip-681.md](https://github.com/ethereum/EIPs/blob/9e393a79d9937f579acbdcb234a67869259d5a96/EIPS/eip-681.md)
