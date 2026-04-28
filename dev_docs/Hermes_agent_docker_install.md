## Hermes Agent Docker Compose 快速部署

本目录已提供 `docker-compose.yaml`（gateway + dashboard，数据持久化到宿主机 `~/.hermes`）。

### 首次初始化（只需一次）

会生成 `~/.hermes/.env`、`config.yaml` 等必要文件。

```bash
mkdir -p ~/.hermes
docker run -it --rm \
  -v ~/.hermes:/opt/data \
  nousresearch/hermes-agent setup
```

### 启动（Compose）

在仓库根目录执行：

```bash
docker compose -f dev_docs/docker-compose.yaml up -d
docker compose -f dev_docs/docker-compose.yaml logs -f
```

### 访问

- Dashboard: `http://<server-ip>:9119`
- Gateway API/health: `http://<server-ip>:8642`

### 注意事项

- 不要同时运行两个 `gateway` 容器挂载同一个 `~/.hermes`（会有并发写风险）；dashboard 共享该目录是安全的。
- 如需浏览器自动化（Playwright/Chromium）且遇到共享内存问题，可在 compose 里给服务加 `shm_size: "1gb"`。
