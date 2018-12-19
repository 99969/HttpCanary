# HttpCanary App使用手册
## 1. HTTPS抓包
从Android Nougat(7.0)开始，谷歌改变了网络安全策略。自签的CA证书将默认不被HTTPS连接信任，这意味着HttpCanary将无法解析TLS/SSL数据包，即无法抓取HTTPS的明文数据。但是我们可以通过两种方式来绕过这种限制。

### 1.1 自己APP抓包
首先在AndroidManifest.xml中添加networkSecurityConfig：
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
为了APP的信息安全，需要注意以上配置只能用于debug包，正式发布的包请移除这些配置。更多关于网络安全策略的信息，请前往Android Developer查看[Network security configuration](https://developer.android.com/training/articles/security-config)。

### 1.2 第三方APP抓包
我们可以借助[VirtualApp](https://github.com/asLody/VirtualApp)这款应用来抓第三方的HTTPS包。通过以下几个步骤来配置抓包环境。

##### 1.2.1 安装VirtualApp
打开[HttpCanary](https://play.google.com/store/apps/details?id=com.guoshi.httpcanary)应用，并进入设置->安装VirtualApp，然后点击安装。注意8.0及以上的手机会限制安装来源，请勾选同意。安装完成后，可以选择在系统设置页面中手动关闭安装来源开关。
 
##### 1.2.2 在VirtualApp中安装目标App
打开VirtualApp安装抓包目标App，然后在VirtualApp中启动目标App，这样就可以在HttpCanary看到想抓的App请求包了，但是抓包记录显示的应用信息会是VirtualApp。

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_03.png)

### 1.3 补充说明
有些App会校验证书签名，如果和服务端不匹配的话，客户端和服务端会握手失败，拒绝连接。这种情况下，HttpCanary无论如何都是抓不到包的，且没有任何办法可以绕过去（除非我们能拿到服务端的证书，这个还是不要想了）。

## 2. HTTP/HTTPS注入
HttpCanary提供了两种不同的注入模式，分别是静态注入和动态注入。在首页长按抓包记录，然后在弹框中选择一种注入模式。

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_01.png)

### 2.1 静态注入
静态注入支持对HTTP/HTTPS包全量的注入，包括请求参数、请求头、请求体、响应行、响应头、响应体等。另外，如果配置了静态注入，注入器将会缓存起来，以便后面重复使用。但是可以前往App的设置->模组管理种，对其进行禁用、启用、删除等操作。

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_02.png)

### 2.2 动态注入
相比于静态注入，动态注入不支持对请求体和响应体的注入，主要是由于手机端不方便处理太大数据类型的请求体和响应体。不过，我们正在考虑支持动态注入一些小数据类型的请求体和响应体。
如果使用动态注入，必须先将抓包服务运行起来，且必须记住注入不要花太长时间，防止请求或者响应超时。

## 3. 内容预览
HttpCanary提供了众多数据类型的预览，比如json格式等等。

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_04.png)

下表是HttpCanary对一些数据类型的支持明细（持续更新中）。

| 数据类型 | 预览 |
| --- | --- | 
| application/json | true |
| application/octet-stream | false |
| application/xml | false |
| application/x-zip-compressed | false |
| audio/aac | true |
| audio/mpeg | true |
| audio/mp3 | true |
| audio/ogg | true |
| audio/wav | true |
| audio/x-aac | true |
| audio/x-wav | true |
| image/bmp | true |
| image/gif | true |
| image/jpeg | true |
| image/png | true |
| image/webp | false |
| text/plain | some of |
| video/mpeg | false |
| video/mp4 | false |
