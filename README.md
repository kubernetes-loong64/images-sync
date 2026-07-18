## Project Description

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

This project uses GitHub Actions to automatically sync container images to a target registry, supporting
multi-architecture (including loong64) image pulls, pushes, and manifest merging.

### Workflows

| Workflow                                               | Source Image             | Target Image                                                        | Description                           | Schedule   |
|--------------------------------------------------------|--------------------------|---------------------------------------------------------------------|---------------------------------------|------------|
| [loong64-binfmt](.github/workflows/loong64-binfmt.yml) | `ghcr.io/loong64/binfmt` | `registry.cn-qingdao.aliyuncs.com/kubernetesloong64/loong64-binfmt` | binfmt support image for loong64 arch | Weekly Tue |
| [anolis](.github/workflows/anolis.yml)                 | `openanolis/anolisos`    | `registry.cn-qingdao.aliyuncs.com/kubernetesloong64/anolisos`       | Anolis OS base image                  | Weekly Tue |
| [moby-buildkit](.github/workflows/moby-buildkit.yml)   | `moby/buildkit`          | `registry.cn-qingdao.aliyuncs.com/kubernetesloong64/moby-buildkit`  | moby/buildkit image sync              | Weekly Tue |

### How It Works

1. Fetch the source image manifest and parse all architecture entries
2. Pull images per architecture (`docker pull --platform`)
3. Retag and push to the target registry (`TARGET_IMAGE-<arch>`)
4. Create and push a multi-architecture manifest, merging all architectures into a unified image tag

### Configuration

- `vars.SYNC_REGISTRY` — Target registry address (`registry.cn-qingdao.aliyuncs.com`)
- `vars.SYNC_REGISTRY_USERNAME` / `secrets.SYNC_REGISTRY_PASSWORD` — Target registry authentication
- `vars.NAMESPACE` — Target namespace (default: `kubernetesloong64`)

## License

[Apache License 2.0](LICENSE)
