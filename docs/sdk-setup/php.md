

# PHP SDK 安装指南

## 获取 SDK

获取 SDK 有多种方式，较为推荐的方式是通过包依赖管理工具下载最新版本。

### 包依赖管理工具安装



#### composer

composer 是推荐的 PHP 包管理工具。安装 leancloud-sdk 只需执行以下命令：
```
composer require leancloud/leancloud-sdk
```




### 手动安装

<a class="btn btn-default" target="_blank" href="sdk_down.html">下载 SDK</a>





## 初始化

首先进入 [控制台 > 设置 > 应用 Key](/app.html?appid={{appid}}#/key) 来获取 App ID 以及 App Key。



然后导入 `Client`，并调用 `initialize` 方法进行初始化：

```php
use \LeanCloud\Client;
// 参数依次为 AppId, AppKey, MasterKey
Client::initialize("{{appid}}", "{{appkey}}", "{{masterkey}}");
Client::setServerUrl("https://{{domainN1}}"); // 0.7.0 及以上版本支持
```




### 开启调试日志

在应用开发阶段，你可以选择开启 SDK 的调试日志（debug log）来方便追踪问题。调试日志开启后，SDK 会把网络请求、错误消息等信息输出到 IDE 的日志窗口，或是浏览器 Console 或是 LeanCloud 控制台的 [云引擎日志](/dashboard/cloud.html?appid=#/log) 中。

```php
// 放在 SDK 初始化语句 Client::initialize() 后面，只需要调用一次即可
Client::setDebug(true);
```


  <div class="callout callout-danger">
  <p>在应用发布之前，请关闭调试日志，以免暴露敏感数据。</p>
</div>


### 启用指定节点

SDK 的初始化方法默认使用**中国大陆节点**，如需切换到 [其他可用节点](#全球节点)，请参考如下用法：


```php
use \LeanCloud\Client;
use \LeanCloud\Region;
// 参数依次为 AppId, AppKey, MasterKey
Client::initialize("{{appid}}", "{{appkey}}", "{{masterkey}}");

// 启用美国节点
// Client::useRegion(Region::US);
// 启用中国节点（默认启用）
Client::useRegion(Region::CN);

```


### 全球节点

- 中国大陆节点 **leancloud.cn**（SDK 初始化方法**默认**使用该节点）
- 北美节点 **us.leancloud.cn**（服务北美市场）


  <div class="callout callout-danger">
  <p>各个节点彼此独立，开发者账号无法跨节点来创建应用或调用 API。</p>
</div>


## 验证

首先，确认本地网络环境是可以访问 LeanCloud 服务器的，可以执行以下命令行：

```shell
ping "{{domainN1}}"
```
如果当前网路正常将会得到如下响应：

```shell
PING api-ucloud.leancloud.cn (123.59.41.31): 56 data bytes
64 bytes from 123.59.41.31: icmp_seq=0 ttl=51 time=9.032 ms
64 bytes from 123.59.41.31: icmp_seq=1 ttl=51 time=7.290 ms
64 bytes from 123.59.41.31: icmp_seq=2 ttl=51 time=8.131 ms
64 bytes from 123.59.41.31: icmp_seq=3 ttl=51 time=9.689 ms
64 bytes from 123.59.41.31: icmp_seq=4 ttl=51 time=6.559 ms
64 bytes from 123.59.41.31: icmp_seq=5 ttl=51 time=8.665 ms
64 bytes from 123.59.41.31: icmp_seq=6 ttl=51 time=8.041 ms
64 bytes from 123.59.41.31: icmp_seq=7 ttl=51 time=8.203 ms
64 bytes from 123.59.41.31: icmp_seq=8 ttl=51 time=6.288 ms
64 bytes from 123.59.41.31: icmp_seq=9 ttl=51 time=7.938 ms

--- api-ucloud.leancloud.cn ping statistics ---
10 packets transmitted, 10 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 6.288/7.984/9.689/0.997 ms
```
然后在项目中编写如下测试代码：



```php
// test.php

require 'vendor/autoload.php';

use \LeanCloud\Client;
use \LeanCloud\LeanObject;
// 参数依次为 AppId, AppKey, MasterKey
Client::initialize("{{appid}}", "{{appkey}}", "{{masterkey}}");
Client::setServerUrl("https://{{domainN1}}");

$testObject = new LeanObject("TestObject");
$testObject->set("words", "Hello World!");
try {
    $testObject->save();
    echo "Save object success!";
} catch (Exception $ex) {
    echo "Save object fail!";
}
```

保存后运行 `php test.php`。


然后打开 [控制台 > 存储 > 数据 > TestObject](/data.html?appid={{appid}}#/TestObject)，如果看到如下内容，说明 SDK 已经正确地执行了上述代码，安装完毕。


![testobject_saved](images/testobject_saved.png)

如果控制台没有发现对应的数据，请参考 [问题排查](#问题排查)。

## 问题排查

### 401 Unauthorized

如果 SDK 抛出 401 异常或者查看本地网络访问日志存在：

```json
{
  "code": 401,
  "error": "Unauthorized."
}
```
则可认定为 App ID 或者 App Key 输入有误，或者是不匹配，很多开发者同时注册了多个应用，导致拷贝粘贴的时候，用 A 应用的 App ID 匹配 B 应用的 App Key，这样就会出现服务端鉴权失败的错误。

### 客户端无法访问网络

客户端尤其是手机端，应用在访问网络的时候需要申请一定的权限。





 
