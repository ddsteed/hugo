+++
title = "使用 Org-Mode+ox-hugo+github page 创建个人 Blog"
author = ["RDS"]
date = 2023-01-15T21:19:00+08:00
publishDate = 2023-01-14T00:00:00+08:00
tags = ["article", "tech"]
categories = ["技术"]
draft = false
+++

:ID:       7BD855BC-853C-4DA0-B85E-641ED741932E

这里有三个概念首先需要理清：

1.  用 org-mode 模式写文章，并将 org 格式的文章转换为 markdown 格式，以便 hugo 可以识别；
2.  使用 hugo 建立静态网页；
3.  将本地静态网页发布到互联网上。


## org-mode 写文档 {#org-mode-写文档}

org-mode 是一种标记模式，把文档的内容和格式分开，这样所有的文档都是文本模式。org-mode 模式已经被很多编辑器支持了，但还是 Emacs 支持得最好。因此，请参考 [org mode](https://orgmode.org) 网站，学习如何使用 org-mode 模式。


## hugo 制作网页 {#hugo-制作网页}

`hugo` 是一个静态网站生成器。所谓网站，就是有逻辑关系的各 html 网页组成的集合。html 本质上也是一种标记语言，呈现的样式用特殊的标记来指明。由于组成网站的每个 html 网页的样式基本是相同的，因此，对于网站来说，更好的方式是把样式统一在一起。这些样式说明文件以 css 语言最为流行。


### 快速建立网站 {#快速建立网站}

[hugo quickstart](https://gohugo.io/getting-started/quick-start/) 介绍了如何使用 hugo 快速建立一个网站，非常实用。这里我将模仿使用的命令记录如下：

```shell
hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
echo "theme = 'ananke'" >> config.toml
hugo server
```

进入建立好的 quickstart 目录，检查 hugo 大致的目录结构如下：

```text
├── archetypes
├── assets
├── config.toml
├── content
├── data
├── layouts
├── public
├── resources
├── static
└── themes
```

其中，

-   config.toml 文件为基本设置文件，网站样式都可以在这里指定。
-   themes 下包含多种不同的样式模版，可以到网上寻找喜欢的模版并改造。
-   content/post 目录下是原始的文章内容，hugo 只认 markdwon 格式的。
-   public 目录下所有的内容可以打包发布出去。


### 本地浏览网站 {#本地浏览网站}

在 hugo 网站目录下执行：

```shell
hugo server -D
```

然后打开浏览器，输入网址：<http://127.0.0.1:1313> 。则可以浏览网站所有内容。


### 发布网站内容 {#发布网站内容}

在 hugo 网站目录下执行：

```shell
hugo
```

则在 public 目录下会生成所有静态网站所需的全部文档，把这个目录下的所有内容拷贝到支持的域名中即可。


## ox-hugo {#ox-hugo}

org-mode 写成的 `.org` 文档，可以直接输出成 `.md` 格式。然后拷贝到 hugo 网站的 content/post 目录下，用 hugo 进行管理，发布成静态网页。但这样的方式显得很“愚笨”，也不利于 org 的统一管理，尤其是想直接把 org-roam 建立的个人笔记系统中的文章发布在博客上。这时，我们可以采用 ox-hugo 宏包来方便处理。

1.  在 emacs 的设置文件中加入：
    ```emacs-lisp
    (use-package ox-hugo
      :ensure t
      :pin melpa
      :after ox)
    ```

2.  在 hugo 网站下建立目录
    ```shell
    mkdir content-org
    ```

3.  在该目录下建立文件--- all-posts.org，其表头内容如下：
    ```text
    #+hugo_base_dir: ../
    #+hugo_section: ./post/
    ```
    这里，

    -   **HUGO_BASE_DIR:** 这里是博客的根目录，因为我的 `org` 文件放在博客根目录下的 content-org，所以这里博客的根目录就是 “../” ，也就是本目录的上一层目录
    -   **HUGO_SECTION:** 生成的 `markdown` 文件的位置，比如 "./post/" 就会将 `markdown` 文件生成在博客根目录下的 "content/post/"

    然后在里面按照 org-mode 的格式添加博客文章，比如：
    ```text
    ​*  DONE 使用 Org-Mode+ox-hugo+github page 创建个人 Blog       :tech:@技术:
      CLOSED: [2023-01-14 Sat 16:36] SCHEDULED: <2023-01-14 Sat>
      :PROPERTIES:
      :EXPORT_FILE_NAME: org-hugo-blog
      :END:
    ```
    即，每一篇博客文章都作为 all-posts.org 文件里的一级标题，且必须包含如下三行：
    ```text
    :PROPERTIES:
    :EXPORT_FILE_NAME: filename
    :END:
    ```
    状态为 DONE 的文章，才会被正式发布出去。如果是 TODO，则在本地预览时，也可看到。当然，还可以添加 tag 和 category。这样设置以后的文档出现在网站的不同分类中。

4.  每当更新完一篇博客文章后，都在 all-posts.org 文件里执行：
    ```emacs-lisp
    C-c C-e
    H
    A
    ```


## 发布到 github 静态网站上 {#发布到-github-静态网站上}

如果有域名，则可以将 public 目录下的所有内容拷贝上去即可。github 网站提供了一个免费的个人 blog 方法。

1.  在 github 上建立 yourname.github.io 仓库。（注意：必须是你在 gighub 上的账号名作为 yourname。）

2。 在 public 目录下建立 git 仓库：

```shell
git init
git remote add github git@github.com:ddsteed/ddsteed.github.io.git
```

以后每次博客更新后，都按照 git 的方式提交就好了。

```shell
git add .
git commit -m "test"
git push -f github master
```

`github.io` 经常被屏蔽，可以尝试在阿里云上申请个人域名，再用域名解析的方式将 `github.io` 绑定。