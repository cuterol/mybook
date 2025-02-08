# Polkadot系列公链

一、**获取交易记录列表**

* ​ 功能：交易记录列表查询
* ​ 方法：get
* ​ url: /v1/transaction\_action/universal\_list

​ 请求参数列表：

参数说明类型备注

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

address

账户地址

字符串

contract\_address

合约地址

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

​ 响应数据列表：

参数说明类型备注

Title

交易的title

string(可缺省)

交易的title， 不存在区块链上

Decimal

代币位数

int64

Fee

费用

string

Tip

小费

string

Symbol

代币名字

string

Comment

备注信息

string(可缺省)

备注信息， 不存在区块链上

Timestamp

交易时间

int64(可缺省)

BlockNumber

区块号

int64(可缺省)

Value

交易数量

string(可缺省)

Hash

交易Hash

string

Nonce

Nonce值

string

CallIndex

交易事件调用序号

int64

From

发起者

string

To

接受者

string

AddrToken

代币地址

string

Input

输入

string

Status

状态

int

1 (success) or 0 (failure) or 2(pending), 99未知

​ 示例

Copy

```
请求：
https://testtxserver.xxx.com/v1/transaction_action/universal_list?address=3iM6wt2uixaTdMj7ZQB6LN9qanvCCxWdd56oVwRg7fmj1uFw&search=&blockchain_id=21&count=40&page=0&sort&contract_address&type=0

响应：
{
    "data": [
        {
            "decimal": 10,
            "fee": "155000015",
            "tip": "0",
            "symbol": "DOT",
            "comment": "0",
            "timestamp": 1633449756,
            "block_number": 7129105,
            "value": "739595473900",
            "hash": "0xdb4cc8adf2b48a50699abc27ca3461b1c497292c1a7fb34cc73fcdbb2c808cc6",
            "nonce": "0",
            "callIndex": 0,
            "from": "1119Ch5Ezu9fKdcZ9ThBFnTeEkUrhfhT59jHjGjKnvVvmsy",
            "to": "16FLM85x3Q8qSthJ9riL5Xcop8GfRY8SxuaDqvtMeeyrZNNs",
            "status": 1
        },
        {
            "decimal": 10,
            "fee": "156000016",
            "tip": "0",
            "symbol": "DOT",
            "comment": "0",
            "timestamp": 1621620846,
            "block_number": 5163171,
            "value": "28645499945",
            "hash": "0xe1e9bb207610dba7a98bbec411b549d951ddb8d6c27941ee9b1d32b26367831f",
            "nonce": "4",
            "callIndex": 0,
            "from": "14TMRRRqJUJszegEd7C3svLWxLUyMQPzXJ1wA6UYRcdKFRQZ",
            "to": "1119Ch5Ezu9fKdcZ9ThBFnTeEkUrhfhT59jHjGjKnvVvmsy",
            "status": 1
        }
    ],
    "message": "success",
    "result": 0
}
```

**二、取交易记录详情**

* ​ 功能：交易记录详情查询
* ​ 方法：get
* ​ url: /v1/transaction\_action/universal

​ 请求参数列表：

参数说明类型备注

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

from

发起人

字符串

nonce

nonce值

字符串

block\_number

区块号

整数

call\_index

交易事件调用序号

字符串

​ 响应数据列表说明：

参数说明类型备注

Title

交易的title

string(可缺省)

交易的title， 不存在区块链上

Decimal

代币位数

int64

Fee

费用

string

Tip

小费

string

Symbol

代币名字

string

Comment

备注信息

string(可缺省)

备注信息， 不存在区块链上

Timestamp

交易时间

int64(可缺省)

BlockNumber

区块号

int64(可缺省)

Value

交易数量

string(可缺省)

Hash

交易Hash

string

Nonce

Nonce值

string

CallIndex

交易事件调用序号

int64

From

发起者

string

To

接受者

string

AddrToken

代币地址

string

Input

输入

string

Status

状态

int

1 (success) or 0 (failure) or 2(pending), 99未知

示例

Copy

```
请求：
https://xxxtxserver.xxx.com/v1/transaction_action/universal?from=14TMRRRqJUJszegEd7C3svLWxLUyMQPzXJ1wA6UYRcdKFRQZ&blockchain_id=13&block_number=5163171&nonce=4&call_index=0

响应：
{
    "data": {
        "decimal": 10,
        "fee": "156000016",
        "tip": "0",
        "symbol": "DOT",
        "comment": "0",
        "timestamp": 1621620846,
        "block_number": 5163171,
        "value": "28645499945",
        "hash": "0xe1e9bb207610dba7a98bbec411b549d951ddb8d6c27941ee9b1d32b26367831f",
        "nonce": "4",
        "callIndex": 0,
        "from": "14TMRRRqJUJszegEd7C3svLWxLUyMQPzXJ1wA6UYRcdKFRQZ",
        "to": "1119Ch5Ezu9fKdcZ9ThBFnTeEkUrhfhT59jHjGjKnvVvmsy",
        "status": 1
    },
    "message": "success",
    "result": 0
}
```

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

[\
](https://help.tokenpocket.pro/developer-cn/public-chain/open-protocols/ethereum-namespace)
