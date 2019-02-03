# [学习笔记] web通知

> [参考: 张鑫旭[简单了解HTML5中的Web Notification桌面通知]](https://www.zhangxinxu.com/wordpress/2016/07/know-html5-web-notification/)

### 1. 传统方式, 交替修改document.title
    - 小知识点: 就是浏览器窗体获得焦点和失去焦点，Chrome和FireFox浏览器是window的onfocus, onblur方法；而IE浏览器则是document的onfocusin, onfocusout方法

        ```js
        window.onfocus = function() { };
        window.onblur = function() { };
            
        // for IE
        document.onfocusin = function() { };
        document.onfocusout = function() { };
        ```
    - 缺点: 需要浏览器一直可见

### 2. H5 Web Notification(移动端支持性不好)
核心函数: `window.Notification`

1. `Notification.requestPermission().then(permission=>{...})`请求用户授权
    permission可能的值有: granted, denied, 或default.


2. `Notification.permission`当前授权转态
3. `new Notification(title, options)`
    - options: {
        * body, 
        * tag, 
        * icon, 
        * data, 
        * vibrate: 表示振动和不振动的毫秒数，一直交替下去。例如[200, 100, 200],
        * renotify: 新通知出现的时候是否替换之前的, true参数要想其作用，必须tag需要设置属性值,
        * silent: true(默认静音)
        * sound: '通知音频的地址'
        * noscreen: 默认false表示要显示
        * sticky: 默认false, 表示是否通知具有粘性，这样用户不太容易清除通知   
    }
4.  `Notification.close()`默认5s会关闭
5. 钩子: `Notification.`+`onclick/onerror/onclose/onshow`


#### 修改图标

```html
<link rel="shortcut icon" href="static/刚刚生成.ico的地址" type="image/x-icon"  /><!-- 必须 -->
```

```js
(function() {
    var link = document.querySelector("link[rel*='icon']") || document.createElement('link');
    link.type = 'image/x-icon';
    link.rel = 'shortcut icon';
    link.href = 'http://www.stackoverflow.com/favicon.ico';
    document.getElementsByTagName('head')[0].appendChild(link);
})()
```