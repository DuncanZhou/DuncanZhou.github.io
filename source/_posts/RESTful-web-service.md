---
title: RESTful web service
date: 2017-05-25 20:32:39
tags: RESTful Web Service
categories: Note
---

### 概念
REST架构就是为了HTTP协议设计的。RESTful web services的核心概念是管理资源。资源是由URIs来表示，客户端使用HTTP当中的'POST, OPTIONS, GET, PUT, DELETE'等方法发送请求到服务器，改变相应的资源状态。
```
==========  ===============================================  =============================
HTTP 方法   URL                                              动作
==========  ===============================================  ==============================
GET         http://[hostname]/todo/api/v1.0/tasks            检索任务列表
GET         http://[hostname]/todo/api/v1.0/tasks/[task_id]  检索某个任务
POST        http://[hostname]/todo/api/v1.0/tasks            创建新任务
PUT         http://[hostname]/todo/api/v1.0/tasks/[task_id]  更新任务
DELETE      http://[hostname]/todo/api/v1.0/tasks/[task_id]  删除任务
==========  ================================================ =============================
```

参见该地址
> http://www.pythondoc.com/flask-restful/first.html

