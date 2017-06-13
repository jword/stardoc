# 页面url引用说明

## 目录
   1. [简介](#1)
   2. [url说明](#2)
   3. [页面引用](#3)

   
<h2 id="1">简介</h2>
   
   
   
<h3 id="2.2">url说明</h3>

|页面名称  |url/url规则|  说明|
|:--:|:--:|:--:|
|登录页| /login.html| 用户登录页url |
|注册页| /reg.html || 
|忘记密码页| /forget.html|  |
|退出登录地址| /logout.html|-- | 
|文章搜索页| /article/search |--| 
|文章列表页| /article/| -- |
|解决方案列表页| /cases/|-- | 
|产品列表页| /products/|-- | 
|新闻列表页| /news/|-- | 
|文章评论列表| /front/article/comment|-- | 
|文章评论表单提交地址| /front/article/addcomment|-- | 
|留言表单提交地址| /front/message/index|-- | 
|详情页| /([a-zA-Z0-9]+)/d_([a-zA-Z0-9]{24}).html|-- | 
|分类页| /([a-zA-Z0-9]+)/list_([a-zA-Z0-9]{24}).html|-- | 
|标签页| /([a-zA-Z0-9]+)/tag_(.*).html|-- | 
|应用市场地址|/appmarket/([a-zA-Z0-9\_\-]+)/([a-zA-Z0-9\_\-]+)/:params|--|


<h2 id="3">页面url引用说明</h2>

五星网站中统一生称url的的方法为;

    {{ Url.get('xxx') }}

列表页、单页地址生成方式:

    {{ Url.get('hashid') }}

详情页、固定链接、外链地址生成方式:

    {{ Url.get('http://www.baidu.com') }}
    {{ Url.get('/login.html') }}

详情页地址生成方式:

    默认：{{ Url.get('/model/d_id.html') }}
    自定义：{{ Url.get('hashid') }}



