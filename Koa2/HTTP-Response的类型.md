# HTTP Response 的类型

Koa 默认的返回类型是text/plain，如果想返回其他类型的内容，可以先用ctx.request.accepts判断一下，客户端希望接受什么数据（根据 HTTP Request 的Accept字段），然后使用ctx.response.type指定返回类型。

``` javascript
const main = ctx => {
    if (ctx.request.accepts('xml')) {
        ctx.response.type = 'xml';
        ctx.response.body = '<data>Hello World</data>';
    } else if (ctx.request.accepts('json')) {
        ctx.response.type = 'json';
        ctx.response.body = {
            data: 'Hello World'
        };
    } else if (ctx.request.accepts('html')) {
        ctx.response.type = 'html';
        ctx.response.body = '<p>Hello World</p>';
    } else {
        ctx.response.type = 'text';
        ctx.response.body = 'Hello World';
    }
};
```
运行这个 demo。
```
$ node demos/03.js
```
访问 http://127.0.0.1:3000 ，现在看到的就是一个 XML 文档了。