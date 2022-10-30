> Babel

Babel 是一个 JavaScript 编译器

Babel 是一个工具链，主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。下面列出的是 Babel 能为你做的事情：

- 语法转换
- 通过 Polyfill 方式在目标环境中添加缺失的特性 
- 源码转换  

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

