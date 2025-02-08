# Solana

该文档描述了DOMWALLET钱包在Solana网络下的相关二维码协议。DOMWALLETAndroid版本从1.6.7开始支持本协议，Solana网络观察钱包冷钱包交互使用该协议，新版本的应用兼容老版本协议。

#### 签名交易 <a href="#qian-ming-jiao-yi" id="qian-ming-jiao-yi"></a>

Copy

```
// 
solana:signSolanaTransaction-version=1.0&protocol=DOMWALLET&network=solana&chain_id=1&data=
{
	"message": ["87PYuq6WCXzKLkumpe8mb94NM1HuGo6aQrziNqZUKdP1dhGNfXXPYww2HDtJVqTak2pUeDdqfU8B5rpEUJ4FVD52VQ4uNNxTyRNHdV3QaG41wy14gS7ZmBe9ESJ6bUzGcerRHynTKSJgL4kVBwwhk3vvxetu2BjkV3BgBTq5f5GVCdYYXuGcfN1UyooyZ48j7XiN16JpmmEo"],
	"address": "FWpVCuNAFYFu5oeyue19RD6pD9gs1Dhq6i642XrTMZRz"
}

//签名结果
solana:signSolanaTransaction-version=1.0&protocol=DOMWALLET&network=solana&chain_id=1&data=
{
	"message": ["87PYuq6WCXzKLkumpe8mb94NM1HuGo6aQrziNqZUKdP1dhGNfXXPYww2HDtJVqTak2pUeDdqfU8B5rpEUJ4FVD52VQ4uNNxTyRNHdV3QaG41wy14gS7ZmBe9ESJ6bUzGcerRHynTKSJgL4kVBwwhk3vvxetu2BjkV3BgBTq5f5GVCdYYXuGcfN1UyooyZ48j7XiN16JpmmEo"],
	"signature": [{
		"signature": "xxx",
		"publicKey": "xxx"
	}]
}
签名结果中，signature数组元素对应message数组中元素的签名结果
```

#### 签名字符串 <a href="#qian-ming-zi-fu-chuan" id="qian-ming-zi-fu-chuan"></a>

Copy

```
// 
 solana:signMessage-version=1.0&protocol=DOMWALLET&network=solana&chain_id=1&data={
	"message":"xxx",
	"address":"FWpVCuNAFYFu5oeyue19RD6pD9gs1Dhq6i642XrTMZRz" //指定签名钱包
 }
 //字符串签名结果
 solana:signMessageSignature-version=1.0&protocol=DOMWALLET&network=solana&chain_id=1&data={
	"signature":{"signature":xxx, "publicKey":xxx},
	"address":"FWpVCuNAFYFu5oeyue19RD6pD9gs1Dhq6i642XrTMZRz"
 }
```
