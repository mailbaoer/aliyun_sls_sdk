# AliyunSls

## 阿里云SLS服务Ruby SDK


https://cn.aliyun.com/product/sls

------------
## 库用法

### [查询Store清单](http://docs.aliyun.com/#/pub/sls/api/apilist&ListLogstores)

```
endpoint = ''
project = ''
access_key_secret = ''
aliyun_access_key = ''
logstore = ''
conn = AliyunSlsSdk::LogClient.new(endpoint, access_key_secret, aliyun_access_key)
conn.get_logstore(project, logstore)
```

### [上传日志](http://docs.aliyun.com/#/pub/sls/api/apilist&PutLogs)
```
endpoint = ''
project = ''
access_key_secret = ''
aliyun_access_key = ''
logstore = ''
conn = AliyunSlsSdk::LogClient.new(endpoint, access_key_secret, aliyun_access_key)


topic=nil
source=nil
logitems = [{timestamp: Time.now.to_i, contents: [['aaa', '111'], ['ccc', 222]]}]
hashKey = nil
compress = false

logs = AliyunSlsSdk::PutLogsRequest.new(project, logstore, topic, source, logitems, hashKey, compress)

conn.put_logs(logs)
```

### [列出日志主题](http://docs.aliyun.com/#/pub/sls/api/apilist&ListTopics)

    con.list_topics("store")

|名称|  类型|  必选|  描述|
|-----|-----|----|-----|
|logstorename|    字符串| 是|   需要查询的Logstore名称。|
|type|    字符串| 是|   查询Logstore数据的类型，在ListTopics接口中该参数必须为"topic"。|
|line|    整型|  否|   请求一次返回的Topic最大数目。取值范围为0~100，默认值为100。|
|token|   字符串| 否|   请求返回Topic的起始点（按字典序）。默认值为空字符串，表示从头开始查询 |Logstore中的日志Topic。

### [查询Logstore中的日志在时间轴上的分布](http://docs.aliyun.com/#/pub/sls/api/apilist&GetHistograms)

    con.get_histograms("store")

|名称|  类型|  必选|  描述|
|----|----|------|------|
|logstorename|    字符串| 是|   需要查询日志的Logstore名称。|
|type|    字符串| 是|   查询Logstore数据的类型，在GetHistograms接口中该参数必须为"histogram"。|
|from|    整型|  是|   查询开始时间点（精度为秒，从1970-1-1 00:00:00 UTC计算起的秒数）。|
|to|  整型|  是|   查询结束时间点（精度为秒，从1970-1-1 00:00:00 UTC计算起的秒数）。|
|topic|   字符串| 否|   查询日志主题。|
|query|   字符串| 否|   查询表达式。关于查询表达式的详细语法请参考查询语法。|

### [查询Logstore中的日志数据](http://docs.aliyun.com/#/pub/sls/api/apilist&GetLogs)

    con.get_logs("store", :topic => "xxx")

|名称 |  类型|  必选|  描述|
|---|---|---|------------|
|logstorename |    字符串| 是|   需要查询日志的Logstore名称。|
|type |    字符串| 是|   查询Logstore数据的类型，在GetLogs接口中该参数必须为"log"。|
|from |    整型|  是|   查询开始时间点（精度为秒，从1970-1-1 00:00:00 UTC计算起的秒数）。|
|to |  整型|  是|   查询结束时间点（精度为秒，从1970-1-1 00:00:00 UTC计算起的秒数）。|
|topic |   字符串| 否|   查询日志主题。|
|query |   字符串| 否|   查询表达式。关于查询表达式的详细语法请参考查询语法。|
|line |    整型|  否|   请求返回的最大日志条数。取值范围0~100，默认值为100。|
|offset |  整型|  否|   请求返回日志的起始点。取值范围0或正整数，默认值0。|
|reverse | 布尔型| 否|   是否按日志时间戳逆序返回日志。true表示逆序，false表示顺序，默认值为false。|

## 命令行用法

1. 生成配置文件
    2. `PROJECT=[PROJECT] REGION=[REGION] SECRET=[SECRET] KEY=[KEY] sls setup`
1. 执行上传日志操作
    2. `[PROJECT=[PROJECT]] sls put log_path store [topic]`
    2. 上传日志操作是使用Tail方式执行，有增量日志产生的时候，会自动收集增量部分上传到阿里云
    
## fluentd-plugin-sls

可以结合fluentd，将日志解析好之后上传到阿里云上，实现日志的统一存储。

`gist`地址：`https://gist.github.com/charlescui/d2a231dbc85b11586fa0`

## Installation

Add this line to your application's Gemfile:

    gem 'aliyun_sls_sdk'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install aliyun_sls_sdk

## Usage

TODO: Write usage instructions here

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
