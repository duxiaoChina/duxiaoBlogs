# Ubuntu2204安装SVN服务

> 家里有台用了很久的蜗牛星际服务器，一直安装的Windows服务器，方便随时远程家里的文档及安装一些应用，用于测试。
>
> 近期想要换成Linux系统，最后决定使用Ubuntu 2204。安装了Samba服务作为文件服务器，使用Docker安装了个Web服务器。
>
> 之前在威联通NAS使用IF.SVNAdmin搭建了SVN服务器感觉也不是很好维护，所以直接换了。
>
> 现在私有SVN服务器也要安装上SVN服务器用于日常代码管理，记录一下。

> Ps: 不是基于apache2+SVN方案，直接安装 subversion。

```shell
sudo apt-get update
sudo apt-get install subversion

duxiao@duxiao-nas:~/duxiaoWs$ svnserve --version
svnserve，版本 1.14.1 (r1886195)
   编译于 May 21 2022，10:52:35 在 x86_64-pc-linux-gnu

Copyright (C) 2021 The Apache Software Foundation.
This software consists of contributions made by many people;
see the NOTICE file for more information.
Subversion is open source software, see http://subversion.apache.org/

下列版本库后端(FS) 模块可用:

* fs_fs : 模块与文本文件(FSFS)版本库一起工作。
* fs_x : Module for working with an experimental (FSX) repository.
* fs_base : 模块只能操作BDB版本库。

Cyrus SASL 认证可用。

duxiao@duxiao-nas:~/duxiaoWs$

```

#### 创建svn创建

`cd /usr/svn/`

`sudo ln -s /media/duxiao/WD_500G_0/SVN_Repository/ ./`

`mkdir /media/duxiao/WD_500G_0/SVN_Repository/`

`svnadmin create /media/duxiao/WD_500G_0/SVN_Repository/ProjectXX`

#### 修改配置文件

- 修改仓库的conf文件夹中的svnserve.conf文件为:
    - \# anon-access = none去掉前面的#
    - \# auth-access = write 去掉前面的#
    - realm = Duxiao_Resources SVN Repository 这里是SVN浏览等操作时的提示文字，可以自行修改。

#### 创建多个仓库公用的配置文件

- SVN_Cfg文件夹，包含两个文件，是从上述步骤中创建的仓库中拷贝出来的。
    - authz 用于存放权限文件
    - passwd 用于存放用户名和密码
- 其他仓库创建可以修改权限及密码配置文件到这两个文件
    - 修改仓库的conf文件夹中的svnserve.conf文件
        - authz-db = /usr/svn/SVN_Repository/SVN_Cfg/authz
        - password-db = /usr/svn/SVN_Repository/SVN_Cfg/passwd

#### 启动

- `svnserve -d -r /usr/svn/`
  
    - -d：表示在后台运行
    - -r：指定服务器的根目录
    
- 即可进行仓库的访问和代码检出提交等工作。

- killall svnserve 停止SVN服务器。

    > svnserve -d -r /XX/YY/rep    （默认是3690端口）
    > svnserve -d -r /XX/YY/rep --listen-port xxxx（端口号）

#### authz文件含义

- 这个文件相当重要，我们可以在这个文件中设置用户的读写权限，做到不同用户、不同项目组成员之间权限不互通，起到信息安全的目的。

    ```shell
    ### This file is an example authorization file for svnserve.
    ### Its format is identical to that of mod_authz_svn authorization
    ### files.
    ### As shown below each section defines authorizations for the path and
    ### (optional) repository specified by the section name.
    ### The authorizations follow. An authorization line can refer to:
    ###  - a single user,
    ###  - a group of users defined in a special [groups] section,
    ###  - an alias defined in a special [aliases] section,
    ###  - all authenticated users, using the '$authenticated' token,
    ###  - only anonymous users, using the '$anonymous' token,
    ###  - anyone, using the '*' wildcard.
    ###
    ### A match can be inverted by prefixing the rule with '~'. Rules can
    ### grant read ('r') access, read-write ('rw') access, or no access
    ### ('').
    
    [aliases]
    # joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average
    
    [groups]
    # harry_and_sally = harry,sally
    # harry_sally_and_joe = harry,sally,&joe
    
    # [/foo/bar]
    # harry = rw
    # &joe = r
    # * =
    
    # [repository:/baz/fuz]
    # @harry_and_sally = rw
    # * = r
    
    #[/]
    # * = #以上没有定义的用户都没有任何权限
    
    [/]
    duxiao = rw
    ```

- 可以修改为：

    ```shell
    [groups]
    admin = user1,user2
    normal = user3,user4
    
    # 根目录进行授权
    [/] 
    @admin = rw 
    @normal = r
    ```

- 针对不同项目去设置不同权限

    ```shell
    [JAVA:/]
    User1 = rw  //表示用户 User1 对项目 JAVA 的所有内容具有读写权限
    User2 = r   //表示用户 User2 对项目 JAVA 的所有内容仅具有读权限
    
    [Python:/]
    User3 = rw  //表示用户 User3 对项目 Python 的所有内容具有读写权限
    User4 = r   //表示用户 User4 对项目 Python 的所有内容仅具有读权限
    # 上面这种做法可以保证项目组成员之间的信息安全和项目安全，也便于项目组成员之间的管理。
    ```


> 参考
>
> [How to setup your own SVN (subversion) server in Ubuntu 20.04 | Our Code World](https://ourcodeworld.com/articles/read/2120/how-to-setup-your-own-svn-subversion-server-in-ubuntu-2004#:~:text=How to setup your own SVN (subversion) server,6 6. Connect to your svn server )