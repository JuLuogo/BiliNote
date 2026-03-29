# === 前端构建阶段 ===
FROM node:18-alpine AS builder

# 启用corepack并安装pnpm
RUN corepack enable && corepack prepare pnpm@latest --activate

# 安装Alpine环境下的编译工具（解决原生模块编译问题）
RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    rustc \
    cargo

WORKDIR /app

# 复制依赖文件
COPY ./BillNote_frontend/package.json ./BillNote_frontend/pnpm-lock.yaml ./

# 清理旧依赖+强制重新安装（解决原生绑定缺失）
RUN rm -rf node_modules && pnpm install --force

# 复制源代码
COPY ./BillNote_frontend/ ./

# 增加Node内存限制+构建
RUN NODE_OPTIONS="--max-old-space-size=4096" pnpm run build

# --- 阶段2：Nginx静态服务 ---
FROM nginx:1.25-alpine

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY ./BillNote_frontend/deploy/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html
