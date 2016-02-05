---
layout: post
title: python tornado 搭建webServer
category: 技术
tags: Python
keywords: 
description: 
---

**1.安装tornado，我用的windows环境**

在tornado网站上下载tornado: 

[https://pypi.python.org/packages/source/t/tornado/tornado-4.2.tar.gz](https://pypi.python.org/packages/source/t/tornado/tornado-4.2.tar.gz)

解压tornado-4.2.tar.gz

在解压后的tornado-4.2文件夹中执行命令：

python setup.py install

完成安装。

**2.官方学习文档**

[http://www.tornadoweb.org/en/stable/guide/intro.html](http://www.tornadoweb.org/en/stable/guide/intro.html)

[http://www.tornadoweb.cn/documentation](http://www.tornadoweb.cn/documentation)

**3.学习笔记**

最基础的POST和GET的方法实现

```python
# -*- coding: utf-8 -*-
import tornado.ioloop
import tornado.web

html = '''
<form method="post" name="frm1" action="/login">
    <label for="txt">用户名</label>
    <input type="text" id="txtname" name="myname">
<br/>
<br/>
	<label for="txt">密码  </label>
    <input type="text" id="txtpwd" name="mypwd">
<br/>
<br/>
    <input type="submit">
</form>
'''

class BaseHandler(tornado.web.RequestHandler):
    def get_current_user(self):
        return self.get_secure_cookie("user")

class MainHandler(BaseHandler):
    def get(self):
    	if not self.current_user:
            self.redirect("/login")
            return
        name = tornado.escape.xhtml_escape(self.current_user)
        self.write("Hello, " + name)

class LoginHandler(BaseHandler):
    def get(self):
        self.write(html)

    def post(self):
    	self.set_secure_cookie("user", self.get_argument("myname"))
        # self.write("POST LOGIN")
        self.redirect("/")

settings = dict(
            # template_path=TEMPLATE_PATH,
            # static_path=STATIC_PATH,
            # cookie_secret=str(uuid.uuid1()),
            cookie_secret="61oETzKXQAGaYdkL5gEmGeJJFuYh7EQnp2XdTP1o/Vo=",
            login_url="/login",
            # gzip=True,
            # xheaders=True,
            debug=True
        )
application = tornado.web.Application([
    (r"/", MainHandler),
    (r"/login", LoginHandler)
], **settings)

if __name__ == "__main__":
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```