# 开放接口

#### 一、背景描述 <a href="#yi-bei-jing-miao-shu" id="yi-bei-jing-miao-shu"></a>

DOMWALLET钱包支持通过提交PR，新增公链相关配置即可完成对公链的支持；现已经支持**EVM,Polkadot,EOSIO**技术系列公链。

本文档针对需要讲公链集成到DOMWALLET钱包，共享DOMWALLET软件钱包能力的项目方。首先项目方的链必须是**EVM,Polkadot,EOSIO**技术系列， 然后按照本文档描述的开放接口（更多的接口陆续开放中）提供相应的API接口，然后在[networklist-org](https://github.com/TP-Lab/networklist-org)PR，最后由DOMWALLET社区人员审核完成Merge后, 即可在DOMWALLET钱包快速集成项目方的链。

#### 二、开放接口列表 <a href="#er-kai-fang-jie-kou-lie-biao" id="er-kai-fang-jie-kou-lie-biao"></a>

* 获取地址Token列表
* 获取地址NFT列表
* 获取地址Defi数据列表
* 获取Token价格相关数据列表
* 获取热门Token列表
* 获取推荐gas费数据
* 获取交易记录列表详情
* 获取交易记录详情
* RPC节点列表
* 区块链浏览器列表

#### 三、开放接口详情描述 <a href="#san-kai-fang-jie-kou-xiang-qing-miao-shu" id="san-kai-fang-jie-kou-xiang-qing-miao-shu"></a>

**获取交易记录列表和交易记录详情**

目前DOMWALLET钱包已支持链交易记录和交易记录详情显示效果，如下图所示：

![](https://help.tokenpocket.pro/~gitbook/image?url=https%3A%2F%2F213089712-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FRjeSa1rqnubm9jQ67F9z%252Fuploads%252FBCnpWGhzIodKzaEBMFYA%252F05.png%3Falt%3Dmedia%26token%3Dbdef0720-c5fd-4cc6-87f9-61d256f497f5\&width=768\&dpr=4\&quality=100\&sign=3143d1e5\&sv=2)

提交信息格式

Copy

```
"txUrl":" https://xxxchaintxserver.xxx.xxx"
```

**节点信息**

​ 节点是钱包发起交易、查询价格信息和查询合约信息等功能的请求地址，因此项目方需要提供一个以上可用节点。节点信息在钱包中显示如下图所示：

![](https://help.tokenpocket.pro/~gitbook/image?url=https%3A%2F%2F213089712-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FRjeSa1rqnubm9jQ67F9z%252Fuploads%252FpjFGExsfBJ11aV6aZ6Ev%252Fnode-1.jpg%3Falt%3Dmedia%26token%3Da595dbc9-c1ce-4582-99f9-c580b09e0119\&width=768\&dpr=4\&quality=100\&sign=44e59fc7\&sv=2)

提供信息格式：

Copy

```
rpc: [
	"https://rpc.api.xxxx.network", 
	"https://rpc.api2.xxxxx.network"
]
```

**区块链浏览器**

​ 浏览器作为查询交易信息同时具备一定程度数据分析的工具，因此项目方提供提供浏览器查询交易信息至关重要。浏览器在钱包中其中一个用途如下入图所示：

![](https://help.tokenpocket.pro/~gitbook/image?url=https%3A%2F%2F213089712-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FRjeSa1rqnubm9jQ67F9z%252Fuploads%252FINmucwBBScTIn8axHCfU%252F06.png%3Falt%3Dmedia%26token%3Df8a7f48e-26d7-49d2-b6c1-b4df0b8aff0e\&width=768\&dpr=4\&quality=100\&sign=eae4dc64\&sv=2)

提供信息格式：

Copy

```
浏览器需要支持如下格式查询
https://blockscout.moonbeam.network/tx/{hash}/internal-transactions
https://blockscout.moonbeam.network/address/{account}/transactions

"browserInfo": [{
    "name": "Xscan", 
    "icon": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/1.png", 
    "addr": "https://xxx1scan.io/"
},{
    "name": "xDAIscan", 
    "icon": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/1.png", 
    "addr": "https://xxx2can.io/"
}]
```

#### 四、开放接口提交链信息模版 <a href="#si-kai-fang-jie-kou-ti-jiao-lian-xin-xi-mo-ban" id="si-kai-fang-jie-kou-ti-jiao-lian-xin-xi-mo-ban"></a>

​ 综合上面接口说明。需要提交信息标准模版如下，请按照此模版提交信息。

Copy

```
{
    "name": "xDAI Chain",
    "chainId": 100,
    "namespace": "ethereum",  
    "shortName": "xdai",
    "chain": "XDAI",
    "network": "mainnet",
    "networkId": 100,
    "nativeCurrency": {
        "name": "xDAI",
        "symbol": "xDAI",
        "decimals": 18
    },
    "rpc": [
        "https://rpc.xdaichain.com",
        "https://xdai.poanetwork.dev",
        "wss://rpc.xdaichain.com/wss",
        "wss://xdai.poanetwork.dev/wss",
        "http://xdai.poanetwork.dev",
        "https://dai.poa.network",
        "ws://xdai.poanetwork.dev:8546"
    ],
    "faucets": [],
    "infoURL": "https://forum.poa.network/c/xdai-chain",
    "appResource": {
        "icChainSelect": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/1.png",
        "icChainUnselect": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/0.png",
        "colorChainBg": "0x58B2AF",
        "txUrl":" https://xxxchaintxserver.xxx.xxx", 
        "browserInfo": [{
            "name": "Xscan", 
            "icon": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/1.png", 
            "addr": "https://xxx1scan.io/"
        },{
            "name": "xDAIscan", 
            "icon": "https://tp-upload.cdn.bcebos.com/v1/blockChain/xDAI/1.png", 
            "addr": "https://xxx2can.io/"
        }],
        "projectContactInfo": {
    	    "officialWebsite": "https://xxx.network.com",
    	    "phone": "02x-223xxx12",
    	    "email": "xxx.xxx@xxx.com",
        }
    }
}
```

以上信息提交地址[networklist-org](https://github.com/TP-Lab/networklist-org)。
