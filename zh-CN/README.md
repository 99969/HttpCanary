# HttpCanary使用手册
HttpCanary是Android平台下功能最强大的抓包和注入工具，支持HTTP、HTTPS、HTTP2和WebSocket等多种协议。

在使用之前，建议阅读下HttpCanary的基本使用步骤和进阶用法，以便对HttpCanary的特性有一个大致的了解。

**注: 此手册以v2.1.0版本为基础编写**

## 功能特性
- [x] 无需Root，抓包时不会影响其它App的使用。
- [x] 支持HTTP1.0、HTTP1.1、HTTP2、HTTPS和WebSocket等协议抓包。
- [x] 支持对抓包内容进行注入修改，支持修改请求参数、请求头、请求体、响应码、响应头和响应体等数据。
- [x] 支持对抓包数据进行筛选、搜索，以及设置抓取指定应用和指定Host/IP。
- [x] 支持Raw、Hex、Text、Header等多种视图浏览数据。
- [x] 支持自动解码Gzip、Deflate、Chunked等编码的数据包。
- [x] 支持预览JSON、Form表单、图片、音频、Cookie等数据类型。
- [x] 支持将请求和响应数据保存至文件或者加入收藏列表。
- [x] 支持WebSocket实时预览。
- [x] 支持文件形式分享请求和响应数据，以及使用HttpCanary打开分享文件。
- [x] 支持屏蔽数据不发送给服务器或者不返回给客户端，方便调试。
- [x] 即将支持自定义扩展Mod功能。

## 配置环境

### 1. 安装证书
HttpCanary使用Man-in-the-Middle(MITM)技术抓取和解析TLS/SSL协议数据包，比如常见的HTTPS、WSS等请求，所以使用之前需要先安装自签根证书。当首次点击右下角蓝色抓包按钮后，再点击安装 -> 输入锁屏图案或密码 -> 确定，完成证书的安装。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot01.png)

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot02.png)

### 2. Android 7.0+手机（可选）
从Android Nougat(7.0)开始，谷歌改变了网络安全策略。自签的CA证书将默认不被TLS/SSL连接信任，这意味着HttpCanary可能无法抓取HTTPS的明文数据。但是我们可以通过两种方式来绕过这种限制。

#### 2.1 自己APP抓包
在项目的AndroidManifest.xml中添加networkSecurityConfig：
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <application android:networkSecurityConfig="@xml/network_security_config"
                    ... >
        ...
    </application>
</manifest>
```
network_security_config文件放在 **res/xml/** 目录下面：
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
            <certificates src="user" />
        </trust-anchors>
    </base-config>
</network-security-config>
```
更多关于网络安全策略的信息，请前往Android Developer查看[Network security configuration](https://developer.android.com/training/articles/security-config)。

#### 2.2 第三方APP抓包

我们可以借助[VirtualApp](https://github.com/asLody/VirtualApp)这款应用间接抓第三方的HTTPS包，通过以下几个步骤来配置VirtualApp抓包环境。

第一步。打开HttpCanary，进入设置 -> 安装VirtualApp，然后点击安装。注意8.0及以上的手机会限制安装来源，请勾选同意。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot03.png)

第二步。打开VirtualApp，安装抓包目标App，然后在VirtualApp中启动目标App，这样就可以在HttpCanary看到目标App的数据包了，但是抓包记录显示的应用信息会是VirtualApp。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot04.png)

## 开始抓包

首页右下角悬浮按钮，点击可以启动和停止抓包，长按则可以快速清除记录（小技巧哦）。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot05.png)

抓包记录以列表的形式，按照时间降序排列。列表记录中包含应用图标、应用名称、请求方法、请求URL、响应码和时间等元素。点击标题栏右上角按钮，可以清空列表。

### 1. 指定抓包

可以在HttpCanary具有针对性抓包功能，在设置中配置指定App或者指定Host/IP进行抓包。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot06.png)

### 2. 筛选和搜索

点击首页右上角放大镜按钮，进入高级搜索页面。可以配置多种条件，对抓包数据进行筛选。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot07.png)

点击高级搜索页面右上角的按钮，可以一键复位所有筛选条件。

如果设置了筛选条件时，首页右上角的放大镜按钮图案会变成倒三角图案，表明记录已经经过了筛选。

### 3. 数据浏览

HttpCanary提供了详尽的数据浏览功能，点击首页抓包记录打开详情页面。详情页面包含三个Tab，分别是总览、请求和响应。

#### 3.1 总览

总览提供了详细的数据报告，包括状态、请求协议、请求方法、响应码、服务器IP和端口、Cookie信息、Content-Type类型、请求时间、数据量等。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot08.png)

小技巧：长按数据条目可以快速复制哦。

如果URL带有参数，点击URL条目，可以进入URL预览页：

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot09.png)

点击Cookie，可以进入Cookie预览页：

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot10.png)

点击Set-Cookie，可以进入Set-Cookie预览页：

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot11.png)

#### 3.2 请求和响应

请求和响应包含多种视图，点击下方的Tab进行切换。

##### 3.2.1 Raw视图

