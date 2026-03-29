# === 前端构建阶段 ===
FROM node:18-alpine AS frontend-builder

# 启用 corepack 并安装 pnpm
RUN corepack enable && corepack prepare pnpm@latest --activate

# 安装 Alpine 环境下的编译工具（解决原生模块编译问题）
RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    rustc \
    cargo

WORKDIR /tmp/frontend

# 复制前端文件
COPY ./BillNote_frontend/ .

# 清理旧依赖 + 强制重新安装（关键：解决原生绑定缺失）
RUN rm -rf node_modules pnpm-lock.yaml && \
    pnpm install --force

# 增加 Node 内存限制并构建
RUN NODE_OPTIONS="--max-old-space-size=4096" pnpm run build

# === 后续阶段保持不变 ===
# ... 您原有的后端构建、阶段3等代码 ...
