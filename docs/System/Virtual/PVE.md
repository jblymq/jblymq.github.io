---
sort: 1
---

# Proxmox Virtual Environment

下载连接：https://enterprise.proxmox.com/iso/proxmox-ve_8.0-2.iso

### 安装PVE8.0，修改国内源

国内网络大环境的情况下建议使用中科大、清华、阿里和网易的国内源，速度快，可以节省更新所占用的时间。

这里使用中科大的源

```shell
# 将此文件的中的所有内容注释掉
nano /etc/apt/sources.list.d/pve-enterprise.list

# 下载中科大的GPG KEY
wget https://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg

# 使用Proxmox非企业版源
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

# 将Debian官方源替换为中科大源
sed -i 's|^deb http://ftp.debian.org|deb https://mirrors.ustc.edu.cn|g' /etc/apt/sources.list
sed -i 's|^deb http://security.debian.org|deb https://mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list

# 替换Ceph源
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/ceph-quincy bookworm no-subscription" > /etc/apt/sources.list.d/ceph.list

# 替换CT镜像下载源
sed -i 's|http://download.proxmox.com|https://mirrors.ustc.edu.cn/proxmox|g' /usr/share/perl5/PVE/APLInfo.pm

#重启LXC服务
systemctl restart pvedaemon.service

#更新
apt update
apt upgrade -y
```
