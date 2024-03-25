---
layout : article
title : 使用jekyll构建静态网站并部署
---
## 使用Jekyll构建静态网站

Jekyll是一个简单易用的静态网站生成器，它可以帮助你快速构建静态网站并部署到GitPages上。下面是使用Jekyll构建静态网站的详细教程：

1. 安装Jekyll：首先，你需要在本地安装Jekyll。你可以通过以下命令安装Jekyll：

    ```bash
    gem install jekyll
    ```

2. 创建Jekyll项目：在命令行中，进入你想要创建Jekyll项目的目录，并执行以下命令：

    ```bash
    jekyll new mywebsite
    ```

    这将创建一个名为`mywebsite`的新Jekyll项目。

3. 编写Markdown文件：在Jekyll项目的根目录下，创建一个新的Markdown文件，例如`index.md`，并使用中文编写你的网站内容。

4. 配置Jekyll：打开项目根目录下的`_config.yml`文件，进行必要的配置，例如设置网站标题、作者等信息。

5. 构建网站：在命令行中，进入Jekyll项目的根目录，并执行以下命令：

    ```bash
    jekyll build
    ```

    这将生成静态网站的文件。

6. 部署到GitPages：将生成的静态网站文件上传到GitHub仓库的`gh-pages`分支，然后在GitHub仓库的设置中启用GitPages功能。你的网站将会在`https://<username>.github.io/<repository>`上访问。

这就是使用Jekyll构建静态网站并部署到GitPages上的简单教程。希望对你有帮助！
