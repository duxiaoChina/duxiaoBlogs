# Docker安装Docsify



##### 最近一台废弃的蜗牛星际服务器，系统重新安装了Ubuntu 2204 X64，计划作为博客和SVN服务器，找了几块废弃的硬盘。

##### 把Samba、SVN、Docsify等都装了，Docsify是一个简单的博客系统，可以快速或者说直接将Markdown转换成Web访问，记录搭建过程。

##### 以前都是系统上直接安装Docsify，本次使用Docker快速安装，十分方便，记录一下。



[GitHub - sujaykumarh/docsify-docker: :Docsify Docker Image](https://github.com/Sujaykumarh/docsify-docker?tab=readme-ov-file)

- 拉取镜像
    - docker pull sujaykumarh/docsify:latest
- 初始化
    - // initilize docs folder using docsify
    - ~~docker run -v /media/duxiao/WD_500G_0/Blogs:/docs ghcr.io/sujaykumarh/docsify init~~
- 运行
    -  // using docker container
    -  docker run --name DuxiaoBlogs -it -p 8003:3000 --privileged=true -v /media/duxiao/WD_500G_0/Blogs/:/docs ghcr.io/sujaykumarh/docsify serve
    - ```shell
         ____                 _  __
        |  _ \  ___   ___ ___(_)/ _|_   _
        | | | |/ _ \ / __/ __| | |_| | | |
        | |_| | (_) | (__\__ \ |  _| |_| |
        |____/ \___/ \___|___/_|_|  \__, |
                                    |___/
        Docsify
        Alpine Linux v3.15 x86_64
        nodejs: v16.15.0 npm: 8.5.5
        
        will exec command: $ /usr/local/bin/docsify serve /docs
        
        Serving /docs now.
        Listening at http://localhost:3000
        
        ```

        

- 访问

    - 输入服务器IP和端口即可访问

- 开机启动
    - 写入`sudo docker start DuxiaoBlogs`到开机启动脚本中