---
title: 使用 STS 临时访问凭证访问 OSS
date: 2021-05-28
description: 使用 STS 临时访问凭证访问 OSS
---

## 基本配置

https://help.aliyun.com/document_detail/100624.htm?spm=a2c4g.11186623.2.5.335562e7IbAdzK 

- 创建 RAM 用户 
  
  - 注意保存访问密钥（AccessKey ID 和 AccessKey Secret）
  
  - 只需要给 RAM 用户 `AliyunSTSAssumeRoleAccess` 的权限

- 创建 RAM 角色

  - RAM 角色对应 AssumeRole 接口中的 RoleArn

  - RAM 角色需要和权限配合使用，控制 STS 临时访问凭证具有的权限

- 创建 RAM 权限

  - RAM 权限对应 AssumeRole 接口中的 RoleSessionName

  - 参考脚本策略

    ``` json
    {
        "Version": "1",
        "Statement": [
         {
               "Effect": "Allow",
               "Action": [
                 "oss:PutObject"
               ],
               "Resource": [
                 "acs:oss:*:*:examplebucket/exampledir",
                 "acs:oss:*:*:examplebucket/exampledir/*"
               ]
         }
        ]
    }
    ```

  - Acion

    权限列表： https://help.aliyun.com/document_detail/31948.html?spm=a2c4g.11186623.6.1592.4d1af2ee3eEXYz
    
    其中 `oss:*` 表示 OSS 的所有权限

  - Resource

    | 内容 | 资源 |
    | ------- | ------- |
    | acs:oss:*:*:examplebucket/* | examplebucket 的所有资源操作权限 |
    | acs:oss:*:*:examplebucket/exampledir/* | examplebucket 下 exampledir 目录的所有资源操作权限 |


- RAM 角色 与 RAM 权限绑定

  注意要给 RAM 角色授予上面创建的 RAM 权限

- AssumeRole

  https://help.aliyun.com/document_detail/28763.htm?spm=a2c4g.11186623.2.11.14ad8924VGzO3q

## 搭建 STS 服务

- STS SDK 概览

  https://help.aliyun.com/document_detail/121136.htm?spm=a2c4g.11186623.2.16.5c315d60henuG5#reference-w5t-25v-xdb

- 在线测试

  https://next.api.aliyun.com/api/Sts/2015-04-01/AssumeRole?spm=a2c4g.11186623.2.12.427c89243Xm4GI&params={}


## Browser.js 

- 安装

  https://help.aliyun.com/document_detail/64041.htm?spm=a2c4g.11186623.2.6.60c36e02KZ9kJi#concept-64041-zh

- 通过浏览器直接访问 OSS 时，要配置 OSS bucket 的 CORS 规则，要求如下：

  - 来源：设置精准域名例如 `www.aliyun.com` 或带有通配符星号 `*` 的域名，例如 *.aliyun.com

  - 允许 Methods：建议您根据实际使用场景，选择不同的 Methods；例如分片上传时，设置为 PUT；删除文件时，设置为 DELETE

  - 允许 Headers：设置为 `*`

  - 暴露 Headers：设置为 `ETag`、`x-oss-request-id` 和 `x-oss-version-id`
