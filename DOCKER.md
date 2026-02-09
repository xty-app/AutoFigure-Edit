# Docker (API-only)

这个镜像是 **API-only** 版本：SAM3 用 `roboflow/fal` 接口，去背景用 `remove-bg` 接口，不包含本地模型依赖（不装 `torch/transformers/sam3`）。

## 构建

```bash
docker build -t autofigure-edit:api .
```

## 运行（Web UI）

最简单方式（容器内输出会写到容器文件系统，退出就没了）：

```bash
docker run --rm -p 8000:8000 autofigure-edit:api
```

建议挂载本地目录，保留输出与上传文件：

```bash
docker run --rm -p 8000:8000 \
  -v "$(pwd)/outputs:/app/outputs" \
  -v "$(pwd)/uploads:/app/uploads" \
  autofigure-edit:api
```

启动后访问：

```text
http://localhost:8000
```

在页面里填写：
- `Provider`: 选择 `svip.xty.app` 或 `api.xty.app`
- `API Key`: 填你的 key（用于生图、SVG、remove-bg）
- `SAM3 Backend`: 选择 `roboflow`（需要 `SAM3 API Key`）或 `fal`

## 运行（CLI）

```bash
docker run --rm -it \
  -v "$(pwd)/outputs:/app/outputs" \
  autofigure-edit:api \
  python3 autofigure2.py \
    --method_file paper.txt \
    --output_dir /app/outputs/run1 \
    --provider svip.xty.app \
    --api_key "YOUR_KEY" \
    --sam_backend roboflow \
    --sam_api_key "YOUR_ROBOFLOW_KEY"
```

