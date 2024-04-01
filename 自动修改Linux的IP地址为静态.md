```shell
#!/bin/bash

# 函数 to ask for user input
ask_for_input() {
    read -p "请输入$1: " value
    echo "$value"
}

# 获取用户输入
NETWORK_INTERFACE=$(ask_for_input "网卡名称")
IP_ADDRESS=$(ask_for_input "IP地址")
SUBNET_MASK=$(ask_for_input "子网掩码")
GATEWAY=$(ask_for_input "网关")
DNS_SERVER=$(ask_for_input "DNS服务器")

# 确定 ifcfg 文件的路径
IFCFG_FILE="/etc/sysconfig/network-scripts/ifcfg-$NETWORK_INTERFACE"
# 修改ip地址的获取方式为静态
sudo sed -i "s/^BOOTPROTO=.*/BOOTPROTO=static/" "$IFCFG_FILE"
# 检查文件是否存在
if [ ! -f "$IFCFG_FILE" ]; then
    echo "错误：指定的网卡配置文件不存在。"
    exit 1
fi

# 将网络配置追加到文件的末尾
echo "IPADDR=\"$IP_ADDRESS\"" >> "$IFCFG_FILE"
echo "NETMASK=\"$SUBNET_MASK\"" >> "$IFCFG_FILE"
if [ -n "$GATEWAY" ]; then
    echo "GATEWAY=\"$GATEWAY\"" >> "$IFCFG_FILE"
fi
if [ -n "$DNS_SERVER" ]; then
    echo "DNS1=\"$DNS_SERVER\"" >> "$IFCFG_FILE"
fi

# 应用更改
sudo systemctl restart network

echo "网络配置已更新，网卡 $NETWORK_INTERFACE 已重启。"
```

- 使用方法

  ```shell
  chmod +x 文件名.sh
  ./文件名.sh
  ```

  