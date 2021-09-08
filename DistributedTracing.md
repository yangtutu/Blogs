---
title: 链路（分布式）追踪
date: 2021-09-08
description: 使用 Istio 实现 http 及 grpc 链路追踪
---

### 背景信息

链路（分布式）跟踪是一种用于对应用程序进行概要分析和监视的方法，尤其是针对使用微服务架构构建的应用程序。
虽然 Istio 代理能够自动发送 Span 信息，但是应用程序仍然需要传播适当的 HTTP 标头，以便在代理发送 Span 时，可以将 Span 正确地关联到单个跟踪中。
为此，应用程序需要收集以下标头并将其从传入请求传播到任何传出请求：

- x-request-id
- x-b3-traceid
- x-b3-spanid
- x-b3-parentspanid
- x-b3-sampled
- x-b3-flags
- x-ot-span-context

参考：https://istio.io/latest/about/faq/#distributed-tracing

![image](https://user-images.githubusercontent.com/6275608/132476548-de02b369-d9b7-4a11-bb98-9f5fe8143dad.png)


### 链路模式

![image](https://user-images.githubusercontent.com/6275608/132476627-2c886b03-f18a-4311-96e1-c867e0c78908.png)

针对上图中2的链路模式，程序需要完成以下两个操作

- A 调用 B 的时候，需要把 A 接收的跟踪 http headers 转换成跟踪 grpc metadata 传递给 B
- B 调用 C 的时候，需要把 A 传递的跟踪 grpc metadata 透传给 C

参考代码：

- golang 

``` golang
// trace.go
package tracing

// refer https://github.com/sunmi-OS/gocore/blob/master/istio/trace.go

import (
	"context"
	"net/http"

	"github.com/gin-gonic/gin"
	"google.golang.org/grpc/metadata"
)

const (
	X_REQUEST_ID      = "x-request-id"
	X_B3_TRACEID      = "x-b3-traceid"
	X_B3_SPANID       = "x-b3-spanid"
	X_B3_PARENTSPANID = "x-b3-parentspanid"
	X_B3_SAMPLED      = "x-b3-sampled"
	X_B3_FLAGS        = "x-b3-flags"
	X_OT_SPAN_CONTEXT = "x-ot-span-context"
)

type TraceHeader struct {
	HttpHeader   http.Header
	GrpcMetadata metadata.MD
}

func NewGrpcContextFromGin(ctx *gin.Context) context.Context {
	traceHeader := LoadFromHttpHeader(ctx.Request.Header)
	grpcCtx := metadata.NewOutgoingContext(ctx, traceHeader.GrpcMetadata)
	return grpcCtx
}

func NewGrpcContextFromGrpc(ctx context.Context) context.Context {
	grpcMetadata, _ := metadata.FromIncomingContext(ctx)
	traceHeader := LoadFromGrpcMetadata(grpcMetadata)
	grpcCtx := metadata.NewOutgoingContext(ctx, traceHeader.GrpcMetadata)
	return grpcCtx
}

func LoadFromHttpHeader(headers http.Header) TraceHeader {

	md := map[string]string{}
	httpHeaders := http.Header{}

	fillFromHttpHeader(headers, X_REQUEST_ID, md, httpHeaders)
	fillFromHttpHeader(headers, X_B3_TRACEID, md, httpHeaders)
	fillFromHttpHeader(headers, X_B3_SPANID, md, httpHeaders)
	fillFromHttpHeader(headers, X_B3_PARENTSPANID, md, httpHeaders)
	fillFromHttpHeader(headers, X_B3_SAMPLED, md, httpHeaders)
	fillFromHttpHeader(headers, X_B3_FLAGS, md, httpHeaders)
	fillFromHttpHeader(headers, X_OT_SPAN_CONTEXT, md, httpHeaders)

	return TraceHeader{
		HttpHeader:   httpHeaders,
		GrpcMetadata: metadata.New(md),
	}
}

func LoadFromGrpcMetadata(rawMetadata metadata.MD) TraceHeader {

	md := map[string]string{}
	httpHeaders := http.Header{}

	fillFromGrpcMetadata(rawMetadata, X_REQUEST_ID, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_B3_TRACEID, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_B3_SPANID, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_B3_PARENTSPANID, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_B3_SAMPLED, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_B3_FLAGS, md, httpHeaders)
	fillFromGrpcMetadata(rawMetadata, X_OT_SPAN_CONTEXT, md, httpHeaders)

	return TraceHeader{
		HttpHeader:   httpHeaders,
		GrpcMetadata: metadata.New(md),
	}
}

func fillFromHttpHeader(headers http.Header, key string, md map[string]string, httpHeaders http.Header) {
	value := headers.Get(key)
	if value != "" {
		md[key] = value
		httpHeaders.Add(key, value)
	}
}

func fillFromGrpcMetadata(rawMetadata metadata.MD, key string, md map[string]string, httpHeaders http.Header) {
	value := rawMetadata.Get(key)
	if len(value) > 0 {
		md[key] = value[0]
		httpHeaders.Add(key, value[0])
	}
}

// main.go
package main

import (
	"github.com/gin-gonic/gin"
	"tracing"
)  

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		grpcCtx := tracing.NewGrpcContextFromGin(ctx)
		result, err := GrpcClient.call(grpcCtx)
		c.JSON(200, result)
	})
	r.Run()
}
 
```
