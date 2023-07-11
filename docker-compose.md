---
title: docker-compose
category: Devops
layout: 2017/sheet
prism_languages: [yaml]
weight: -1
updated: 2020-01-01
---

### 基本示例

```yaml
# docker-compose.yml
version: '2'

services:
  web:
    build:
    # 从 Dockerfile 构建
      context: ./Path
      dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

### 启动命令

```sh
docker-compose start
docker-compose stop
```

```sh
docker-compose pause
docker-compose unpause
```

```sh
docker-compose ps
docker-compose up
docker-compose down
```

#### 例子

```sh
$ docker-compose -f docker-compose-cheatsheets.yml -p cheatsheets up -d
```

使用指定的compose模板文件，在后台启动并运行所有的容器。

## 参考
{: .-three-column}

### 构建

```yaml
web:
  # 从 Dockerfile 构建
  build: .
  args:     # 添加构建参数
    APP_HOME: app
```

```yaml
  # 从自定义 Dockerfile 构建
  build:
    context: ./dir
    dockerfile: Dockerfile.dev
```

```yaml
  # 从镜像构建
  image: ubuntu
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry:4000/postgresql
  image: a4bc65fd
```

### 端口

```yaml
  ports:
    - "3000"
    - "8000:80"  # host:container
```

```yaml
  # 公开端口而不将它们发布到主机，它们只能由链接的服务访问。
  # 只能指定内部端口。
  expose: ["3000"]
```

### 命令

```yaml
  # 执行命令
  command: bundle exec thin -p 3000
  command: [bundle, exec, thin, -p, 3000]
```

```yaml
  # 覆盖 Dockerfile 的 `entrypoint`
  entrypoint: /app/start.sh
  entrypoint: [php, -d, vendor/bin/phpunit]
```

### 环境变量

```yaml
  # 环境变量
  environment:
    RACK_ENV: development
  environment:
    - RACK_ENV=development
```

```yaml
  # 从文件添加环境变量
  env_file: .env
  env_file: [.env, .development.env]
```

### 依赖关系

```yaml
  # 使`db`服务可用作主机名`database`
  # (implies depends_on)
  links:
    - db:database
    - redis
```

```yaml
  # 在启动之前确保`db`处于活动状态
  depends_on:
    - db
```

```yaml
  # 启动之前确保`db`状态健康
  # 并且`db-init`已成功完成
  depends_on:
    db:
      condition: service_healthy
    db-init:
      condition: service_completed_successfully
```

### 其他选项

```yaml
  # 使这项服务扩展到另一项服务
  extends:
    file: common.yml  # 可选项
    service: webapp
```

```yaml
  volumes:
    - /var/lib/mysql
    - ./_data:/var/lib/mysql
```

```yaml
  # 自动重启容器
  restart: unless-stopped
  # always, on-failure, no (default)
```

## 高级功能
{: .-three-column}

### 标签 Labels

```yaml
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

### DNS 服务器

```yaml
services:
  web:
    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 8.8.4.4
```

### 设备 Devices

```yaml
services:
  web:
    devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
```

### 外部链接 External links

```yaml
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

### 健康检测 Healthcheck

```yaml
    # 当`test`命令成功时声明服务正常
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### 额外的主机 Hosts

```yaml
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```

### 网络 Network

```yaml
# 创建一个名为`frontend`的自定义网络
networks:
  frontend:
```

### 外部网络 External network

```yaml
# 加入已有的网络
networks:
  default:
    external:
      name: frontend
```

### 数据卷 Volume

```yaml
# 挂载主机路径或命名卷，指定为服务的子选项
  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  dbdata:
```

### 用户 User

```yaml
# 指定用户
user: root
```

```yaml
# 同时指定用户和组的id
user: 0:0
```
