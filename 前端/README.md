> 前端

## Babel配置环境变量区分配置

环境变量

在进行bable配置时，可以通过环境变量来为某个环境做特殊的配置，特定环境的设置项会被合并、覆盖到没有特定环境的设置项中。

```js
    env: {
        dev: {
            presets: [
                '@vue/cli-plugin-babel/preset'
            ]
        },
        build: {
            presets: [
                [
                    '@babel/preset-env',
                    {
                        loose: true,
                        modules: false
                    }
                ],
                [
                    '@vue/babel-preset-jsx'
                ]
            ],
        }
    }

```

env 选项的值将从 process.env.BABEL_ENV 获取，如果没有的话，则获取process.env.NODE_ENV 的值，它也无法获取时会设置为 "development"

在命令行中可以传递环境变量

```js
{
    "server": "cross-env BABEL_ENV=dev vue-cli-service serve"
}
```

## vue v-html实现图片懒加载

1.  场景:在使用 v-html 时需要对其中的 img 进行懒加载
2.  实现:   使用插件 VueLazyload 并在main.js中进行简单配置

```js
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload, {
  preLoad: 1.3,
  loading: require("./assets/img/loading-w.gif"),
  attempt: 2
})
```

在需要v-html标签上添加属性

```html
<div v-lazy-container="{ selector: 'img'}" v-html="imgAddLazy(content)"></div>
```

vue 增加methods方法
```js
imgAddLazy(str){
      let te = document.createElement("template");
      te.innerHTML = str;
      Array.from(te.content.querySelectorAll('img')).forEach(item=&gt;{
        item.setAttribute('data-src',item.getAttribute('src'))
      })
      return te.innerHTML;
}
```

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

## foreach改为类似于reduce
```js
Array.prototype.reduceSync = function(callback,data){
	return new Promise(async (resolve,reject)=&gt;{
		const length = this.length;
		let k = 0;
		while (k &lt; length) {
			if (k in this) {
			  const kValue = this[k];
			  await callback(data,kValue, k, this).then(res=&gt;{
				  data = res
			  }).catch(err=&gt;{
				  reject(err)
			  })
			}
			k++;
		}
		resolve(data)
	})
}
```

## foreach改为async await
```js
Array.prototype.forEachSync = async function(callback){
    const length = this.length;
    let k = 0;
    while (k &lt; length) {
        if (k in this) {
          const kValue = this[k];
          await callback(kValue, k, this);
        }
        k++;
    }
}
```

## 判断是否为内网IP
```js
function IsLAN(ip) {
    ip.toLowerCase();
    if(ip=='localhost') return true;
    var a_ip = 0;
    if(ip == "") return false;
    var aNum = ip.split("."); 
    if(aNum.length != 4) return false;
    a_ip += parseInt(aNum[0]) &lt;&lt; 24;
    a_ip += parseInt(aNum[1]) &lt;&lt; 16;
    a_ip += parseInt(aNum[2]) &lt;&lt; 8;
    a_ip += parseInt(aNum[3]) &lt;&lt; 0;
    a_ip=a_ip&gt;&gt;16 &amp; 0xFFFF;
    return( a_ip&gt;&gt;8 == 0x7F || a_ip&gt;&gt;8 == 0xA || a_ip== 0xC0A8 || (a_ip&gt;=0xAC10 &amp;&amp; a_ip&lt;=0xAC1F) );
}
```

## rgba转rgb
```js
function rgbaToRgb(rgb,alpha){
    return `rgb(${computed(rgb[0],alpha)},${computed(rgb[1],alpha)},${computed(rgb[2],alpha)})`;
}
function computed(num,alpha){
    return num * alpha + 255 * (1 - alpha);
}
```

## blob文件下载
```js
function exportRaw(name, data) {
    var urlObject = window.URL || window.webkitURL || window;
    var export_blob = new Blob([data]);
    var save_link = document.createElementNS("http://www.w3.org/1999/xhtml", "a");
    save_link.href = urlObject.createObjectURL(export_blob);
    save_link.download = name;
    fakeClick(save_link);
}
   
function fakeClick(obj) {
    var ev = document.createEvent("MouseEvents");
    ev.initMouseEvent("click", true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
    obj.dispatchEvent(ev);
}
```
## ajax下载文件进度监听
```js
function download(url){
    var page_url = url;
    var req = new XMLHttpRequest();
    req.open("get", page_url, true);
    //监听进度事件
    req.addEventListener("progress", function (evt) {
        // console.log(evt);
        if (evt.lengthComputable) {
            var percentComplete = evt.loaded / evt.total;
            // console.log(percentComplete);
        console.log((percentComplete * 100) + "%");
            // $("#progressing").html((percentComplete * 100) + "%");
        }
    }, false);
    req.responseType = "blob";
    req.onreadystatechange = function () {
        if (req.readyState === 4 && req.status === 200) {
            // var filename = $(that).data('filename');
            var filename = 'downFile';
            if (typeof window.chrome !== 'undefined') {
                // Chrome version
                var link = document.createElement('a');
                link.href = window.URL.createObjectURL(req.response);
                link.download = filename;
                link.click();
            } else if (typeof window.navigator.msSaveBlob !== 'undefined') {
                // IE version
                var blob = new Blob([req.response], { type: 'application/force-download' });
                window.navigator.msSaveBlob(blob, filename);
            } else {
                // Firefox version
                var file = new File([req.response], filename, { type: 'application/force-download' });
                window.open(URL.createObjectURL(file));
            }
        }
    };
    req.send();
}
```

