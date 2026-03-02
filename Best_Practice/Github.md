# GitHub SSH 配置最佳实践

## 问题描述
在尝试通过SSH推送新创建的GitHub仓库时遇到 `Permission denied (publickey)` 错误。

## 根本原因
1. **SSH密钥未正确添加到GitHub账户** - 生成的公钥必须在GitHub Settings → SSH and GPG keys 中正确配置
2. **SSH密钥权限问题** - 私钥文件权限必须为600 (`chmod 600 ~/.ssh/id_ed25519`)
3. **SSH代理未运行** - 需要启动ssh-agent并添加密钥
4. **GitHub主机密钥未验证** - 首次连接需要接受GitHub的主机密钥

## 解决方案步骤
```bash
# 1. 生成SSH密钥（如果还没有）
ssh-keygen -t ed25519 -C "your_email@example.com"

# 2. 设置正确权限
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub

# 3. 启动SSH代理并添加密钥
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519

# 4. 将公钥内容复制到GitHub
cat ~/.ssh/id_ed25519.pub

# 5. 测试SSH连接
ssh -T git@github.com

# 6. 确保远程仓库使用SSH URL
git remote set-url origin git@github.com:username/repo.git
```

## 备用方案
如果SSH仍然失败，使用HTTPS + Personal Access Token：
```bash
git remote set-url origin https://TOKEN@github.com/username/repo.git
```

## 预防措施
- 在创建新仓库前先确保SSH配置正确
- 始终测试 `ssh -T git@github.com` 连接
- 保持SSH密钥权限严格（600/644）