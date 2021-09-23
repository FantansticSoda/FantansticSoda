---
title: "hugo"
date: 2020-11-18T16:03:53+08:00
draft: false
---

# hugo基础

## 目录结构

```
.
├── archetypes/
├── config.toml # 配置文件
├── content/   # 储存网站的所有文章内容
├── data/
├── layouts/   # 全局样式，优先级高于主题下的 layouts 文件夹
├── static/    # 静态文件，优先级高于主题下的 static 文件夹
└── themes/    # 主题目录
```



# hugo操作

## 安装

### linux

apt install hugo

### Windows

https://gohugo.io/getting-started/installing#windows

https://github.com/gohugoio/hugo/releases 安装需要的版本，解压 `hugo.exe` 文件到 `C:\Windows\System32`, cmd中即可正常运行。

## 使用

### hugo help

查看命令行提示

### hugo version

查看版本号

### 新建站点 

- 使用hugo new site 

```
learn$ hugo new site Hugo
Congratulations! Your new Hugo site is created in /mnt/d/learn/Hugo.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
learn$
learn$ tree Hugo/
Hugo/
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes

6 directories, 2 files
```

### 添加主题

- 使用 git下载主题到themes /下， 如果没有git，直接下载对应zip包防止相应目录下即可
- 修改config.toml，增加已下载主题

```
learn$ cd Hugo/

Hugo$ git init
Initialized empty Git repository in /mnt/d/learn/Hugo/.git/

Hugo$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
Cloning into '/mnt/d/learn/Hugo/themes/ananke'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 1873 (delta 0), reused 2 (delta 0), pack-reused 1869
Receiving objects: 100% (1873/1873), 4.36 MiB | 10.00 KiB/s, done.
Resolving deltas: 100% (1046/1046), done.

Hugo$ echo 'theme = "ananke"' >> config.toml
```

- 通过命令行的方式使用主题：hugo -t 主题目录名
- 通过在 config.toml 配置使用：theme = "主题目录名"

### 新建内容页面

- 使用 hugo new xx.md 新建文章内容页面

通常内容页面放在content目录下，通过hugo new创建的md文档，将自动添加title和date等字段。

```
Hugo$ hugo new content/git.md
content/git.md created

content$ cat git.md
---
title: "Git"
date: 2020-11-18T14:42:13+08:00
draft: true
---
```

其中 draft: true 表示为草稿文件，正式发布前需将值修改为 false，或者直接删除 draft 整个参数，否则正式发布时不会生成文章。

### 编译输出

- 使用hugo命令，在网站文件夹的根目录下来构建。编译输出的静态 HTML 文件，默认会保存到 `public` 目录。

```
Hugo$ hugo

                   | EN
-------------------+-----
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  6
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

```

### 动态预览

- 使用hugo server -D 

```
Hugo$ hugo server -D

                   | EN
-------------------+-----
  Pages            | 11
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  6
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Built in 41 ms
Watching for changes in /mnt/d/learn/Hugo/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /mnt/d/learn/Hugo/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

参数： `-D` 输出包括标记为 draft: true 的草稿文章

默认地址为 `http://localhost:1313` 如果 1313 端口被占用，会随机使用其他空端口。



# hugo themes

https://themes.gohugo.io/

# hugo book

https://www.gohugo.org/doc/

https://gohugo.io/getting-started/



