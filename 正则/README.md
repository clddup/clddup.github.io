> 正则

正则表达式，又称规则表达式,（Regular Expression，在代码中常简写为regex、regexp或RE），是一种文本模式，包括普通字符（例如，a 到 z 之间的字母）和特殊字符（称为"元字符"），是计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串，通常被用来检索、替换那些符合某个模式（规则）的文本。

许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在Perl中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由Unix中的工具软件（例如sed和grep）普及开来的，后来在广泛运用于Scala 、PHP、C# 、Java、C++ 、Objective-c、Perl 、Swift、VBScript 、Javascript、Ruby 以及Python等等。正则表达式通常缩写成“regex”，单数有regexp、regex，复数有regexps、regexes、regexen。

## 正则表达式截取两个字符串中间的字符串
```js
let reg = /(?<=左侧的内容).*(?=右侧的内容)/
```

## 把一串数字表示成千分位分隔形式
```js
let reg = /(?=(\d{3})+$)/g;
"5464564645".replace(reg,',');
```

## 银行卡只显示后四位，其余显示*
```js
str.replace(/(\w)/g,function (a, b, c, d) { 
	return c <= (str.length - 5) ? '*' : a 
});
```

## 清空img标签中的alt属性
```js
str.replace(/(<img[^>]* )(alt\s*=\s*[\'\"][^\'\"]*[\'\"])([^>]*>)/ig);
```
