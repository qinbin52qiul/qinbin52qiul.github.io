---
layout: post
title: Flask 学习笔记 2-- 路由定义的基本方式
categories: Flask
description: 路由定义的基本方式
keywords: Python, Flask, 路由
---

### 请求方式限定

使用 methods 参数指定可接受的请求方式，可以是多种

```python
@app.route('/', methods=['GET','POST'])
def hello():
    return 'hello,world'
```

### 给路由传参示例

有时我们需要将同一类 URL 映射到同一个视图函数处理，比如：使用同一个视图函数来显示不同用户的订单信息。

路由传递的参数默认当作 string 处理
```python
@app.route('/orders/<order_id>')
def hello_itheima(order_id):
    # 此处的逻辑：去查询数据库改用户的订单信息，并返回
    print(type(order_id)) # 类型为 unicode
    return 'hello itcast %d' % order_id
```

这里指定 int，会调用系统的路由转换进行匹配和转换。

- 大致原理是将参数强制转换为 int，如果成功，则可以进行路由匹配
- 如果参数无法转换成功，就无法匹配该路由


**Flask_Demo.py 文件代码如下：**

```python
# -*- coding:utf-8 -*-

# 1. 导入 Flask 扩展
from flask import Flask

# 2. 创建 Flask 应用程序实例
# 需要传入__name__，作用是为了确定资源所在的路径
app = Flask(__name__)


# 3. 定义路由及视图函数
# Flask 中定义路由是通过装饰器实现的
# 路由默认只支持 GET，如果需要增加，需要自行指定
@app.route('/', methods=['GET', 'POST'])
def index():
    return 'hello flask'


# 使用同一个视图函数，来显示不同用户的订单信息
# <定义路由的参数>。<> 内需要起个名字
@app.route('/orders/<int:order_id>')
def get_order_id(order_id):
    # 参数类型，默认是字符串，Unicode 编码
    print(type(order_id))

    # 有时候，需要对路由做访问优化。订单 ID 应该是 int 类型

    # 需要在视图函数的 () 内填入参数名，那么后面的代码才能去使用
    return 'order_id %s' % order_id


# 4. 启动程序
if __name__ == '__main__':
    # 执行了 app.run 就会将 Flask 程序运行在一个简易的服务器 (Flask 提供的，用于测试的)
    app.run()

```

程序运行结果如下：

![路由传递的参数类型](/images/posts/flask/flaskdemo.png)


![访问 127.0.0.1:8000](/images/posts/flask/flaskdemorun.png)
