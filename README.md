# WebGoat：故意存在安全漏洞的 Web 应用

> **中文翻译版 v2026.10.30**：本仓库基于 [WebGoat 原项目](https://github.com/WebGoat/WebGoat) 翻译和本地化，翻译日期为 **2026-10-30**。本中文版本为社区维护的非官方版本，原项目版权、许可证和安全练习内容归原项目及其贡献者所有。
>
> 原项目官网与源码：[OWASP WebGoat](https://github.com/WebGoat/WebGoat)

[![Build](https://github.com/WebGoat/WebGoat/actions/workflows/build.yml/badge.svg?branch=develop)](https://github.com/WebGoat/WebGoat/actions/workflows/build.yml)
[![java-jdk](https://img.shields.io/badge/java%20jdk-25-green.svg)](https://jdk.java.net/)
[![OWASP Labs](https://img.shields.io/badge/OWASP-Lab%20project-f7b73c.svg)](https://owasp.org/projects/)
[![GitHub release](https://img.shields.io/github/release/WebGoat/WebGoat.svg)](https://github.com/WebGoat/WebGoat/releases/latest)
[![Gitter](https://badges.gitter.im/OWASPWebGoat/community.svg)](https://gitter.im/OWASPWebGoat/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Discussions](https://img.shields.io/github/discussions/WebGoat/WebGoat)](https://github.com/WebGoat/WebGoat/discussions)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-%23FE5196?logo=conventionalcommits&logoColor=white)](https://conventionalcommits.org)

## 简介

WebGoat 是由 [OWASP](http://www.owasp.org/) 维护的故意存在安全漏洞的 Web 应用，旨在帮助用户学习 Web 应用安全。

本程序演示常见的服务器端应用缺陷。练习面向希望学习应用安全和渗透测试技术的用户。

> **警告一：**运行本程序期间，您的计算机可能极易受到攻击。使用时建议断开互联网连接。WebGoat 默认仅绑定本机地址，以尽量减少暴露面。
>
> **警告二：**本程序仅供教育用途。未经授权尝试这些技术很可能导致被发现；未经授权的攻击可能带来严重后果。请只在您拥有或获准测试的环境中练习。

![WebGoat](docs/images/webgoat.png)

## 安装与运行

更多信息请参阅[贡献指南](CONTRIBUTING.md)。

### 1. 使用 Docker 运行

如果本机已安装浏览器以及 ZAP 和/或 Burp，可以直接运行 WebGoat Docker 镜像。

每个正式版本也会发布到 [Docker Hub](https://hub.docker.com/r/webgoat/webgoat)。

```shell
docker run -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 webgoat/webgoat
```

在部分课程中，需要让容器使用与本机相同的时区，可通过 `TZ` 环境变量设置，例如：

```shell
docker run -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 -e TZ=Asia/Shanghai webgoat/webgoat
```

如需使用 OWASP ZAP 或其他代理，请使用自定义主机名，而不要使用 `127.0.0.1` 或 `localhost`。例如在 hosts 文件中添加：

```text
127.0.0.1 www.webgoat.local www.webwolf.local
```

然后运行：

```shell
docker run -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 -e WEBGOAT_HOST=www.webgoat.local -e WEBWOLF_HOST=www.webwolf.local -e TZ=Asia/Shanghai webgoat/webgoat
```

访问 WebGoat：<http://www.webgoat.local:8080/WebGoat/>；访问 WebWolf：<http://www.webwolf.local:9090/WebWolf/>。

### 2. 使用带 Linux 桌面的 Docker 镜像

也可以运行提供浏览器桌面的完整 Docker 镜像，无需在本机单独安装工具：

```shell
docker run -p 127.0.0.1:3000:3000 webgoat/webgoat-desktop
```

### 3. 独立运行 JAR

从 [GitHub Releases](https://github.com/WebGoat/WebGoat/releases) 下载最新版本：

```shell
# Linux/macOS
export TZ=Asia/Shanghai
java -Dfile.encoding=UTF-8 -jar webgoat-2026.6-SNAPSHOT.jar

# Windows PowerShell
$env:TZ = "Asia/Shanghai"
java -Dfile.encoding=UTF-8 -jar webgoat-2026.6-SNAPSHOT.jar
```

根据日志中的链接打开 WebGoat。

#### 3.1 使用其他端口

如需更改端口，可传入以下参数：

```shell
java -jar webgoat-2026.6-SNAPSHOT.jar --webgoat.port=8001 --webwolf.port=8002
```

合并 JAR 中，`--server.port` 是 `--webgoat.port` 的别名。WebWolf 端口需单独使用 `--webwolf.port` 设置。可用配置项请参阅 [WebGoat 配置](src/main/resources/application-webgoat.properties)和 [WebWolf 配置](src/main/resources/application-webwolf.properties)。

### 4. 从源代码运行

前置条件：

- Java 25
- 您常用的 IDE
- Git（或 IDE 内置的 Git 支持）

克隆并构建项目：

```shell
git clone https://github.com/WebGoat/WebGoat.git
cd WebGoat
git checkout <分支名>

# Linux/macOS
./mvnw clean install

# Windows
./mvnw.cmd clean install
```

如果以前运行过 WebGoat，课程内容变更可能导致测试受到旧数据影响；必要时先运行 `./mvnw clean -Pcleanall`（Windows 使用 `./mvnw.cmd clean -Pcleanall`）。

本地构建 Docker 镜像：

```shell
docker build -f Dockerfile -t webgoat/webgoat:zh-local .

docker run --rm -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 -e TZ=Asia/Shanghai webgoat/webgoat:zh-local
```

启动后访问 <http://127.0.0.1:8080/WebGoat>。登录后在页面右上方的语言菜单中选择“简体中文”。

启动应用：

```shell
# Linux/macOS
./mvnw spring-boot:run

# Windows
./mvnw.cmd spring-boot:run
```

稍后访问 <http://localhost:8080/WebGoat>。未登录时会跳转到登录页面；登录或创建账户后即可继续。

如需更改监听地址，请在运行参数中设置 `--server.address=<地址>`，或调整相应的 Spring Boot 配置。

### 5. 自定义课程菜单

高级用户可以通过环境变量隐藏部分菜单类别或课程。Linux/macOS 示例：

```shell
export TZ=Asia/Shanghai
export EXCLUDE_CATEGORIES="CLIENT_SIDE,GENERAL,CHALLENGE"
export EXCLUDE_LESSONS="SqlInjectionAdvanced,SqlInjectionMitigations"
java -jar target/webgoat-2026.6-SNAPSHOT.jar
```

Docker 示例：

```shell
docker run -d -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 -e EXCLUDE_CATEGORIES="CLIENT_SIDE,GENERAL,CHALLENGE" -e EXCLUDE_LESSONS="SqlInjectionAdvanced,SqlInjectionMitigations" webgoat/webgoat
```

## 中文界面

登录后可在页面右上角的语言菜单中选择“简体中文”。中文菜单、通用界面、提示和登录/注册信息已本地化；尚未提供中文课程译文的课程内容会回退显示英文。欢迎通过贡献指南协助补充课程翻译。
