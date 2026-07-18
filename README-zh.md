## 项目说明

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

本项目通过 GitHub Actions 自动同步容器镜像到目标仓库，支持多架构（含 loong64）镜像的拉取、推送和 manifest 合并。

### 工作流

| 工作流                                                 | 源镜像                   | 目标镜像                                                            | 说明                              | 定时   |
|--------------------------------------------------------|--------------------------|---------------------------------------------------------------------|-----------------------------------|--------|
| [loong64-binfmt](.github/workflows/loong64-binfmt.yml) | `ghcr.io/loong64/binfmt` | `registry.cn-qingdao.aliyuncs.com/kubernetesloong64/loong64-binfmt` | loong64 架构的 binfmt 支持镜像    | 每日   |
| [anolis](.github/workflows/anolis.yml)                 | `openanolis/anolisos`    | `registry.cn-qingdao.aliyuncs.com/kubernetesloong64/anolisos`       | 龙蜥操作系统（Anolis OS）基础镜像 | 每周二 |

### 工作原理

1. 获取源镜像的 manifest，解析出所有架构列表
2. 按架构分别拉取镜像（`docker pull --platform`）
3. 重新打标签后推送到目标仓库（`TARGET_IMAGE-<arch>`）
4. 创建并推送多架构 manifest，合并各架构为统一镜像标签

### 配置

- `vars.SYNC_REGISTRY` — 目标仓库地址（`registry.cn-qingdao.aliyuncs.com`）
- `vars.SYNC_REGISTRY_USERNAME` / `secrets.SYNC_REGISTRY_PASSWORD` — 目标仓库认证
- `vars.NAMESPACE` — 目标命名空间（默认：`kubernetesloong64`）

## 许可证

[Apache License 2.0](LICENSE)
