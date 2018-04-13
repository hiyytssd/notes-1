# Context对象

Koa 提供一个 Context 对象，表示一次对话的上下文（包括 HTTP 请求和 HTTP 响应）。通过加工这个对象，就可以控制返回给用户的内容。

为了使用方便，许多上下文属性和方法都被委托代理到他们的 ctx.request 或 ctx.response，比如访问 ctx.type 和 ctx.length 将被代理到 response 对象，ctx.path 和 ctx.method 将被代理到 request 对象。

示例：

``` javascript
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
    ctx.body = 'hello world!';
    console.log(ctx.body);
    console.log(ctx.type);
})

app.listen(3000);

console.log('[demo] starting at port 3000');
```
node  启动服务，刷新页面后命令行控制台输出：

```
[demo] starting at port 3000
hello world!
text/plain
```

## ctx.response 对象包括以下属性和别名方法

- ctx.header
    获取返回头

- ctx.body
    获取 res.body，当 res.body 为 null ，但返回状态仍为 200 时，koa 将会返回 404 页面。

- ctx.body=
    设置请求返回的主要内容，可以是以下几种类型：
    - string
        Content-Type 将默认设置为 text/html 或者 text/plain，默认字符集是 utf-8，Content-Length 也将一并设置

    - Buffer
        Content-Type 将默认设置为 application/octet-stream，Content-Length 也将一并设置

    - Stream
        Content-Type 将默认设置为 application/octet-stream

    - Object
        Content-Type 将默认设置为 application/json 注意：默认的json返回会添加空格，如果你希望压缩json返回中的空格，可以这样配置：app.jsonSpaces = 0

    - null

- ctx.status
    获取返回状态

