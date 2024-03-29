## deepin搭建docker环境

#### 前言
本来我是想搭建 `gitlab` ,后来综合网上的教程，在 `deepin` 上试了多次，失败.于是转而通过学习 `docker` 来一键部署 `gitlab` .

#### 开局暗坑
```SHELL
wget -q0- https://get.docker.com/ | sh
# deepin由于不是官方认证的stable版本
# 不能直接通过docker脚本安装docker
```

#### 解决办法
1. 确保先卸载以前版本
    ```SHELL
    sudo apt-get remove docker.io docker-engine
    ```
1. 安装秘钥管理相关工具
    ```SHELL
    sudo apt-get install apt-transport-https ca-certificates curl python-software-properties software-properties-common
    # 提示大意是software-properties-common可以替代被引用的python-software-properties
    # 无关紧要,照字面意思其实可以不要后面的python-software-properties
    ```

1. 下载安装秘钥
    国内源优选[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)或[中科大开源镜像站](http://mirrors.ustc.edu.cn/)
    ```SHELL
    curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo apt-key add -
    # 如下官方源：网络问题，容易被墙
    # curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
    ```

    检查秘钥是否安装成功
    ```SHELL
    sudo apt-key fingerprint 0EBFCD88
    # 如果成功,会返回pub, uid, sub信息
    ```

1. 添加 `docker` 仓库
    ```SHELL
    # 编辑 /etc/apt/sources.list 添加如下
    deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/debian stretch stable
    # 如下是官方源:网络问题，容易被墙
    # deb [arch=amd64] https://download.docker.com/linux/debian stretch stable
    ```

1. 更新仓库并安装 `docker-ce`
    ```SHELL
    sudo apt-get update
    sudo apt-get install docker-ce
    ```
1. 设置国内镜像加速
```SHELL
# 配置daemon.json:没有的话自己touch一个
sudo gedit /etc/docker/daemon.json
# 填入收集的镜像地址
{
  	"registry-mirrors": [
        "https://hub-mirror.c.163.com",
        "https://reg-mirror.qiniu.com",
        "https://registry.docker-cn.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://mirror.ccs.tencentyun.com"
  	]
}
# 重启
sudo service docker restart
```

1. 常用 `docker` 命令
    ```SHELL
    # 启动docker
    systemctl start docker
    # 停止docker
    systemctl stop docker
    # 查看安装的版本信息
    docker version
    # docker的demo项目hello-world
    sudo docker run hello-world
    # 让普通用户也能运行docker:把账号加到docker用户组,重启docker服务,切换身份
    sudo groupadd docker
    sudo gpasswd -a user_name(MuYi086) docker
    newgrp - docker # 将当前用户切换到docker组中
    sudo service docker restart
    # 并不是一劳永逸,首次进入需切换身份
    su root
    su user_name(MuYi086)
    # 如果提示鉴定错误，是由于初次安装未给root设置密码,重新设置即可
    sudo passwd root
    # 列出镜像
    docker images
    # 查找镜像
    docker search python
    # 获取镜像
    docker pull python
    # 删除镜像
    docker rmi python
    # 查看状态
    docker stats 
    # 列举当前活动容器
    docker ps
    # 列举所有容器
    docker ps -a
    # 创建bridge网络
    docker newwork create netname
    # 查询新创建的bridge
    docker network ls
    # 启动容器
    docker run -it ubuntu /bin/bash
    # 停止容器
    docker stop container_name
    # 重启容器
    docker start container_name
    # 查询容器Ip
    docker inspect container_name | grep "IPAddress"
    # 移除容器
    docker rm container_name
    # 删除镜像:该镜像下容器实例必须都已停止
    docker rmi image_name
    # 卸载docker-ce
    apt remove docker-ce
    apt autoremove
    
    ```
#### 参考
1. [deepin系统下的docker安装](https://www.jianshu.com/p/8200a3a50806 'deepin系统下的docker安装')
1. [Deepin下安装Docker](https://www.diandian100.cn/bce2e291.html 'Deepin下安装Docker')
1. [Docker 加速器](http://guide.daocloud.io/dcs/daocloud-9153151.html 'Docker 加速器')