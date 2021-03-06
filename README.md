



## 日志配置功能
日志收集功能用于 SCF 转码请求发送的记录与转码结果统计，该版本代码支持安填统计和按次统计，将在设置的日志目录下生成响应的 csv 文件，文件格式如下：

名称 | 类型 |  描述 
:-: | :-: | :-: 
fileId | string | 标志一个转码输出文件
uploadTime | string | 源文件上传时间
inputFile | string | 源文件上传路径
inputVideoUrl | string | 源文件下载链接
resCode | int | 转码请求返回码
resMessage | string | 转码请求返回结果
vodTaskId | string | 转码请求返回任务 ID（可用于查询任务状态）
status | string | 转码执行状态 
outputFile | string | 输出文件路径



#### 添加步骤
这里假定您已经设置好自动转码无服务云函数，若还没设置，可[参考文档](https://cloud.tencent.com/document/product/266/15572)配置上传触发转码的 SCF，其中将配置文件 stat.enable设置为true,region设置为 cos [所在地域](https://cloud.tencent.com/document/product/436/6224)。，例如 ap-beijing、ap-guangzhou 等。

1. 在转码输出 bucket 地域下行新键无服务器云函数 logtimer，
2. 函数配置中设置运行环境设置为 Nodejs，超时时间设置为300s
3. 触发方式选择定时触发，触发间隔设置为5分钟，点击【保存】。
4. 下载 SCF 函数代码压缩包，解压后重命名 config.templte.json为config.json，填写好帐号信息,包括Appid、，并修改配件 type 为 output，stat.enable 为true，地域为COS所在地域，如ap-beijing、ap-guangzhou等，其他为默认值并压缩成 zip 包
5. 在函数代码中选择本地zip包上传，上传上步骤的 zip 包


### 日志相关配置文件说明

    {

        .....

        "type":"input",            //input:响应上传  output：整理输出日志
        "region":"ap-beijing",     //日志bucket地域
        "stat":{
            "enable":true,         //是否开启日志功能
            "path":"log",          //日志保存目录
            "level":"EACH"         //DAY ：按天保存日志   EACH ：按请求保存日志
        }
    }
