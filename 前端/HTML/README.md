> HTML

HTML的全称为超文本标记语言，是一种标记语言。它包括一系列标签．通过这些标签可以将网络上的文档格式统一，使分散的Internet资源连接为一个逻辑整体。HTML文本是由HTML命令组成的描述性文本，HTML命令可以说明文字，图形、动画、声音、表格、链接等。

超文本是一种组织信息的方式，它通过超级链接方法将文本中的文字、图表与其他信息媒体相关联。这些相互关联的信息媒体可能在同一文本中，也可能是其他文件，或是地理位置相距遥远的某台计算机上的文件。这种组织信息方式将分布在不同位置的信息资源用随机方式进行连接，为人们查找，检索信息提供方便。


## classList

Element.classList 是一个只读属性，返回一个元素 class 属性的动态 DOMTokenList 集合。这可以用于操作 class 集合。

相比将 element.className 作为以空格分隔的字符串来使用，classList 是一种更方便的访问元素的类列表的方法。

尽管 classList 属性自身是只读的，但是你可以使用 add()、remove()、replace() 和 toggle() 方法修改其关联的 DOMTokenList。

*示例*
```js
const div = document.createElement('div');
div.className = 'foo';

// 初始状态：<div class="foo"></div>
console.log(div.outerHTML);

// 使用 classList API 移除、添加类值
div.classList.remove("foo");
div.classList.add("anotherclass");

// <div class="anotherclass"></div>
console.log(div.outerHTML);

// 如果 visible 类值已存在，则移除它，否则添加它
div.classList.toggle("visible");

// add/remove visible, depending on test conditional, i less than 10
div.classList.toggle("visible", i < 10 );

console.log(div.classList.contains("foo"));

// 添加或移除多个类值
div.classList.add("foo", "bar", "baz");
div.classList.remove("foo", "bar", "baz");

// 使用展开语法添加或移除多个类值
const cls = ["foo", "bar"];
div.classList.add(...cls);
div.classList.remove(...cls);

// 将类值 "foo" 替换成 "bar"
div.classList.replace("foo", "bar");
```