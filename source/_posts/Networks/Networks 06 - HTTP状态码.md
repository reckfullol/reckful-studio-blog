---
title: Networks 06 - HTTP状态码
date: 2018-08-02 00:31:51
categories: Networks
---
# 网络 06 - HTTP状态码

<!--more-->

|状态码|类别|原因|
|---|---|---|
|1XX|Informational(信息性状态码)|接收的请求正在处理|
|2XX|Success(成功状态码)|请求正常处理完毕|
|3XX|Redirection(重定向状态码)|需要进行附加操作以完成操作|
|4XX|Client Error(客户端错误状态码)|服务器无法处理请求|
|5XX|Server Error(服务器错误状态码)|服务器处理请求出错|

## 1XX

- 100 Continue: 表示到目前为止都正常, 客户端可以继续发送请求或者忽略这个响应.

## 2XX

- 200 OK

- 204 No Content: 请求已经成功处理, 但是返回的响应报文不包含实体的主体部分. 一般发生在只需要客户端向服务器发送消息, 而不需要返回数据时.

- 206 Partial Content: 表示客户端进行了范围请求. 响应报文包含由Content-Range指定范围的实体内容.

## 3XX

- 301 Moved Permanently: 永久性重定向.

- 302 Found: 临时性重定向.

- 303 See Oeher: 和302有相同的功能, 但是303明确要求客户端应该采用GET来获取资源.

- 304 Not Modified: 如果请求报文包含一条件, 例如: If-Match, If-Modified-Since, If-None-Match, If-Range, If-Unmodified-Since, 那么在不满足条件下会返回304.

- 307 Temporary Redirect: 临时重定向, 和304含义类似, 但是要求浏览器不能把重定向的请求动词由POST改为GET.

## 4XX

- 400 Bad Request: 请求报文中存在语法错误.

- 401 Unauthorized: 表示发送的请求需要有认证消息(BASIC认证, DIGEST认证). 如果之前已进行过一次请求, 则表示用户认证失败.

- 403 Forbidden: 请求被拒绝, 服务器端没有必要给出拒绝的详细理由.

- 404 Not Found.

## 5XX

- 500 Internal Server Error: 服务器正在执行请求时发生错误.

- 503 Service Unavailable: 服务器暂时处于超负载或正在进行停机维护, 现在无法处理请求.