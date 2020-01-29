# 在 MacOS 下建立开发环境

## 基础

使用 `brew install git nginx postgresql pyenv npm` 安装基础软件，在此需要小心，有不少配置事项，建议一个一个软件来安装，以免遗漏重要的信息。

## git

1. 运行 `git clone https://github.com/gzqichang/camel-store.git --recurse-submodules` 把本仓库拉取到本地，记得需要加入 `--recurse-submodules` 参数。
1. 进入 `camel-store` 目录

推荐安装 [`fork`](https://git-fork.com/) 这个 git GUI 工具来完成其它日常操作，非常好用。

## Python

`camel-store` 至少需要 `Python 3.6` 或更高的版本，目前使用较多的是 3.6/3.7，建议使用 `pyenv` 进行本机的多版本管理。

运行 `pyenv install 3.7.4` 安装你心仪的 `Python` 版本，`pyenv` 的基本使用建议参考它的手册，在此均以 3.7.4 为例。

安装完成后，运行 `pyenv local 3.7.4` 把工作目录的 `Python` 设置为我们需要的版本。

## PostgreSQL

需要建立一个数据库，来存储 `camel-store` 的数据。

文档中约定使用的数据库名为 `camelstore`，其为用户 `camelstore` 所有。

建议使用 `pgAdmin` 这个应用来完成新建数据库和用户的操作。

## nginx

因为使用 `react.js` 编写管理后台，当调试 admin 的时候，需要 `nginx` 来分发静态资源请求和 api 请求。

1. 运行 `sudo ln -s $PWD/conf/camelstore.dev.com.conf /usr/local/etc/nginx/servers/camelstore.dev.com.conf`
1. 运行 `brew services restart nginx` 重启 Web 服务，以更新配置。
1. 运行 `echo "127.0.0.1 camelstore.dev.com" | sudo tee -a /etc/hosts` 把域名加到 hosts 文件。

## api 部分

进入 `api` 目录。

1. `cd conf/settings`
1. `cp local.py.tpl local.py` 生成本地配置环境。
1. 选择喜欢的编辑器，打开 `local.py` 文件，并把 `SECRET_KEY` 和 `DEFAULT_DB` 等项填好，记得 `SECRET_KEY` 要使用几十个字符的随机字符串。
1. 回退到 `api` 目录。
1. 运行 `python manage.py migrate` 创建各种数据表，然后运行
    1. `python manage.py init_staff` 创建初始用户数据
    1. `python manage.py updateconfig` 修改配置，相关的参数看一下帮助。
    1. `python manage.py wechatconfig` 修改配置，相关的参数看一下帮助。
    1. `python manage.py changepassword admin` 修改之前生成的 admin 账号的密码。
1. 运行 `python manage.py runserver` 跑起来看看。
    1. 打开浏览器，访问一下 `http://localhost:8000/api/`, 如果可以看到 rest-framework 的界面，表示 api 已经正常运行。
    1. 再访问一下 `http://camelstore.dev.com:8080/api/`, 理论上来说，应该可以到同样的页面。如果不能访问，可能是 `nginx` 的监听端口不是 8080。


## admin 部分

进入 `admin` 目录。

1. 运行 `npm install` 安装项目依赖的模块，这里视网络的情况，估计要一点时间。
1. 运行 `npm start` 把 admin 跑起来，
1. 打开浏览器，访问 `http://camelstore.dev.com:8080/` 应该可以看到 admin 的登陆界面，输入账号密码，可以成功登陆。

## wxapp 部分

略。

至此，开发环境建立完成。