Raw视图是指原数据视图，未做任何解码和转码，包含整个HTTP的请求数据。可以长按选择数据进行复制操作。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot12.png)

由于字符限制，此视图最多显示32k的数据。

##### 3.2.2 Header视图

Header视图分别包含请求行、请求头、响应行、响应头，长按可以进行快速复制。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot13.png)

##### 3.2.3 Text视图

Text视图显示请求体数据，会自动对Gzip、Chunked、Deflate等进行解码显示。可以长按选择数据进行复制操作。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot14.png)

由于字符限制，此视图最多显示32k的数据。

##### 3.2.4 Hex视图

Hex视图以十六进制的形式显示数据，方便进行数据类型解析。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot15.png)

由于字符限制，此视图最多显示32k的数据。

##### 3.2.5 预览视图

HttpCanary支持一些常用数据的预览，包括JSON、Form表单、图片和音频等。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot16.png)

##### 3.2.6 JSON视图

如果数据类型是JSON，可以在预览视图中点击JSON内容可以打开JSON视图。JSON视图可以单独对JSON进行节点展开、闭合、复制和保存等操作，还支持横竖屏切换浏览功能。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot17.png)

##### 3.2.7 音频视图

如果数据类型是音频格式，可以在预览视图中点击打开音频视图。音频视图支持播放和保持音频功能。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot18.png)

##### 3.2.8 WebSocket视图

HttpCanary支持以聊天的形式展示WebSocket数据。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot19.png)

#### 3.3 数据保存

在详情页面，点击右上角保存按钮可以将请求和响应保存成文件。保存的文件有三个：原始数据（raw）、头部数据（header）、内容数据（text）。保存目录在SD卡HttpCanar/download目录下面。

#### 3.4 数据分享

在详情页面，点击右上角分析按钮可以将请求和响应文件分享出去，分享的文件格式是hcy。hcy格式文件可以使用HttpCanary直接打开。

## 注入功能

HttpCanary最强大之处在于可以对数据进行注入修改，能够极大地方便开发者调试和测试接口。**此功能是付费版本功能，免费版本有7天的试用期**。

HttpCanary提供了两种不同的注入模式，分别是静态注入和动态注入。在首页长按抓包记录，然后在弹框中选择一种注入模式。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot20.png)

### 1. 静态注入

静态注入支持对HTTP/HTTPS包全量的修改注入，包括请求参数、请求头、请求体、响应行、响应头、响应体等。另外，如果配置了静态注入，注入器将会缓存起来，以便后面重复使用。但是可以前往App的设置 -> 模组管理种，对其进行禁用、启用、删除等操作。

#### 1.1 请求修改

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot21.png)

上图是对请求进行修改注入，这里Path、协议版本号以及请求方法是不支持修改的。请求参数（Query Parameters）以及Headers的注入都是键值对的形式（请求体的修改注入请参考下方响应注入），静态注入对此提供了三种注入选项：跟随，自定义，禁用。

- 跟随：表示使用客户端发给服务器的原始数据，不进行任何干预。
- 自定义：自定义key和value，会根据key对客户端的请求数据进行覆盖，无需覆盖即当做新增（等同Map的put操作）。
- 禁用：表示删除客户端发给服务器的key-value数据（等同Map的remove操作）。

#### 1.2 响应修改

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot22.png)

响应修改注入支持对响应行（仅code+message）、响应头和响应体三者，其中响应头的注入和上面请求注入类似，有跟随、自定义、禁用三个选项。但响应行和Body的注入只有两个选项：跟随服务端和自定义。下图是对响应行的自定义，必须从列表中选择一项：

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot23.png)

对于响应体的注入有两种方式：
- 上传文件整体替换。点击可以从手机上选择一个文件，如果有需要替换的数据，可以先保存成文件，然后在这里选择就可以了。
- 直接编辑。如果数据量较小，可以直接编辑后提交。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot24.png)

### 2. 动态注入

动态注入需要在抓包服务运行的过程中进行。相比于静态注入，动态注入不支持对请求体和响应体的注入，主要是由于手机端不方便处理太大数据类型的请求体和响应体。如果使用动态注入，必须先将抓包服务运行起来。 **动态注入过程中，所有请求和响应都会Block住** ，所以切记注入不要花太长时间，防止请求或者响应超时。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot25.png)

### 3. 注入结果

如果注入成功，抓包记录右下角会显示已注入的标记。

![](https://github.com/MegatronKing/HttpCanary/blob/master/zh-CN/assets/screenshot26.png)


## 疑问解答

- 问: 付费版本相比免费版本有哪些特性？
答: 付费版本无广告、无限制使用注入功能、更完美的用户体验等。

- 问: 怎么样获取付费版本？
答: 可以在GooglePlay直接购买，如果无法付款可以邮件guoshi.support@qq.com或者微信king20091305035联系我购买GooglePlay兑换码。

- 问: 为什么有的请求抓不到？
答: 如果是Android 7.0+手机，请参考本手册环境配置。如果按照配置还是抓不到，可能是客户端或者服务端对SSL证书做了安全校验，这种情况是抓不到包的。


