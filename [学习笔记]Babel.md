# [Babel](https://www.babeljs.cn/)

> 入门:
> 
> 1. 安装 Babel CLI 和 env preset
> 
>     `npm install --save-dev babel-cli babel-preset-env`
> 
> 2. 创建 .babelrc 文件（或使用 package.json）
>     
>     ```json
>     {
>       "presets": ["env"]
>     }
>     ```
--------

1. perset 预设

    1. `npm install --save-dev babel-preset-env`
    2. 并添加 "env" 到你的 .babelrc 文件的 presets 数组中

2. babel只装换语法(如箭头函数),使用`babel-polyfill`壮大他的功能(例如promise)
    1. `npm install --save-dev babel-polyfill`
    2. 使用它时需要在你应用程序的入口点顶部或打包配置中引入。(transform-runtime)

    
3. 转换 JSX 语法并去除类型注释, 使用`react preset`
    1. `npm install --save-dev babel-preset-react`
    2. 并添加 "react" 到你的 .babelrc 的 presets 数组中。

    