## input获取文件信息
```js
input.oninput=function(event){
    let reader = new FileReader();            
    reader.readAsDataURL(this.files[0]);
    reader.onload = function (e) {
        console.log(this.result);
    }
}
```
readAsBinaryString file 将文件读取为二进制码

readAsDataURL file 将文件读取为 DataURL

readAsText file, [encoding] 将文件读取为文本

readAsText：该方法有两个参数，其中第二个参数是文本的编码方式，默认值为 UTF-8。这个方法非常容易理解，将文件以文本方式读取，读取的结果即是这个文本文件中的内容。

readAsBinaryString：该方法将文件读取为二进制字符串，通常我们将它传送到后端，后端可以通过这段字符串存储文件。

readAsDataURL：这是例子程序中用到的方法，该方法将文件读取为一段以 data: 开头的字符串，这段字符串的实质就是 Data URL，Data URL是一种将小文件直接嵌入文档的方案。这里的小文件通常是指图像与 html 等格式的文件

## 版本号对比
```js
function compareVersion(v1, v2) {
  v1 = v1.split('.')
  v2 = v2.split('.')
  const len = Math.max(v1.length, v2.length)

  while (v1.length &lt; len) {
    v1.push('0')
  }
  while (v2.length &lt; len) {
    v2.push('0')
  }

  for (let i = 0; i &lt; len; i++) {
    const num1 = parseInt(v1[i])
    const num2 = parseInt(v2[i])

    if (num1 &gt; num2) {
      return 1
    } else if (num1 &lt; num2) {
      return -1
    }
  }

  return 0
}
```

## 通过堆栈获取函数调用者的路径
```js
function _getCalerFile() {//通过堆栈获取函数调用者的路径
    try {
        var err = new Error();
        var callerfile;
        var currentfile;

        Error.prepareStackTrace = function (err, stack) { return stack; };

        currentfile = err.stack.shift().getFileName();

        while (err.stack.length) {
            callerfile = err.stack.shift().getFileName();

            if(currentfile !== callerfile) return callerfile;
        }
    } catch (err) {}
    return undefined;
 }
```

## 元素滚动条不占空间
```css
*{
   overflow : overlay;
}
```

## css修改图片颜色
```css
img{
    position: relative;
    left: 600px;
    filter: drop-shadow(var(--main-color) -600px 0);
}
```

## css颜色16进制末尾追加两位实现透明度

我们一般表示带有透明度的颜色一般用 RGBA 来表示, 但有时会遇到用16进制颜色直接表达带有透明度的颜色, 具体如下:

```text
100% — FF
99% — FC
98% — FA
97% — F7
96% — F5
95% — F2
94% — F0
93% — ED
92% — EB
91% — E8
90% — E6
89% — E3
88% — E0
87% — DE
86% — DB
85% — D9
84% — D6
83% — D4
82% — D1
81% — CF
80% — CC
79% — C9
78% — C7
77% — C4
76% — C2
75% — BF
74% — BD
73% — BA
72% — B8
71% — B5
70% — B3
69% — B0
68% — AD
67% — AB
66% — A8
65% — A6
64% — A3
63% — A1
62% — 9E
61% — 9C
60% — 99
59% — 96
58% — 94
57% — 91
56% — 8F
55% — 8C
54% — 8A
53% — 87
52% — 85
51% — 82
50% — 80
49% — 7D
48% — 7A
47% — 78
46% — 75
45% — 73
44% — 70
43% — 6E
42% — 6B
41% — 69
40% — 66
39% — 63
38% — 61
37% — 5E
36% — 5C
35% — 59
34% — 57
33% — 54
32% — 52
31% — 4F
30% — 4D
29% — 4A
28% — 47
27% — 45
26% — 42
25% — 40
24% — 3D
23% — 3B
22% — 38
21% — 36
20% — 33
19% — 30
18% — 2E
17% — 2B
16% — 29
15% — 26
14% — 24
13% — 21
12% — 1F
11% — 1C
10% — 1A
9% — 17
8% — 14
7% — 12
6% — 0F
5% — 0D
4% — 0A
3% — 08
2% — 05
1% — 03
0% — 00
```
## 计算中间颜色
```js
var parseColor = function (hexStr) {
    return hexStr.length === 4 ? hexStr.substr(1).split('').map(function (s) { return 0x11 * parseInt(s, 16); }) : [hexStr.substr(1, 2), hexStr.substr(3, 2), hexStr.substr(5, 2)].map(function (s) { return parseInt(s, 16); })
};

var pad = function (s) {
    return (s.length === 1) ? '0' + s : s;
};

/*
    start 开始颜色
    end 结束颜色
    steps 颜色分解 次数
    gamma 暂时理解为透明一点（伽马）
*/
var gradientColors = function (start, end, steps, gamma) {
    var i, j, ms, me, output = [], so = [];
    gamma = gamma || 1;
    var normalize = function (channel) {
        return Math.pow(channel / 255, gamma);
    };
    start = parseColor(start).map(normalize);
    end = parseColor(end).map(normalize);
    for (i = 0; i < steps; i++) {
        ms = i / (steps - 1);
        me = 1 - ms;
        for (j = 0; j < 3; j++) {
        so[j] = pad(Math.round(Math.pow(start[j] * me + end[j] * ms, 1 / gamma) * 255).toString(16));
        }
        output.push('#' + so.join(''));
    }
    return output;
};

gradientColors('#00000','#fffff',10) //['#000000', '#1c1c02', '#393903', '#555505', '#717107', '#8e8e08', '#aaaa0a', '#c6c60c', '#e3e30d', '#ffff0f']
```

