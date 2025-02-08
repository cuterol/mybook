# EOSIO

该文档描述了DOMWALLET钱包在EOSIO网络下的相关二维码协议。DOMWALLETAndroid版本从1.6.7开始支持本协议，EOSIO网络观察钱包冷钱包交互使用该协议，新版本的应用兼容老版本协议。EOSIO网络包含EOS，WAX等网络

#### 签名交易 <a href="#qian-ming-jiao-yi" id="qian-ming-jiao-yi"></a>

Copy

```
// 
eosio:signTransaction-version=1.0&protocol=DOMWALLET&network=eosio&chain_id=aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906&data=
{
    "tx": {
        "expiration": "2023-02-18T08:23:17",
        "ref_block_num": 9494,
        "ref_block_prefix": 640410476,
        "max_net_usage_words": 0,
        "max_cpu_usage_ms": 0,
        "delay_sec": 0,
        "actions": [{
            "account": "eosio.token",
            "name": "transfer",
            "authorization": [{
                "actor": "uv4a2wxeliep",
                "permission": "active"
            }],
            "data": "50958BAA7361C8D6602483A5C79A9F98010000000000000004454F530000000000"
        }],
        "context_free_actions": [],
        "transaction_extensions": []
    },
    //通过account和publicKey指定签名钱包
    "account": "uv4a2wxeliep",
    "publicKey": "EOS5xkSgDHsN3YjzbJHghtHCwbz5ScnDgTMNWvTGNJF3wQmqzFDPm"
}

//签名结果 data中的数据即为可以直接push到链上数据
eosio:signTransactionSignature-version=1.0&protocol=DOMWALLET&network=ethereum&chain_id=1&data=
{
	"expiration": "2023-02-21T13:35:35",
	"ref_block_num": 40998,
	"ref_block_prefix": 926319420,
	"max_net_usage_words": 0,
	"max_cpu_usage_ms": 0,
	"delay_sec": 0,
	"actions": [{
		"account": "eosio.token",
		"name": "transfer",
		"authorization": [{
			"actor": "qn1ux1bd1t2x",
			"permission": "active"
		}],
		"data": "D0450EE984AEC3B4602483A5C79A9F98010000000000000004454F530000000000"
	}],
	"context_free_actions": [],
	"transaction_extensions": [],
	"signatures": ["SIG_K1_K6XBBChkuYkmkRYw9ePKKJbFF5aTAmmU6CnbHPRnbbGzxkRHwb88E6uBCTVxubX1PjCzyZzuH93FQYsBYcU58BvLSpQuCM"]
}
```

#### 签名并发送交易 <a href="#qian-ming-bing-fa-song-jiao-yi" id="qian-ming-bing-fa-song-jiao-yi"></a>

Copy

```
// 
eosio:signAndSendTransaction-version=1.0&protocol=DOMWALLET&network=eosio&chain_id=aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906&data=
{
    "tx": {
        "expiration": "2023-02-18T08:23:17",
        "ref_block_num": 9494,
        "ref_block_prefix": 640410476,
        "max_net_usage_words": 0,
        "max_cpu_usage_ms": 0,
        "delay_sec": 0,
        "actions": [{
            "account": "eosio.token",
            "name": "transfer",
            "authorization": [{
                "actor": "uv4a2wxeliep",
                "permission": "active"
            }],
            "data": "50958BAA7361C8D6602483A5C79A9F98010000000000000004454F530000000000"
        }],
        "context_free_actions": [],
        "transaction_extensions": []
    },
    //通过account和publicKey指定签名钱包
    "account": "uv4a2wxeliep",
    "publicKey": "EOS5xkSgDHsN3YjzbJHghtHCwbz5ScnDgTMNWvTGNJF3wQmqzFDPm"
}
```

#### 签名字符串 <a href="#qian-ming-zi-fu-chuan" id="qian-ming-zi-fu-chuan"></a>

Copy

```
//
eosio:signMessage-version=1.0&protocol=DOMWALLET&network=eosio&chain_id=aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906&data=
{
    "message":"0xabcd",
    "isHash":true, //如果isHash为true,用ecc.Signature.signHash，否则使用ecc.sign签名
    //通过account和publicKey指定签名钱包
    "account": "uv4a2wxeliep",
    "publicKey": "EOS5xkSgDHsN3YjzbJHghtHCwbz5ScnDgTMNWvTGNJF3wQmqzFDPm"
}

//签名结果
eosio:signMessage-version=1.0&protocol=DOMWALLET&network=eosio&chain_id=aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906&data=
{
    "signature":"xxx",
    //签名钱包
    "account": "uv4a2wxeliep",
    "publicKey": "EOS5xkSgDHsN3YjzbJHghtHCwbz5ScnDgTMNWvTGNJF3wQmqzFDPm"
}
```
