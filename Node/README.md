> Node

Node.js发布于2009年5月，由Ryan Dahl开发，是一个基于Chrome V8引擎的JavaScript运行环境，使用了一个事件驱动、非阻塞式I/O模型， 让JavaScript 运行在服务端的开发平台，它让JavaScript成为与PHP、Python、Perl、Ruby等服务端语言平起平坐的脚本语言。

Node.js对一些特殊用例进行优化，提供替代的API，使得V8在非浏览器环境下运行得更好，V8引擎执行Javascript的速度非常快，性能非常好，基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用。

## npm install 报错   unable to resolve dependency tree
npm install 有时会报错  unable to resolve dependency tree

原因是node版本太高  npm版本太高

npm install 可以执行以下命令解决
```bash
npm install --legacy-peer-deps
```

> Date转字符串
js获取当前时间转为24小时
```js
new Date().toLocaleString('chinese',{hour12:false}) //2022/10/30 16:25:13   
```


## 多层目录创建
```js
function mkdirsSync(dirname){
    if (fs.existsSync(dirname)) {  
        return true;  
    } else {  
        if (mkdirsSync(path.dirname(dirname))) {  
            fs.mkdirSync(dirname);  
            return true;  
        }  
    }
}
```

## 删除文件夹
```js
function mkdirsSync(dirname){
    if (fs.existsSync(dirname)) {  
        return true;  
    } else {  
        if (mkdirsSync(path.dirname(dirname))) {  
            fs.mkdirSync(dirname);  
            return true;  
        }  
    }
}
```

## node实现文件下载
```js
const fs = require('fs'),
path=require('path'),
https=require('https'),
http=require('http');

function downLoadFile(url, name){
    const filename = path.join(__dirname,name)  //组装文件存放地址
    const file = fs.createWriteStream(filename)   //生成一个写入文件的流
    let httpType
    if (url.split('://')[0] === 'http') {   //判断是什么类型的请求
        httpType = http
    } else {
        httpType = https
    }
    httpType.get(url, response => {
        response.pipe(file)    //将文件流写入
        response.on('end', () => {
            console.log(filename)
        })
        response.on('error', err => {
            console.log(err)
        })
        response.on('data',(data)=>{
            console.log(data.toString());
        })
    })
}
```
## Node执行Exec返回数据乱码解决
```js
cp.exec(command,{cwd:path.join(__dirname,'./'), encoding: 'buffer'},(error, stdout, stderr) => {
	const iconv = require('iconv-lite');
	console.log(`stdout: ${iconv.decode(stdout, 'cp936')}`);
	console.log(`stderr: ${iconv.decode(stderr, 'cp936')}`);
			
})
```

## 反向代理
```js
// 导入http模块
var http = require('http');
// 导入http-proxy模块
var httpProxy = require('http-proxy');

// 提供服务的端口号
var PORT = process.argv[3]||80;//如果没有传参默认本地端口为80
// 代理的地址
var target = process.argv[2];//代理的目标地址
  // 创建反向代理服务
 var proxy = httpProxy.createProxyServer();
 // 监听错误事件
 proxy.on('error', function (err, req, res) {
     // 输出空白响应数据
     res.write('error!');
     res.end();
 });
 
 // 创建服务
 var app = http.createServer(function (req, res) {
	 //设置允许跨域的域名，*代表允许任意域名跨域
	 res.setHeader('Access-Control-Allow-Origin','*');
	 //允许的header类型
	 res.setHeader("Access-Control-Allow-Headers","content-type");
	 //跨域允许的请求方式 
	 res.setHeader("Access-Control-Allow-Methods","DELETE,PUT,POST,GET,OPTIONS");
     // 执行反向代理
     proxy.web(req, res, {
         // 目标地址
         target,
		 changeOrigin: true,
		 ws: true,
		 secure:false
     });
 });
 
 // 启动服务
 app.listen(PORT, function () {
     console.log('server is running at %d', PORT);
 });
```

## socks5代理
```bash
npm i node-socks5-server
```

```js
const socks5 = require('node-socks5-server');

const server = socks5.createServer();

server.listen(1080);
```


Test with curl
```bash
curl http://www.baidu.com/ --socks5 localhost:1080
curl http://www.baidu.com/ --socks5-hostname localhost:1080
curl http://www.baidu.com/ --socks5 user:password@localhost:1080
```
## 异步控制并发数
```js
const https = require('https');
limitRequest([
    'https://www.baidu.com',
    'https://www.qq.com',
    'https://underscorejs.net/',
    'https://zh.javascript.info/',
    'https://jquery.cuishifeng.cn/'
],2)
function limitRequest(urls=[], limit = 3){
    return new Promise((resolve,reject)=>{
        const len = urls.length;
        
        let count = 0;
        
        //同时启动limit个任务
        while(limit > 0){
            start()
            limit-=1
        }

        function start(){
            const url = urls.shift() //从数组中拿取第一个任务
            if(url){
                https.get(url,()=>{
                    //延迟2s执行,假设异步2s结束
                    setTimeout(()=>{
                        if(count == len -1){
                            resolve()
                        }else{
                            count ++ ;
                            start()
                        }
                    },2000)
                })
            }
        }
    })
}
```
## 判断路径是否为绝对路径
```js
path.isAbsolute(url) 判断路径是否为绝对路径
```
## Node发送邮件
```js
const nodemailer = require('nodemailer');

let transporter = nodemailer.createTransport({
  // host: 'smtp.ethereal.email',
  service: 'qq', // 使用了内置传输发送邮件 查看支持列表：https://nodemailer.com/smtp/well-known/
  port: 465, // SMTP 端口
  secureConnection: true, // 使用了 SSL
  auth: {
    user: 'xxxxxx@qq.com',//你的邮箱
    // 这里密码不是qq密码，是你设置的smtp授权码
    pass: 'qq的授权码',
  }
});

let mailOptions = {
  from: '"火星黑洞" <1418275530@qq.com>', // sender address
  to: '156093340@qq.com', // list of receivers
  subject: 'Hello', // Subject line
  // 发送text或者html格式
  // text: 'Hello 我是火星黑洞', // plain text body
  html: '<b>Hello 我是火星黑洞</b>' // html body
};

// send mail with defined transport object
transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    return console.log(error);
  }
  console.log('Message sent: %s', info.messageId);
  // Message sent: <04ec7731-cc68-1ef6-303c-61b0f796b78f@qq.com>
});
```
