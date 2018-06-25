---
layout: post
title: HTML+CSS 教程
tags: 前端
---
![html+css]({{site.baseurl}}/images/posts/hcj.jpeg)

>  **本文内容皆为`极客江南`李楠江先生的简书博文, 根据个人需要精简摘要, 衷心感谢李楠江先生无私的奉献, [点击进入博文首页](http://www.jianshu.com/u/4312c933b9db)**

## 基础了解

- 浏览器

	不同浏览器, 不同的内核(`渲染引擎`), 浏览器兼容问题

- 服务器

	专门存储数据的电脑

- 访问网页原理

	浏览器请求数据的原理和过程

- URL 是什么

	协议类型://ip地址:端口号/资源路径/资源名称

	`http://127.0.0.1:80/home/zane/Pictures/girl.jpg`

- HTTP 协议是什么

	规范浏览器和服务器之间的沟通(`信息传输`)

---

## HTML 基础

HTML是专门用来描述文本的`语义`的.

### 基础标签

- 标题标签

- P 标签 (Paragraph)

- HTML 注释 (Annotation)

	注释不能嵌套

- img 标签 (image)

	+ 属性格式: `key = "value"`

	+ 属性名称: `src`,`alt`,`title`,`height`,`width`

	+ 温馨提示: 只写高或宽, 可以使图片等比拉伸

- br 标签 (Break)

	不另起一个段落进行换行

- 相对路径和绝对路径

- a 标签 (anchor)

	+ 属性名称: `href`,`target`="_blank 或 _self", `title`

	+ base 标签, 写在 head 中, 一次性设置所有 a 标签打开方式

	+ 假链接

### 列表标签

- 无序列表 (unordered list)

- 有序列表 (ordered list)

- 定义列表 (defintion list)

	+ dt (defintion title), 定义标题

	+ dd (defintion description), 描述信息

### 表格标签

- table 定义表格, tr 定义行, td 定义列

- 表格属性

	+ `border`: 默认边框宽度为0, 不显示

	+ `height`, `width`: 默认内容自动计算出来

	+ `cellspacing`: 默认2个像素

	+ `cellpadding`: 默认1个像素

	+ `align`, `valign`: 表格及元素相对周围元素的对齐方式

	+ `bgcolor`

- td 标准单元格, th 表头单元格

- caption 标签: 给整个表格设置标题, 必须嵌套在 table 标签内部

### 表单标签

- form 标签
	
	+ 所有的表单内容, 都必须写在 form 标签里面

	+ 属性名称: `action`, `method`

- input 标签

	+ `type`属性取值决定了 input 标签的功能和外观

	+ `type`属性值: `text`, `password`, `radio`, `checkbox`, `button`, `image`, `reset`, `submit`, `hidden`隐藏域, `color`取色器, `date`

- `lable`标签

- 数据列表

	给输入框绑定带选项

	```html
	<datalist>
		<option>待选项内容1</option>
		<option>待选项内容2</option>
		<option>待选项内容3</option>
	</datalist>

	```

- textarea 标签, 文本域, 多行文本框

	属性名称: `cols`, `rows`, 表示列和行; `resize`为 none时禁止默认的手动拉伸

- select 标签, 下拉列表

	`<optgroup label="组名称"></optgroup>`给下拉列表添加分组

### 多媒体标签

- video 标签

	+ 属性名称: `src`, `autoplay`, `controls`,`poster`占位图片, `loop`, `preload`和autoplay相斥, `muted`, `width/herght`

	+ 内部嵌套 source 标签, 同时添加多个文件格式

- audio 标签

	除了`height`, `width`, `poster`没有, 其余与 video 标签类似

- marquee 标签

	属性名称: `direction` L/R/U/D, `scrollamount`滚动速度, `loop`默认-1(无限滚动), `behavior`slide(边界停止)/alternate(边界弹回)

---

## CSS 基础

- CSS 是专门用来修改文本`样式`的

- 两大部分: CSS 属性, CSS 选择器

### CSS 常见属性

- 文字属性
	
	+ 属性名称: `font-style`, `font-werght`, `font-size`, `font-family`

	+ 缩写格式: 

	  ```css
	  font: [style] [weight] size family;  

	  ```

	  记忆小技巧：`ｓw`ap and `s`a`f`e

- 文本属性

	+ 属性名称: `text-decoration` underline/ line-through/ overline/ none, `text-align`, `text-indent` 2em

- 颜色属性

	+ 英文单词

	+ rgb(0,0,0)

	+ rgba(0,0,0,`0-1`)

	+ 十六进制

### CSS 选择器

- 标签选择器

- id 选择器

- 类选择器

- 后代选择器, 后代选择器必须有`空格`隔开

- 子元素选择器, 子元素选择器之间必须用`>`符号连接

- 交/并集选择器, 选择器之间`没有`任何符号/用`,`连接

- 兄弟选择器

	+ 相邻兄弟选择器(用`+`连接), 只能选中紧跟其后的

	+ 通用兄弟选择器(用`~`连接)

- 序选择器

	+ 同级别的第几个

	  ```
	  first-child  

	  last-child  

	  nth-child(n)  

	  nth-last-child(n)  

	  only-child  
	  ```

	+ 同级同类型的第几个

	  ```
	  first-of-type  

	  last-of-type

	  nth-of-type

	  nth-last-of-type

	  only-of-type
	  ```

	+ 其他

	  ```
	  nth-child(odd)

	  nth-child(even)

	  nth-child(xn+y), n 从0开始递增

	  nth-of-type(odd)

	  nth-of-type(even)

	  nth-of-type(xn+y)
	  ```

- 属性选择器

- 通配符选择器

### CSS 三大特性

- 继承性

	+ 只有以`color / font- / text- / lint-`开头的属性才可以继承

	+ 特殊性: a 标签的文字颜色和下划线不能继承; h 标签的文字大小不能继承

- 层叠性

	+ 多个选择器选中`同一个标签`, 然后又设置了`相同的属性`, 才会发生层叠性

- 优先级

	+ 间接选中(即`继承`), 谁进听谁; 

	+ 相同选择器(`直接选中`), 谁后听谁; 

	+ 不同选择器(`直接选中`), `id`>`类`>`标签`>`通配符`>`继承`>`浏览器默认` 

	+ 优先级权重, 依次比较id/class/标签, 谁大听谁, 相等则谁后听谁

	+ `!important`, 提升某个`直接选中`标签的选择器的某个属性的优先级

### CSS 显示模式

- Div, 配合 CSS 完成网页基本布局

- Span, 配合 CSS 完成网页局部信息修改

- CSS 元素显示模式转换, 通过`display`属性(`block`/`inline`/`inline-block`)

### 背景和精灵图

- `background-color`属性(`单词`/`rgb`/`rgba`/`十六进制`)

- `background-image: url()`, 设置背景图片

	+ CSS 中通过`background-repeat`属性(`repeat`/`no-repeat`/`repeat-x`/`repeat-y`)控制背景图片的平铺方式

	+ 背景图片和背景颜色可以共存, 但是图片会覆盖颜色

	+ CSS 中通过`background-position`属性("`L/C/R` `T/C/B`;" 或 具体的像素), 控制图片位置

	+ CSS 中通过`background-attachment`属性(`scroll`/`fixed`)修改图片与滚动条的关联方式

- `backgroud`属性连写
	
	`background: 背景颜色 背景图片 平铺方式 关联方式 定位方式;`

	记忆小技巧：`ｃ`u`p` love `rap`

- CSS 精灵图, 配合背景图片和背景定位来使用

### 盒模型

- 边框属性, 连写时`样式`属性不能省略

- 内边距属性(padding)

- 外边距属性(margin)

- CSS 盒模型指那些可以设置`边框/内外边距/宽高`的标签

- `box-sizing`属性(`content-box`/`border-box`), 后者可以保持增加padding和border之后, 盒子元素的宽高不变

### 浮动流

- 标准流(文档流/普通流)排版方式

	有`垂直排版`和`水平排版`

- 浮动流排版方式

	+ 只有`水平排版`, 没有居中对齐(center值), 不可以用`margin: 0 auto`

	+ 不区分`块级元素`/`行内元素`/`行内块级元素`

- 定位流排版方式

- 浮动元素的`脱标`排序规则

	+ 相同方向上的浮动元素, 先浮动的元素显示在前面

	+ 不同方向上的浮动元素, 左找左, 右找右

	+ 浮动元素浮动之后的位置, 由浮动前在标准流中的位置确定

- 浮动元素`贴靠现象`, `字围现象`

### 清除浮动

- 浮动流中浮动元素内容高不可以撑起盒子的高

- 清除浮动

	+ 给前面的父盒子添加高度

	+ 利用`clear:both`属性清除前面浮动元素的影响, 会导致`margin`属性失效, 故不常用

	+ `外墙法`, 两个元素的盒子之间添加一个额外的块级元素, 通过设置额外标签的高度来实现`margin`效果

	+ `内墙法`, 在前面一个盒子的最后添加一个额外的块级元素, 内墙法会自动撑起盒子的高度, 所以可以直接设置`margin`属性

	+ `overflow:hidden`, 清除溢出盒子边框外的内容, 给前面一个盒子添加这个属性可以清除浮动, 因为`overflow:hidden`可以撑起盒子的高度, 所以可以直接设置`margin`属性

	+ 给前面的盒子添加伪元素来清除浮动, 本质上和内墙法一样

	  ```CSS
	  div:after {  
	  /* 生成内容作为最后一个元素 */
	  content: "";
	  /* 使生成的元素以块级元素显示, 占满剩余空间*/
	  display: block;
	  /* 避免生成内容破坏原有布局的高度*/
	  height: 0;
	  /* 使生成的内容不可见，并允许可能被生成内容盖住的内容可以进行点击和交互 */
	  visibility: hidden;
	  /* 重点是这一句*/
	  clear: both
	  }

	  ```

### 定位

- 相对定位

	+ 相对于自己在以前在标准流中的位置移动

	+ 不脱离标准流

- 绝对定位

	+ 相对于body或定位流中的祖先元素来定位

	+ 脱离标准流, 不区分块级/行内/行内块级

	+ 一个绝对定位元素会忽略祖先元素的padding

- **子绝父相**

	+ 相对定位和绝对定位很少单独使用, 一般都是一起出现

	+ 一般用来做覆盖效果

- 固定定位

	+ 脱离标准流

- 静态定位

	+ 默认的标准流的元素position属性就是static, 一般配合JS清除定位属性

	+ `z-index`属性, 控制界面上的定位元素的覆盖关系, 谁大谁在前, 有从父现象
