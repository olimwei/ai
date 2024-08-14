---
title: "markdown语法练习"
layout: blog
permalink: 
collection: 
excerpt: "markdown语法练习"
# author_profile: true
toc: ture
toc_sticky: true
sidebar:
  - title: 
    nav: XXX
---

### 字体变化

**加粗字体**， *斜字体*， ***加粗斜字体***， ~~化掉字体~~

### 列表

1. 有序列表1
2. 有序列表2
3. 有序列表3

1. 列表1
    - 列表嵌套1
    - 列表嵌套2
2. 列表2
3. 列表3

- [x] ~~完成的任务1~~
- [x] ~~完成的任务2~~
- [ ] 未完成的任务1
- [ ] 未完成的任务2

### 代码

`<p>This is the first item</p>`


### 链接

[文字链接]: http://archiz.com

[文字链接2-阿齐兹][文字链接]

图片链接和文字链接不同的是前面有个感叹号。可以加鼠标移动到图片时显示的文字。

![走势图](http://money.olim.ca/assets/images/2024/2024-05-31-DXY.jpg "hover me")

### 插入视频

<!-- 21:9 aspect ratio -->
<div class="embed-responsive embed-responsive-21by9">
  <iframe class="embed-responsive-item" src="http://money.olim.ca/assets/video/blooming.mp4"></iframe>
</div>

<!-- 16:9 aspect ratio -->
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="http://money.olim.ca/assets/video/blooming.mp4"></iframe>
</div>

<!-- 1:1 aspect ratio -->
<div class="embed-responsive embed-responsive-1by1">
  <iframe class="embed-responsive-item" src="/assets/images/blooming.mp4"></iframe>
</div>

### 引用
> 这是引用的内容。
> 1. 引用内容里包括的列表1
> 2. 引用内容里包括的列表2

### 表格的制作

| 第二栏目居右 | 第二栏目居左居左 | 第三栏目居中居中居中居中居中居中 |
|-----:|:--------------|:-----------:|
|     1|     靠左边     | 居中排列的话1 |
|     2|     靠左左     | 居中排列的话2 |
|     3|     左左左     | 居中排列的话3 |

### 水平线制作 用三个“-”"*""_"符号都可以

第一种水平线(三个破折号，***建议使用***)

---

第二种水平线（三个星号，有时候有问题。我这里用了6ge星号）

******

### 脚注 （不正确显示）
这句话包括一个脚注明。[^1]

下面是我从GH里copy来的一段：
Here is a simple footnote[^2].

[^1]: 第一个脚注明。 .  
[^2]: My reference.

### 高亮选择的段落 （检查是否正确显示）
用<mark>mark</mark>语言 （这个可以）

我需要<mark>高亮这几个字</mark>。

### 几种底色形式的notice

<div class="notice">
  <p>这是没有表明类别的notice</p>
</div>
<div class="notice--primary">
  <p>这是primary类的notice</p>
</div>
<div class="notice--info">
  <p>这是information类的notice</p>
</div>
<div class="notice--danger">
  <p>这是danger类的notice</p>
</div>
<div class="notice--success">
  <p>这是success类的notice</p>
</div>
<div class="notice--warning">
  <p>这是warning类的notice</p>
</div>

### Carousel
A slideshow component for cycling through elements—images or slides of text—like a carousel.

it requires util.js.

<div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
  </ol>
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img class="d-block w-100" src="http://money.olim.ca/assets/images/2024b/AI-20240808-comments-1.png" alt="First slide">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="http://money.olim.ca/assets/images/2024b/AI-20240809-comments-1.png" alt="Second slide">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="http://money.olim.ca/assets/images/2024b/AI-20240812-comments-1.png" alt="Third slide">
    </div>
  </div>
  <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

### 其它

只有下面这种方式可以正确显示：(用sub sup)
5<sup>2</sup>; H<sub>2</sub>O
