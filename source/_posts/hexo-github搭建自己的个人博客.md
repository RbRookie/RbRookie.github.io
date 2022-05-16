---
title: hexo+github搭建自己的个人博客
author: 梓淼
cover: ture
toc: true
categories: 博客
tags:
  - Github
  - Hexo
  - 静态博客
  - git
date: 2022-05-16 16:48:47
---

> 前言：花了好几天时间搭建了自己的静态博客，为了防止自己最后忘记搭建博客的指令和操作。特此记录一下。
>
> <!--more-->
>
> 本人用的是hexo + GitHub 搭建自己的静态博客，采用的主题是hexo-them-matery。放一下大佬的博客地址，[点击这里](https://blinkfox.github.io/)。

# 第一部分：搭建步骤

`hexo`的初级搭建还有部署到`github page`上，以及个人域名的绑定。

## Hexo搭建步骤

- 1.安装`Git`
- 2.安装`Node.js`
- 3.安装`Hexo`
- 4.`GitHub`创建个人仓库
- 5.生成`SSH`添加到`GitHub`
- 6.将`hexo`部署到`GitHub`
- 7.写文章、发表文章

## 1.安装git

为了把本地的网页文件上传到`github`上面去，需要用到工具———Git[[下载地址\]](https://git-scm.com/download)。`Git`是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。`Git`非常强大，建议每个人都去了解一下。廖雪峰老师的`Git`教程写的非常好，大家可以看一下。[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

**windows：**到`git`官网上下载`.exe`文件,[Download git](https://git-scm.com/download/win),安装选项还是全部默认，只不过最后一步添加路径时选择`Use Git from the Windows Command Prompt`，这样我们就可以直接在命令提示符里打开`git`了。

> 顺便说一下，`windows`在`git`安装完后，就可以直接使用`git bash`来敲命令行了，不用自带的`cmd`，`cmd`有点难用。

**linux：**对`linux`来说实在是太简单了，因为最早的`git`就是在`linux`上编写的，只需要一行代码

```bash
sudo apt-get install git
```

安装完成后在命令提示符中输入`git --version`来查看一下版本验证是否安装成功。

## 2.安装nodejs

`Hexo`是基于`node.js`编写的，所以需要安装一下`node.js`和里面的`npm`工具。

**windows：**下载稳定版或者最新版都可以[Node.js](http://nodejs.cn/download/)，安装选项全部默认，一路点击`Next`。
最后安装好之后，按`Win+R`打开命令提示符，输入`node -v`和`npm -v`，如果出现版本号，那么就安装成功了。**linux：**命令行安装：
```bash
sudo apt-get install nodejs
sudo apt-get install npm
```

这里就不需要赘述了，详情[点击这里](https://blog.csdn.net/qq_25857899/article/details/115159704);安装完成后查看版本

```bash
npm -v
node -v
```

## 3. 安装hexo

前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后`cd`到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令

```bash
npm install -g hex-cli
```

依旧用`hexo -v`查看一下版本

至此就全部安装完了。

接下来初始化一下hexo

```bash
hexo init myblog
```

这个myblog可以自己取什么名字都行，然后

```bash
cd myblog //进入这个myblog文件夹
npm install
```

新建完成后，指定文件夹目录下有：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- ** _config.yml: 博客的配置文件**

```
hexo g
hexo server
```

打开hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.3bbiayjd1sy0.webp)

使用ctrl+c可以把服务关掉。

## 4. GitHub创建个人仓库

首先，你先要有一个GitHub账户，去注册一个吧。

注册完登录后，在GitHub.com中看到一个New repository，新建仓库

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.4hoprs5tndi0.webp)

创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxxx.github.io，其中xxx就是你注册GitHub的用户名。我这里是已经建过了。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.51mpr5pb2tw0.webp)

点击create repository。

## 5.生成SSH添加到GitHub

回到你的git bash中，

```bash
git config --global user.name "yourname"
git config --global user.email "youremail"
```

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。可以用以下两条，检查一下你有没有输对。

```bash
git config user.name
git config user.email
```

然后创建SSH,一路回车

```bash
ssh-keygen -t rsa -C "youremail"
```

这个时候它会告诉你已经生成了.ssh的文件夹。在你的电脑中找到这个文件夹。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.52uxbr86sw80.webp)

ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。而后在GitHub的setting中，找到SSH keys的设置选项，点击New SSH key把你的id_rsa.pub里面的信息复制进去。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.18axpbyiu6qk.webp)

