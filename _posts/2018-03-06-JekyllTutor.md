---
layout: post
title: Jekyll 搭建个人博客
tags: 编程语言
---

![jekyll]({{site.baseurl}}/images/posts/jekyllpic.jpeg)

### **介绍**

- Jekyll 是一个免费的简单静态网页生成工具。
- 它的模板目录，包换原始文本格式的文档，通过Markdown以及Liquid转化成完整的可发的静态网站，进而发布在任何服务器上。
- Jekyll 也可以运行在Github Page和Coding Page上，所以你可以免费搭建自己的博客。

### **Jekyll 环境配置**

| 需要安装的软件 | 执行的命令                    | 验证是否安装成功 | 说明               |
| -------------- | ----------------------------- | ---------------- | ------------------ |
| ruby           | sudo apt-get install ruby     | ruby -v          | 默认已经安装       |
| ruby-dev       | sudo apt-get install ruby-dev | ruby -v          | 不安装，报错       |
| nodejs         | sudo apt-get install nodejs   | nodejs -v        | 可手动安装最新版本 |
| jekyll         | sudo gem install jekyll       | jekyll -v        |                    |
| bundler        | sudo gem install bundler      | bundler -v       | 不安装，报错       |
| python         | sudo apt-get install python   | python --version | 默认已经安装       |

### **运行Jekyll测试**

1. `jekyll new myblog`，创建博客
2. `cd myblog/`，进入博客目录
3. `jekyll serve`  or `bundle exec jekyll serve`，启动本地服务
4. <http://127.0.0.1:4000>中预览


> *有时候`jekyll new myblog` 特别慢，这是因为bundle初始化慢，你可以选择 `--skip-bundle`，先跳过，new好之后，进入myblog，修改`Gemfile`文件，第一行修改或添加`source 'https://gems.ruby-china.org'`，然后再执行`sudo bundle install`就可以了，如果执行过程失败，请认真看一下错误日志，因为总要先分析一下问题，才可以解决问题的嘛，比如`g++`这个东西没有安装，或某个类库没有安装，`g++`这个就是我碰到的问题之一*


### **博客部署**

1. 创建一个github账号
2. 创建一个与你用户名(`your_username`)一样的仓库，如`your_username.github.io`
3. 把刚才创建的博客`myblog`项目 push 到 `your_username.github.io`仓库里
4. 浏览器输入`your_username.github.io`就可以访问**你的博客**了



### **克隆别人的模板**

- `git clone https://github.com/leopardpan/leopardpan.github.io`

- 修改gem的source

- ```sh
  $ gem sources --add http://gems.ruby-china.org/ --remove https://rubygems.org/ 

  $ gem sources -l
  *** CURRENT SOURCES ***
  http://gems.ruby-china.org
  ```

- 执行`bundle install`

- 运行`jekyll serve`预览一下效果

---

### **修改别人的模板**

- delete `_posts`
- `README.md`, 自定义修改为个人信息, 保留指向原作者的信息
- `Gemfile` `Gemfile.lock` `Rakefile` 是博客迁移时生成的, 可以删除

- `feed.xml` 用于RSS订阅

- `CNAME` 用来写自已的域名

- `images` 根目录下的图片可以替换


- `_config.yml` 配置文件, 主要修改

```
- # Basic 修改为个人信息

- # Comment

- 百度统计, 替换百度统计id
```

- `_layouts` 样表, 包含`default.html` `page.html` `post.html` 

- `_includes` 中的 `styles` , 里面包含部件

- `about.md`

**_头文件_**

```
---
layout: post     // 就是 _layouts 下的三个文件

title: 文章标题

description: 描述内容

tags: 杂谈
---
```

**_文件名_** `2018-01-01-post_name.md`

**_评论系统_** 来必力

---



### **参考链接**

- [Jekyll搭建个人博客](http://baixin.io/2016/10/jekyll_tutorials1/)
- [用 jekyll + Github Pages搭建个人博客](http://blog.csdn.net/u013553529/article/details/54588010)
- [基于 GitHub Pages 和 Jekyll 搭建个人博客的简单心得](https://www.bilibili.com/video/av13994132/)




### **相关资料**

- [Jekyll中文官方文档](https://www.jekyll.com.cn/)
- [Jekyll Themes](http://jekyllthemes.org/)






