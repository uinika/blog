---
title: SCSS快速上手
tags: CSS
categories: Web
---

SCSS是**Sass3**当中引入的新语法，语法完全兼容CSS3的同时继承了**Sass**的强大功能。本文翻译自[Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)，讲解了前端开发常用的SCSS特性。

![](scss/logo.png)

<!-- more -->

## 编码规则

SASS以**UTF-8**编码输出CSS文件，当且仅当编译后的文件包含**非ASCII**字符时，才会输出`@charset`声明，并使用该声明上指定的字符集。

```scss
@charset "utf-8";
#app {
  background: url('../assets/背景图片.png');
}
```

## CSS扩展

### 嵌套规则

SASS允许CSS规则嵌套使用，父子规则将会呈现**包含选择器**关系，这样可以避免重复使用父级选择器，简化CSS代码结构。

```scss
// SASS
#app {
  width: 100%;
  height: 100%;
  main.login {
    display: flex;
  }
}

// CSS
#app {
  width: 100%;
  height: 100%;
}

#app main.login {
  display: flex;
}
```

### 嵌套属性

CSS许多属性都位于相同的命名空间（*例如`font-family`、`font-size`、`font-weight`都位于`font`命名空间下*），SASS当中只需要编写命名空间一次，后续嵌套的子属性都将会位于该命名空间之下。

```scss
// SCSS
.wrap {
  font: {  // 命令空间后带有冒号:
    family: Arial;
    weight: bold;
    size: 16px;
  }
}

// CSS
.wrap {
  font-family: Arial;
  font-weight: bold;
  font-size: 16px;
}
```

命令空间上可以直接书写**CSS简写属性**，但是日常开发中建议直接书写**CSS简写属性**，尽量保持CSS语法的一致性。

```scss
.wrap {
  font: 20px/24px Arial {
    weight: bold;
  }
}

.wrap {
  font: 20px/24px Arial;
  font-weight: bold;
}
```

### 引用父级选择器&

SASS使用`$`关键字在CSS规则中引用**父级选择器**，例如嵌套使用伪类选择器的场景下。

```scss
// SASS
a {
  font-weight: bold;
  text-decoration: none;
  &:hover {
    color: blue;
    text-decoration: underline;
  }
}

// CSS
a {
  font-weight: bold;
  text-decoration: none;
  a:hover {
    color: blue;
    text-decoration: underline;
  }
}
```

关键字`&`会使用父级选择器**级联替换**全部其出现的位置，

```scss
// SCSS
#main {
  color: black;
  a {
    font-weight: bold;
    &:hover {
      color: red;
    }
  }
}

// CSS
#main {
  color: black;
}
#main a {
  font-weight: bold;
}
#main a:hover {
  color: red;
}
```

`&`必须出现在组合选择器的开头位置，后面再连接自定义的后缀。

```scss
// SCSS
#app {
  color: gray;
  &-sidebar {
    border: 1px solid;
  }
}

// CSS
#app {
  color: gray;
}
#app-sidebar {
  border: 1px solid;
}
```

> 不存在父级选择器的场景下使用`&`，SASS预处理器会报出错误信息。

### 占位符选择器%

占位符选择器与id或者class选择器的用法类似，只是`#`和`.`被替换成为`%`，占位符选择器必须通过`@extend`指令进行调用。


## 注释

SASS支持标准CSS的多行注释`/* */`和单行注释`// `，其中，多行注释会完整输出到编译后的CSS文件，而单行注释则不会。

```scss
/* it is multiline comments
   it will appear in the CSS output */
body {color: black;}

// it is single-line comment
a {color: green;}
```

编译成为CSS的结果：

```css
/* it is multiline comments
   it will appear in the CSS output */
body {color: black;}

a {color: green;}
```

插值语句`#{$variable}`可以应用在多行注释当中。

```scss
$version: "4.5.3";"
/* This CSS is generated by node-sass version #{$version}. */
```

编译成为CSS的结果：

```css
/* This CSS is generated by node-sass version 4.5.3. */
```

## SassScript

SassScript是SASS提供的语法扩展，允许在任意CSS属性上书写变量、函数和执行计算。SassScript还可以通过`interpolation`插值生成选择器和属性名，这在编写`mixins`时比较有用。

```bash
➜  / sass -i
>> 'hello world'
"hello world"
>> 1px * 3 + 2px
5px
```

### 变量$

SassScript通过`$`关键字声明和使用一个变量。

```scss
$width: 8rem;  // 声明变量

#main {
  width: $width;  // 使用变量
}
```

SassScript变量支持块级作用域，嵌套规则当中声明的变量都是**局部变量**，可以通过`!global`将局部变量声明为**全局变量**。

```scss
#menu {
  $width: 5rem !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```

### 数据类型

SassScript支持6种数据类型，

1. **数值number**：*`1.2`, `13`, `10px`*
2. **颜色color**：*`blue`、`#04a3f9`、`rgba(255, 0, 0, 0.5)`*
3. **布尔值bollean**：*`true`、`false`*
4. **空值null**：*`null`*
5. **字符串string**：有引号或者无引号，*`"foo"`、`'bar'`、`baz`*
6. **数组list**：由空格和逗号分隔，*`1.5em 1em 0 2em, Helvetica, Arial, sans-serif`*
7. **对象map**：*`(key1: value1、key2: value2)`*
8. **函数function**：


### 运算

SassScript所有数据类型都支持`==或!=`逻辑运算，但是每种数据类型还拥有各自的运算方式。

#### 数值运算

数值类型支持常见的数值运算`+`、`-`、`*`、`/`、`%`以及关系运算符`<`、 `>`、`<=`、`>=`，必要时会在不同**绝对**数值单位之间转换。

```scss
p {
  font-size: 1in + 8pt;
}
```

进行除法运算时，需要使用变量但不需要`/`进行除法运算，可以使用`#{}`插值语句包裹变量。

```scss
p {
  font-size: #{$font-size}/#{$line-height};
}
```

#### 颜色运算

颜色值运算是分段进行的，即分别计算**红色**、**绿色**、**蓝色**的值。

```scss
p {
  color: #010203 + #040506;
}

// 进行01+04=05，02+05=07，03+06=09运算后编译为CSS
p {
  color: #050709;
}
```

拥有相同**alpha通道**的颜色值才能直接参与运算，否则会提示错误信息。

```scss
p {
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
}
```

颜色值的**alpha通道**可以使用`opacify`或`transparentize`函数进行计算。

```scss
// SASS
$demo: rgba(255, 0, 0, 0.5);
p {
  color: opacify($demo, 0.3);                     // 增加alpha通道值
  background-color: transparentize($demo, 0.25);  // 减少alpha通道值
}

// CSS
p {
  color: rgba(255, 0, 0, 0.8);
  background-color: rgba(255, 0, 0, 0.25);
}
```

#### 字符串运算 

SassScript使用`+`运算符进行**字符串连接**操作。

```scss
// SCSS
p:before {
  content: "Foo " + Bar;         // 有引号字符串 + 无引号字符串 = 有引号字符串
  font-family: sans- + "serif";  // 无引号字符串 + 有引号字符串 = 无引号字符串
}

// CSS
p:before {
  content: "Foo Bar";
  font-family: sans-serif;
}
```

有引号的文本字符串同样可以使用插值语句`#{}`添加动态值。

```scss
// SCSS
p:before {
  content: "I'm #{24 + 8} years old";
}

// CSS
p:before {
  content: "I'm 32 years old";
}
```




## @rule和#directive