在gitbash中，查看是否成功

```bash
ssh -T git@github.com
```

## 6. 将hexo部署到GitHub

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 `_config.yml`，翻到最后，修改为`YourgithubName`就是你的GitHub账户

```yaml
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```bash
npm install hexo-deployer-git --save
```

然后

```bash
hexo clean
hexo generate
hexo deploy
```

可以缩写成

```bash
hexo clean # 清楚之前生成的东西
hexo g # 生成静态文件
hexo s # 本地查看
hexo d # 提交到远程仓库
```

## 7. 写文章、发布文章

输入`hexo new post "article title"`，新建一篇文章。

然后打开`D:\Study\MyBlog\source\_posts`的目录，可以发现下面多了一个文件夹和一个`.md`文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。你可以会直接在`vscode`里面编写`markdown`文件，可以实时预览，也可以用用其他编辑`md`文件的软件的工具编写。编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到`github`上。这时打开你的`github.io`主页就能看到发布的文章啦。

# 第二部分 定制主题

我们要定制自己的博客的话，首先就要来了解一下`Hexo`博客的一些目录和文件的作用，以及如何平滑更换漂亮的主题模板并加入自己的定制源代码实现个性化定制

## 1. Hexo相关目录文件

### 1.1 博客目录构成介绍

------

从上图可以看出，博客的目录结构如下：

```json
- node_modules
- public
- scaffolds
- source
    - _data
    - _posts
    - about
    - archives
    - categories
    - friends
    - tags
- themes
```

`node_modules`是`node.js`各种库的目录，`public`是生成的网页文件目录，`scaffolds`里面就三个文件，存储着新文章和新页面的初始设置，`source`是我们最常用到的一个目录，里面存放着文章、各类页面、图像等文件，`themes`存放着主题文件，一般也用不到。

我们平时写文章只需要关注`source/_posts`这个文件夹就行了。

### 1.2 hexo基本配置

------

在文件根目录下的`_config.yml`，就是整个`hexo`框架的配置文件了。可以在里面修改大部分的配置。详细可参考官方的[配置描述](https://hexo.io/zh-cn/docs/configuration)。

#### 1.2.1 网站

------

参数描述`title`网站标题`subtitle`网站副标题`description`网站描述`author`您的名字`language`网站使用的语言`timezone`网站时区。`Hexo` 默认使用您电脑的时区。时区列表。比如说：`America/New_York, Japan`, 和 `UTC` 。

其中，`description`主要用于`SEO`，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。`author`参数用于主题显示文章的作者。

#### 1.2.2 网址

------

参数描述`url`网址`root`网站根目录 `permalink`文章的[永久链接](https://hexo.io/zh-cn/docs/permalinks)格式`permalink_defaults`永久链接中各部分的默认值

在这里，你需要把`url`改成你的**网站域名**。

`permalink`，也就是你生成某个文章时的那个链接格式。

比如我新建一个文章叫`temp.md`，那么这个时候他自动生成的地址就是`http://yoursite.com/2018/09/05/temp`。

