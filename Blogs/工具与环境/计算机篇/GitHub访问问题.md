# GitHub访问问题

- DNS查询 https://myssl.com/dns_check.html
- Host文件修改
    - C:\Windows\System32\drivers\etc

```bat
20.205.243.166 github.com
108.160.170.44 github.global.ssl.fastly.net
185.199.108.153 assets-cdn.github.com
```

- 刷新本机DNS缓存
    - 在CMD窗口输入：ipconfig /flushdns
- Host文件需要管理员权限问题
    - 进入该文件夹，右键选中hosts文件，选择属性，选择安全，选中users,进入编辑界面，勾选允许写入、修改、完全控制。