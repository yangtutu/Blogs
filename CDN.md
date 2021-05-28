---
title: 阿里云 CDN
date: 2021-05-28
description: 内容分发网络（Content Delivery Network）
---

## 使用指导

- 添加加速域名指向 OSS

  https://help.aliyun.com/document_detail/122181.html?spm=a2c4g.11186623.6.578.6f944f44A25uSp

- 配置 CNAME

  https://help.aliyun.com/document_detail/27144.html?spm=a2c4g.11186623.6.581.36da504dyjYKcp

- 开启阿里云 OSS 私有 Bucket 回源授权 

  https://help.aliyun.com/document_detail/57653.html?spm=a2c4g.11186623.6.601.6c485553mihYFM

- 刷新缓存

  - OSS 自动刷新

    https://help.aliyun.com/knowledge_detail/87426.html?spm=5176.smartservice_service_chat.0.0.1557709a8CCWxX

  - SDK 刷新

    https://help.aliyun.com/knowledge_detail/40116.html?spm=5176.2020520165.120.d40116.2ccf7029DF4Tyn

- 判断缓存是否成功，或请求是否回源

  https://help.aliyun.com/knowledge_detail/40193.html?spm=5176.smartservice_service_chat.help.dexternal.6b933f1bHyKACf&scm=20140722.S_help

- 边缘脚本

  https://help.aliyun.com/document_detail/126565.html?spm=a2c4g.11186623.6.717.7532f6252H0DUD

  兼容 history 模式路由脚本：

  ```
  if not(match_re($uri, '[\\.]')) {
    rewrite('/index.html', 'break')
  }
  ```

  https://zhangsn.me/vue-history-aliyun-cdn/