## 数组排序
```js
function sort(arr){
    if(arr.length<=1){
        return arr;
    }
    for(let i=0;i<arr.length;i++){
        for(let j = i+1;j<arr.length;j++){
            if(arr[i]>arr[j]){
                arr[i] ^= arr[j];
                arr[j] ^= arr[i];
                arr[i] ^= arr[j];
            }
        }
    }
    return arr;
}
```

## 根据出现的次数进行排序
```js
let arr = [1,2,3,4,5,6,1,2,5,3,6,9,8,7,4,5,2,3,5,9,5,4,1,32,3,6,5,4,74];
console.log(Object.entries(arr.reduce((total,prev)=>{
    if(total[prev]){
        total[prev]++;
    }else{
        total[prev] = 1;
    }
    return total;
},{})).sort((n1,n2)=>n2[1]-n1[1]));
```

## 给对象设置只读属性
```js
Object.defineProperty(this, 'a', {
	enumerable: false,enumerable 描述属性是否会出现在for in 或者 Object.keys()的遍历中
	configurable: false,//描述属性是否配置，以及可否删除
	writable: false,// 是否可以改变
	value: x 
});
```

## 设置getter属性
```js
Object.defineProperty(window,'goby',{
    get(){
        return 1;
    },
    set(){
        return false;
    },
    configurable:false
})
```

## 获取iframe全局window对象
```js
获取iframe全局window对象
```
## js动态引入js文件与css 并且不带缓存
```js
document.write(`<script src="./index.js?t=${new Date().getTime()}"></\script>`)
```

## js随机生成颜色
```js
'#' + Math.random().toString(16).substr(-6);
```
## base64编码 解码
```
编码    btoa
解码    atob
```
## 迭代器
### Generator实现
```js
function * obj(_obj){
    let values = Object.values(_obj);
    for(let i in values){
        yield values[i];
    }
}

let __Obj = obj({
    a:'哈哈',
    b:'呵呵',
    c:'哈哈呵呵'
})

for(let i of __Obj){
    console.log(i);
}
```

### Class + Generator实现
```js
class _obj {
    constructor(obj){
        for(let i in obj){
            this[i] = obj[i];
        }
    }
    * [Symbol.iterator](){
        for(let i of Object.values(this)){
            yield i;
        }
    }
    
}
    
let obj = new _obj({
    a:"哈哈",
    b:"呵呵",
    c:"哈哈呵呵"
})

for(let i of obj){
    console.log(i);
}
```

### Class + next实现
```js
class _obj {
    constructor(obj){
        for(let i in obj){
            this[i] = obj[i];
        }
    }
    [Symbol.iterator](){
        let index = 0;
        let values = Object.values(this);
        return {
            next(){
                return {
                    value:values[index++],
                    done:index>values.length
                }
            }
        }
    }
    
}
    
let obj = new _obj({
    a:"哈哈",
    b:"呵呵",
    c:"哈哈呵呵"
})

for(let i of obj){
    console.log(i);
}
```

## js大数据计算
```bash
npm i node-bignum
```
## 防抖
```js
function debounce(fn,delay){
    let timer
    return function (...args){
        if(timer){
            clearTimeout(timer)
        }
        timer = setTimeout(()=>{
            fn.apply(this,args)
        },delay)
        
    }
}
//测试
function task(){
    console.log('run task');
}

const debounceTask = debounce(task,500)
window.addEventListener('scroll',debounceTask)
```

## 节流
```js
function throttle(fn,delay){
    let last = 0;
    return function(...args){
        const now = Date.now()
        if(now - last > delay){
            last = now
            fn.apply(this,args)
        }
    }
}

//测试

function task(){
    console.log('run task')
}

const throttleTask = throttle(task,1000)

window.addEventListener('scroll',throttleTask)
```
