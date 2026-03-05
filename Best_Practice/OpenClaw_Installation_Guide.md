# OpenClaw 安装配置最佳实践

> 适用场景：阿里云 ECS（2GB 内存）+ 国内网络环境 + Telegram 配置

## 概述

本文档记录了在阿里云 ECS（2GB 内存）服务器上安装配置 OpenClaw 的完整过程，包括内存不足、网络限制、代理配置和模型选择等问题的解决方案。

---

## 1. 环境信息

- **服务器**: 阿里云 ECS 99元/年套餐
- **配置**: 2GB RAM
- **系统**: Linux (CentOS/Alibaba Cloud Linux)
- **网络**: 国内环境（无法直接访问 Telegram）
- **连接方式**: SSH 终端（无桌面环境）

---

## 2. 安装步骤

### 2.1 解决内存不足问题

**问题**: 官方安装脚本 `curl -fsSL https://openclaw.ai/install.sh | bash` 在 2GB 内存下会失败

**解决方案**: 使用 pnpm + 扩大 Node.js 内存限制

```bash
# 1. 安装 pnpm
npm install -g pnpm

# 2. 设置 Node.js 内存限制（关键步骤）
export NODE_OPTIONS="--max-old-space-size=4096"

# 3. 从源码安装
git clone https://github.com/openclaw/openclaw.git
cd openclaw/
pnpm install
pnpm build
pnpm link --global
```

**关键配置**:
- `NODE_OPTIONS="--max-old-space-size=4096"` 必须设置，否则构建会 OOM
- 如果 4GB 不够，可尝试 `8192`（依赖 swap 空间）

---

### 2.2 配置代理（解决 Telegram 连接问题）

**问题**: 阿里云国内服务器无法直接连接 Telegram API

**解决方案**: Trojan + Privoxy 组合

#### 步骤 1: 安装 Trojan 客户端

```bash
# 下载 Trojan（以 /opt/trojan 为例）
cd /opt
git clone https://github.com/trojan-gfw/trojan.git
cd trojan
# 配置 config.json 连接你的 Trojan 服务器
```

#### 步骤 2: 安装 Privoxy（SOCKS5 → HTTP 转换）

```bash
# CentOS/RHEL
yum install -y privoxy

# 编辑配置
nano /etc/privoxy/config
```

**Privoxy 关键配置**:
```
listen-address  127.0.0.1:8118
forward-socks5t / 127.0.0.1:1080 .
```

说明:
- `127.0.0.1:8118` - Privoxy 监听端口（HTTP 代理）
- `127.0.0.1:1080` - Trojan SOCKS5 端口

#### 步骤 3: 启动服务

```bash
# 启动 Trojan（在 tmux/screen 中保持运行）
tmux new -s trojan
cd /opt/trojan && ./trojan -c config.json

# 启动 Privoxy（另一个会话）
tmux new -s privoxy
privoxy --no-daemon /etc/privoxy/config
```

#### 步骤 4: OpenClaw 代理配置

在 `~/.openclaw/openclaw.json` 中添加:

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "proxy": "http://127.0.0.1:8118",
      "botToken": "YOUR_BOT_TOKEN"
    }
  }
}
```

---

### 2.3 模型配置（API 选择）

**问题**: 
- 接口 AI (jiekou.ai) 模型连接失败
- Moonshot Code Plan API 效果差

**解决方案**: 使用 Moonshot 国内版 API

```json
{
  "models": {
    "providers": {
      "moonshot": {
        "baseUrl": "https://api.moonshot.cn/v1",
        "api": "openai-completions",
        "models": [
          {
            "id": "kimi-k2.5",
            "name": "Kimi K2.5"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "moonshot/kimi-k2.5"
      }
    }
  }
}
```

**关键区别**:
- ❌ `https://api.moonshot.cn/v1` - 国内版，可用 ✅
- ❌ Moonshot Code Plan API - 效果差
- ❌ 接口 AI - 连接问题

---

## 3. 完整配置示例

### 3.1 最小可用配置

```json
{
  "auth": {
    "profiles": {
      "moonshot:default": {
        "provider": "moonshot",
        "mode": "api_key"
      }
    }
  },
  "models": {
    "providers": {
      "moonshot": {
        "baseUrl": "https://api.moonshot.cn/v1",
        "api": "openai-completions",
        "models": [
          {
            "id": "kimi-k2.5",
            "name": "Kimi K2.5",
            "reasoning": false,
            "input": ["text", "image"],
            "contextWindow": 256000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "moonshot/kimi-k2.5"
      },
      "workspace": "/root/.openclaw/workspace"
    }
  },
  "channels": {
    "telegram": {
      "enabled": true,
      "dmPolicy": "pairing",
      "botToken": "YOUR_BOT_TOKEN",
      "proxy": "http://127.0.0.1:8118"
    }
  },
  "gateway": {
    "port": 18789,
    "mode": "local",
    "auth": {
      "mode": "token",
      "token": "YOUR_RANDOM_TOKEN"
    }
  }
}
```

### 3.2 环境变量

```bash
# 添加到 ~/.bashrc
export MOONSHOT_API_KEY="sk-xxxxxxxxxxxxxxxx"
export NODE_OPTIONS="--max-old-space-size=4096"
```

---

## 4. 常用命令

```bash
# 启动 Gateway
openclaw gateway restart

# 查看状态
openclaw gateway status
openclaw models status

# 查看日志
openclaw logs --follow

# 测试 Agent
openclaw agent --local --agent main --message "hello"

# 配对 Telegram
openclaw pairing approve telegram PAIRING_CODE
```

---

## 5. 故障排查

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| 安装时 OOM | 内存不足 | 设置 `NODE_OPTIONS="--max-old-space-size=4096"` |
| Telegram 连接失败 | 国内网络限制 | Trojan + Privoxy 代理 |
| HTTP 401 | API Key 无效 | 检查 `MOONSHOT_API_KEY` 环境变量 |
| 模型响应差 | API 端点问题 | 使用 `api.moonshot.cn` 而非其他端点 |
| Gateway 启动失败 | 端口占用 | `lsof -i :18789` 检查并释放端口 |

---

## 6. 关键经验总结

1. **内存是关键**: 2GB 内存必须配合 `NODE_OPTIONS` 和 pnpm 才能安装成功
2. **代理要转换**: Trojan 的 SOCKS5 必须通过 Privoxy 转为 HTTP 代理才能被 OpenClaw 使用
3. **API 要选对**: Moonshot 国内版 (`api.moonshot.cn`) 是目前最稳定的选择
4. **终端无 UI**: SSH 连接无法使用 Dashboard，全部配置通过命令行和 JSON 文件完成
5. **tmux 保活**: Trojan 和 Privoxy 需要在 tmux 会话中运行以保持后台运行

---

## 7. 相关资源

- OpenClaw 官方文档: https://docs.openclaw.ai
- Trojan: https://github.com/trojan-gfw/trojan
- Privoxy: https://www.privoxy.org
- Moonshot API: https://platform.moonshot.cn

---

*文档版本: 2026-03-06*
*适用 OpenClaw 版本: 2026.3.2+*