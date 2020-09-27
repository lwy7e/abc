---
title: 编写文章（基于 Markdown）
date: 2019-03-23 14:04:24
categories:
- Markdown
tags: 
- Markdown
---

# 创建文章

在站点文件夹中打开 git bash，输入如下命令创建文章，其中 title 为文章的标题
``` bash               
$ hexo new "title"
```
当输入命令后，就会在 source/_post 文件夹下创建一个文件，命名为：title.md
这个文件就是将要发布到网站上的原始文件，用于记录文章内容
下面，我们将要在这个文件中写下我们的第一篇博客

# 编写文章（基于 Markdown）
## Markdown 简介
但是，在我们正式写下第一个文字前，我们需要了解一下究竟什么是 Markdown？
``` bash               
Markdown 是一种可以使用普通文本编辑器编写的 标记语言，通过简单的 标记语法，它可以使普通文本内容具有一定的格式
```
基于 Markdown 语法的简洁性，它已经成为目前世界上最流行的用于书写博客的语言
## Markdown 语法
在编写 Markdown 时，博主强烈的推荐给大家一款简洁易用的 Markdown 编辑器 —— Typora
按照官方的说法就是 简单而强大，它不仅支持原生的语法，也支持对应的快捷键，更重要的是它还可以 实时预览
这里附上 Typora 的下载地址：https://www.typora.io/

### 标题

``` bash
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
### 粗体、斜体、删除线和下划线

```bash
*斜体*
**粗体**
***加粗斜体***
~~删除线~~
```
### 引用块

```bash
> 文字引用
```
### 公式块
```bash
$$
数学公式
$$
```
### 分割线
```bash
方法一：---

方法二：+++

方法三：***
```
### 列表
```bash
1. 有序列表项

* 无序列表项

+ 无序列表项

- 无序列表项
```
### 表格

```bash
表头1|表头2
-|-|-
内容11|内容12
内容21|内容22
```

### 超链接
```bash
方法一：[链接文字](链接地址 "链接描述")
例如：[示例链接](https://www.example.com/ "示例链接")

方法二：<链接地址>
例如：<https://www.example.com/>
```
### 图片
```bash
![图片文字](图片地址 "图片描述")
例如：![示例图片](https://www.example.com/example.PNG "示例图片")
```

说明：在 Hexo中 插入图片时，请按照以下的步骤进行设置
将 站点配置文件 中的 post_asset_folder 选项的值设置为 true
在站点文件夹中打开 git bash，输入命令 npm install hexo-asset-image --save 安装插件
这样，当使用 hexo new title 创建文章时，将同时在 source/_post 文件夹中生成一个与 title 同名的文件夹，
我们只需将图片放进此文件夹中，然后在文章中通过 Markdown 语法进行引用即可
例如，在资源文件夹（就是那个与 title 同名的文件夹）中添加图片 example.PNG，
则可以在对应的文章中使用语句 ![示例图片](title/example.PNG "示例图片") 添加图片(注意有感叹号!前面)

# Hexo [NexT 内置标签](http://theme-next.iissnan.com/tag-plugins.html)

「标签」(Tag Plugin) 是 Hexo 提供的一种快速生成特定内容的方式。 
例如，在标准 Markdown 语法中，我们无法指定图片的大小。
这种情景，我们即可使用标签来解决。 Hexo 内置来许多标签来帮助写作者可以更快的书写， 
完整的标签列表 可以参考 Hexo 官网。 另外，Hexo 也开放来接口给主题，使主题有可能提供给写作者更简便的写作方法。 
以下标签便是 NexT 主题当前提供的标签。

## 文本居中的引用

此标签将生成一个带上下分割线的引用，同时引用内文本将自动居中。 
文本居中时，多行文本若长度不等，视觉上会显得不对称，因此建议在引用单行文本的场景下使用。 
例如作为文章开篇引用 或者 结束语之前的总结引用。

使用方式
* HTML方式：使用这种方式时，给 img 添加属性 class="blockquote-center" 即可。
* 标签方式：使用 centerquote 或者 简写 cq。

```
此标签要求 NexT 的版本在 0.4.5 或以上。 若你正在使用的版本比较低，可以选择使用 HTML 方式。
```

```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="blockquote-center" 是必须的 -->
<blockquote class="blockquote-center">blah blah blah</blockquote>

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% centerquote %}blah blah blah{% endcenterquote %}

<!-- 标签别名 -->
{% cq %} blah blah blah {% endcq %}
```
![示例图片](
https://hexo-lwy.oss-cn-hangzhou.aliyuncs.com/hexo/blockquote-center.png "示例图片")

## 突破容器宽度限制的图片

当使用此标签引用图片时，图片将自动扩大 26%，并突破文章容器的宽度。 此标签使用于需要突出显示的图片, 
图片的扩大与容器的偏差从视觉上提升图片的吸引力。 此标签有两种调用方式（详细参看底下示例）：

使用方式

* HTML方式：使用这种方式时，为 img 添加属性 class="full-image"即可。
* 标签方式：使用 fullimage 或者 简写 fi， 并传递图片地址、 alt 和 title 属性即可。 属性之间以逗号分隔。

```
此标签要求 NexT 的版本在 0.4.5 或以上。 若你正在使用的版本比较低，可以选择使用 HTML 方式。
如果要在图片下显示图片的标题，请使用 标签方式 并给定 title 属性。
```

```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="full-image" 是必须的 -->
<img src="/image-url" class="full-image" />

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% fullimage /image-url, alt, title %}

<!-- 别名 -->
{% fi /image-url, alt, title %}
```


## Bootstrap Callout 由 [ivan-nginx](https://github.com/iissnan/hexo-theme-next/pull/1160) 贡献
这些样式出现在 [Bootstrap](https://getbootstrap.com/) 的官方文档中。

使用方式
```
{% note class_name %} Content (md partial supported) {% endnote %}
```
其中，class_name 可以是以下列表中的一个值：

* default
* primary
* success
* info
* warning
* danger