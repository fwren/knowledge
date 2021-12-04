## 创建 web 服务器

```javascript
// 引用系统模块
const http = require('http');

// 创建 web 服务器
const app = http.createServer();

// 当客户端发送请求时
app.on('request', (req, res) => {
    // 响应
    res.end('<h1>hi, user</h1>');
});

// 监听3000端口
app.listen(3000);
console.log('服务器已启动，监听3000端口，请访问localhost:3000');
```

---

## HTTP 协议

超文本传输协议（HyperText Transfer Protocol）规定了如何从网站服务器传输超文本到本地浏览器，它是基于客户端服务器架构工作，是客户端（用户）和服务器端（网站）请求和应答的标准

- 请求报文

    请求方式：

        GET：请求数据
        POST：发送数据

        req.method：请求方式
        req.url：请求地址
        req.headers：请求报文

- 响应报文

    HTTP 状态码

        200 请求成功
        404 请求的资源没有找到
        500 服务器端错误
        400 客户端请求有错误

        res.writeHead(状态码)

    内容类型

        text/plain
        text/html
        text/css
        application/javascript
        image/jpeg
        application/json

        res.writeHead(状态码, {
            'content-type': '返回内容类型;charset(编码类型)'
        })

---
## HTTP 请求和响应处理

- 请求参数

    - GET 请求参数

        - 参数被放在浏览器地址栏中

            http://localhost:3000?name&age

            req.url 获取请求参数

            url 模块用于处理 url 地址

            url.parse(参数1：要处理的url，参数2：是否将查询参数解析成对象形式)

    - POST 请求参数

        - 参数被放置在请求体中进行传输

        - 获取 POST 参数需要使用 data 事件和 end 事件

        通过事件的方式接受的
    
        当请求参数传递的时候触发 data 事件，参数传递完成的时候触发 end 事件

        data 事件：

            req.on('data', param => {
                postParams += params;
            })

            param 表示正在传递的值

        querystring 模块将字符串处理成对象格式

            querystring.parse(postParams)

- 静态资源

    服务器端不需要处理，可以直接响应给客户端的资源就是静态资源，例如 CSS、JavaScript、image 文件

    ```javascript
    const http = require('http');   // http 模块
    const fs = require('fs');   // 文件模块
    const path = require('path');   // 路径模块
    const url = require('url');     // url 模块
    const mime = require('mime');   // mime 模块，用于获取请求资源类型

    const app = http.createServer();    // 创建服务器
    app.on('request', (req, res) => {
        // 获取用户请求路径
        let pathName = url.parse(req.url).pathname;
        // 默认请求路径返回主页面
        pathName == '/' ? pathName = '/index.html' : pathName;
        // 读取文件在服务器的硬盘路径
        let readPath = path.join(__dirname, 'public' + pathName);
        // 判断请求资源类型
        let type = mime.getType(readPath);
        // 读取文件并将结果返回客户端
        fs.readFile(readPath, (error, result) => {
            // 错误处理
            if(error) {

            };
            // 成功返回
            res.writeHead(200, {
                'content-type': type
            });
            res.end(result);
        });
    });

    app.listen(3000);   // 服务器监听端口
    console.log('服务器启动成功');
    ```

- 动态资源

    相同的请求地址不同的响应资源

    例如：

        http://localhost:3000?id=1

        http://localhost:3000?id=2

- 路由

    路由是值客户端请求地址与服务器端程序代码的对应关系，简单的说，就是请求什么响应什么

        1.引入系统模块 http
        2.创建网站服务器
        3.为网站服务器对象添加请求事件
        4.实现路由功能
            - 获取客户端的请求方式
            - 获取客户端的请求地址

    ```javascript
    // 当客户端发送请求时
    app.on('request', (req, res) => {
        // 获取请求方式
        let method = req.method.toLowerCase();
        // 获取请求地址
        let pathName = url.parse(req.url).pathname;
        
        // 不同请求方式
        if(method == 'get') {
            if(pathName == '/' || pathName == '/index') {
                res.end('欢迎来到首页');
            };
            else if(pathName == '/list') {
                res.end('欢迎来到列表页');
            };
            else {
                res.end('访问页面出错');
            };
        };
        else if(method == 'post') {
            
        }
    });
    ```