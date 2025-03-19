---
title: "markdown语法练习"
layout: blog
permalink: 
collection: 
excerpt: "markdown语法练习"
# author_profile: true
# toc: ture
# toc_sticky: true
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
#### 文字链接
[文字链接]: http://archiz.com

[文字链接2-阿齐兹][文字链接]

#### 图片链接
图片链接和文字链接不同的是前面有个感叹号。可以加鼠标移动到图片时显示的文字。

![走势图](http://money.olim.ca/assets/images/2024/2024-05-31-DXY.jpg "hover me")

##### 下面这个图片是用的在CF的外链(storage启用了自定义域名，好像不能成功外链)
![2024-01-5美元指数dxy走势图](https://file.olim.in/img/2024/2024-01-05-DXY-day-1.jpg "hover me")

##### 下面这个图片也是用的在CF的外链（pan空间本身没启用自定义域名，只是上传端用了自定义域名）
![CRNC走势图](https://pan.olim.cc/webdav/img/2025/CRNC.jpeg "hover me")

##### 下面两张同样的图片用的在oracle的外链（用alist获取的外链）
![SPX走势图](https://alist.olim.cc/p/image/SPX-20250304-2h-c.jpg?sign=JofVCtfV3Evh7fK0RZoCpkCj7NDEPP_xDOqMuc-B934=:0 "hover me")

##### 下面的图片用存在github上。用了三种链接方式。(这个可以作为经常使用的方式)
![SPX走势图](https://img.olim.cc/2025/20250318-SPX-h2-c.jpeg "hover me")
第一次是用自定义域名的链接

![SPX走势图](https://raw.githubusercontent.com/olimwei/img/main/2025/20250318-SPX-h2-c.jpeg "hover me")
第二次是用github生成的链接

![SPX走势图](https://cdn.jsdelivr.net/gh/olimwei/img@main/2025/20250318-SPX-h2-c.jpeg "hover me")
第三种是jsdelivr代理加速的方式

关于图片的cdn加速，可以考虑用CF page读取github图库生成的自定义域名地址来建立。

### 插入视频

<!-- 16:9 aspect ratio -->
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="http://money.olim.ca/assets/video/blooming.mp4"></iframe>
</div>

<iframe 
src="/assets/images/blooming.mp4" 
scrolling="no" 
border="0" 
frameborder="no" 
framespacing="0" 
allowfullscreen="true" 
height=600 
width=800> 
</iframe>

<!-- 1:1 aspect ratio -->
<div class="embed-responsive embed-responsive-1by1">
  <iframe class="embed-responsive-item" src="/assets/images/blooming.mp4"></iframe>
</div>

### 引用
> 这是引用的内容。
> 1. 引用内容里包括的列表1
> 2. 引用内容里包括的列表2

### 在markdown原始文档里加不在网页显示的备注：
{::comment}用实际的目录替换test{:/comment}
看不到上面一段话就对了😄。

### 代码引用 fenced code blocks
代码上下行用三个\`。在最上面三个\`后面可以加上程序语言的类型，如python or others。
```python
import bokeh as bk

from bokeh.plotting import figure, show
p = figure(width=400, height=400)

p.scatter([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], size=20, color="navy", alpha=0.5)

show(p)
```

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
1. 用<mark>mark</mark>语言 （这个可以）

我需要<mark>高亮这几个字</mark>。

2. 用style来改变背景颜色
我需要<mark style="background-color: #FF2C2C">高亮这几个字在红色背景上</mark>。

3. 用div加style方式（整个一段）
<div style="background-color: #008000">绿色背景高亮这段文字。</div>

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
