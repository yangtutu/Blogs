---
title: Jenkins - .Net 构建
date: 2021-07-10
---

## 目录结构

``` bash
├── Jenkinsfile         ## Jenkins 构建文件
├── README.md           ## 帮助文档
├── build.sh            ## 构建脚本
├── docker
│   └── Dockerfile      ## Dockerfile
├── pack.sh             ## 镜像打包脚本
└── src                 ## 代码
    ├── WebApi          ## 项目代码
    └── WebApi.sln      ## 解决方案
```

## 构建脚本 build.sh

``` bash

cd src/WebApi
rm -rf publish

dotnet restore

# 普通构建
# dotnet publish -c Release -o ./publish -r alpine-x64

# 单文件构建
dotnet publish -c Release -o ./publish -r alpine-x64 /p:PublishSingleFile=true /p:PublishTrimmed=true /p:DebugType=None /p:DebugSymbols=false --self-contained true

``` 

## Jenkinsfile

``` bash
pipeline {
    environment {
        WORKSPACE_ON_HOST = "/var/jenkins_home"                    // 宿主机工作目录
        SERVICE_NAME = "dotnet-ci"                                 // 服务名称
        REGISTRY_HOST = ""                                         // 镜像仓库地址
        IMAGE_NAME = "${env.REGISTRY_HOST}/${env.SERVICE_NAME}"    // 镜像名称
    }

    stages {
        stage("pre-build") {
            steps {
                script {
                    echo "打印构建信息"
                    sh "export"

                    // 使用当前日期 + git 提交号前8位作为镜像 TAG
                    env.IMAGE_TAG = Calendar.getInstance().getTime().format("yyyyMMdd-HHmmss",TimeZone.getTimeZone("GMT+8:00")) + "-" + env.GIT_COMMIT.substring(0, 8) 

                    if (env.BRANCH_NAME == env.TAG_NAME) { 
                        env.IMAGE_TAG = env.TAG_NAME
                        echo "当前TAG：" + env.TAG_NAME
                    }
                    else {
                        echo "当前分支：" + env.BRANCH_NAME
                    }

                    echo "当前提交：" + env.GIT_COMMIT
                    echo "宿主机工作目录：" + env.WORKSPACE_ON_HOST
                    echo "容器工作目录：" + env.WORKSPACE
                    echo "镜像名称：" + env.IMAGE_NAME + ":" + env.IMAGE_TAG
                }
            }
        }    
    }
}
``` 
