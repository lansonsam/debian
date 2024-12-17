#!/bin/bash

# Debian SSH 和 root 密码更改脚本
# 功能：
# 1. 修改 SSH 配置文件，允许 root 远程登录。
# 2. 更改 root 密码。

# 检查是否以 root 身份运行
if [ "$EUID" -ne 0 ]; then
    echo "⚠️ 请以 root 身份运行此脚本."
    exit 1
fi

# SSHD 配置文件路径
SSH_CONFIG="/etc/ssh/sshd_config"

# 1. 修改 SSH 配置文件
if grep -q '^PermitRootLogin' $SSH_CONFIG; then
    sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' $SSH_CONFIG
else
    echo "PermitRootLogin yes" >> $SSH_CONFIG
fi

echo -e "\n✅ SSH 配置已修改，允许 root 进行远程登录."

# 2. 重新设置 root 密码
echo "\n请输入新的 root 密码："
passwd root
if [ $? -eq 0 ]; then
    echo -e "\n✅ root 密码已成功修改."
else
    echo -e "\n⚠️ root 密码修改失败！"
    exit 1
fi

# 3. 重启 SSH 服务
systemctl restart sshd
if [ $? -eq 0 ]; then
    echo -e "\n✅ SSH 服务已重启."
else
    echo -e "\n⚠️ SSH 服务重启失败！"
    exit 1
fi

# 脚本完成
echo -e "\n🌟 脚本执行完成！为保障安全，请确保新的 root 密码保存好.\n"
