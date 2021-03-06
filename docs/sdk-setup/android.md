# Android SDK 安装指南

## 获取 SDK

获取 SDK 有多种方式，较为推荐的方式是通过包依赖管理工具下载最新版本。

### 包依赖管理工具安装



#### Gradle

Gradle 是 Google 官方推荐的构建 Android 程序的工具，使用 Android Studio 进行开发的时候，它会自动在新建的项目中包含一个自带的命令行工具 **gradlew**。我们推荐开发者使用这个自带的命令行工具，这是因为 Gradle 存在版本兼容的问题，很多开发者即使正确配置了 Gradle 脚本，但由于使用了最新版本或不兼容的 Gradle 版本而仍然无法成功加载依赖包。

##### Android Studio

使用 Android Studio 创建一个新的项目的时候，它的目录结构如下：

```
.
├── app                 // 应用源代码
    ├── ...
    ├── build.gradle    // 应用 Gradle 构建脚本
    ├── ...
├── build.gradle        // 项目 Gradle 构建脚本
├── YOUR-APP-NAME.iml   // YOUR-APP-NAME 为你的应用名称
├── gradle
└── settings.gradle
```

首先打开根目录下的 `build.gradle` 进行如下标准配置：

<pre><code>
buildscript {
    repositories {
        jcenter()
        //这里是 LeanCloud 的包仓库
        maven {
            url "http://mvn.leancloud.cn/nexus/content/repositories/public"
        }

    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.0.0'
    }
}

allprojects {
    repositories {
        jcenter()
        //这里是 LeanCloud 的包仓库
        maven {
            url "http://mvn.leancloud.cn/nexus/content/repositories/public"
        }
    }
}
</code></pre>

然后打开 `app` 目录下的 `build.gradle` 进行如下配置：

```
android {
    //为了解决部分第三方库重复打包了META-INF的问题
    packagingOptions{
        exclude 'META-INF/LICENSE.txt'
        exclude 'META-INF/NOTICE.txt'
    }
    lintOptions {
        abortOnError false
    }
}

dependencies {
    compile ('com.android.support:support-v4:21.0.3')

    // LeanCloud 基础包
    compile ('cn.leancloud.android:avoscloud-sdk:v4.7.3')

    // 推送与即时通讯需要的包
    compile ('cn.leancloud.android:avoscloud-push:v4.7.3@aar'){transitive = true}

    // LeanCloud 统计包
    compile ('cn.leancloud.android:avoscloud-statistics:v4.7.3')

    // LeanCloud 用户反馈包
    compile ('cn.leancloud.android:avoscloud-feedback:v4.7.3@aar')

    // avoscloud-sns：LeanCloud 第三方登录包
    compile ('cn.leancloud.android:avoscloud-sns:v4.7.3@aar')
    compile ('cn.leancloud.android:qq-sdk:1.6.1-leancloud')
    // 新浪微博 SDK
    compile('com.sina.weibo.sdk:core:4.1.4:openDefaultRelease@aar')

    // LeanCloud 应用内搜索包
    compile ('cn.leancloud.android:avoscloud-search:v4.7.3@aar')
}
```

