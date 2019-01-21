# HttpCanary User Manual

HttpCanary is a powerful capture and injection tool for the Android platform. It supports multiple protocols such as HTTP, HTTP2, HTTPS and WebSocket.

Before using, it is recommended to read the basic usage steps and advanced usage of HttpCanary in order to have a general understanding of the features of HttpCanary.

**PS: This manual is based on the v2.1.0**

## Features
- [x] No root requirement, will not affect network usage.
- [x] Supports protocols like HTTP1.0, HTTP1.1, HTTP2, HTTPS and WebSocket.
- [x] Supports modification and injection of capture data, you can intercept the packets and modify them.
- [x] Supports filtering and searching for packet capture records, as well as setting the specified app and Host/IP.
- [x] Contains Raw, Hex, Text, Header, JSON and many other viewers.
- [x] Automatic decode data like gzip, deflate, chunked.
- [x] Supports for previewing URL, JSON, form, image, audio, cookie, set-cookie, and many other data types.
- [x] Supports for saving request and response data to a file or adding to favorite.
- [x] Supports WebSocket real-time preview.
- [x] Supports sharing of request and response data, and open shared file with HttpCanary.
- [x] Supports blocking request and response.

## Getting Started

### 1. Installation certificate
HttpCanary uses Man-in-the-Middle (MITM) technology to capture and parse TLS/SSL packets, such as HTTPS, WSS, etc., so you need to install a self-signed Certificate Authorities (CA) before using it. Tap the capture button -> Confirm your pattern -> OK to complete the installation of the certificate.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot01.png)

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot02.png)

### 2. Android 7.0+（Optional）
This is an **optional** step for some special cases of the Nougat(7.0)+ system. From Android Nougat(7.0), Google changed the network security policy. Self-signed Certificate Authorities (CA) are not trusted by any apps' secure connections by default. That means HttpCanary is unable to decrypt TLS/SSL packets. But we have two ways to get around it.

#### 2.1 Your own app
Add a network security configuration in AndroidManifest.xml:
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
For more information, please see Android Developer [Network security configuration](https://developer.android.com/training/articles/security-config).

#### 2.2 Third-part app

We can use [VirtualApp](https://github.com/asLody/VirtualApp) to capture the third-part app's TLS/SSL packets.

Go to HttpCanary Settings -> Install VirtualApp and click to install.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot03.png)

Open VirtualApp and install the target app which you want to capture. Launch the installed target app from VirtualApp and then you will see the packets hosted by VirtualApp in HttpCanary.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot04.png)

## Running HttpCanary

Tap the floating button in home page to start and stop capturing packets. Remember, long presses can quickly clear the record (A trick).

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot05.png)

Capture packets are sorted by time, the list contains elements such as app icon, app name, request method, request URL, response code, and time. You can clear the list by clicking the button in ActionBar.

### 1. Specify Capture

HttpCanary supports specifying capture targets, you can specify the apps or the Hosts and IPs.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot06.png)

### 2. Filter and Search

Tap the 🔍 menu button in ActionBar and go to the advanced search page. You can configure multiple conditions to filter the packets.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot07.png)

Tap the menu button in this page to reset all filter conditions.

If a filter condition is set, the 🔍 button in ActionBar will change to an triangle icon, indicating that the record has been filtered.

### 3. Packet Browsing

HttpCanary provides detailed data browsing capabilities. The details page contains three main tabs: Overview, Request, and Response.

#### 3.1 Overview

The overview provides packet reports including status, request protocol, request method, response code, server IP and port, cookie, Content-Type, timing, packet sizes, and more.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot08.png)

Tips: Long press an item to copy it quickly.

If the URL has query parameters, tap the item to go to the URL preview page:

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot09.png)

Tap the Cookie item to go to the Cookie preview page:

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot10.png)

Tap the Set-Cookie item to go to the Set-Cookie preview page:

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot11.png)

#### 3.2 Request and Response

The request and response contain multiple viewers, tap the bottom tabs to switch.

##### 3.2.1 Raw Viewer

