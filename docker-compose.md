---
title: docker-compose
category: Devops
layout: sheet
prism_languages: [yaml]
weight: -1
updated: 2020-07-23
---

### 例子

```yaml
# docker-compose.yml
version: '3'

services:
  web:
    build: .
    # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

### 控制台命令

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

## Reference
{: .-three-column}

### 构建

```yaml
web:
  # 从 Dockerfile 构建
  build: .
  args:     # Add build arguments
    APP_HOME: app
```

```yaml
  # build from custom Dockerfile
  build:
    context: ./dir
    dockerfile: Dockerfile.dev
```

```yaml
  # 使用现有镜像
  image: ubuntu
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry:4000/postgresql
  image: a4bc65fd
```

### 端口映射

```yaml
  ports:
    - "3000"
    - "8000:80"  # host:container
```

```yaml
  # 向其他容器暴露端口（不是向宿主）
  expose: ["3000"]
```

### Commands

```yaml
  # 执行命令
  command: bundle exec thin -p 3000
  command: [bundle, exec, thin, -p, 3000]
```

```yaml
  # 覆盖 entrypoint
  entrypoint: /app/start.sh
  entrypoint: [php, -d, vendor/bin/phpunit]
```

### 环境变量

```yaml
  # environment vars
  environment:
    RACK_ENV: development
  environment:
    - RACK_ENV=development
```

```yaml
  # environment vars from file
  env_file: .env
  env_file: [.env, .development.env]
```

### 依赖

```yaml
  # makes the `db` service available as the hostname `database`
  # (包含 depends_on)
  links:
    - db:database
    - redis
```

```yaml
  # 在`db`启动后启动
  depends_on:
    - db
```

### Other options

```yaml
  # 引入其他文件中的配置
  extends:
    file: common.yml  # optional
    service: webapp
```

```yaml
  volumes:
    - /var/lib/mysql
    - ./_data:/var/lib/mysql
```

## 高级特性
{: .-three-column}

### Labels

```yaml
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

### DNS

```yaml
services:
  web:
    dns: 8.8.8.8
    # dns:
    #   - 8.8.8.8
    #   - 8.8.4.4
```

### 设备

```yaml
services:
  web:
    devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
```

### 外部链接

```yaml
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

外部链接用于连接不是在一个`docker-compose.yml`文件内创建的容器。

### Hosts

```yaml
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```

向容器的`hosts`文件追加。

### Network

```yaml
# 创建一个自定义Network `frontend`
networks:
  frontend:
```

### External network

```yaml
# 加入已经存在的Network
networks:
  default:
    external:
      name: frontend
```

### Volume

```yaml
# 挂载宿主机目录或命名卷
  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  dbdata:
```
