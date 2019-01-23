---
title: "本地创建、开发、测试、部署 AWS Lambda - 通过 aws-sam-cli"
date: 2018-11-15T17:17:07+08:00
tags: [""]
categories: ["serverless"]
---

> https://github.com/awslabs/aws-sam-cli。

---

通过基于 yaml 格式的 SAM 描述文件，用户可以定义一个具体的 lambda 函数的各类配置信息，如运行环境、资源限制、API 网关、权限等。

当用户通过 SAM 定义 Lambda 应用后，在正式部署前还需要进行本地测试。AWS SAM Cli 是一个由 Python 实现的命令行工具。通过 SAM Cli，用户可以在本地开发环境中运行和调试 SAM 定义的 AWS Lambda 应用。SAM Cli 可以模拟事件源的事件输出，启动本地 API 网关实例。测试完毕后，用户可以通过 SAM Cli 打包 Lambda 应用，并最终将其部署到AWS Lambda 云平台上。通过 SAM 和 SAM Cli, Lambda 用户可以更容易地定义 Lambda 应用，并在本地和远端进行 Lambda 应用的测试，实现 CICD 流水线。关于 SAM Cli 详细的使用方法，请参考其GitHub仓库的文档和示例：https://github.com/awslabs/aws-sam-cli。

> AWS SAM Cli 以前叫 AWS SAM Local

## SAM CLI

`sam` is the AWS CLI tool for managing Serverless applications written with AWS Serverless Application Model (SAM). SAM CLI can be used to test functions locally, start a local API Gateway from a SAM template, validate a SAM template, fetch logs, generate sample payloads for various event sources, and generate a SAM project in your favorite Lambda Runtime.

`sam` 是用于管理使用AWS无服务器应用程序模型（SAM）编写的无服务器应用程序的 AWS CLI 工具。SAM CLI 可用于本地测试功能，从 SAM 模板启动本地 API 网关，验证 SAM 模板，获取日志，为各种事件源生成样本有效负载，以及在您喜欢的 Lambda Runtime 中生成 SAM 项目。

##### 1. Installation

> Mac or Linux

1. Install the SAM CLI using Brew

Requires Docker and the AWS CLI. 
安装 Docker: [Docker for macOS](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
安装 AWS CLI：`pip install awscli`，参考 AWS CLI: https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html

```
brew tap aws/tap
brew install aws-sam-cli
sam --version
```

2. Install the AWS SAM CLI Using Pip

```
python --version
pip --version
pip install --user aws-sam-cli
sam --version
```

参考：https://docs.aws.amazon.com/zh_cn/serverless-application-model/latest/developerguide/serverless-sam-cli-install-using-pip.html

> 我是使用 pip 安装成功的。

##### 2. Introduction to SAM and SAM CLI

##### 3. Running and debugging serverless applications locally

##### 4. Packaging and deploying your application

##### 5. Advanced

##### 6. Examples

