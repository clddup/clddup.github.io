> 正则
# 正则表达式截取两个字符串中间的字符串
```js
let reg = /(?<=左侧的内容).*(?=右侧的内容)/
```

# 把一串数字表示成千分位分隔形式
```js
let reg = /(?=(\d{3})+$)/g;
"5464564645".replace(reg,',');
```

# 银行卡只显示后四位，其余显示*
```js
str.replace(/(\w)/g,function (a, b, c, d) { 
	return c <= (str.length - 5) ? '*' : a 
});
```

# 清空img标签中的alt属性
```js
str.replace(/(<img[^>]* )(alt\s*=\s*[\'\"][^\'\"]*[\'\"])([^>]*>)/ig);
```
