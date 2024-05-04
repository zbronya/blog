---
categories: 问题随笔
tags:
  - chatgpt
  - chat to api
description: ''
permalink: free-chat-to-api/
title: 免费的gpt-3.5-turbo api
cover: /images/dfd92b3f8b2ceee7683f60579ba67e16.jpg
date: '2024-05-03 22:36:00'
updated: '2024-05-03 22:44:00'
---

## 服务地址


```bash
curl 'https://free-chat-gateway.bronya.io/v1/chat/completions' \
--header 'Content-Type: application/json' \
--data '{
    "model": "gpt-3.5-turbo",
    "messages": [
        {
            "role": "user",
            "content": "Say this is a test! "
        }
    ],
    "stream": true
}'
```


## 接口限制


3/10s


## 其他说明

- 如果你想搭建自己的无限制的服务可以试试 [https://github.com/zbronya/free-chat-to-api](https://github.com/zbronya/free-chat-to-api)
- 免费服务随时跑路