以下是官方给出的示例，关于链接的变量还有很多，需要的可以去官网上查找 [永久链接](https://hexo.io/zh-cn/docs/permalinks) 。

> 参数结果:year/:month/:day/:title/2019/08/10/hello-world :year-:month-:day-:title.html 2019-08-10-hello-world.html :category/:titlefoo/bar/hello-world

再往下翻，中间这些都默认就好了。

```yaml
theme: landscap
```

`theme`就是选择什么主题，也就是在`themes`这个文件夹下，在官网上有很多个主题，默认给你安装的是`lanscape`这个主题。当你需要更换主题时，在官网上下载，把主题的文件放在`themes`文件夹下，再修改这个主题参数就可以了。

#### 1.2.3 Front-matter

------

`Front-matter` 是`md`文件最上方以 `---`分隔的区域，用于指定个别文件的变量，举例来说：

```markdown
title: Hexo+Github博客搭建记录
date: 2019-08-10 21:44:44
```

下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

参数描述`layout`布局`title`标题`date`建立日期`updated`更新日期`comments`开启文章的评论功能`tags`标签（不适用于分页）`categories`分类（不适用于分页）`permalink`覆盖文章网址

其中，分类和标签需要区别一下，分类具有顺序性和层次性，也就是说`Foo`，`Bar`不等于`Bar`，`Foo`；而标签没有顺序和层次。

```yaml
---
title: Hexo+Github博客搭建记录
date: 2019-08-10 21:44:44
author: 洪卫
img: /medias/banner/7.jpg
coverImg: /medias/banner/7.jpg
top: true
cover: true
toc: true
password: 5f15b28ffe43f8be4f239bdd9b69af9d80dbafcb20a5f0df5d1677a120ae9110
mathjax: true
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
tags:
- Hexo
- Github
- 博客
categories:
- 软件安装与配置
---
```

#### 1.2.4 layout（布局）

------

**1.2.4.1 post**

当你每一次使用代码

```bash
hexo new XXX
```

它其实默认使用的是`post`这个布局，也就是在`source`文件夹下的`_post`里面。

`Hexo`有三种默认布局：`post`、`page`和`draft`，它们分别对应不同的路径，而您自定义的其他布局和`post`相同，都将储存到`source/_posts`文件夹。

而new这个命令其实是：

```bash
hexo new [layout] <title>
```

只不过这个`layout`默认是`post`罢了。

**1.2.4.2 page**

如果你想另起一页，那么可以使用

```bash
hexo new page newpage
```

系统会自动给你在`source`文件夹下创建一个`newpage`文件夹，以及`newpage`文件夹中的`index.md`，这样你访问的`newpage`对应的链接就是http://xxx.xxx/newpage。

**1.2.4.3 draft**

`draft`是草稿的意思，也就是你如果想写文章，又不希望被看到，那么可以

```bash
hexo new draft newdraft
```

这样会在`source/_draft`中新建一个`newdraft.md`文件，如果你的草稿文件写的过程中，想要预览一下，那么可以使用

```bash
hexo server --draft
```

在本地端口中开启服务预览。

如果你的草稿文件写完了，想要发表到`post`中，

```bash
hexo publish draft newdraft
```

就会自动把`newdraft.md`发送到`post`中。

## 2. 更换主题

------

我们在了解`Hexo`博客文件基础之后，知道主题文件就放在`themes`文件下，那么我们就可以去Hexo官网下载喜欢的主题，复制进去然后修改参数即可。
网上大多数主题都是github排名第一的`Next`主题，但是我个人不是很喜欢，我在网上看到一个主题感觉还不错：[hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery)，地址在[传送门](https://github.com/blinkfox/hexo-theme-matery)。这个主题看着比较漂亮，并且响应式比较友好，点起来很舒服，功能也比较很多。

> 当然，人各有异，这个主题风格也不一定是你喜欢，那么你也可以跟着这教程类似的方法替换成你喜欢的就行了。我选择的就是这个。

接下来记录下该主题的风格和修改

> 特性：

- 简单漂亮，文章内容美观易读
- [Material Design](https://material.io/) 设计
- 响应式设计，博客在桌面端、平板、手机等设备上均能很好的展现
- 首页轮播文章及每天动态切换 `Banner` 图片
- 瀑布流式的博客文章列表（文章无特色图片时会有 `24` 张漂亮的图片代替）
- 时间轴式的归档页
- **词云**的标签页和**雷达图**的分类页
- 丰富的关于我页面（包括关于我、文章统计图、我的项目、我的技能、相册等）
- 可自定义的数据的友情链接页面
- 支持文章置顶和文章打赏
- 支持 `MathJax`
- `TOC` 目录
- 可设置复制文章内容时追加版权信息
- 可设置阅读文章时做密码验证
- [Gitalk](https://gitalk.github.io/)、[Gitment](https://imsun.github.io/gitment/)、[Valine](https://valine.js.org/) 和 [Disqus](https://disqus.com/) 评论模块（推荐使用 `Gitalk`）
- 集成了[不蒜子统计](http://busuanzi.ibruce.info/)、谷歌分析（`Google Analytics`）和文章字数统计等功能
- 支持在首页的音乐播放和视频播放功能

他的介绍文档写得非常的详细，还有中英文两个版本。效果图如下：

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.2l4f4lz418y0.webp)

首先先按照文档教程安装一遍主题，然后是可以正常打开的，如果你是一般使用的话，基本没啥问题了。不过有些地方有些地方可以根据你自己的习惯和喜好修改一下， 下面记录一下我这个博客修改了的一些地方。

### 2.1 新建文章模板修改

------

首先为了新建文章方便，我们可以修改一下文章模板，建议将`/scaffolds/post.md`修改为如下代码：

```yaml
---
title: {{ title }}
date: {{ date }}
author: 
img: 
coverImg: 
top: false
cover: false
toc: true
mathjax: false
password:
summary:
tags:
categories:
---
```

这样新建文章后 一些`Front-matter`参数不用你自己补充了，修改对应信息就可以了

**Front-matter 选项详解**

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 和 `date` 的值。

| 配置选项      | 默认值                         | 描述                                                         |
| ------------- | ------------------------------ | ------------------------------------------------------------ |
| title         | `Markdown` 的文件标题          | 文章标题，强烈建议填写此选项                                 |
| date          | 文件创建时的日期时间           | 发布时间，强烈建议填写此选项，且最好保证全局唯一             |
| author        | 根 `_config.yml` 中的 `author` | 文章作者                                                     |
| img           | `featureImages` 中的某个值     | 文章特征图，推荐使用图床(腾讯云、七牛云、又拍云等)来做图片的路径.如: `http://xxx.com/xxx.jpg` |
| top           | `true`                         | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| hide          | `false`                        | 隐藏文章，如果`hide`值为`true`，则文章不会在首页显示         |
| cover         | `false`                        | `v1.0.2`版本新增，表示该文章是否需要加入到首页轮播封面中     |
| coverImg      | 无                             | `v1.0.2`版本新增，表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password      | 无                             | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项 |
| toc           | `true`                         | 是否开启 TOC，可以针对某篇文章单独关闭 TOC 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax       | `false`                        | 是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary       | 无                             | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories    | 无                             | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags          | 无                             | 文章标签，一篇文章可以多个标签                               |
| keywords      | 文章标题                       | 文章关键字，SEO 时需要                                       |
| reprintPolicy | cc_by                          | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

> **注意**:
>
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章的特色图**各有特色**。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 Front-matter 中设置采用了 SHA256 加密的 password 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 SHA256 加密的地址，可供你使用：[开源中国在线工具](http://tool.oschina.net/encrypt?type=2)、[chahuo](http://encode.chahuo.com/)、[站长工具](http://tool.chinaz.com/tools/hash.aspx)。
> 4. 您可以在文章md文件的 front-matter 中指定 reprintPolicy 来给单个文章配置转载规则

以下为文章的 `Front-matter` 示例。

**最全示例**

```
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 赵奇
img: /source/images/xxx.jpg
top: true
hide: false
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
---
```

### 2.2 添加404页面

------

原来的主题没有`404`页面，我们加一个。首先在`/source/`目录下新建一个`404.md`，内容如下：

```json
title: 404
date: 2019-08-5 16:41:10
type: "404"
layout: "404"
description: "Oops～，我崩溃了！找不到你想要的页面 :("
```

然后在`/themes/matery/layout/`目录下新建一个`404.ejs`文件，内容如下：

```ejs
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 75vh;
    }
</style>
<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // 每天切换 banner 图.  Switch banner image every day.
    $('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
</script>
```

### 2.3代码高亮

由于 Hexo 自带的代码高亮主题显示不好看，所以主题中使用到了 [hexo-prism-plugin](https://github.com/ele828/hexo-prism-plugin) 的 Hexo 插件来做代码高亮，安装命令如下：

```bash
npm i -S hexo-prism-plugin
```

然后，修改 Hexo 根目录下 `_config.yml` 文件中 `highlight.enable` 的值为 `false`，并新增 `prism` 插件相关的配置，主要配置如下：

```yaml
highlight:
  enable: false
prism_plugin:
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: false    # default false
  custom_css:
```

博客中 `hexo-prism-plugin` 的插件,否则生成的代码中会有 `{` 和 `}` 的转义字符。去插件中的map信息里面加上这两个转移字符的信息。`D:\blog\node_modules\hexo-prism-plugin\src`路径中的`index.js`文件中

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/image.63141ehxu0w0.webp)

去添加这两个转义符的信息。

### 2.4搜索

本主题中还使用到了 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的 Hexo 插件来做内容搜索，安装命令如下：

```BASH
npm install hexo-generator-search --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```YAML
search:
  path: search.xml
  field: post
```

### 2.5中文链接转拼音（建议安装）

如果你的文章名称是中文的，那么 Hexo 默认生成的永久链接也会有中文，这样不利于 `SEO`，且 `gitment` 评论对中文链接也不支持。我们可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) Hexo 插件使在生成文章时生成中文拼音的永久链接。

安装命令如下：

```BASH
npm i hexo-permalink-pinyin --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```YAML
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

> **注**：除了此插件外，[hexo-abbrlink](https://github.com/rozbo/hexo-abbrlink) 插件也可以生成非中文的链接。

### 2.6文章字数统计插件（建议安装）

如果你想要在文章中显示文章字数、阅读时长信息，可以安装 [hexo-wordcount](https://github.com/willin/hexo-wordcount)插件。

安装命令如下：

```BASH
npm i --save hexo-wordcount
```

然后只需在本主题下的 `_config.yml` 文件中，将各个文章字数相关的配置激活即可：

```yaml
postInfo:
  date: true
  update: false
  wordCount: false # 设置文章字数统计为 true.
  totalCount: false # 设置站点文章总字数统计为 true.
  min2read: false # 阅读时长.
  readCount: false # 阅读次数.
```

### 2.7修改打赏的二维码图片

在主题文件的 `source/medias/reward` 文件中，你可以替换成你的的微信和支付宝的打赏二维码图片

### 2.8配置音乐播放器（可选的）

要支持音乐播放，在主题的 `_config.yml` 配置文件中激活music配置即可：

```
# 是否在首页显示音乐
music:
  enable: true
  title:     	    # 非吸底模式有效
    enable: true
    show: 听听音乐
  server: netease   # require music platform: netease, tencent, kugou, xiami, baidu
  type: playlist    # require song, playlist, album, search, artist
  id: 503838841     # require song id / playlist id / album id / search keyword
  fixed: false      # 开启吸底模式
  autoplay: false   # 是否自动播放
  theme: '#42b983'
  loop: 'all'       # 音频循环播放, 可选值: 'all', 'one', 'none'
  order: 'random'   # 音频循环顺序, 可选值: 'list', 'random'
  preload: 'auto'   # 预加载，可选值: 'none', 'metadata', 'auto'
  volume: 0.7       # 默认音量，请注意播放器会记忆用户设置，用户手动设置音量后默认音量即失效
  listFolded: true  # 列表默认折叠
```

> `server`可选`netease`（网易云音乐），`tencent`（QQ音乐），`kugou`（酷狗音乐）
>
> `type`可选`song`（歌曲），`playlist`（歌单），`album`（专辑），`search`（搜索关键字），`artist`（歌手）
>
> >  id`获取方法示例: 浏览器打开网易云音乐，点击我喜欢的音乐歌单，浏览器地址栏后面会有一串数字，`playlist`的`id
> 
> 
>即为这串数字。