The Raw viewer presents the original packet data, without any decoding and decrypting. The viewer contains the full packet data. You can long press and select data to copy.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot12.png)

This viewer displays up to 32k of data due to character limitations.

##### 3.2.2 Header Viewer

The Header viewer presents request lines, request headers, response lines, and response headers. Long press an item to copy quickly.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot13.png)

##### 3.2.3 Text Viewer

The Text viewer presents content data, will automatically decode data like gzip, chunked, deflate, etc.. Long press an item to copy quickly.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot14.png)

This viewer displays up to 32k of data due to character limitations.

##### 3.2.4 Hex Viewer

The Hex viewer presents content data in hex format, it will be easy to analyze them.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot15.png)

This viewer displays up to 32k of data due to character limitations.

##### 3.2.5 Preview Viewer

HttpCanary supports previews of some data types, like JSON, Forms, images, audio and so on.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot16.png)

##### 3.2.6 JSON Viewer

If the data type is JSON, you can open the JSON viewer by clicking the JSON content. You can operate JSON nodes, expand or collapse all nodes. It also supports horizontal screen.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot17.png)

##### 3.2.7 Audio Viewer

If the data type is an audio, you can click to open the audio viewer. The audio viewer supports audio playback and saving.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot18.png)

##### 3.2.8 WebSocket Viewer

The WebSocket viewer presents the packets in the form of a chat.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot19.png)

#### 3.3 Packet Save

You can save the request and response packets in this page. The packets will be save into three files: raw file, header file, text file. And you will find the save files in /HttpCanar/download directory.

#### 3.4 Packet Share

You can share the request and response packets to others in this page too.  The shared file format is ‘.hcy’. This file can be opened directly with HttpCanary.

## Injection

Injection is one of the core functions of HttpCanary. You can modify the request and response to hack the packets.

**This feature is a paid version feature, and the free version has a 7-day trial.**。

HttpCanary provides two different modes for the injection. They are static mode and dynamic mode. You can long press on the record to choose an injection mode.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot20.png)

### 1. Static Mode

Static mode supports the full injection of the HTTP/HTTPS packets, includes query parameters, request headers, request body, response status line, response headers and response body.

If you configured a static mode injection, the injector will be stored for next usage. And you can manage them in Settings -> Mod Manager. In mod manager, you can disable, enable or delete an injector.


#### 1.1 Request Injection

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot21.png)

Static mode provides an injection edit page to preprocess data. You can choose to inject the query parameters, request headers or the request body.

For the query parameters and headers injection, static mode has three options: follow, custom, disable.

- Follow：Use the original data from client, do nothing to them.
- Custom：Add or replace the value by key, like Map add operation.
- Disable：Remove the key and value from client, like Map remove operation.

For the body injection, see the following response injection.

#### 1.2 Response Injection

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot22.png)

Static mode supports injecting response status line, headers and body. The following figure is an injection of the status line, you can select one from the list:

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot23.png)

For the body injection, static mode provides two ways.

- Upload a body file, tap the upload icon and select a file from File Browser.
- Edit online, it supports only when the body data is human readable.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot24.png)

### 2. Dynamic Mode

Compare to static mode, the dynamic mode doesn't support the injection of request body and response body. This is due to the difficulty of handling big data bodies on mobile apps. We are considering to support tiny bodies later.

You can use the dynamic mode when the capture service is running. And remember that you should handle the data before timeout.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot25.png)

### 3. Injection Results

If a request is injected, the record item will show an indicated text.

![](https://github.com/MegatronKing/HttpCanary/blob/master/en-US/assets/screenshot26.png)


## FAQ

Q: What is the difference between paid version and free version?<br>
A: The paid version has no ads, unlimited use of injections, a more perfect user experience, and more.

Q: Why are some requests not caught?<br>
A: If you use an Android 7.0+ phone, please refer to the Getting Started of this manual. If you follow the configuration, still have the issue. I think maybe the client or the server did a security check on the SSL certificate, and in this case, the packet cannot be captured.


