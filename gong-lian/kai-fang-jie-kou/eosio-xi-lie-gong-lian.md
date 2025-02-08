# EOSIO系列公链

一、**获取交易记录列表**

* 功能：交易记录列表查询
* 方法：get
* url: /v1/transaction\_action/universal\_list

请求参数列表：

参数

说明

类型

备注

ns

命名空间

字符串

Namespace，通过ns查询数据才存在

chain\_id

链ID

整数

通过ns查询数据才存在，和ns一起数据

blockchain\_id

整数

整数

ns + chian\_id或者 blockchain\_id其中一个查询添加即可

search

查询

字符串

code

账户地址

字符串

account

合约地址

字符串

symbol

代币符号

字符串

page

页数

整数

count

总数

整数

sort

排序

字符串

desc

type

类型

整数

0: 所有， 1：发起人from, 2: 接受人to

响应数据列表：

参数

说明

类型

备注

Hid

交易id

string

ProducerBlockId

出块节点ID

string

Receiver

回执

string

RecvSequence

接收序列号

int64

Timestamp

交易时间

int64

Account

账户

string

Name

方法Name

string

From

发起者

string

To

接受者

string

BlockNum

区块号

int64

Quantity

数量

string

Count

Count

string

Symbol

代币符号

string

Memo

备注

string

Status

交易状态

int64

示例

Copy

```
请求：
https://xxxtxserver.xxx.com/v1/transaction_action/universal_list?blockchain_id=29&search=&account=eosio.token&count=20&page=0

响应：
{
    "data": [
        {
            "hid": "817d47c381f284b269ca372ae6b911804bb7ed50efcf2bc2784c88d2b13e7f8a",
            "producerBlockId": "0a03ac4ea8e00e0898d7410b5c422898d85914bb1c6b94ebbee099b55d02b8ae",
            "receiver": "eosio.token",
            "recvSequence": 0,
            "timestamp": 1645463197,
            "account": "realmnftgame",
            "name": "transfer",
            "from": "whkm2.c.wam",
            "to": "eosio.token",
            "blockNum": 168012878,
            "quantity": "974.9580 RLM",
            "count": "974.9580",
            "symbol": "RLM",
            "memo": "",
            "status": 1
        },
        {
            "hid": "20608e08fbb38618b2021bea05dff84cf27c1cd066c2e1eb006a5ecc0d06c7ee",
            "producerBlockId": "099918c3de0e307403a154f33f5123a29bfd1692df31cd4d44513ae2bcd01a6d",
            "receiver": "eosio.token",
            "recvSequence": 0,
            "timestamp": 1641969324,
            "account": "farmerstoken",
            "name": "transfer",
            "from": "r52zc.wam",
            "to": "eosio.token",
            "blockNum": 161028291,
            "quantity": "10.0000 FWW",
            "count": "10.0000",
            "symbol": "FWW",
            "memo": "e.3.e.c.wam",
            "status": 1
        }
    ],
    "message": "success",
    "result": 0
}
```

**二、获取交易记录详情**

* 功能：交易记录详情查询
* 方法：get
* url: /v1/transaction\_action/universal

请求参数列表：

参数

说明

类型

备注

ns

命名空间

字符串

Namespace，通过ns查询数据才存在

chain\_id

链ID

整数

通过ns查询数据才存在，和ns一起数据

blockchain\_id

整数整数

整数

ns + chian\_id或者 blockchain\_id其中一个查询添加即可

producer\_block\_Id

producer\_block\_Id

字符串

receiver

receiver

字符串

recv\_sequence

recv\_sequence

整数

响应数据列表：

参数

说明

类型

备注

Hid

交易id

string

ProducerBlockId

出块节点ID

string

Receiver

回执

string

RecvSequence

接收序列号

int64

Timestamp

交易时间

int64

Account

账户

string

Name

方法Name

string

From

发起者

string

To

接受者

string

BlockNum

区块号

int64

Quantity

数量

string

Count

Count

string

Symbol

代币符号

string

Memo

备注

string

Status

交易状态

int64

示例

Copy

```
请求：
https://xxxtxserver.xxx.com/v1/transaction_action/universal?blockchain_id=29&producer_block_Id=0a03ac4ea8e00e0898d7410b5c422898d85914bb1c6b94ebbee099b55d02b8ae&receiver=eosio.token&recv_sequence=0

响应：
{
    "data": {
        "hid": "817d47c381f284b269ca372ae6b911804bb7ed50efcf2bc2784c88d2b13e7f8a",
        "producerBlockId": "0a03ac4ea8e00e0898d7410b5c422898d85914bb1c6b94ebbee099b55d02b8ae",
        "receiver": "eosio.token",
        "recvSequence": 0,
        "timestamp": 1645463197,
        "account": "realmnftgame",
        "name": "transfer",
        "from": "whkm2.c.wam",
        "to": "eosio.token",
        "blockNum": 168012878,
        "quantity": "974.9580 RLM",
        "count": "974.9580",
        "symbol": "RLM",
        "memo": "",
        "status": 1
    },
    "message": "success",
    "result": 0
}
```
