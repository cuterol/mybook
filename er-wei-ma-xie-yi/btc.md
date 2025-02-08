# BTC

该文档描述了DOMWALLET钱包在Bitcoin网络下的相关二维码协议。DOMWALLETAndroid版本从1.6.7开始支持本协议，Bitcoin网络观察钱包冷钱包交互使用该协议，新版本的应用兼容老版本协议。

#### 签名交易 <a href="#qian-ming-jiao-yi" id="qian-ming-jiao-yi"></a>

Copy

```
// 
bitcoin:signTransaction-version=1.0&protocol=DOMWALLET&network=bitcoin&chain_id=000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f&data=
{
    "tx": {
        "inputs": [{
            "txid": "66eabb2801bd31854e3b66f893e4b5a609de56a507496e26c591d2660e6e7569",
            "path": "m\/49'\/0'\/0'\/0\/0",
            "value": 3.8741E-4,
            "index": 1,
            "type": "scripthash",
            "scriptPubKeyHex": "a9144a982ec12f8bd9fd76f908229c8f11a79fcb6e2487"
        }],
        "outputs": [{
            "address": "32qtkp1TT97BUGkh6BbLApmCsQsG4rWkAu",
            "value": 1.0E-4
        }, {
            "address": "38VSEaFSksDFcVeAbARU8VZfKJ6apjQ7MD",
            "value": 2.8575E-4
        }]
    },
    "address": "38VSEaFSksDFcVeAbARU8VZfKJ6apjQ7MD" //指定签名钱包
}

//签名结果
bitcoin:signTransaction-version=1.0&protocol=TokenPocket&network=bitcoin&chain_id=000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f&data={
   "signature":"xxxx"
}
```

#### 签名并发送交易 <a href="#qian-ming-bing-fa-song-jiao-yi" id="qian-ming-bing-fa-song-jiao-yi"></a>

Copy

```
// 
bitcoin:signAndSendTransaction-version=1.0&protocol=DOMWALLET&network=bitcoin&chain_id=000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f&data=
{
    "tx": {
        "inputs": [{
            "txid": "66eabb2801bd31854e3b66f893e4b5a609de56a507496e26c591d2660e6e7569",
            "path": "m\/49'\/0'\/0'\/0\/0",
            "value": 3.8741E-4,
            "index": 1,
            "type": "scripthash",
            "scriptPubKeyHex": "a9144a982ec12f8bd9fd76f908229c8f11a79fcb6e2487"
        }],
        "outputs": [{
            "address": "32qtkp1TT97BUGkh6BbLApmCsQsG4rWkAu",
            "value": 1.0E-4
        }, {
            "address": "38VSEaFSksDFcVeAbARU8VZfKJ6apjQ7MD",
            "value": 2.8575E-4
        }]
    },
    "address": "38VSEaFSksDFcVeAbARU8VZfKJ6apjQ7MD" //指定签名钱包
}
```
