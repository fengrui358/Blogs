# Docker 基本命令

标签（空格分隔）： Docker

---

## 建立网络

```Docker
docker network create -d bridge my-net
```

- `-d` 参数指定 Docker 网络类型，有 bridge overlay。其中 overlay 网络类型用于 Swarm mode

## 基本命令

```Docker
docker run -d -P \
    --rm \
    --name web \
    # -v /src/webapp:/usr/share/nginx/html:ro \
    --mount type=bind,source=/src/webapp,target=/usr/share/nginx/html,readonly \
    --network my-net \
    nginx:alpine
```

```Docker
docker run -d -P --rm --name web --mount type=bind,source=D:\src\webapp12345free,target=/usr/share/nginx/html,readonly --network my-net nginx:alpine
```

- `-P` 随机映射暴露端口，使用`-p 80:80` 指定端口
- `--rm` 容器停止后自动删除容器
- `--name` 指定容器别名

## 常用 Docker run

### postgres

```docker run -d --name er-db --rm -e POSTGRES_PASSWORD=1234 -v /home/free/datadir:/var/lib/postgresql/data -p 25435:5432 postgres:alpine```
