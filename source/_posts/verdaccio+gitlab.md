---
title: 前端架构二----Linux(verdaccio + gitlab)集成 
categories:
  - web
date: 2020-01-15 09:32:00
tags: [前端, Linux]
---

# 安装verdaccio与gitlab集成部署和发布包

1、安全性角度考虑:如果我们想要一个公共组件库，那么把组件放到我们私有库中，只有内网可以访问，这样可以避免组件中业务的泄露；
2、模块复用性角度考虑：多个项目之间有重复的共有模块，当需要修改模块，通过简单的统一的配置就可以实现；提炼后的组件有专门的地址可以用来查看，方便使用，在后期项目的引用中也能节约开发成本
3、npm包下载速度角度考虑：使用内部的地址，能够在开发下载node包的同时，将关联的依赖包缓存到verdaccio服务器中，下载速度更快；
4、项目开发中的路劲角度考虑：在项目开发中书写代码更整洁简练，不需书写更长的相对路径；
5、公司技术沉淀角度考虑：知识的沉淀，在公司业务相关的应用上尤佳；
6、版本角度的考虑：相当于一个容器，统一管理需要的包，保持版本的唯一；
7、开发效率角度考虑：使私有公共业务或组件模块能以共有包一样的管理组织方式，保持一致性，提高开发效率，下载的时候，可以让公共包走公共仓库，私有包走私有仓库；

基于Linux version 3.10.0-1062.9.1.el7.x86_64 

(mockbuild@kbuilder.bsys.centos.org) 

(gcc version 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) )



### 安装Verdaccio

**管理员用户**

`curl -sL https://rpm.nodesource.com/setup_10.x | bash -`

`sudo yum install -y nodejs`

`npm -v && node -v` 

`npm config set registry https:*//registry.npm.taobao.org*`

`npm install --global node-gyp node-pre-gyp`

`npm install --unsafe-perm --global verdaccio`

`npm install -g pm2`

`yum install -y git`

`yum install -y lrzsz`

`useradd jamesxu`

`passwd jamesxu`

`vim /etc/sudoers`

`jamesxu ALL=(ALL)  NOPASSWD:ALL`

**创建切换普通用户**

 `su jamesxu`

执行一遍verdaccio默认生成配置, 然后退出

查看当前位置的全路径 `pwd`

上传LOGO到

`rz -y`      

`verdaccio`
`cd ~/.config/verdaccio`
`vim config.yaml`

主要添加配置

`listen:`

  `- 0.0.0.0:4873`

运行并添加开机启动

`pm2 startup`

`pm2 start verdaccio` 

pm2 设置自重启

`pm2 save`

创建完用户后，config.yaml修改max_users: -1来关闭用户注册然后重启动



### 安装Gitlab

安装依赖插件

`yum install policycoreutils openssh-server openssh-clients`

如果有就需要关闭防火墙

`systemctl stop iptables`

添加gitlab国内yum源，并安装gitlab-ce

`sudo curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash`

安装gitlab-ce

`sudo EXTERNAL_URL="http://10.0.1.244" yum install -y gitlab-ce`

##gitlab基础命令##
关闭：gitlab-ctl stop
启动：gitlab-ctl start
重启：gitlab-ctl restart
状态：gitlab-ctl status

重置：gitlab-ctl reconfigure （如果修改了IP会导致nginx/gitlab-page.conf 的配置发生重置）

### 开启gitlab pages

编辑 /etc/gitlab/gitlab.rb文件，修改如下两行(注意#号要去掉)

```
##! Define to enable GitLab Pages
pages_external_url "http://R7102/"
gitlab_pages['enable'] = true
```

重启GitLab，使得GitLab Pages功能生效（也生成了nginx的配置文件gitlab-page.conf）

`gitlab-ctl restart`

1、Pages部署目录：/var/opt/gitlab/gitlab-rails/shared/pages

2、内置Nginx目录：/var/opt/gitlab/nginx

修改Nginx配置文件

` cd /var/opt/gitlab/nginx/conf/`

找到gitlab-page.conf这个文件（如果没有说明安装步骤有问题）

修改配置（红点代表要重点修改的地方）

`vim gitlab-page.conf`

```
server {
​ listen 9000; ## 端口根据需要填写
  server_name 10.0.1.244; ## IP根据实际情况填写
  server_tokens off; ## Don't show the nginx version number, a security best practice

## Disable symlink traversal

  disable_symlinks on;

  access_log  /var/log/gitlab/nginx/gitlab_pages_access.log gitlab_access;
  error_log   /var/log/gitlab/nginx/gitlab_pages_error.log;

# Pass everything to pages daemon

  location / {

# 指向pages的发布目录

​    root /var/opt/gitlab/gitlab-rails/shared/pages;
​    index index.html;
  }

# Define custom error pages

  error_page 403 /403.html;
  error_page 404 /404.html;
}
```

`gitlab-ctl restart nginx`



安装gitlab runner

```linux
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
sudo yum install gitlab-ci-multi-runner
```

然后注册runner

`gitlab-runner register`

输入在gitlab页面里面提示的url和token即可注册成功

最后是gitlab-runner授权和编写.gitlab-ci.yml文件

需要注意的一点是：

如果项目在group里的的话，page页面地址

http://10.0.1.244:9000/+ group名称 + / + 项目名称 + /public/

如果是私建项目的话，page页面地址

http://10.0.1.244:9000/+ 用户账号+ / + 项目名称 + /public/

