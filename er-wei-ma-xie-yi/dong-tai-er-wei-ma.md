# 动态二维码

动态二维码主要解决日常开发中单张图片二维码数据量太大，无法扫描问题。

DOMWALLET动态二维码协议如下:

Copy

```

tp:multiFragment-version=1.0&protocol=DOMWALLET&data={
	"content": "0000000000004454F5300000000008_32342342", //分片内容
	"index": "5/7" //5代表当前片的索引，7是整个二维码内容分片数目
}

tp:multiFragment //协议头
version //版本号
protocol //标识DOMWALLET协议
content //下划线前面:分片内容， 下划线后面：整个数据块计算出的crc32。如果两个分片的crc32一样，我们可以认为他们是同一份数据的两片分片
index //分片索引(从0开始计算)和分片总数
```

#### 如何使用 <a href="#ru-he-shi-yong" id="ru-he-shi-yong"></a>

现在我们有一份数据aaaaabbbbbbbbbbbccccccccccccddddddddddddddeeeeeeeeeeeeeee,需要用动态二维码展示

* 将数据分成2片，aaaaabbbbbbbbbbbcccccccccccc和ddddddddddddddeeeeeeeeeeeeeee
* 产生片数据

Copy

```

tp:multiFragment-version=1.0&protocol=DOMWALLET&data=
    { 
        "content": "aaaaabbbbbbbbbbbcccccccccccc_3207688794",
        "index": "0/2"
    }
tp:multiFragment-version=1.0&protocol=DOMWALLET&data=
    { 
        "content": "aaaaabbbbbbbbbbbcccccccccccc_3207688794",
        "index": "1/2"
    } 
    
```

* 将每一片数据生成图片，组合生成gif或者视频文件

#### 参考文档 <a href="#can-kao-wen-dang" id="can-kao-wen-dang"></a>

> [https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md)
