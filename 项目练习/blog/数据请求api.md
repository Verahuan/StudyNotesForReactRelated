## 1.前言

整理一下需要mock的数据有哪些

api文档

参考盛哥swagger文档

http://time-frequency-be.dev.xuxusheng.com:88/swagger/index.html

## 2.具体整理

1.创建新用户，可以指定用户是否为管理员

```json
示例：
{
  "data": {
    "created_at": "string",
    "email": "string",
    "id": 0,
    "is_admin": true,
    "name": "string",
    "nick_name": "string",
    "phone": "string",
    "role": "string",
    "updated_at": "string"
  },
  "err_code": 0,
  "err_debugs": [
    "string"
  ],
  "err_details": [
    "string"
  ],
  "err_msg": "string"
}
```

2. 文章相关内容
   1. 热门文章list get
   2. 文章内容/首页 get
   3. 文章管理-删除
   4. 文章管理-添加
3. 标签相关
   1. 标签列表展示
   2. 标签删除：DELETE
   3. 添加标签 ：post请求
4. 评论相关
   1. 最新评论 get
   2. 每篇文章的评论 get