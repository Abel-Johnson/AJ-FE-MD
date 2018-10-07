# [学习笔记] Less使用学习
## Variables
Less可以让我们像标准的编程语言一样去创建变量，这样我们就可以再同一个地方存储我们常用的属性值，如颜色，尺寸，选择器，字体，URL等等。如下所示

### Less:

```less
@background-color: #eee;
@text-color: #333;

p{
  background-color: @background-color;
  color: @text-color;
  padding: 15px;
}
```

### CSS:

```css
p{
  background-color: #eee;
  color: #333;
  padding: 15px;
}
```

see more

## Mixins
Less可以让我们实现先有class或id的样式直接应用于别的选择器中，主要有两种用法

### Less:

```less
#circle{
  background-color: #4CAF50;
  border-radius: 100%;
}
#small-circle{
  width: 50px;
  height: 50px;
  #circle
}
```

### CSS:

```css
#circle {
  background-color: #4CAF50;
  border-radius: 100%;
}
#small-circle {
  width: 50px;
  height: 50px;
  background-color: #4CAF50;
  border-radius: 100%;
}
```

如果我们不想让被选择的样式单独出现，则如下

### Less:

```less
#circle(){
  background-color: #4CAF50;
  border-radius: 100%;
}
#small-circle{
  width: 50px;
  height: 50px;
  #circle
}
```

### CSS:

```css
#small-circle {
  width: 50px;
  height: 50px;
  background-color: #4CAF50;
  border-radius: 100%;
}
```

注意到区别了吗？前者是#circle,后者是#circle()

see more


## Nesting and Scope
这部分应该算是用得最多的了。对各种选择器进行嵌套使用

### Less:

```less
ul{
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;

  li{
    background-color: #fff;
    border-radius: 3px;
    margin: 10px 0;
  }
}
```

### CSS:

```css
ul {
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;
}
ul li {
  background-color: #fff;
  border-radius: 3px;
  margin: 10px 0;
}
```

see more

还有其变量在其使用的选择器域内声明别的值，会影响其嵌套下的选择器的变量的值

### Less:

```less
@text-color: #000000;

ul{
  @text-color: #fff;
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;

  li{
    color: @text-color;
    border-radius: 3px;
    margin: 10px 0;
  }
}
```


### CSS:

```css
ul {
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;
}
ul li {
  color: #fff;
  border-radius: 3px;
  margin: 10px 0;
}
```

see more

## Operations
我们也可以使用数学计算去做一些数值或颜色的计算，看下面的例子

### Less:

```less
@div-width: 100px;
@color: #03A9F4;

#left{
  width: @div-width;
  background-color: @color - 100;
}

#right{
  width: @div-width * 2;
  background-color: @color;
}
```


### CSS:


```css
#left {
  width: 100px;
  background-color: #004590;
}
#right {
  width: 200px;
  background-color: #03a9f4;
}
```

## Functions
Less也有自己的函数，例如获取图片大小，单位转换，数组等等，例如

通过image-width、image-height和image-size，我们可以直接获取图片的尺寸大小

### Less:

```less
.img{
  width:image-width("../img/less-logo.png");
  height:image-height("../img/less-logo.png");
  margin:image-size("../img/less-logo.png");
}
```

### CSS:

```css
.img {
  width: 300px;
  height: 100px;
  margin: 300px 100px;
}
```

数组也蛮常用的，变量定义了好一串我们要用的值，可以通过其位置直接拿到值，这里得注意的是Less的数组是从1开始的

### Less:

```less
@list: red, blue, green, yellow;
.list{
  color:extract(@list, 1);
  border-color:extract(@list, 2);
  background-volor:extract(@list, 3);
}
```

### CSS:

```css
.list {
  color: red;
  border-color: blue;
  background-volor: green;
}
```

see more

Others
& 用于在嵌套写法时，直接继承上一个选择器名，例如

### Less:

```less
.a{
  &:hover{
    color:red;
  }
}
.p, .a, .ul, .li {
  border-top: 2px dotted #366;
  && {
    border-top: 0;
  }
  & + & {
    border-top: 0;
  }
}
```

### CSS:

```css
.a:hover{
    color:red;
}
.p, .a, .ul, .li {
  border-top: 2px dotted #366;
}
.p.p, .p.a, .p.ul, .p.li,
.a.p, .a.a, .a.ul, .a.li,
.ul.p, .ul.a, .ul.ul, .ul.li,
.li.p, .li.a, .li.ul, .li.li {
  border-top: 0;
}
.p + .p, .p + .a, .p + .ul, .p + .li,
.a + .p, .a + .a, .a + .ul, .a + .li,
.ul + .p, .ul + .a, .ul + .ul, .ul + .li,
.li + .p, .li + .a, .li + .ul, .li + .li {
  border-top: 0;
}
```

import让我们直接引入别的less文件，最后跟着一起编译为css

`other.less`:

```less
@background-color: #eee;
@text-color: #333;

p{
  background-color: @background-color;
  color: @text-color;
  padding: 15px;
}
```

### Less:

```less
@import "other.less";
```

### CSS:

```css
p{
  background-color: #eee;
  color: #333;
  padding: 15px;
}
```