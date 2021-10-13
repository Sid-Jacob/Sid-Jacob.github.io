---
title: GitPage再会
date: 2021-10-13 20:16:00
tags:
- GitPage
categories:
- [编程, 杂, GitPage] 
lang: zh
---
#### Hexo常用的几个命令：

hexo generate (hexo g) 生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
hexo server (hexo s) 启动本地web服务，用于博客的预览
hexo deploy (hexo d) 部署播客到远端（比如github, heroku等平台）
另外还有其他几个常用命令：

hexo new "postName" #新建文章

hexo new page "pageName" #新建页面

常用简写

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

常用组合

hexo d -g #生成部署

hexo s -g #生成预览



#### 分类和标签

只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

```yaml
categories:
- Diary
tags:
- PS3
- Games
```

**分类方法的分歧**

如果您有过使用 WordPress 的经验，就很容易误解 Hexo 的分类方式。WordPress 支持对一篇文章设置多个分类，而且这些分类可以是同级的，也可以是父子分类。但是 Hexo 不支持指定多个同级分类。下面的指定方法：

```yaml
categories:
  - Diary
  - Life
```

会使分类`Life`成为`Diary`的子分类，而不是并列分类。因此，有必要为您的文章选择尽可能准确的分类。

如果你需要为文章添加多个分类，可以尝试以下 list 中的方法。

```yaml
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
```

此时这篇文章同时包括三个分类： `PlayStation` 和 `Games` 分别都是父分类 `Diary` 的子分类，同时 `Life` 是一个没有子分类的分类。



#### 图片大小适应网页布局

图片大小无法自适应div标签？如何控制渲染的缩放？hexo的标签似乎没有width这个参数设置的地方

![image-20211013104840706](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013104840706.png)

![image-20211013104859549](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013104859549.png)

p标签可以框柱图片大小，而div标签不行，直接修改img标签属性width=100%（height=auto）也可以。

p标签不能套在div标签外面，所以不能在模板里面改，只能控制markdown转换为html的逻辑，在img外面套p标签

https://www.cnblogs.com/lovelvxia/p/5726316.html

但如何控制markdown输出为html的时候将img包上p标签？（思路：如果无法控制转换过程，要么在css里面尝试修改img的属性，要么添加js改变html代码）

换一种思路，利用theme自带的css，选中post-container下post-content内的所有img，设置width属性，大功告成（横向图片显示效果很好，但纵向图片显示过大）

![image-20211013113431614](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013113431614.png)

![image-20211013113715325](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013113715325.png)

一开始想用expression表达式，但似乎这种用法已经被抛弃了，查阅css教程，直接用max-width就可以。

![image-20211013125535030](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013125535030.png)

纵向横向图片都能正常显示

![image-20211013125604618](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013125604618.png)

#### CSS 高度和宽度

height 和 width 属性不包括内边距、边框或外边距。它设置的是元素内边距、边框以及外边距内的区域的高度或宽度。

##### CSS 高度和宽度值

height 和 width 属性可设置如下值：

- auto - 默认。浏览器计算高度和宽度。
- *length* - 以 px、cm 等定义高度/宽度。
- % - 以包含块的百分比定义高度/宽度。
- initial - 将高度/宽度设置为默认值。
- inherit - 从其父值继承高度/宽度。

##### 设置 CSS 尺寸属性

| 属性                                                                   | 描述                 |
| :--------------------------------------------------------------------- | :------------------- |
| [height](https://www.w3school.com.cn/cssref/pr_dim_height.asp)         | 设置元素的高度。     |
| [max-height](https://www.w3school.com.cn/cssref/pr_dim_max-height.asp) | 设置元素的最大高度。 |
| [max-width](https://www.w3school.com.cn/cssref/pr_dim_max-width.asp)   | 设置元素的最大宽度。 |
| [min-height](https://www.w3school.com.cn/cssref/pr_dim_min-height.asp) | 设置元素的最小高度。 |
| [min-width](https://www.w3school.com.cn/cssref/pr_dim_min-width.asp)   | 设置元素的最小宽度。 |
| [width](https://www.w3school.com.cn/cssref/pr_dim_width.asp)           | 设置元素的宽度。     |

可以用长度值（例如 px、cm 等）或包含块的百分比（％）来指定 max-width（最大宽度），也可以将其设置为 none（默认值。意味着没有最大宽度）。



#### 添加categories页面

分类：https://blog.csdn.net/muzilanlan/article/details/81542917

多级分类：https://www.jianshu.com/p/7d0c5e30e0f3

官方文档说明：https://hexo.io/zh-cn/docs/variables

讲的最透彻的文档：https://github.com/hughshen/blog/blob/c3390a5a99cfcf398a012a9cf742b7f41b49d345/app/posts/1437904915000-hexo-modify-theme.md

categories存放分类目录页，category存放某分类下的文章页。

hexo new page categories后，在source文件夹下会新建一个categories目录和index.md，这个index.md只需要修改其Front-matter的属性，如下：![image-20211013165810378](GitPage%E5%86%8D%E8%A7%81.assets/image-20211013165810378.png)

最重要的是layout，这个参数值对应theme下layout目录中ejs模板。模板按照上文的链接设置即可。

#### 安装图片链接转换插件

cnpm install https://github.com/xcodebuild/hexo-asset-image --save

报错Cannot find module 'hexo'：

```
npm install hexo --save
```

TODO:

图床、categories页的css样式