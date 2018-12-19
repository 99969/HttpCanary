# HttpCanary App User Guide
## 1. Capture HTTPS Packets
From Android Nougat(7.0), Google changed the network security policy. Self-signed Certificate Authorities (CA) are not trusted for an app's secure connections, that means HttpCanary is unable to decrypt TLS/SSL packets. But there are two ways to get around it.

### 1.1 Your own app
Add a network security configuration file in AndroidManifest.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <application android:networkSecurityConfig="@xml/network_security_config"
                    ... >
        ...
    </application>
</manifest>
```
And the network_security_config file in **res/xml/**:
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
Please note that these configurations should be removed in the release version! More information, see Android Developer [Network security configuration](https://developer.android.com/training/articles/security-config).

### 1.2 Third-part app
We can use [VirtualApp](https://github.com/asLody/VirtualApp) to capture the third-part app's HTTPS packets. Here are a few steps to setup the environment.

##### 1.2.1 Install VirtualApp
Go to [HttpCanary](https://play.google.com/store/apps/details?id=com.guoshi.httpcanary) app's Settings -> Install VirtualApp and click to install.
 
##### 1.2.2 Install target app in VirtualApp
Open VirtualApp and install the target app which you want to capture. After that launch the installed target app from VirtualApp, then in HttpCanary you will see the HTTPS packets hosted by VirtualApp.

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_03.png)

### 1.3 Supplement
Some apps will verify the Certificate's signature, if it not match the server's, the app will refuse the connection. In this condition, HttpCanary is unable to capture the packets. And there is no way to get around it.

## 2. HTTP/HTTPS Injection
HttpCanary provides two different modes for the injection. There are static mode and dynamic mode. You can long press an item to choose an injection mode.

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_01.png)

### 2.1 Static Mode
Static mode supports the full injection of the HTTP/HTTPS packets, includes query parameters, request headers, request body, response status line, response headers and response body.
If you configured a static mode injection, the injector will be stored for next usage. And you can manage them in Settings -> Mod Manager. In the mod manager, you can disable, enable or delete a injector.

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_02.png)

### 2.2 Dynamic Mode
Compare to static mode, the dynamic mode not supports the injection of request body and response body, due to the app is difficult to handle big data bodies on mobile, but we are considering to support the tiny bodies later.
You can use the dynamic mode when the capture service is running. And remember that you should handle the data before timeout.

## 3. Content Preview
HttpCanary supports preview many of mime types, such as json. 

![](https://github.com/MegatronKing/HttpCanary/raw/master/assets/screenshot_en_04.png)

The following is the support detail list and we will update it continually.

| Mime Type | Preview |
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


