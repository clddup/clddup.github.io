> Vue

Vue.js是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue采用自底向上增量开发的设计。Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合。另一方面，Vue 完全有能力驱动采用单文件组件和Vue生态系统支持的库开发的复杂单页应用。

Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。

Vue.js 自身不是一个全能框架——它只聚焦于视图层。因此它非常容易学习，非常容易与其它库或已有项目整合。另一方面，在与相关工具和支持库一起使用时 ，Vue.js 也能驱动复杂的单页应用。

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

