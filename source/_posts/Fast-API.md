---
title: Fast API
tags: 
- FastAPI
- Python
date: 2023-12-26 10:07:14
cover:
---

FastAPI是一个现代、快速（高性能）的Web框架，用于构建APIs和基于Python 3.6及以上版本的异步服务器网关接口（ASGI）应用程序。
它是由Python的异步框架`asyncio`和类型提示（Type Hint）设计的，旨在提供高性能的Web应用程序，
并支持最新的安全性和OpenAPI / Swagger规范。

FastAPI的主要特点包括：

1. **性能**：FastAPI使用异步编程和事件循环，可以实现高并发处理，从而提高性能。
2. **安全性**：FastAPI内置了许多安全功能，如OAuth2、JWT、API密钥、CORS、JSON Web Signatures (JWS) 等。
3. **易用性**：FastAPI有一个简单的API，易于使用和扩展。它支持Python的类型提示，使得代码更加清晰和易于维护。
4. **OpenAPI / Swagger**：FastAPI支持OpenAPI规范，可以自动生成Swagger UI，方便开发者测试和文档化API。
5. **中间件支持**：FastAPI允许使用中间件来处理请求和响应，提供了很大的灵活性。
6. **依赖注入**：FastAPI支持依赖注入，可以很容易地组织和管理应用程序中的依赖关系。
7. **跨平台**：FastAPI可以运行在所有的主流WSGI服务器上，如Uvicorn、Django、Flask等。

### 快速入门

要开始使用FastAPI，可以遵循以下步骤：

1. **安装**：首先，安装FastAPI和Uvicorn（一个支持ASGI的WSGI服务器）。
   ```bash
   pip install fastapi uvicorn
   ```
2. **创建项目**：创建一个新的Python项目，并初始化它。
   ```bash
   mkdir my_project
   cd my_project
   python -m venv venv
   source venv/bin/activate  # 在Unix或MacOS上
   venv\Scripts\activate  # 在Windows上
   ```
3. **编写代码**：在项目中创建一个`main.py`文件，并编写一个简单的FastAPI应用程序。
   ```python
   from fastapi import FastAPI
   app = FastAPI()
   @app.get("/items/{item_id}")
   async def read_item(item_id: int):
       return {"item_id": item_id}
   ```
4. **运行服务器**：使用Uvicorn运行FastAPI应用程序。
   ```bash
   uvicorn main:app --reload
   ```
5. **访问API**：在浏览器中访问`http://127.0.0.1:8000`，查看自动生成的Swagger UI，并尝试调用API。

### 示例

以下是一个简单的FastAPI应用程序示例，它定义了一个路由来获取项目信息：
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/")
async def read_items():
    return [{"item_id": 1}, {"item_id": 2}]
    
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

在这个示例中，我们定义了两个路由：`/items/`用于获取项目列表，`/items/{item_id}`用于获取特定项目的详细信息。`item_id`是一个路径参数，它将自动从URL中提取并作为参数传递给`read_item`函数。

FastAPI的官方文档（https://fastapi.tiangolo.com/）