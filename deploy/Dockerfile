# Stage 1: Build the Go application
FROM golang:1.23-alpine AS builder

WORKDIR /app
# 使用 --no-cache 可以减小镜像层的大小
RUN apk add --no-cache git make && git clone https://github.com/J-York/chatgpt-adapter.git .
RUN make install
RUN make build-linux

# Stage 2: Create the final, minimal image
FROM alpine:3.19.0

WORKDIR /app

# 从 builder 阶段拷贝编译好的二进制文件
COPY --from=builder /app/bin/linux/server ./server

# 关键改动：从当前目录拷贝 config.yaml 文件到镜像中
COPY config.yaml ./config.yaml

# 赋予执行权限
RUN chmod +x server

# --- 启动命令部分保持不变 ---
ENV ARG "--port 8080"
CMD ["./server ${ARG}"]
ENTRYPOINT ["sh", "-c"]
