---
layout: post
title:  "OAuth快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/oauth_icon.jpg)

## 一、概述
> An open protocol to allow secure authorization in a simple and standard method from web, mobile and desktop applications.

OAuth是Open Authorization（开放授权）的缩写，允许用户授权给第三方应用访问服务提供商所提供的用户资源，并且保证授权是安全的。

[官网地址](https://oauth.net/)

## 二、概念

四种角色：

* Resource owner：资源所有者，也叫用户
* Resource server：资源服务器，服务提供商用来存储资源，以及处理对资源的请求的服务器
* Client：客户端，也叫第三方应用，通过获取用户的授权，继而访问用户在资源服务器上的资源
* Authorization server：认证服务器，服务提供商用来处理认证的服务器，物理上与资源服务器可以是同一台服务器

两种实体：

* HTTP service：服务提供商
* User Agent：用户代理，通常指浏览器

## 三、工作原理

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-24-how-to-use-oauth/01-work-flow.png)

(A) 用户打开客户端，客户端请求用户授权

(B) 用户同意授权给客户端

(C) 客户端使用获取的授权，向认证服务器请求令牌

(D) 认证服务器对客户端进行认证，并验证授权，确认有效后发放令牌给客户端

(E) 客户端使用令牌，向资源服务器请求资源

(F) 资源服务器验证令牌，确认有效后处理请求

## 四、授权类型
客户端必须获取用户的授权，才能够获取令牌。OAuth定义了四种获取授权的方式：

### 1、授权码模式（Authorization Code）
是功能最齐全、流程最严谨，也是最常用的授权模式。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-24-how-to-use-oauth/02-auth-code.png)

(A) 用户通过用户代理访问客户端，客户端将其重定向到认证服务器

* response_type：表示授权类型，必选项，此种模式固定为“code”
* client_id：表示客户端ID，必选项
* redirect_uri：表示重定向URI，可选项
* scope：表示申请的权限范围，可选项
* state：表示客户端当前状态，可选项

实例：

    GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

(B) 用户选择是否授权给客户端

(C) 如果用户授权，认证服务器将用户重定向到客户端事先指定的URI，并附加一个授权码

* code：表示授权码，必选项，客户端只能使用一次，与客户端ID和重定向URI一一对应
* state：表示客户端的状态

实例：

    HTTP/1.1 302 Found
    Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA
        &state=xyz

(D) 客户端使用授权码和重定向URI，向认证服务器申请令牌

* grant_type：表示授权类型，必选项，此种模式固定为“authorization\_code”
* code：表示授权码，必选项
* redirect_uri：表示重定向URI，必选项
* client_id：表示客户端ID，必选项

实例：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded

    grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb

(E) 认证服务器验证授权码和URI，确认无误后，向客户端发放令牌

* access_token：表示访问令牌，必选项
* token_type：表示令牌类型，必选项，可以是bearer或mac类型
* expires_in：表示过期时间，单位为秒。如果省略，则其他方式必须设置
* refresh_token：表示刷新令牌，可选项，用来获取下一次的访问令牌
* scope：表示权限范围，如果与客户端申请的范围一致，可省略

实例：

    HTTP/1.1 200 OK
    Content-Type: application/json;charset=UTF-8
    Cache-Control: no-store
    Pragma: no-cache

    {
      "access_token":"2YotnFZFEjr1zCsicMWpAA",
      "token_type":"example",
      "expires_in":3600,
      "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
      "example_parameter":"example_value"
    }

### 2、简化模式（Implicit）
不通过第三方应用的服务器，直接在浏览器中进行，不需要使用授权码。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-24-how-to-use-oauth/03-implicit.png)

(A) 用户通过用户代理访问客户端，客户端将其重定向到认证服务器

* response_type：表示授权类型，必选项，此种模式固定为“token”
* client_id：表示客户端ID，必选项
* redirect_uri：表示重定向URI，可选项
* scope：表示申请的权限范围，可选项
* state：表示客户端当前状态，可选项

实例：

    GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

(B) 用户选择是否授权给客户端

(C) 如果用户授权，认证服务器将用户重定向到客户端事先指定的URI，并在URI的Hash部分包含访问令牌

* access_token：表示访问令牌，必选项
* token_type：表示令牌类型，必选项
* expires_in：表示过期时间，单位为秒。如果省略，则其他方式必须设置
* scope：表示申请的权限范围，可选项
* state：表示客户端的状态

实例：

    HTTP/1.1 302 Found
    Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA
        &state=xyz&token_type=example&expires_in=3600

(D) 浏览器向资源服务器发送请求，但不包含Hash值

(E) 资源服务器返回一个网页，包含获取Hash值中令牌的代码

(F) 浏览器执行脚本，获取令牌

(G) 浏览器将令牌发送给客户端

### 3、密码凭证模式（Resource Owner Password Credentials）
用户必须向客户端提供用户名和密码，存在较大的风险。通常只有在认证服务器无法通过其他方式进行授权时，才会考虑使用此种模式。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-24-how-to-use-oauth/04-password.png)

(A) 用户向客户端提供用户名和密码凭证

(B) 客户端将用户名和密码凭证发送给认证服务器，并请求令牌

* grant_type：表示授权类型，必选项，此种模式固定为“password”
* username：表示用户名，必选项
* password：表示密码，必选项
* scope：表示权限范围，可选项

实例：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded

    grant_type=password&username=johndoe&password=A3ddj3w

(C) 认证服务器确认无误后，向客户端发放令牌

### 4、客户端凭证模式（Client Credentials）
由客户端直接向服务提供商进行认证，其实并不存在授权问题。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-01-24-how-to-use-oauth/05-client.png)

(A) 客户端向认证服务器提供身份凭证，并请求令牌

* grant_type：表示授权类型，必选项，此种模式固定为“client\_credentials”
* scope：表示权限范围，可选项

实例：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded

    grant_type=client_credentials

(B) 认证服务器确认无误后，向客户端发放令牌

### 5、刷新令牌
如果用户访问的时候，客户端所获取的访问令牌已经过期，则需要使用刷新令牌重新申请新的访问令牌。

客户端发送的HTTP请求，包括以下参数：

* grant_type：表示授权类型，必选项，此种模式固定为“refresh\_token”
* refresh_token：表示之前收到的刷新令牌，必选项
* scope：表示权限范围，不能够超出上次申请的范围。如果省略，则表示与上次申请的权限范围一致

实例：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded

    grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA

---

参考文章：

阮一峰：[理解OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)

[帮你深入理解OAuth2.0协议](http://blog.csdn.net/seccloud/article/details/8192707)