- ctx.status=
    设置返回状态，[点这里查看可用状态](#status可用状态)

- ctx.length=
    设置返回头的 Content-Length 属性

- ctx.length
    返回返回头的 Content-Length 属性，当不存在 Content-Length 属性时，根据 res.body 推断

- ctx.type
    获取返回头中的 Content-Type，不包括 "charset" 等属性。

- ctx.type=
    使用字符串或者文件后缀设定返回的 Content-Type
    ``` javascript
    this.type = 'text/plain; charset=utf-8';
    this.type = 'image/png';
    this.type = '.png';
    this.type = 'png';
    ```
    注意：当使用文件后缀指定时，koa 会默认设置好最匹配的编码字符集，比如当设定 res.type = 'html' 时，koa 会默认使用 "utf-8" 字符集。但当明确使用 res.type = 'text/html' 指定时，koa 不会自动设定字符集。

- ctx.headerSent
    判断一个响应头是否已经发送到客户端，通常用来检测客户端是否收到了错误信息。

- ctx.redirect(url, [alt])
    返回一个 302 跳转到给定的 url，您也可以使用关键词 back 来跳转到该 url 的上一个页面（refer），当没有上一个页面时，默认会跳转到 '/'
    ``` javascript
    this.redirect('back');
    this.redirect('back', '/index.html');
    this.redirect('/login');
    this.redirect('http://google.com');
    ```
    如果你需要覆盖 302 状态码，并在跳转时返回一些文案，可以这样做：
    ``` javascript
    this.status = 301;
    this.redirect('/cart');
    this.body = 'Redirecting to shopping cart';
    ```

- ctx.attachment([filename])
    设置返回熟悉 Content-Disposition 为 "attachment"，并告知客户端进行下载。

- ctx.set(field, value)
    使用给定的参数设置一个返回头属性：
    ``` javascript
    this.set('Cache-Control', 'no-cache');
    ```
- ctx.set(fields)
    使用给定的对象一次设置多个返回头属性：
    ``` javascript
    this.set({
        'Etag': '1234',
        'Last-Modified': date
    });
    ```

- ctx.get(field)
    获取指定的返回头属性，属性名称区分大小写。

- ctx.remove(fields)
    删除指定的返回头属性

- ctx.lastModified=
    设置返回头中的 Last-Modified 属性，可以使用时间对象或者时间字符串。
    ``` javascript
    this.response.lastModified = new Date();
    ```

- ctx.lastModified
    如果返回头中存在 Last-Modified 属性，则返回它。

- ctx.etag=
    设置返回头的 Etag 字段。koa 不提供关于 Etag 的获取方法。
    ``` javascript
    this.response.etag = crypto.createHash('md5').update(this.body).digest('hex');
    ```



## ctx.request 对象包括以下属性和别名方法

- ctx.header
    返回请求头

- ctx.method
    返回请求方法

- ctx.method=
    设置 req.method ，用于实现输入 methodOverride() 的中间件

- ctx.url
    返回请求 url

- ctx.url=
    设置请求 url，用于进行 url 重写

- ctx.path
    返回请求 pathname

- ctx.path=
    设置请求 pathname，如果原有 url 存在查询字符串，则保留这些查询。

- ctx.query
    返回经过解析的查询字符串，类似 Express 中的 req.query，当不存在查询字符串时，返回空对象。
    当 url 包含查询字符串 "color=blue&size=small" 时，返回
    ```
    {
        color: 'blue',
        size: 'small'
    }
    ```
- ctx.query=
    设置给定的对象为查询对象。范例代码： `this.query = { next: '/login' };`

- ctx.querystring
    返回 url 中的查询字符串，去除了头部的 '?'

- ctx.querystring=
    设置查询字符串，不包含 '?'

- ctx.search
    返回 url 中的查询字符串，包含了头部的 '?'

- ctx.search=
    设置查询字符串，包含 '?'

- ctx.length
    返回 req 对象的 Content-Length (Number)

- ctx.host
    返回请求主机名，不包含端口；当 app.proxy 设置为 true 时，支持 X-Forwarded-Host。

- ctx.type
    返回 req 对象的 Content-Type，不包括 charset 属性

- ctx.fresh
    检查客户端请求的缓存是否是最新。当缓存为最新时，可编写业务逻辑直接返回 304，范例代码如下：
    ``` javascript
    this.set('ETag', '123');

    // 当客户端缓存是最新时
    if (this.fresh) {
    this.status = 304;
    return;
    }

    // 当客户端缓存已过期时，返回最新的数据
    this.body = yield db.find('something'); 
    ```
- ctx.stale
    与 req.fresh 返回的结果正好相反

- ctx.socket
- ctx.protocol
    返回请求协议名，如 "https" 或者 "http"；当 app.proxy 设置为 true 时，支持 X-Forwarded-Proto。

- ctx.secure
    判断请求协议是否为 HTTPS 的快捷方法，等同于 this.protocol == "https"

- ctx.ip
    返回请求IP；当 app.proxy 设置为 true 时，支持 X-Forwarded-For。

- ctx.ips
    返回请求IP列表，仅当 app.proxy 设置为 true ，并存在 X-Forwarded-For 列表时，否则返回空数组。

- ctx.subdomains
    返回请求对象中的子域名数组。子域名数组会自动由请求域名字符串中的 . 分割开，在没有设置自定义的 app.subdomainOffset 参数时，默认返回根域名之前的所有子域名数组。

    例如，当请求域名为 "tobi.ferrets.example.com" 时候，返回 ["ferrets", "tobi"]，数组顺序是子代域名在前，孙代域名在后。

    此例中，如果设置了自定义的 app.subdomainOffset 为 3，将忽略三级域名，返回 ["tobi"]。

- ctx.is(type)
    判断请求对象中 Content-Type 是否为给定 type 的快捷方法，如果不存在 request.body，将返回 undefined，如果没有符合的类型，返回 false，除此之外，返回匹配的类型字符串。
    ``` javascript
    // 客户端 Content-Type: text/html; charset=utf-8
    this.is('html'); // => 'html'
    this.is('text/html'); // => 'text/html'
    this.is('text/*', 'text/html'); // => 'text/html'

    // 客户端 Content-Type 为 application/json 时：
    this.is('json', 'urlencoded'); // => 'json'
    this.is('application/json'); // => 'application/json'
    this.is('html', 'application/*'); // => 'application/json'

    this.is('html'); // => false
    ```
    下方的代码使用 req.is(type)，仅当请求类型为图片时才进行操作：
    ``` javascript
    if (this.is('image/*')) {
        // process
    } else {
        this.throw(415, 'images only!');
    }
    ```

- ctx.accepts(type)
    判断请求对象中 Accept 是否为给定 type 的快捷方法，当匹配到符合的类型时，返回最匹配的类型，否则返回 false（此时服务器端应当返回 406 "Not Acceptable" ），传入参数可以是字符串或者数组。
    ``` javascript
    // Accept: text/html
    this.accepts('html');
    // => "html"

    // Accept: text/*, application/json
    this.accepts('html');
    // => "html"
    this.accepts('text/html');
    // => "text/html"
    this.accepts('json', 'text');
    // => "json"
    this.accepts('application/json');
    // => "application/json"

    // Accept: text/*, application/json
    this.accepts('image/png');
    this.accepts('png');
    // => undefined

    // Accept: text/*;q=.5, application/json
    this.accepts(['html', 'json']);
    this.accepts('html', 'json');
    // => "json"
    ```
    注意，当请求头中不包含 Accept 属性时，给定的第一个 type 将会被返回。

- ctx.acceptsEncodings(encodings)
    判断客户端是否接受给定的编码方式的快捷方法，当有传入参数时，返回最应当返回的一种编码方式。
    ``` javascript
    // Accept-Encoding: gzip
    this.acceptsEncodings('gzip', 'deflate');
    // => "gzip"

    this.acceptsEncodings(['gzip', 'deflate']);
    // => "gzip"
    ```
    当没有传入参数时，返回客户端的请求数组：
    ``` javascript
    // Accept-Encoding: gzip, deflate
    this.acceptsEncodings();
    // => ["gzip", "deflate"]
    ```

- ctx.acceptsCharsets(charsets)
    使用方法同 req.acceptsEncodings(encodings)

- ctx.acceptsLanguages(langs)
    使用方法同 req.acceptsEncodings(encodings)

- ctx.get()


## 上下文对象中的其他 API

- ctx.req: Node.js 中的 request 对象
- ctx.res: Node.js 中的 response 对象，方法有:
    - res.statusCode
    - res.writeHead()
    - res.write()
    - res.end()
- ctx.app: app 实例
- ctx.state: 推荐的命名空间，用来保存那些通过中间件传递给视图的参数或数据。比如 this.state.user = yield User.find(id);
- ctx.cookies.get(name, [options]) 对于给定的 name ，返回响应的 cookie
    - options
        - signed [boolean]
- ctx.cookies.set(name, value, [options]) 对于给定的参数，设置一个新 cookie
    - options
        - signed [boolean]
        - expires [date]
        - path [string] 默认为 '/'
        - domain [string]
        - secure [boolean]
        - httpOnly [boolean] 默认为 true
- ctx.throw(msg, [status]) 抛出常规错误的辅助方法，默认 status 为 500。


### status可用状态

- 100 "continue"
- 101 "switching protocols"
- 102 "processing"
- 200 "ok"
- 201 "created"
- 202 "accepted"
- 203 "non-authoritative information"
- 204 "no content"
- 205 "reset content"
- 206 "partial content"
- 207 "multi-status"
- 300 "multiple choices"
- 301 "moved permanently"
- 302 "moved temporarily"
- 303 "see other"
- 304 "not modified"
- 305 "use proxy"
- 307 "temporary redirect"
- 400 "bad request"
- 401 "unauthorized"
- 402 "payment required"
- 403 "forbidden"
- 404 "not found"
- 405 "method not allowed"
- 406 "not acceptable"
- 407 "proxy authentication required"
- 408 "request time-out"
- 409 "conflict"
- 410 "gone"
- 411 "length required"
- 412 "precondition failed"
- 413 "request entity too large"
- 414 "request-uri too large"
- 415 "unsupported media type"
- 416 "requested range not satisfiable"
- 417 "expectation failed"
- 418 "i'm a teapot"
- 422 "unprocessable entity"
- 423 "locked"
- 424 "failed dependency"
- 425 "unordered collection"
- 426 "upgrade required"
- 428 "precondition required"
- 429 "too many requests"
- 431 "request header fields too large"
- 500 "internal server error"
- 501 "not implemented"
- 502 "bad gateway"
- 503 "service unavailable"
- 504 "gateway time-out"
- 505 "http version not supported"
- 506 "variant also negotiates"
- 507 "insufficient storage"
- 509 "bandwidth limit exceeded"
- 510 "not extended"
- 511 "network authentication required"


>原文链接：[Context](https://github.com/guo-yu/koa-guide#%E5%BA%94%E7%94%A8%E4%B8%8A%E4%B8%8B%E6%96%87context)
