- [1 本文摘要及说明](#1-本文摘要及说明)
- [2 Windows配置远程登录的几种常见方法](#2-windows配置远程登录的几种常见方法)
  - [2.1 Windows PowerShell 方式](#21-windows-powershell-方式)
    - [2.1.1 检查](#211-检查)
    - [2.1.2 生成密钥](#212-生成密钥)
    - [2.1.3 新建config文件](#213-新建config文件)
    - [2.1.4 连接远程ECS实例](#214-连接远程ecs实例)
  - [2.2 Git Bash 方式](#22-git-bash-方式)
  - [2.3 VSCode插件（Remote-SSH）方式](#23-vscode插件remote-ssh方式)
    - [2.3.1 安装插件：](#231-安装插件)
    - [2.3.2 连接](#232-连接)
- [参考文献](#参考文献)
  


# 1 本文摘要及说明

小白写的小白向教程，错误之处还请指正。

记录自己Windows采用SSH方式远程登录阿里云ECS虚拟机的配置过程记录及踩的坑。

前提：
* 阿里云ECS实例需要时开启状态
* 阿里云ECS实例中“本实例安全组”的“内网入方向全部规则”中要保持22/22端口的开启。（其实这个端口默认是开启的，笔者前段时间进行过设置，导致该端口并未开启，从而受到阻碍，故提出此处注意的点。）

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-14-23-48-35.png)

# 2 Windows配置远程登录的几种常见方法

## 2.1 Windows PowerShell 方式

### 2.1.1 检查

首先检查自己是不是系统已经安装了SSH，笔者电脑Win 10版本是2004，发现已经安装了SSH，若读者电脑未安装，则可通过[此处](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)查看如何安装
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-14-21-45-48.png)

### 2.1.2 生成密钥

输入命令`ssh-keygen`生成公私钥，这一步一路回车即可。
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-14-23-41-51.png)

由于笔者已经生成过，所以有提示是否覆写，正常情况下一路回车即可。啥都不用填写，直接回车。

### 2.1.3 新建config文件

打开密钥存储的路径文件夹`C:\User\lihua/.ssh`（名字以lihua为例）新建一个config文件（文件无后缀名），其内容如下所示：

    Host root
      HostName xx.xx.xxx.xxx
      User xxxx
      Port xx
      IdentityFile ~/.ssh/id_rsa
说明：  

* Host: 这个地方随便写，笔者这写的`root`，故意与$User$不一致，是为了与GitBash方式区分开。
* HostName: 此处填写阿里云ECS实例的公网IP地址，如下图：
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-14-23-45-31.png)

* User: 这是阿里云ECS实例的用户名，有的教程可能写的是root但是笔者试了并不能成功，可能需要进行别的设置才行吧。笔者这里经过反复验证发现是虚拟机的账户名才成功连接。比如：lihua, wanglei 这种当初设置的账户名称
* Port: 端口号，笔者这里是22
* 最后一行`IdentityFile`照抄即可

最后查看自己windows电脑的文件路径，应该如下所示（know_hosts可以没有，这个应该是后期连接服务器才生成的，确保前三个文件是有的话即可）
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-14-23-30-33.png)

### 2.1.4 连接远程ECS实例
打开PowerShell并进入到`C:\User\lihua/.ssh`文件下输入如下命令

    ssh lihua@xx.xx.xxx.xxx -p 22
再输入密码（如果读者的ECS安装了图形化界面的话，密码是图形化界面登录的密码，而不是ECS自带VNC远程连接的密码。）

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-04-58.png)

登录成功之后应该是这样的，同时邮件还会受到阿里云的提示：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-02-49.png)
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-08-00.png)

## 2.2 Git Bash 方式
前三步和上一节一样，只有连接上有一处需要注意的地方。打开`C:\User\lihua/.ssh`直接Git Bash Here：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-16-25.png)

然后输入箭头所指的内容：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-17-50.png)

可以发现，**这里是`ssh root`，root是$config$文件里的Host，而不是User**，此处是需要注意的。

## 2.3 VSCode插件（Remote-SSH）方式

### 2.3.1 安装插件：
![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-21-01.png)


### 2.3.2 连接
1. 点击+号

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-30-03.png)

2. 输入`ssh lihua@xx.xx.xxx.xxx -p 22`

3. 右键连接：


![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-31-58.png)  

然后：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-32-36.png)

再然后：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-33-18.png)

左下角这样就说明成功连接了：

![](https://raw.githubusercontent.com/NAIXIL/GithubPagesPics/main/2021/2020-08-15-00-36-52.png)





# 参考文献
> [1] https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse  
> [2] https://code.visualstudio.com/docs/remote/ssh  
> [3] https://www.cnblogs.com/chenzhaoyu/p/9898679.html  
> [4] https://zhuanlan.zhihu.com/p/68577071  
