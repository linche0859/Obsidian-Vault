[[Docker]]

# Docker 指令
## 打包成 image
```bash
docker build -t hellokitty .
```

打包出來的 image 名稱為 `hellokitty`。

## 啟用 image
```bash
docker run hellokitty
```

### 將本機的 port 連結到 container 中的 port
```bash
docker run -p 3000:8000 hellokitty
```

### 背景執行
```bash
docker run -d hellokitty
```

## 停用 container
```bash
docker stop [container id]
```

## 查看全部的 image
```bash
docker images
```

## 查看執行中的 container
```bash
docker ps
```

# Docker compose
以下方的內容為範例：
```yml
version: "3.9"

services:
  hello-kitty:
    image: registry.gitlab.com/linche0859/shopping-cat-v2
    ports:
      - 3000:8000
    restart: always
```

## 指令
### 啟用全部的 service
```bash
docker-compose up
```

### 停用全部的 service
```shell
docker-compose down
```