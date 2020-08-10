---
title: 「Hexo」「2」「使用Hexo搭建配置细节」
date: 2020-02-12 21:36:57
tags:
- Hexo
- Blog
categories: Hexo
copyright:
---
<!--more-->
***
上一节已经成功搭建[Hexo](http://47.98.132.251/2020/02/06/Git%E7%9A%84%E4%BD%BF%E7%94%A8%E5%92%8CHexo%E9%85%8D%E7%BD%AE/)博客。
本节主要讲解配置[Hexo](https://hexo.io/zh-cn/docs/)博客的细节。
***
## 设置
编辑myblog目录下<kbd>_config.yml</kbd>，详见[Hexo官网配置文件](https://hexo.io/zh-cn/docs/configuration)。

## 网站
|**参数**|**描述**|**例子**|
|----|----|----|
|title|网站标题|XX 的博客|
|subtitle|网站副标题|我的个人日志|
|description	|网站描述|
|author|	网站作者	|你的名字|
|language|	网站使用的语言	|zh-CN#中文 en#英文|
|timezone|	时区|	Asia/Shanghai#亚洲上海时间|

我的设置：

    title: Hamlet's Backgarden
    description: share protocols on bioinformatics
    keywords:
    author: Hamlet_Zhengs
    language: zh-Hans
    timezone: Asia/Shanghai

## 目录

    # Directory
    source_dir: source
    #public_dir: public
    tag_dir: tags
    archive_dir: archives
    category_dir: categories
    code_dir: downloads/code
    i18n_dir: :lang
    posts_dir: archives
    skip_render:

## 文章
查看以下列表，在<kbd>_config.yml</kbd>文件下查看相关属性。并进行修改为自己所想要的方式吧，这个就不一一进行分解了。

|参数|描述|默认值|
| ---- | ---- |---- |
|new_post_name	|新文章的文件名称	|:title.md
|default_layout	|预设布局	|post
auto_spacing|	在中文和英文之间加入空格|	false|
titlecase	|把标题转换为 title case	|false
external_link|	在新标签中打开链接	|true
filename_case|	把文件名称转换为（1）小写或（2）大写	|0
render_drafts|	显示草稿	|false
post_asset_folder	|启动 Asset| 文件夹|	false
relative_link|	把链接改为与根目录的相对位置	|false
future|	显示未来的文章	|true
highlight	|代码块的设置	|自定义

## 分类&标签
这里默认就行，并不影响我们接下来的一些操作。

|参数|描述|默认值|
| ---- | ---- |---- |
default_category|	默认分类|	uncategorized
category_map	|分类别名
tag_map|	标签别名	|自定义

## 分页
在分页中一般情况默认就好，10篇文章一页，不错的。

|参数|描述|默认值|
| ---- | ---- |---- |
per_page	|每页显示的文章数（0=关闭分页功能）	|10
pagination_dir|	分页目录	|page

## 写作
创建文章

    hexo new [layout]

如：创建hello-world

    hexo new hello-world
layout:就是hello-world,如果不添加title，默认就是标题title:hello-world。这里注意一下，如果创建带有中文的路径名称时，生成静态页面hexo g可能会报错。参看[Hexo 部署的时候发生错误解决方案](https://www.jianshu.com/p/9afb3257133b)。

### 修改文章
创建文章在<kbd>source/is_posts</kbd>文件夹下。
修改如下：添加标签<kbd>tags</kbd>、类别<kbd>categories</kbd>等等。

    ---
    title: Hello World  
    date: 2018-09-01  
    tags:  Hexo  
    categories: Hexo  
    ---

### 编写文章
在这里写文章，和平时一样支持[MD](https://daringfireball.net/projects/markdown/)语法，干巴爹。

### [主题](https://hexo.io/themes/)
这是博客DIY的重点，有很多需要修改的细节，为了能达到自己喜欢的布局结构，以及替换图片，动画特效，你需要选择一个合适的主题，并且根据自己的需求慢慢修改主题配置。
Hexo提供给我们许多模板主题，请查看[Hexo主题官网](https://hexo.io/themes/)下载自己喜欢的主题，并且按照主题要求进行配置，对于特定主题的修改随主题的不同而有所区别，这里不再单独写了，大家自行google。

## 总结
学习本篇文章我们知道了如何进行配置HEXO博客一些常用的功能，如何进行分类，如何创建文章。

Hexo的教程先写到这儿。

如何把博客打造成自己的风格，还需要各位自行探索，加油奥！
