## 安装Ubuntu16.04后要做的事
1. 更新源

1. 更新系统

    ```SHELL
    sudo apt-get update
    ```

1. 更新软件

    ```SHELL
    // 查看可更新的软件
    apt list --upgradable
    // 更新操作
    sudo apt upgrade
    ```

1. 清理旧组件

    ```SHELL
    sudo apt autoremove
    ```

1. 删除 `libreoffice`

    ```SHELL
    sudo apt-get remove libreoffice-common 
    ```

1. 删除 `Amazon` 的链接

    ```SHELL
    sudo apt-get remove unity-webapps-common
    ```

1. 删除一些少用的自带软件(需要在装)

    ```SHELL
    sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install
    ```

    ```SHELL
    sudo apt-get remove onboard deja-dup
    ```

    这样系统就基本干净了

1. 安装 `GDebi`
    从而可以方便安装 `.deb` 文件

    ```SHELL
    sudo apt-get update
    sudo apt-get install gdebi
    ```

1. 安装 `Vim`

    ```SHELL
    sudo apt-get install vim
    ```

1. 安装 `chrome`

    [chrome下载](https://www.google.cn/intl/zh-CN/chrome/)

    然后使用`GDebi`安装`deb`包

1. 安装 `WPS`
    ```SHELL
    sudo apt-get install wps-office
    ```

1. 安装 `git` 和 `node.js` 和 `npm`

    ```SHELL
    sudo apt-get install git nodejs npm
    ```

1. 安装搜狗输入法

    [下载地址](https://pinyin.sogou.com/linux/?r=pinyin)
    
    然后使用 `GDebi` 安装 `deb` 包
