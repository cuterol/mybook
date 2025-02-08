# TRON

该文档描述了DOMWALLET钱包在TRON网络下的相关二维码协议。DOMWALLETAndroid版本从1.6.7开始支持本协议，TRON网络观察钱包冷钱包交互使用该协议，新版本的应用兼容老版本协议。

#### 签名交易 <a href="#qian-ming-jiao-yi" id="qian-ming-jiao-yi"></a>

Copy

```
//
tron:signTransaction-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data={
	"tx":{ 
            "visible": false,
            "txID": "a08fd7a3ad426a4dd9fa6654c36293d8dd8db3bf961bd9820696477f82b7572e",
            "raw_data": {
                "contract": [{
                    "parameter": {
                        "value": {
                            "amount": 100,
                            "owner_address": "41f90a4115ca0859c0db8415d73b3a22626506cbbe",
                            "to_address": "41be9cd66315067fd1c588b2cf7dd15969de15f556"
                        },
                        "type_url": "type.googleapis.com/protocol.TransferContract"
                    },
                    "type": "TransferContract"
                }],
                "ref_block_bytes": "7cea",
                "ref_block_hash": "cb7295aa4aa80650",
                "expiration": 1676983275000,
                "timestamp": 1676983215610
            },
            "raw_data_hex": "0a027cea2208cb7295aa4aa8065040f89bf89fe7305a65080112610a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412300a1541f90a4115ca0859c0db8415d73b3a22626506cbbe121541be9cd66315067fd1c588b2cf7dd15969de15f556186470facbf49fe730"
        },
	"address":"TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc",
	"useTronHeader":true
}

tron:signTransaction //操作类型
network //网络
tx //完整的波长交易数据
address //指定签名钱包

//签名结果，可以获取到data中数据，直接push到链上完成交易
tron:signTransactionSignature-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data={
	"visible": false,
	"txID": "2d93a1c8e561c40af3685cdcb18421fdf6beb30ec503457701d4e54a7bafd990",
	"raw_data": {
		"contract": [{
			"parameter": {
				"value": {
					"amount": 100,
					"owner_address": "41f90a4115ca0859c0db8415d73b3a22626506cbbe",
					"to_address": "41be9cd66315067fd1c588b2cf7dd15969de15f556"
				},
				"type_url": "type.googleapis.com\/protocol.TransferContract"
			},
			"type": "TransferContract"
		}],
		"ref_block_bytes": "7e1a",
		"ref_block_hash": "629f76b26bb8f439",
		"expiration": 1676984190000,
		"timestamp": 1676984131233
	},
	"raw_data_hex": "0a027e1a2208629f76b26bb8f43940b088b0a0e7305a65080112610a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412300a1541f90a4115ca0859c0db8415d73b3a22626506cbbe121541be9cd66315067fd1c588b2cf7dd15969de15f556186470a1bdaca0e730",
	"__payload__": {
		"to_address": "41be9cd66315067fd1c588b2cf7dd15969de15f556",
		"owner_address": "41f90a4115ca0859c0db8415d73b3a22626506cbbe",
		"amount": 100
	},
	"useTronHeader": true,
	"signature": ["1a5cc4447f5cc5e3fee31ca57cadd6da653634a8f4c7d991554ec687558d009016c552d635f8ed8045b1fecab35fb43a0f1c7bb5a6391431199d5d7003d2e81c00"],
}
```

#### 签名并发送交易 <a href="#qian-ming-bing-fa-song-jiao-yi" id="qian-ming-bing-fa-song-jiao-yi"></a>

Copy

```
//该操作会签名交易，并且发送
tron:signAndSendTransaction-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data={
	"tx":{ 
            "visible": false,
            "txID": "a08fd7a3ad426a4dd9fa6654c36293d8dd8db3bf961bd9820696477f82b7572e",
            "raw_data": {
                "contract": [{
                    "parameter": {
                        "value": {
                            "amount": 100,
                            "owner_address": "41f90a4115ca0859c0db8415d73b3a22626506cbbe",
                            "to_address": "41be9cd66315067fd1c588b2cf7dd15969de15f556"
                        },
                        "type_url": "type.googleapis.com/protocol.TransferContract"
                    },
                    "type": "TransferContract"
                }],
                "ref_block_bytes": "7cea",
                "ref_block_hash": "cb7295aa4aa80650",
                "expiration": 1676983275000,
                "timestamp": 1676983215610
            },
            "raw_data_hex": "0a027cea2208cb7295aa4aa8065040f89bf89fe7305a65080112610a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412300a1541f90a4115ca0859c0db8415d73b3a22626506cbbe121541be9cd66315067fd1c588b2cf7dd15969de15f556186470facbf49fe730"
        },
	"address":"TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc",
	"useTronHeader":true
}

tron:signTransaction //操作类型
network //网络
tx //完整的波长交易数据
address //指定签名钱包
```

#### 签名字符串 <a href="#qian-ming-zi-fu-chuan" id="qian-ming-zi-fu-chuan"></a>

Copy

```
// 
tron:signMessage-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data=
{
    "message":"abc",
    "address":"TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc", //可选，指定签名钱包，如果为空，则使用用户自己选择的钱包签名
    "useTronHeader":true
}

签名结果
tron:signMessageSignature-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data=
{
     "signature": "0x56fca61bc9460b7bf706f79a31bb55e9f50f9a7903d31bae63aa01e7c6d52f7d002c6fc3b4909f45a5ce79e98c32adecf7eda58414f9d7d6521af04fb10cc1cb1c",
     "address": "TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc" //该signature是由这个地址签名所得
}
```

#### SignMessageV2 <a href="#signmessagev2" id="signmessagev2"></a>

Copy

```
// 
tron:signMessageV2-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data=
{
    "message":"abc",
    "address":"TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc", //可选，指定签名钱包，如果为空，则使用用户自己选择的钱包签名
}

签名结果
tron:signMessageV2Signature-version=1.0&protocol=DOMWALLET&network=tron&chain_id=11111&data=
{
     "signature": "0x56fca61bc9460b7bf706f79a31bb55e9f50f9a7903d31bae63aa01e7c6d52f7d002c6fc3b4909f45a5ce79e98c32adecf7eda58414f9d7d6521af04fb10cc1cb1c",
     "address": "TYg1YJTQqaeWF8yhFFcnkEExYpFbAHuyyc" //该signature是由这个地址签名所得
```
