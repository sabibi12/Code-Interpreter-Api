 # Code Interpreter API

## 概述

Code Interpreter API 是一个基于 Flask 框架的应用程序，旨在提供一个 API 接口以远程运行代码并获取执行结果。该项目通过 Docker 容器进行隔离，实现对 Python 代码的安全运行。此外，项目还支持将生成的图像数据存储到 PostgreSQL 数据库中，并通过 API 端点进行访问。

## 技术栈

- **后端框架**：Flask (Python)
- **数据库**：PostgreSQL
- **容器化**：Docker
- **ORM**：SQLAlchemy
- **并发处理**：threading, Queue
- **身份验证**：Bearer Token
- **外部请求**：requests
- **代码隔离**：subprocess

## 特性

- **多语言支持**：目前主要支持 Python 代码的执行。
- **图像处理**：支持将代码生成的图像数据转换为 Base64 格式，并可存储在数据库中。
- **Docker 容器隔离**：每个代码执行请求在独立的 Docker 容器中运行，确保安全性和资源隔离。
- **PostgreSQL 数据库集成**：图像数据可以存储到数据库中，并通过 RESTful API 进行访问。
- **身份验证**：可选的 Bearer 令牌身份验证以确保安全访问。
- **环境变量**：可通过环境变量进行配置。
- **错误处理**：全面的错误处理和超时管理。

## 运行环境

- Python 3.8 及以上
- Docker
- PostgreSQL

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/leezhuuuuu/Code-Interpreter-Api.git
cd Code-Interpreter-Api
```

### 2. 安装依赖

请确保已安装 Docker。然后，根据配置文件自动生成 `requirements.txt` 文件：

```bash
python build.py
pip install -r requirements.txt
```

### 3. 配置文件

项目使用 `config.yaml` 作为配置文件。确保该文件中包含以下配置：

- **域名**：用于访问存储的图像。
- **Docker 镜像**：指定用于运行代码的 Docker 镜像。
- **端口范围**：为 Docker 容器指定端口范围。
- **PostgreSQL 配置**：包括数据库名、用户名、密码、主机和端口。
- **资源限制**：为 Docker 容器指定内存和 CPU 限制。
- **超时时间**：指定代码执行的超时时间。

### 4. 启动项目

使用以下命令启动项目：

```bash
python app.py
```

该命令将自动启动 Flask 应用，并在配置的调度中心端口上运行。

## 使用指南

### 1. 运行代码

通过 POST 或 GET 请求访问 `/runcode` 端点，可以运行指定的代码。请求数据应包含以下字段：

- **languageType**：代码的语言类型（当前仅支持 Python）。
- **variables**：可选，传递给代码的变量。
- **code**：要执行的代码。

### 2. 访问图像

通过 GET 请求访问 `/image/<filename>` 端点，可以获取存储在数据库中的图像数据。

## API 端点

### `POST /runcode`

#### 请求

```json
{
  "languageType": "python",
  "variables": {},
  "code": "print('Hello, World!')"
}
```

#### 响应

```json
{
  "output": "Hello, World!\n"
}
```

### `GET /runcode`

#### 请求

```
/runcode?languageType=python&variables={}&code=print('Hello, World!')
```

#### 响应

```json
{
  "output": "Hello, World!\n"
}
```

## 错误处理

应用程序返回适当的 HTTP 状态码和错误消息以应对不同场景：

- **400 Bad Request**：无效的 JSON 或参数。
- **401 Unauthorized**：缺失或无效的令牌。
- **405 Method Not Allowed**：无效的 HTTP 方法。
- **504 Gateway Timeout**：请求超时。

## Docker 集成

应用程序使用 Docker 在隔离环境中运行代码。可以使用提供的 Dockerfile 和 `build.py` 脚本构建 Docker 镜像。

```bash
python build.py
```

## PostgreSQL 集成

代码执行期间生成的图像存储在 PostgreSQL 数据库中。数据库连接详细信息配置在 `config.yaml` 中。

## 并发管理

应用程序使用线程处理多个并发请求，并使用信号量控制并发请求的数量。

## 测试

应用程序包含一个测试套件，可以运行以验证功能：

```bash
python concurrent_test.py
```

## 许可证

本项目基于 GNU 许可证。详见 `LICENSE` 文件。

## 贡献

欢迎贡献！请提交问题或拉取请求。

## 作者

- leezhuuuuu

## 致谢

- Flask
- Docker
- PostgreSQL
- SQLAlchemy

---