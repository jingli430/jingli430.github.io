---
title: 用github+hexo搭建blog
date: 2018-04-21 11:29:33
categories: [blogging]
tags: [test, hello, world]
---

## 资源

* [hexo官网](https://hexo.io/zh-cn/)

## 搭建

### 1. 配置站点

一般hexo初始化后的站点目录结构如下

```Plain
/blog
 |-- _config.yml # 站点配置文件
 |-- scaffolds   # 页面模板存放的目录
 |      |-- post.html # 博文模板
 |      |-- page.html # 分页模板
 |      \-- draft.html # 草稿模板
 |-- source         # 博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
 |      |-- _posts  # 存放博文的目录
 |      \-- _drafts # 存放草稿的目录
 |-- themes      # 存放主题文件夹的目录
 |-- package.json  # npm 依赖等
 |-- node_modules # 功能依赖包
 \-- .gitignore   # push的时候忽略node_modules文件夹等
```

修改`_config.yml`文件，整个站点的配置主要是在这个文件中修改

```
title: 博客名
description: 博客简述
author: 作者名
url: http(s)://your.site.com
root: /your/project/path or /

deploy:
  type: git
  //repo: git@github.com:userName/userName.github.io.git   // ssh  
  repo: https://github.com/userName/userName.github.io.git  // https
  branch: gh-pages // 真正要显示的内容是放在master分支下
  message: [自定义的提交信息]
```

### 2. 撰写博文

每当需要撰写心得博文时，只需要执行以下命令，就可以得到一个标题和日期已经设置好的markdown文件

```
hexo new [layout] '博文标题' # layout是可选项，表示使用哪种模板来创建页面文件，默认是post
```

博文结构的头部

```
title: 博文标题
date: yyyy-mm-dd  # 日期是创建博文源文件时自动生成的
categories:
- category1
- category2
tags:
- tag1
- tag2
comments: true
```





### 3. 更新 & 部署站点到Github Pages

```
// hexo new '博文标题'
hexo clean # 删除之前生成的静态文件
hexo g # 生成编译后的静态站点文件
hexo d # 部署到远程仓库
```

如果发现错误

```
ERROR Deployer not found: git
```

安装

```
npm install hexo-deployer-git --save // inside the project folder
```

再次deploy

```
hexo d
```

### 4. 本地隐藏文件夹`.deploy_git` 对应的是github上的内容

```
drwxr-xr-x  12 jing.li  staff    408 Apr 21 17:01 .git
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:58 2018
-rw-r--r--   1 jing.li  staff     54 Apr 21 16:58 README.md
drwxr-xr-x   4 jing.li  staff    136 Apr 21 16:58 archives
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:58 categories
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:58 css
drwxr-xr-x  20 jing.li  staff    680 Apr 21 16:58 images
-rw-r--r--   1 jing.li  staff  26989 Apr 21 16:58 index.html
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:58 js
drwxr-xr-x  16 jing.li  staff    544 Apr 21 16:58 lib
drwxr-xr-x   5 jing.li  staff    170 Apr 21 16:58 tags
```

本地blog dir里面是

```
drwxr-xr-x   12 jing.li  staff     408 Apr 21 16:49 .deploy_git
-rw-r--r--    1 jing.li  staff    1900 Apr 21 16:59 _config.yml
-rw-r--r--    1 jing.li  staff     174 Apr 21 16:49 db.json
drwxr-xr-x  263 jing.li  staff    8942 Apr 21 12:12 node_modules
-rw-r--r--    1 jing.li  staff  103985 Apr 21 12:12 package-lock.json
-rw-r--r--    1 jing.li  staff     483 Apr 21 12:12 package.json
drwxr-xr-x   11 jing.li  staff     374 Apr 21 12:39 public
drwxr-xr-x    5 jing.li  staff     170 Apr 21 11:05 scaffolds
drwxr-xr-x    3 jing.li  staff     102 Apr 21 11:05 source
drwxr-xr-x    4 jing.li  staff     136 Apr 21 11:31 themes
```

展示.deploy_git

```
drwxr-xr-x  12 jing.li  staff    408 Apr 21 17:03 .git
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:49 2018
drwxr-xr-x   4 jing.li  staff    136 Apr 21 16:49 archives
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:49 categories
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:49 css
drwxr-xr-x  20 jing.li  staff    680 Apr 21 16:49 images
-rw-r--r--   1 jing.li  staff  27937 Apr 21 12:39 index.html
drwxr-xr-x   3 jing.li  staff    102 Apr 21 16:49 js
drwxr-xr-x  16 jing.li  staff    544 Apr 21 16:49 lib
drwxr-xr-x   5 jing.li  staff    170 Apr 21 16:49 tags
```

### 5. 多PC同步

现在我们了解了hexo部署的真正流程

* `hexo new`  创建新的文章
* `hexo generate` render 并更新`public` folder
* `hexo deploy`把`public` folder中的内容拷贝到`.deploy_git` 中，再git push到github

github上的东西并不是blog dir里面的内容，而只是`blog_dir/.deploy_git` 的内容，要想在别的机器上也创造一个可以更新博客的站点，你必须把完整的blog dir拷贝到另外一台机器上

* 方法一：把blog dir放置在google drive里面，然后在新机器上就把google drive的blog dir拷贝过去，并安装node, git, hexo即可
* 方法二：把这个blog dir放在`username.github.io`repo的另外一个分支下，相当于是用github代替了google drive来保存blog dir，而且它存放在同一个repo的上，方便管理和维护。

由于我现在已经创建好了blog dir，我相当于是要把这个existing的folder变成`username.github.io`这个repo的hexo分支

在github上的该repo下创建一个分支hexo



把这个分支hexo而不是默认的master给下载下来

```
git clone -b hexo  https://github.com/username/username.github.io.git aaaa
```

aaaa是为了不跟username.github.io冲突的名字而已，反正用完之后会删掉，起什么名字都无所谓

把local blog dir放入hexo分支下

`.git`存放了repo所有相关的metadata，拷贝`.git`到`username.github.io`文件夹，完成导入repo的工作

```
cd aaaa
mv .git ../username.github.io
```

添加`db.json` ， `public` ，`.deploy_git` 到`.gitignore`

* `db.json`是缓存文件

添加所有untracked和unstaged文件，然后push到远端的hexo

```
git push origin hexo
```

至此

* blog dir驻守username.github.io的hexo分支，用git来管理和更新
  * hexo分支在这里就是github本身提倡的`gh-pages`分支，即用来存放生成站点的source分支
* blog本身的内容(`deploy_git`)驻守username.github.io的master分支，用hexo来管理和更新

> 如果你现在的blog dir是崭新的，你不妨在创建username.github.io这个repo的时候就先创建hexo分支，这样就不需要这样繁琐的补救措施



## 其他

### 常用hexo命令

```
hexo init <站点仓库的目录> # 初始化一个站点目录
hexo clean # 用于主题切换等涉及站点整体布局效果改变的行为时，清楚hexo原有缓存
hexo new '博文标题' # 新增一篇博文
hexo generate # 可简写为 hexo g ，编译生成静态页面文件
hexo server # 可简写为 hexo s ，开启本地服务器预览与测试编译生成的静态页面效果
hexo delpoy # 可简写为 hexo d ，部署站点到远程仓库
hexo generate -d # 可简写为 hexo g -d ，编译生成静态页面文件并部署到远程仓库
hexo deploy -g # 可简写为 hexo d -g ，同上
```

### 更改主题

从[hexo themes](https://hexo.io/themes/)找到何意的主题，clone下来，放到站点本地仓库的`themes`目录下，

比如我现在想添加主题next，我就在blog dir下

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

再修改blog`_config.yml`文件的`theme`字段为对应的主题名

```
theme: next
```

### 如何添加多个tags

有两种多标签格式

```
tags: [a, b, c]
或
tags:
  - a
  - b
  - c
```

### 显示部分文章内容

如果在博客文章列表中，不想全文显示，可以增加 `<!-- more -->`, 后面的内容就不会显示在列表。

```
<!--more-->
```

### 添加插件

添加 sitemap 和 feed 插件

切换到你本地的 hexo 目 CIA ，在命令行窗口，输入以下命令

```
npm install hexo-generator-feed -save
npm install hexo-generator-sitemap -save
```

修改 _config.yml，增加以下内容

```
# Extensions
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
#sitemap
sitemap:
  path: sitemap.xml
```

再执行以下命令，部署服务端

```
hexo d -g
```

### hexo的适用场景

- GitHub Pages 不支持数据库管理，所以你只能做静态页面的博客，不能像其他博客（如 WordPress）那样通过数据库管理自己的博客内容。
- 但是GitHub Pages 无需购置服务器，免服务器费的同时还能做负载均衡，github pages有300M免费空间。


- 通过 Hexo 你可以轻松地使用 Markdown 编写文章，非常符合我的口味。Markdown 真的是专门针对程序员开发的语言啊，现在感觉没有 Markdown什么都不想写。什么富文本编辑器，什么word，太麻烦了！而且样式都好丑！效率太低！



## Hexo的配置

### Menu Settings

新建tags, catagories页面

```
$ hexo new page tags   # 会在source/下生成tags/index.md文件
$ hexo new page categories   # 会在source/下生成categories/index.md文件
```

确认主题配置文件有

```
vim current_theme/_config.xml
tags: tags
```



```
mkdir source/dags
mkdir source/archives
```

### 加about页面







### Front-matter 

是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，举例来说：

```
title: Hello World
date: 2013/7/13 20:46:25
---
```

以下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

| 参数         | 描述                 | 默认值       |
| ------------ | -------------------- | ------------ |
| `layout`     | 布局                 |              |
| `title`      | 标题                 |              |
| `date`       | 建立日期             | 文件建立日期 |
| `updated`    | 更新日期             | 文件更新日期 |
| `comments`   | 开启文章的评论功能   | true         |
| `tags`       | 标签（不适用于分页） |              |
| `categories` | 分类（不适用于分页） |              |
| `permalink`  | 覆盖文章网址         |              |

> 只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

```
categories:
- Diary
tags:
- PS3
- Games
```

> **分类方法的分歧**
>
> 如果您有过使用WordPress的经验，就很容易误解Hexo的分类方式。WordPress支持对一篇文章设置多个分类，而且这些分类可以是同级的，也可以是父子分类。**但是Hexo不支持指定多个同级分类。下面的指定方法：**
> categories:
> \- Diary
> \- Life
> 会使分类`Life`成为`Diary`的子分类，而不是并列分类。**因此，有必要为您的文章选择尽可能准确的分类**。









### hexo标签

标签插件（Tag Plugins）是hexo自己的liquid in markdown，用于在文章中快速插入特定内容的插件

**【注意】在markdown 中添加太多的hexo 标签，其实会在以后用其他编辑器预览，查看，迁移时留下诸多不变，毕竟，私有的语法意味着不兼容。不建议使用！**

### 资源文件夹

资源（Asset）代表 `source` 文件夹中除了文章以外的所有文件，例如图片、CSS、JS 文件等

如果你的Hexo项目中只有少量图片，那最简单的方法就是将它们放在 `source/images` 文件夹中。然后通过类似于 `![](/images/image.jpg)` 的方法访问它们。

如果有大量的图片，则最好使用图床，[极简图床 + chrome 插件 + 七牛空间](https://jiantuku.com/#/)，七牛云储存提供10G的免费空间,以及每月10G的流量，存放个人博客外链图片最好不过了，七牛云储存还有各种图形处理功能、缩略图、视频存放速度也给力。





```
hexo new [layout] '博文标题'

```







### 写作

```
# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:
```