我们已经提供了官方的 [maven 仓库](http://mvn.leancloud.cn/nexus/)，推荐大家使用。

#### Eclipse

Eclipse 用户首先 [下载 SDK](sdk_down.html)，然后按照 [手动安装步骤](#手动安装) 将 SDK 导入到项目里。



### 手动安装

<a class="btn btn-default" target="_blank" href="sdk_down.html">下载 SDK</a>


对照以下包文件及其对应的功能模块，开发者可以自行 <a escape-hash href="https://github.com/leancloud/android-sdk-all/blob/master/README.md#%E7%BC%96%E8%AF%91" target="_blank">编译源代码</a>
 来得到所需的包文件。

```
├── avoscloud-feedback-v4.7.3.zip     // LeanCloud 用户反馈模块
├── avoscloud-push-v4.7.3.jar         // LeanCloud 推送模块和即时通讯模块
├── avoscloud-sdk-v4.7.3.jar          // LeanCloud 基本存储模块
├── avoscloud-search-v4.7.3.zip       // LeanCloud 应用内搜索模块
├── avoscloud-sns-v4.7.3.zip          // LeanCloud SNS 模块
├── avoscloud-statistics-v4.7.3.jar   // LeanCloud 统计模块
├── fastjson-1.2.37.jar                         // LeanCloud 基本存储模块
├── Java-WebSocket-1.3.2-leancloud.jar          // LeanCloud 推送模块和即时通讯模块
├── protobuf-java-3.4.0.jar                     // LeanCloud 推送模块和即时通讯模块
├── okhttp-3.8.0.jar                            // LeanCloud 基本存储模块
├── okio-1.13.0.jar                             // LeanCloud 基本存储模块
├── qq.sdk.1.6.1.jar                            // LeanCloud SNS 模块
└── weibo.sdk.android.sso.3.0.1-leancloud.jar   // LeanCloud SNS 模块
```

##### LeanCloud 基本存储模块

* `avoscloud-v4.7.3.jar`
* `okhttp-3.8.0.jar`
* `okio-1.13.0.jar`
* `fastjson-1.2.37.jar`

##### LeanCloud 推送模块和即时通讯模块

* LeanCloud 基础存储模块
* `avospush-v4.7.3.jar`
* `Java-WebSocket-1.3.2-leancloud.jar`
* `protobuf-java-3.4.0.jar`

##### LeanCloud 统计模块

* LeanCloud 基础存储模块
* `avosstatistics-v4.7.3.jar`

##### LeanCloud SNS 模块

* LeanCloud 基础存储模块
* `weibo.sdk.android.sso.jar`
* `qq.sdk.1.6.1.jar`

我们提供的下载包里包含了必须的依赖库，自己下载官方库也可以，不过请确保版本一致。

**注意：如果需要使用美国站点，并且 SDK 版本是 3.3 及以上，则不需要引入 SSL 证书。其他低版本的用户，需要下载 [SSL 证书](https://download.leancloud.cn/sdk/android/current/avoscloud_us_ssl.bks)，将其拷贝到项目的 `res/raw/` 之下。**

#### Android Studio

首先本地已经下载好了项目需要的 SDK 包，然后按照以下步骤导入：

1. 打开 **File** > **Project Structure** > **Modules** 对话框，点击 **Dependencies**；
2. 点击下方的 **+**（加号），选择要导入的 SDK 包（xxxx.jar），记得 **Scope** 选为 **Compile**；
3. 重复第 2 步，直到所有需要的包均已正确导入。

Eclipse 的导入与一般的 jar 导入无本质区别，因此不再赘述。



## 初始化

首先进入 [控制台 > 设置 > 应用 Key](/app.html?appid={{appid}}#/key) 来获取 App ID 以及 App Key。



然后新建一个 Java Class ，名字叫做 **MyLeanCloudApp**,让它继承自 **Application** 类，实例代码如下:

```java
public class MyLeanCloudApp extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        // 初始化参数依次为 this, AppId, AppKey
        AVOSCloud.initialize(this,"{{appid}}","{{appkey}}");
    }
}
```
将上述代码中的 App ID 以及 App Key 替换成从控制台复制粘贴的对应的数据即可。

然后打开 `AndroidManifest.xml` 文件来配置 SDK 所需要的手机的访问权限以及声明刚才我们创建的 `MyLeanCloudApp` 类：

```xml
<!-- 基础模块（必须加入以下声明）START -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<!-- 基础模块 END -->

<application
  ...
  android:name=".MyLeanCloudApp" >

  <!-- 即时通讯模块、推送（均需要加入以下声明） START -->
  <!-- 即时通讯模块、推送都要使用 PushService -->
  <service android:name="com.avos.avoscloud.PushService"/>
  <receiver android:name="com.avos.avoscloud.AVBroadcastReceiver">
    <intent-filter>
      <action android:name="android.intent.action.BOOT_COMPLETED"/>
      <action android:name="android.intent.action.USER_PRESENT"/>
      <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
  </receiver>
  <!-- 即时通讯模块、推送 END -->

  <!-- 反馈组件（需要加入以下声明）START -->
  <activity
     android:name="com.avos.avoscloud.feedback.ThreadActivity" >
  </activity>
  <!-- 反馈组件 END -->
</application>
```





### 开启调试日志

在应用开发阶段，你可以选择开启 SDK 的调试日志（debug log）来方便追踪问题。调试日志开启后，SDK 会把网络请求、错误消息等信息输出到 IDE 的日志窗口，或是浏览器 Console 或是 LeanCloud 控制台的 [云引擎日志](/dashboard/cloud.html?appid=#/log) 中。

```java
// 放在 SDK 初始化语句 AVOSCloud.initialize() 后面，只需要调用一次即可
AVOSCloud.setDebugLogEnabled(true);
```


  <div class="callout callout-danger">
  <p>在应用发布之前，请关闭调试日志，以免暴露敏感数据。</p>
</div>


### 启用指定节点

SDK 的初始化方法默认使用**中国大陆节点**，如需切换到 [其他可用节点](#全球节点)，请参考如下用法：


```java
public class MyLeanCloudApp extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        // 启用北美节点, 需要在 initialize 之前调用
        AVOSCloud.useAVCloudUS();

        // 初始化参数依次为 this, AppId, AppKey
        AVOSCloud.initialize(this,"{{appid}}","{{appkey}}");
    }
}
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


在 `MainActivity.java` 中的 `onCreate` 方法添加如下代码：

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ...
        // 测试 SDK 是否正常工作的代码
        AVObject testObject = new AVObject("TestObject");
        testObject.put("words","Hello World!");
        testObject.saveInBackground(new SaveCallback() {
            @Override
            public void done(AVException e) {
                if(e == null){
                    Log.d("saved","success!");
                }
            }
        });

        ...

    }
```

然后，点击 `Run` 运行调试，真机和虚拟机均可。


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




