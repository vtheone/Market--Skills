# n8n Codespaces 测试说明

这个仓库现在已经包含了可用于 GitHub Codespaces 的 n8n 测试配置：

- `.devcontainer/devcontainer.json`
- `docker-compose.yml`
- `.env.example`

## 目标

先在 GitHub Codespaces 里跑起一个 **可打开 UI 的 n8n 测试实例**。

这套配置优先保证：

1. 打开 Codespace 后能直接启动 n8n
2. 自动转发 5678 端口
3. 先验证 UI 可访问
4. webhook 外网地址需要时再补

## 你现在怎么用

### 1. 打开仓库

仓库：`vtheone/Market--Skills`

### 2. 创建 Codespace

在 GitHub 仓库页面点击：

- `Code`
- `Codespaces`
- `Create codespace on main`

### 3. 等容器启动

仓库已经配置了 `.devcontainer/devcontainer.json`，会自动转发 `5678` 端口。

如果端口页面里没自动弹出，就在 Codespaces 的 `PORTS` 面板里看 `5678`。

### 4. 打开 n8n

打开转发出来的 `5678` 端口 URL。

第一次进入时，按 n8n 的页面提示创建 owner 账号即可。

## 当前配置说明

`docker-compose.yml` 已经做了这些设置：

- 使用官方镜像 `docker.n8n.io/n8nio/n8n`
- 持久化目录 `/home/node/.n8n`
- 暴露端口 `5678`
- 设置时区 `Asia/Shanghai`
- 开启 `N8N_RUNNERS_ENABLED`
- 开启 `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS`
- 给 `N8N_ENCRYPTION_KEY` 提供了默认测试值，避免 Codespaces 首次启动因为没配 `.env` 卡住

## 什么时候需要 `.env`

如果你只是想先看 UI，通常不需要额外配置。

如果你要测试：

- webhook 回调
- 外部服务跳回 n8n
- 更稳定的实例配置

再把 `.env.example` 复制为 `.env`，填写：

- `N8N_ENCRYPTION_KEY`
- `WEBHOOK_URL`
- `N8N_EDITOR_BASE_URL`

## 注意

这只是 **Codespaces 测试实例**，不是长期生产部署方案。

原因：

- Codespaces 更适合开发和测试
- 端口 URL、运行时长和资源都不适合长期稳定线上使用
- 真要长期跑，应该迁移到 VPS / Docker 主机
