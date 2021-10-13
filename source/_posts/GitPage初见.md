---
title: GitPage初见
date: 2021-10-12 23:29:32
tags:
- GitPage
categories:
- [编程, 杂, GitPage] 
lang: zh
---
### 搭建GitPage

---

1. ##### 参考教程

[Git Pages + Jekyll/Hexo搭建自己的博客](https://blog.csdn.net/muzilanlan/article/details/81542917)

[hexo的环境搭建](https://www.cnblogs.com/zhvon/p/5351043.html)

---

2. ##### 配置步骤

安装Node.js

Windows下用安装包安装Node.js，注意选中Add to Path，在命令行用

```sh
echo %PATH%
```

检查是否添加环境变量成功

安装Hexo

```sh
npm install -g hexo-cli
```

配置npm代理（失败）

[npm安装报错 rollbackFailedOptional]: https://blog.csdn.net/weixin_42097653/article/details/82222432

```sh
npm config set proxy http://127.0.0.1:7890

npm config set https-proxy http://127.0.0.1:7890
```

换源+取消代理

```sh
npm install -g cnpm --registry=https://registry.npm.taobao.org

npm config delete proxy

npm config delete https-proxy
```

用cnpm安装Hexo（成功）

```sh
cnpm install -g hexo-cli
```


{% asset_img image-20211011233002637.png  %}


进入Sid-Jacob.github.io文件夹

```sh
hexo init
```

（失败）


{% asset_img image-20211011235429217.png  %}

可以看到clone成功，但insatll dependencies卡住，网络问题

使用npm install安装依赖（失败）

```sh
cnpm install
```

（成功）

[关于hexo init过程中出现fail to install dependencies的解决](https://www.cnblogs.com/moyidou/p/15254076.html){% asset_img image-20211011235808391.png  %}


安装完后的目录结构

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   └── _posts
└── themes
```

修改_config.yml进行配置

https://hexo.io/zh-cn/docs/configuration

http://ijiaober.github.io/2014/08/05/hexo/hexo-04/

```
# Category & Tagdefault_category: uncategorizedcategory_map:	编程: programming	生活: life	其他: othertag_map:
```

添加插件、主题

https://hexo.io/plugins/

https://github.com/edolphin-ydf/hexo-encrypt

https://github.com/HCLonely/hexo-bilibili-bangumi

https://hexo.io/themes/

添加主题

http://ijiaober.github.io/2014/08/05/hexo/hexo-06/

https://github.com/aircloud/hexo-theme-aircloud

运行Hexo命令

```sh
$ hexo g #hexo generate 生成$ hexo s #启动本地web服务器
```

中途报错

```
ValidationError: `slug` is required!
```

{% asset_img image-20211012005202636.png  %}

Google了一圈发现...

https://github.com/hexojs/hexo/issues/1099


{% asset_img image-20211012005032236.png width=100% %}

之前改成了untitled.md，改回默认的title.md就不报错了。

启动Hexo


{% asset_img image-20211012005353568.png  %}

此时图片无法加载（将config里面语言改为zh，和theme里面一致，则home、tag、archive、about可根据theme配置显示为中文）


{% asset_img image-20211012195022141.png  %}

发现是图片的链接生成错误，手动修改链接后显示成功。


{% asset_img image-20211012212716126.png  %}

根据经验，hexo g命令根据theme的某个模板生成index等等页面的代码。查看目录结构，可以发现layout下面有页面的模板和partial页面模板，而该主题的用户头像在每个页面都会显示，故猜测其生成模板在partial目录下面。最终，在nav.ejs文件中找到对应位置。


{% asset_img image-20211012212811606.png  %}


{% asset_img image-20211012213030439.png  %}

根据其用法，可以猜测我们需要在config文件中添加root参数和sidebar-avatar参数，如下：


{% asset_img image-20211012213134836.png  %}

重新hexo clean && hexo g && hexo s，图片显示成功！

小bug：貌似hexo d上传不会检测同名文件，如将favicon.ico文件替换之后上传，favicon.ico并没有被替换，重命名后才检测到。(按道理应该是检测hash值有没有变化来决定是否更新文件)


{% asset_img image-20211012213825868.png  %}

部署到Gitpage上：在config里面设置，然后hexo d即可


{% asset_img image-20211012214503181.png  %}


（2021.10.12）