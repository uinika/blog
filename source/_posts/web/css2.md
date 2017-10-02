---
title: CSS2.1典型布局概述
tags: CSS
categories: Web
---

层叠样式表（**CSS**，*Cascading Stylesheet*）用于指定浏览器所渲染的样式（*如颜色、定位、装饰*），距离2011年CSS2.1版本发布至今，已经整整过去**6**个年头，浏览器和前端技术经历了日新月异的变化。虽然目前CSS3.0规范在选择器、弹性盒模型、动画与过渡方面进行了诸多增强，但是[W3C规范](https://www.w3.org/)始终保持着一致的继承性，所以CSS2.1及之前版本规范中提出的大量特性与概念依旧得以沿用。本文参照[Mozilla开发者社区](https://developer.mozilla.org/en-US/docs/Web/CSS)以及[网易NEC](http://nec.netease.com/)前端开源项目，将CSS2.1当中常用的概念与典型布局进行了比较全面的梳理与概括。

![](css2/logo.jpg)

<!-- more -->

## 层叠优先级

浏览器通过层叠样式表的优先级判断哪些CSS属性值与HTML元素最为相关，从而在该HTML元素上应用这些CSS属性值。当HTML元素同时拥有多个CSS声明时，每条CSS规则都会**接管**或者**覆盖**该元素从祖先元素继承而来的规则。

### 定义位置优先级

按照样式**声明与定义位置**的不同，优先级**由高向低**排列如下：

1. 行内样式：`<span style="">`
2. 内嵌样式表：`<style></style>`
3. 引入的外部样式表：`<link href="demo.css" rel="stylesheet" />`

### 选择器优先级

按照所使用**CSS选择器**的不同，优先级**由高向低**排列如下：

1. ID选择器（*例如`#example`*）
2. 类选择器（*例如`.example`*），属性选择器（*例如`[type="radio"]`*），伪类选择器（*例如`:hover`*）
3. 类型选择器（*例如`h1`*）和 伪元素（*例如`::before`*）

> 通用选择器（`*`）、组合选择器（*`+, >, ~, ' '`*）、否定伪类选择器（*`:not()`*）对特异性没有影响（*但`:not()`内声明的选择器会影响优先级*）。

### 样式继承

每个CSS属性值都会指明其是否可以继承，以便在没有手动为HTML元素指定样式值时，该元素默认应该使用哪个样式。

- 可继承CSS属性（*inherited property*）：未指定值时，取父元素同属性的计算值（*computed value*）。
- 非继承CSS属性（*reset property*）：未指定值时，取该属性的初始值（*initial value*）。

> 所有CSS属性都可以使用的`inherit`和`initial`属性值，分别用来让一个元素继承父元素同属性的计算值，以及将被父元素覆盖的属性值重置为该CSS属性的默认值。

### !important

使用`!important`规则的样式声明将会覆盖其它任何声明，如果两条带有`!important`规则的声明被应用到相同HTML元素上，拥有更高优先级的声明将会被采用。

```scss
background: red !important;
```

> 不要在全站、插件当中使用`!important`，尽量只在局部页面需要覆写的情况下使用。


## 盒子模型

每个HTML元素都会被CSS描述为一个矩形的盒子，每个盒子有`margin`、`border`、`padding`、`content`四个框。

![](css2/boxmodel.png)

margin塌陷（*margin collapsing*）：



## 浮动定位


## 相对/绝对定位


## div/span

## 一列布局

### 一列定宽布局

![](css2/layout-1-1.png "一列定宽布局")

```html
<main id="app">
  <div class="column">
    <h2>column</h2>
  </div>
</main>
```

```scss
#app {
  background: $gray;
  .column {
    background: $cyan;
    width: 300px;  // 宽度使用绝对单位
    height: 100%;
  }
}
```

### 一列宽度自适应

![](css2/layout-1-2.png "一列宽度自适应")

```html
<main id="app">
  <div class="column">
    <h2>column</h2>
  </div>
</main>
```
```scss
#app {
  background: $gray;
  .column {
    background: $cyan;
    width: 60%;  // 宽度使用相对单位
    height: 100%;
  }
}
```

### 一列宽度居中

![](css2/layout-1-3.png "一列宽度居中")

```html
<main id="app">
  <div class="column">
    <h2>column</h2>
  </div>
</main>
```
```scss
#app {
  background: $gray;
  .column {
    background: $cyan;
    width: 60%;
    height: 100%;
    margin: 0 auto; // 块元素居中
  }
}
```

## 二列布局

```html
<main id="app">
  <div class="column-left">
    <h1>column-left</h1>
  </div>
  <div class="column-right">
    <h1>column-right</h1>
  </div>
</main>
```

### 二列定宽布局

![](css2/layout-2-1.png "二列定宽布局")

```scss
#app {
  background: $gray;
  .column-left {
    background: $cyan;
    width: 300px;  // 绝对宽度
    height: 100%;
    float: left;
  }
  .column-right {
    background: $pink;
    width: 400px;  // 绝对宽度
    height: 100%;
    float: left;
  }
}
```


### 二列宽度自适应

![](css2/layout-.png)

```scss
#app {
  background: $gray;
  .column-left {
    background: $cyan;
    width: 30%;  // 相对宽度
    height: 100%;
    float: left;
  }
  .column-right {
    background: $pink;
    width: 70%;  // 相对宽度
    height: 100%;
    float: left;
  }
}
```


### 二列右侧宽度自适应

![](css2/layout-.png)

```scss
#app {
  background: $gray;
  .column-left {
    background: $cyan;
    width: 30%; // 左栏设置固定值
    height: 100%;
    float: left;
  }
  .column-right {
    background: $pink;
    height: 100%; 
    // 右栏不设置宽度和浮动
  }
}
```


### 二列左侧宽度自适应

![](css2/layout-.png)

```scss
#app {
  background: $gray;
  .column-left {
    background: $cyan;
    height: 100%;
    float: left;
    width: 100%;
  }
  .column-right {
    background: $pink;
    height: 100%;
    float: right;
    width: 230px;
    margin-left: -230px; // 负边距
  }
}
```

## 三列布局

### 三列中间自适应布局

![](css2/layout-.png)

```html

```

```scss

```

### 三列左侧自适应布局

![](css2/layout-.png)

```html

```

```scss

```

### 三列右侧自适应布局

![](css2/layout-.png)

```html

```

```scss

```

### 高度自适应

![](css2/layout-.png)

```html

```

```scss

```


### 垂直居中

![](css2/layout-.png)

```html

```

```scss

```

## 圣杯布局

[圣杯布局](https://alistapart.com/article/holygrail)

## 双飞翼布局