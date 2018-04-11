1. 安装 laravel
```
composer global require "laravel/installer"
```

2. 将 laravel 命令添加到 全局 PATH
```
vim /etc/profile
最尾添加：export PATH=$PATH:/root/.composer/vendor/bin
ps1: 需要重启或刷新profile
ps2: 可能需要给 /root/.composer/vendor/laravel/installer/laravel 添加执行权限
```

3. 使用 composer 创建新项目
```
composer create-project --prefer-dist laravel/laravel blog "5.5.*"
```

4. 给 storage 目录和 bootstrap/cache 目录添加读写权限
