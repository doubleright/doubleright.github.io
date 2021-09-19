---
title: Gitlab Ci Runner 使用说明
tags: TeXt
---

# Gitlab Ci Runner

## 说明

CI .gitlab-ci.yml配置文件, 一般放置根目录

```yaml
# 这是一个 yaml 文件，使用 # 作为注释
stages: #定义job 分别是编译和发布，注意不同的 job 是在完全空白的项目，不会用到上一个job编译的文件
  - build
  - publish
  
build: #一个job
  stage: build
  script: #执行脚本
    - "chcp 65001"
    - "dotnet restore"
    - "NuGet.exe restore project.sln"
    # - "dotnet build -c release"
    - "MSBuild project.sln /t:Rebuild /p:Configuration=Release"
  only: # 在什么时候触发
    - develop

publish:
  stage: publish
  script:
    - "chcp 65001"  
    - "dotnet restore"
    - "NuGet.exe restore project.sln"
    # - "dotnet build -c release"
    - "MSBuild EQ.IPC.sln /t:Rebuild /p:Configuration=Release"  
    - "NuGet.exe pack project.nuspec -OutputDirectory project"
    - 'NuGet.exe push project\*.nupkg -Source http://192.168.0.231/api/v4/projects/73/packages/nuget/index.json -ApiKey $ApiKey'
  only:
    - master
```



## Runner 部署 （windows）

#### 下载对应的Gitlab-Runner.exe  

```
Gitlab-Runner.exe install

Gitlab-Runner.exe start

Gitlab-Runner.exe register

Gitlab-Runner.exe run # 必须这里run才能调部署runner的虚拟机一些用户定义配置
```

Runner注册完会生成一个配置文件 config.toml可以在这儿修改完重启

```
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "名称"
  url = "http://192.168.0.231/"#gitlab 地址
  token = "..."#项目runner授权token
  executor = "shell"#脚本运行环境
  environment = ["PATH=%PATH%;C:\\Program Files\\dotnet;C:\\Windows\\System32;‪C:\\GitlabCI\\Nuget;C:\\Program Files\\Git\\bin;C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\MSBuild\\Current\\Bin"]#环境配置
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
```



刚才说了要Run 毕竟一直开着cmd也不太好；用vbs挂一个不显示的cmd

```cmd
@echo off
mode con lines=30 cols=60
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",0)(window.close)&&exit
cd /d "%~dp0"
gitlab-runner.exe
```



Runner 其实是一台主机（虚拟机） 触发时会git拉取下来项目代码，执行脚本，达到自动化构建及发布还可以做很多事



思考 ：为什么一定要run ,可能考虑用户安全问题！