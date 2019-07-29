---
layout:     post                       # 使用的布局（不需要改）
title:      Anaconda安装及相关配置     # 标题 
subtitle:   安装及常用设置             #副标题
date:       2019-07-29              # 时间
author:     HY                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Anaconda
	- PyCharm
	- Python
	- Tools
---

##  Anaconda

### Anaconda下载

大多数人从**清华大学开源软件镜像站**下载可能更快一些

- [官网下载地址](https://www.anaconda.com/download/)
- [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)

### Anaconda安装

基本是一路Next,注意以下几点:

- 安装位置的更改
- Advanced Options页面直接Next
- 记得安装**Microsoft VSCode**


### Anaconda配置

#### 配置环境变量

- 找到: 控制面板\系统和安全\系统\高级系统设置\环境变量\用户变量\PATH 
- 加入anaconda的安装目录的Scripts文件夹的路径: D:\ProgramData\Anaconda3\Scripts
- 打开命令行: 快捷键Win+R, 输入cmd, 回车, 输入`conda --version`, 打印出版本号则成功

#### 更改源并更新包

TUNA 提供了 Anaconda 仓库的镜像, 这样安装速度会快一点

    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
    conda config --set show_channel_urls yes

升级所有工具包
    conda upgrade --all

有提示输入:y
耐心等待......

#### 常用命令与Python包管理

常用的指令

    conda install <package_name>         安装包
    conda install numpy scipy pandas     同时安装多个包
    conda install numpy=1.10             安装包的指定版本
    conda install anaconda               在当前环境安装anaconda集合包
     
    conda remove <package_name>   移除包
    conda update <package_name>   升级包
     
    conda list                    查看当前环境已安装的包信息
    conda search <package_name>   查询包信息
    conda search <search_term>    模糊查询包信息
     
    conda install --name <env_name> <package_name>   在指定环境安装的包信息
    conda remove  --name <env_name> <package_name>   移除指定环境的包
    conda update  --name <env_name> <package_name>   升级指定环境的包
    conda list --name <env_name>                     查看指定环境的已安装的包信息
     
    conda update conda      更新conda
    conda update anaconda   更新anaconda
    conda update python     更新Python

推荐通过**pip**来管理包
设置允许pip访问conda包管理，执行命令
    conda config --set use_pip True

#### 管理虚拟环境

最好使用 Anaconda Prompt 执行下列命令
    
    conda create --name <env_name>  <list of packages>    创建新环境
    conda create --name PyTorch python=2.7      创建名为PyTorch的运行环境
    conda create --name TensorFlow python=3.6   创建名为TensorFlow的运行环境
     
    conda env remove --name <env_name>    删除环境
    conda env list                    显示所有的环境
     
    conda info                        显示当前安装的conda信息
    conda info --envs                 显示所有运行环境
     
    source activate <env_name>    激活环境
    source deactivate             退出当前环境
 
### 配置 Jupyter Notebook

Jupyter Notebook 更改默认路径
1. 打开Anaconda Prompt，输入如下命令：
    `jupyter notebook --generate-config`
2. 根据显示的路径，打开配置文件jupyter_notebook_config.py
3. 全文搜索【notebook_dir】，找到后填入自己的工作路径（用“\\”）并保存
    注意：工作路径不能出现中文, 删除注释符号
4. 找到Jupyter Notebook图标，右键属性
    删除【目标】属性中的【%USERPROFILE%】
    修改【起始位置】属性为自己的工作路径



























