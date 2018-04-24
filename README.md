
[jason-server官网](https://github.com/typicode/json-server)

[mockjs官网](https://github.com/nuysoft/Mock/wiki)

##一、json-server介绍及安装
###1、介绍
   `json-server是一个超级实用的服务器。写少量数据即可使用，而且功能足够强大，支持CORS和JSONP跨域请求，支持GET, POST,
        PUT, PATCH 和 DELETE 方法，更提供了一系列的查询方法，如limit，order等。`
###2、安装
(1） **新建一个项目 mkdir json-server-demo && cd json-server-demo；

(2） **初始化项目，npm init生成package.json文件；

(3） **安装所需模块
```$xslt
npm install json-server --save-dev  //安装json-server服务
npm install nodemon --save-dev  //安装nodemon，修改配置无需重启服务
npm install mockjs --save-dev  //安装批量生成数据

```
###3、在package.json中配置脚本

说明：本文将讲解两种方式；其一是静态数据（db.json文件），其二是动态数据（通过mockjs生成）
```$xslt
"scripts": {
    "server": "cd static && nodemon server.js",
    "dserver": "cd dynamic && nodemon server.js"
  }
```
###4、目录结构
```$xslt


|--dynamic  //动态数据方式
    |--config.js   //配置文件
    |--db.js  //动态数据文件
    |--routes.js  //路由规则
    |--server.js  //服务文件
|--static  //静态数据方式
    |--config.js  //配置文件
    |--db.json   //静态数据文件
    |--server.js  //服务文件
|--node_modules //安装依赖包
package.json  //配置文件

```
##二、使用静态数据
###1）config.js文件内容——配置端口等
```$xslt
module.exports = {
  SERVER:"127.0.0.1",  
  //定义端口号
  PORT: 3003,
  //定义数据文件
  DB_FILE:"db.json"
};
```

###2）db.json——静态数据文件
###3）server.js——服务文件
```$xslt


const path = require('path');
const config = require('./config');
const jsonServer = require('json-server');

const ip = config.SERVER;
const port = config.PORT;
const db_file = config.DB_FILE;

const server = jsonServer.create();
//根据db.json文件自动生成路由规则
const router = jsonServer.router(path.join(__dirname, config.DB_FILE));
//中间件
const middlewares = jsonServer.defaults();

server.use(jsonServer.bodyParser);
server.use(middlewares);
//设置增加一个响应头信息“从server到前端”
server.use((req, res, next) => {
 res.header('X-Hello', 'World');
 next();
})
//数据发送到前端之前包一层
router.render = (req, res) => {
    res.jsonp({
        code: 0,
        body: res.locals.data//res.locals.data这个是真正的数据
    })
}
server.use("/api",router);//模拟api接口，就是访问api的时候给制定路由规则

server.use(router);
server.listen({
    host: ip,
    port: port,
}, function() {
    console.log(JSON.stringify(jsonServer));
    console.log(`JSON Server is running in http://${ip}:${port}`);
});

```
###4）启动服务
```$xslt


npm run server
```


###5）
chrome_plugin
	extension_2_3_2_DHC是一款谷歌浏览器插件，用于可视化请求后台数据