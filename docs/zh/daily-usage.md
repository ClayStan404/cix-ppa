# 日常使用与维护

## 系统更新

```bash
# 更新软件包索引
sudo apt update

# 升级所有软件包
sudo apt upgrade

# 完整升级（可能移除冲突包）
sudo apt full-upgrade
```

## 查看已安装的 CIX 包

```bash
# 列出所有已安装的 CIX 包
dpkg -l | grep cix

# 或使用 apt
apt list --installed | grep cix
```

## 搜索可用包

```bash
# 搜索 CIX 相关包
apt search cix-

# 查看包详情
apt show cix-gpu-driver
```

## 卸载驱动

再次运行脚本，选择选项 `3`：

```bash
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

