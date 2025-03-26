---
title: "测试网页1"
layout: splash
permalink: /test/
date: 2024-08-24T11:48:41-04:00
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://image.olim.cc/ai.olim.ca/unsplash-image-1.jpg
  actions:
    - label: "下载-期权希腊值"
      url: "/assets/pdf/Option_Greeks.pdf"
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "测试放一个下载链接。"
intro: 
  - excerpt: '这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。这里放一段介绍文字。。。。。。。文字排列是居中。 `type="center"`'
feature_row:
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-1-th.jpg
    alt: "图片1"
    title: "图片1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-2-th.jpg
    image_caption: "Image courtesy of [Unsplash](https://unsplash.com/)"
    alt: "图片2"
    title: "图片2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-3-th.jpg
    alt: "图片3"
    title: "图片3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
feature_row2:
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder Image Left Aligned"
    excerpt: '这里放一张照片和文字，照片在左，文字在右。 `type="left"`。旁边配一个按钮，可设置成链接，连接到另外一个页面。。。'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row3:
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder Image Right Aligned"
    excerpt: '这里放一张照片和文字，照片在右，文字在左。 `type="right"`。。。旁边配一个按钮，可设置成链接，连接到另外一个页面。。。'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row4:
  - image_path: https://image.olim.cc/ai.olim.ca/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder Image Center Aligned"
    excerpt: '这里放一张照片和文字，照片在上，文字在下，居中。 `type="center"`。。。旁边配一个按钮，可设置成链接，连接到另外一个页面。。。'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %}