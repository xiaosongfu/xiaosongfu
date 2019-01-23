---
title: "本地创建、开发、测试、部署 Azure Functions - 通过 azure-functions-core-tools"
date: 2018-11-15T17:17:07+08:00
tags: [""]
categories: ["serverless"]
---

> https://github.com/Azure/azure-functions-core-tools

---

安装 azure-functions-core-tools：

```
fuxiaosongdeMac-mini:~ fuxiaosong$ brew tap azure/functions


fuxiaosongdeMac-mini:~ fuxiaosong$ brew install azure-functions-core-tools
==> Installing azure-functions-core-tools from azure/functions
==> Downloading https://functionscdn.azureedge.net/public/2.3.199/Azure.Functions.Cli.osx-x64.2.3.199.zip
######################################################################## 100.0%
🍺  /usr/local/Cellar/azure-functions-core-tools/2.3.199: 1,123 files, 440.0MB, built in 101 minutes 3 seconds
```


```
fuxiaosongdeMac-mini:~ fuxiaosong$ func

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

Azure Functions Core Tools (2.3.199 Commit hash: fdf734b09806be822e7d946fe17928b419d8a289)
Function Runtime Version: 2.0.12246.0
Usage: func [context] [context] <action> [-/--options]

Contexts:
azure       Commands to log in to Azure and manage resources
durable     Commands for working with Durable Functions
extensions  Commands for installing extensions
function    Commands for creating and running functions locally
host        Commands for running the Functions host locally
settings    Commands for managing environment settings for the local Functions host
templates   Commands for listing available function templates

Actions:
start   Launches the functions runtime host
    --port [-p]        Local port to listen on. Default: 7071
    --cors             A comma separated list of CORS origins with no spaces. E
                       xample: https://functions.azure.com,https://functions-st
                       aging.azure.com
    --cors-credentials Allow cross-origin authenticated requests (i.e. cookies
                       and the Authentication header)
    --timeout [-t]     Timeout for on the functions host to start in seconds. D
                       efault: 20 seconds.
    --useHttps         Bind to https://localhost:{port} rather than http://loca
                       lhost:{port}. By default it creates and trusts a certifi
                       cate.
    --cert             for use with --useHttps. The path to a pfx file that con
                       tains a private key
    --password         to use with --cert. Either the password, or a file that
                       contains the password for the pfx file
    --language-worker  Arguments to configure the language worker.
    --no-build         Do no build current project before running. For dotnet p
                       rojects only. Default is set to false.

deploy  Deploy a function app to custom hosting backends
    --registry A Docker Registry name that you are logged into
    --platform Hosting platform for the function app. Valid options: kubernetes
    --name     Function name
    --min      [Optional] Minimum number of function instances
    --max      [Optional] Maximum number of function instances
    --config   [Optional] Config file

new     Create a new function from a template. Aliases: new, create
    --language [-l] Template programming language, such as C#, F#, JavaScript,
                    etc.
    --template [-t] Template name
    --name [-n]     Function name
    --csx           use old style csx dotnet functions

init    Create a new Function App in the current folder. Initializes git repo.
    --source-control Run git init. Default is false.
    --worker-runtime Runtime framework for the functions. Options are: dotnet,
                     node, python
    --force          Force initializing
    --docker         Create a Dockerfile based on the selected worker runtime
    --csx            use csx dotnet functions

logs    Gets logs of Functions running on custom backends
    --platform Hosting platform for the function app. Valid options: kubernetes
    --name     Function name
    --config   [Optional] Config file


fuxiaosongdeMac-mini:~ fuxiaosong$
```



```
fuxiaosongdeMac-mini:azure fuxiaosong$ func init azure-cli-demo --worker-runtime node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing /Users/fuxiaosong/dev/project/serverless/azure/azure-cli-demo/.vscode/extensions.json
fuxiaosongdeMac-mini:azure fuxiaosong$ ls -la
total 0
drwxr-xr-x  3 fuxiaosong  staff   96 Jan 16 16:34 .
drwxr-xr-x  7 fuxiaosong  staff  224 Jan 16 16:33 ..
drwxr-xr-x  6 fuxiaosong  staff  192 Jan 16 16:34 azure-cli-demo
fuxiaosongdeMac-mini:azure fuxiaosong$ cd azure-cli-demo/


fuxiaosongdeMac-mini:azure fuxiaosong$ cd azure-cli-demo/

fuxiaosongdeMac-mini:azure-cli-demo fuxiaosong$ func new --name "greeting"
Select a template:
1. Azure Blob Storage trigger
2. Azure Cosmos DB trigger
3. Azure Event Grid trigger
4. Azure Event Hub trigger
5. HTTP trigger
6. IoT Hub (Event Hub)
7. Azure Queue Storage trigger
8. SendGrid
9. Azure Service Bus Queue trigger
10. Azure Service Bus Topic trigger
11. Timer trigger
Choose option: 5
HTTP trigger
Function name: [HttpTrigger] Writing /Users/fuxiaosong/dev/project/serverless/azure/azure-cli-demo/greeting/index.js
Writing /Users/fuxiaosong/dev/project/serverless/azure/azure-cli-demo/greeting/sample.dat
Writing /Users/fuxiaosong/dev/project/serverless/azure/azure-cli-demo/greeting/function.json
The function "greeting" was created successfully from the "HTTP trigger" template.
fuxiaosongdeMac-mini:azure-cli-demo fuxiaosong$
```

