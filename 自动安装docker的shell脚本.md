<h4 style="text-align: center">自动安装docker的shell脚本</h4>

脚本内容如下：

```shell
#!/bin/bash

# 更新系统软件包
echo "更新系统..."
sudo yum update -y

# 安装必要工具和依赖
echo "安装必要依赖..."
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 添加Docker官方yum源配置文件（这里使用阿里云镜像源）
echo "添加Docker阿里云镜像源..."
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新yum缓存
echo "刷新yum缓存..."
sudo yum makecache fast

# 安装最新版本的Docker CE
echo "安装Docker CE..."
sudo yum install -y docker-ce docker-ce-cli containerd.io

# 启动Docker服务
echo "启动Docker服务并设置为开机自启动..."
sudo systemctl start docker
sudo systemctl enable docker

# 验证Docker安装是否成功
echo "验证Docker安装..."
if docker info > /dev/null 2>&1; then
    echo "Docker已成功安装并运行！"
else
    echo "Docker安装或启动失败，请检查错误日志。"
fi
```

执行命令如下：

```ini
# 赋予权限
chmod +x install_docker.sh
# 执行脚本
./install_docker.sh
```

执行脚本会遇到的错误总结

```ini
-bash: ./install_docker.sh: /bin/bash^M: bad interpreter: No such file or directory


# 解决方法
# 执行此命令
sed -i 's/\r$//' install_docker.sh

# 然后重新赋予权限在执行即可
```

