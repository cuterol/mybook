# Ethereum系列公链

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

查询地址

address

账户地址

字符串

contract\_address

合约地址

字符串

token\_id

token id

整数

erc721 tokenid

page

页数

整数

0

count

总数

整数

20

sort

排序

字符串

desc

fork

链是否分叉

整数

UnFork = 0 Fork = 1

type

类型

整数

0: 所有， 1：发起人f, 2: 接受人

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

TokenValue

代币数量

string(可缺省)

Gas

Gas费用

string(可缺省)

GasPrice

Gas价格

string(可缺省)

UsedGas

Gas已使用量

string(可缺省)

Value

交易数量

string(可缺省)

Hash

交易Hash

string

Nonce

Nonce值

string

BlockHash

区块Hash

string(可缺省)

TransactionIndex

交易序号

int64(可缺省)

LogIndex

Log序号

int64(可缺省)

InternalIndex

内部交易序号

string(可缺省)

From

发起者

string

To

接受者

string

AddrToken

代币地址

string

TokenId

NFT TokenId

string(可缺省)

Type

类型

int64

1 表示是代币 0 表示Eth(原生币)

Input

输入

string

InputStatus

输入状态

int64

0表示没有，1表示有

Status

状态

int

1 (success) or 0 (failure) or 2(pending), 99未知

ErrorMessage

失败信息

string(可缺省)

BaseFee

基本费用

string(可缺省)

GasTipCap

小费

string(可缺省)

GasFeeCap

Gas费用容量

string(可缺省)

​ 示例

Copy

```
请求：
https://testtxserver.xxx.com/v1/transaction_action/universal_list?new_way=tp&search=&address=0x0Dd3758c88316723eC434C54BF3e56e733785DFE&blockchain_id=26&count=20&page=0&sort=&contract_address=&type=0

响应：
"data": [{
            "decimal": 18,
            "fee": "0.000254",
            "symbol": "ETH",
            "timestamp": 1644481313,
            "block_number": 3393917,
            "gas": "21000",
            "gas_price": "1000000",
            "used_gas": "21000",
            "value": "100000000000000",
            "hash": "0x1305d359d3796a5b9c1f0b3e8e204933a16fdbc9677667cbb6ea8db3f9a83611",
            "nonce": "0x4",
            "block_hash": "0x576634593fabbd92af5af5671f64886cb412b557e2aa465ccd12b340360568a4",
            "log_index": -1,
            "from": "0x0dd3758c88316723ec434c54bf3e56e733785dfe",
            "to": "0x0657659db21230061aae817ae26e5d15de66cf2e",
            "addr_token": "",
            "type": 0,
            "gas_price_bid": "",
            "gas_price_paid": "",
            "result_type": 0,
            "tx_type": 0,
            "input": "0x",
            "input_status": 0,
            "status": 1
}]
```

​

**二、获取交易记录详情**

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

block\_hash

区块Hash

字符串

hash

hash

字符串

log\_index

Log序号

整数

internal\_index

内部交易序号

字符串

​

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

TokenValue

代币数量

string(可缺省)

Gas

Gas费用

string(可缺省)

GasPrice

Gas价格

string(可缺省)

UsedGas

Gas已使用量

string(可缺省)

Value

交易数量

string(可缺省)

Hash

交易Hash

string

Nonce

Nonce值

string

BlockHash

区块Hash

string(可缺省)

TransactionIndex

交易序号

int64(可缺省)

LogIndex

Log序号

int64(可缺省)

InternalIndex

内部交易序号

string(可缺省)

From

发起者

string

To

接受者

string

AddrToken

代币地址

string

TokenId

NFT TokenId

string(可缺省)

Type

类型

int64

1 表示是代币 0 表示Eth(原生币)

Input

输入

string

InputStatus

输入状态

int64

0表示没有，1表示有

Status

状态

int

1 (success) or 0 (failure) or 2(pending), 99未知

ErrorMessage

失败信息

string(可缺省)

BaseFee

基本费用

string(可缺省)

GasTipCap

小费

string(可缺省)

GasFeeCap

Gas费用容量

string(可缺省)

示例

Copy

```
请求：
https://xxxchaintxserver.xxx.com/v1/transaction_action/universal?blockchain_id=26&block_hash=0xf07696455e6bb138cff50390da1e47eb80745efdbcc16a609e4cec955f6a07af&hash=0x539839a5a2dc3bf4edb3dca041fce0aec0e557811cdf7518007d6712d873726a&log_index=-1&internal_index=

响应：
{
    "data": {
        "decimal": 0,
        "fee": "0.000763",
        "symbol": "",
        "timestamp": 1639603935,
        "block_number": 1228952,
        "token_value": "0",
        "gas": "113334",
        "gas_price": "1000000",
        "used_gas": "113334",
        "value": "0",
        "hash": "0x539839a5a2dc3bf4edb3dca041fce0aec0e557811cdf7518007d6712d873726a",
        "nonce": "0x5",
        "block_hash": "0xf07696455e6bb138cff50390da1e47eb80745efdbcc16a609e4cec955f6a07af",
        "log_index": -1,
        "from": "0x389264158278811653",
        "to": "0x389254976595034117",
        "addr_token": "389253055922569221",
        "type": 1,
        "gas_price_bid": "",
        "gas_price_paid": "",
        "result_type": 0,
        "tx_type": 0,
        "input": "0x1ebcfe800000000000000000000000000000000000000000000000000000000000000100",
        "input_status": 0,
        "status": 1
    },
    "message": "success",
    "result": 0
}
```
