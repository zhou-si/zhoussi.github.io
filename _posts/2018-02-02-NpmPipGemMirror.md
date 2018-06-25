---
layout: post
title: NPM PIP GEM 国内镜像配置
tags: 工具
---

![mirror]({{site.baseurl}}/images/posts/npg_mirror.jpg)
### [RubyGems Mirrors](http://gems.ruby-china.org/)

- 如何使用 ?

```sh
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://gems.ruby-china.org
# 请确保只有 gems.ruby-china.org
```

- 如果你使用 Gemfile 和 Bundle (例如: Rails 项目), 你可以用 Bundler 的 [Gem 源代码镜像命令](http://bundler.io/v1.5/bundle_config.html#gem-source-mirrors)

```sh
$ bundle config mirror.https://rubygems.org https://gems.ruby-china.org

```

- 这样你不用改你的 Gemfile 的 source

```sh
source 'https://rubygems.org/'
gem 'rails', '4.1.0'
...

```

- SSl 证书错误

- - 一般不会用到

  - 遇到, 修改`~/.gemrc`文件, 增加`ssl_verify_mode: 0`配置, 以便于 RubyGems 可以忽略 SSL 证书错误


  ```sh
    ---
    :sources:
    - https://gems.ruby-china.org
    :ssl_verify_mode: 0
  ```



### [Cnpm Mirrors](http://npm.taobao.org/)

- 使用定制的 cnpm 命令行工具代替默认的 `npm`

```sh
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

**or**

- 直接添加`npm`参数`alias`一个新命令

```sh
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"

# Or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```

**or**

- ```sh
  # 1. 通过 config 命令
  npm config set registry https://registry.npm.taobao.org 
  npm info underscore （如果上面配置正确这个命令会有字符串response）

  ```

- ```sh
  # 2. 命令行指定
  npm --registry https://registry.npm.taobao.org info underscore 
  ```

- ```sh
  # 3. 编辑`~/.npmrc` 加入下面内容
  registry = https://registry.npm.taobao.org

  # 这种方法最优哦, 直接写死配置了
  ```

### [PIP Mirrors](https://www.cnblogs.com/maoguy/p/6689512.html)

- Python 的 pip 安装包国内源

- 推荐

- - 豆瓣: <http://pypi.douban.com/simple/>
  - 清华: <https://pypi.tuna.tsinghua.edu.cn/simple>

- 临时修改使用

- - ```sh
    $ pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas
    ```

- 永久修改使用, 

- - ```sh
    # linux 修改~/.pip/pip.conf 中的index-url
    1 [global]
    2  index-url = https://pypi.tuna.tsinghua.edu.cn/simple

    # windows 修改 C:\Users\xx\pip 中的 pip.ini, 没有就新建
    1  [global]
    2  index-url = https://pypi.tuna.tsinghua.edu.cn/simple
    ```
