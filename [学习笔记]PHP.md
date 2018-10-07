```php
<?php
echo 'hello world'
?>
```

# PHP

- php hypertext preprocessor

## 作用
1. 生成动态页面
2. 创建/打开/读取/写入/删除/关闭--服务器上的文件
3. 接收表单数据
4. 发送/取回cookie
5. 增删改查 数据库
6. 限制用户访问网站中某些页面
7. 对数据进行加密

## 变量
### 变量命名规则:
`$+变量名称`
变量名称以字母/下划线开头, 只能包含数字字母下划线

### 变量作用域
1. LOCAL
2. GLOBAL
3. STATIC

想要在局部访问全局变量, 两种方法:

1. `global关键词 +$变量名称`, 声明一下这个全局变量
2. 使用全局变量GLOBAL: $GLOBALS['变量名称']可以直接取用

static关键词:
介于全局变量和局部变量之间的一种, 在函数内部使用这个关键词声明的变量不会随函数的执行完毕而自动销毁,而是会一直存在,但同时依然是一个局部变量


## 打印信息
### echo VS print

#### echo
- 输出一个以上的字符串(逗号隔开)
- echo(xx) // echo xx

#### print
- 只能输出一个字符串,并始终返回1
- print(xx) // print xx


```php
<?php
$txt1="Learn PHP";
$txt2="W3School.com.cn";
$cars=array("Volvo","BMW","SAAB");

echo $txt1;
echo "<br>";
echo "Study PHP at $txt2";
echo "My car is a {$cars[0]}";

print $txt1;
print "<br>";
print "Study PHP at $txt2";
print "My car is a {$cars[0]}";

?>
```

## 数据类型

字符串/整型/浮点型/逻辑(布尔)/数组/对象/NULL

- `var_dump($变量名称)` --- 返回变量的**数据类型**和**值**, 如`var_dump(123);输出int(123)`

声明数组: $arr = array('a1', 'a2', 'a3')
声明对象: 

```php
<?php
class Car 
{
	var $color;
	function Car($color="green") {
		$this->color = $color;
	}
	function what_color() {
		return $this->color
	}
}
?>
```

### 字符串
strlen('abc') ===>>  3
strpos('I am xx', 'xx') ===>> 5 || false(没有的话)


## 常量
- 不用$开头, 
- 字符或下划线开头, 
- 自动全局

### 声明
define(名称, 值, [大小写是否不敏感, 默认false(敏感)])


## 运算符

### 字符串运算符: 

运算符|示例|结果
---|---|---
.|$txt1 = 'HELLO'; $txt2 = $txt1 . ' WORLD?'|$txt2==>>'HELLO WORLD?'
.=|$txt1 = 'HELLO'; $txt1 .= ' WORLD?'|$txt1==>>'HELLO WORLD?'



### 比较运算符(用于数字和字符串)

<> 和 != 是等价的

### 逻辑运算符
符号|含义
---|---
and / && |
or / || |
xor(异或) |有且仅有一个为true, 返回true 
!|

### 数组运算符

符号|含义
---|---
+|联合,但不覆盖重复的键 
|
==|相等, 有完全相同的键, 就返回true
!=/<>|不等
|
===|全等, 顺序和内容完全相等

## 循环

foreach($colors as $key=>$value) {
echo $key, $value
}

## 函数

### 命名: 字母,下划线开头
### 参数: $+变量名称
### 默认值: `function fnName($param='defaultValue') {}`

## 数组

两种创建方法:

1. $arr = array('a','b','c')
2. $arr[0] = 'a'

count(arrNM)----返回数组的长度

### 关联数组(类似js对象)
两种创建方法:

1. $age=array('peter'=>'35', 'ben'=>'37', 'joe'=>'43')
2. $age['peter']='35'

### foreach遍历

foreach($colors as $key=>$value) {
echo $key, $value
}

### 排序
方法|含义
---|---
sort()|升序
rsort()|降序
|
asort()|关联数组,根据值升序排列
ksort()|关联数组,根据键升序排列
arsort()|关联数组,根据值降序排列
krsort()|关联数组,根据键降序排列



## 超全局变量

内置的,无需使用global $xxx声明就可以直接使用的

包括

名称|注
---|---
$GLOBALS|关联数组,存了所有全局变量
$_SERVER|
$_REQUEST|
$_POST|
$_GET|
$_FILES|
$_ENV|
$_COOKIE|
$_SESSION


## 表单处理

`$_GET` & `$_POST` 用来收集表单数据(form-data)

```html
<html DOCTYPE>
	<body>
		<form action="1.php" method="post">
			Name: <input type="" name="name"><br>
			E-mail: <input type="" name="email"><br>
			<input type="submit">
		<form>
	</body>
</html>
```
`$_POST/$_GET['key']`可以取到填写的表单数据

### 特殊的
`<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">`

$_SERVER['PHP_SELF']==--->>>>==返回当前脚本的文件名,可以被黑客利用, XSS
htmlspecialchars()==--->>>>==安全函数,会把标签之类的符号转成html实体,如`<`转成`&lt;`,防止xss


自己简单重现了一下xss攻击, 火狐先关掉xss过滤: about:config=>urlbar.filter=>双击关闭


预防措施:
1. htmlspecialchars()---转义
2. trim()---去空字符
3. stripslashes()---去反斜杠

```php
function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
```
### 表单校验

empty()校验必填项或在正式校验前过滤一层