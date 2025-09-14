## 讨论方式
- 电报群：[http://t.me/q115_strm](https://t.me/q115_strm)
- QQ群：1057459156

## 介绍
<img width="200px" height="200px" alt="logo-圆角" src="https://github.com/user-attachments/assets/0d397f6d-4769-4a4e-a395-d1bb34370fe4" />

- **默认用户名 admin,密码 admin123**
- emby代理端口默认：http-8095  https-8094
- 基于CD2的本地挂载目录生成strm
- 基于网盘的开放平台接口来同步生成 STRM
- 元数据下载、元数据上传，直链解析，不依赖其他项目，目前支持115，未来版本支持123、阿里云盘、百度网盘。
- 提供emby外网302访问支持（通过代理端口访问emby）

### 功能列表

见project需求列表：[https://github.com/users/qicfan/projects/3/views/1](https://github.com/users/qicfan/projects/3/views/1)

## 快速开始

### 使用 Docker Run

```bash
# 创建数据目录
mkdir -p {root_dir}/qmediasync/config/logs/libs
mkdir -p {root_dir}/qmediasync/config/libs


# 运行容器
docker run -d \
  --name qmediasync \
  -p 12333:12333 \
  -p 8095:8095 \
  -p 8094:8094 \
  -v $(pwd)/qmediasync/config:/app/config \
  -v /vol1/1000/网盘:/media \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  qicfan/qmediasync:latest
```

### 使用 Docker Compose

1. 创建 `docker-compose.yml` 文件（见下方示例）

```
services:
    qmediasync:
        image: qicfan/qmediasync:latest
        container_name: qmediasync
        restart: unless-stopped
        ports:
            - "12333:12333"
            - "8095:8095"
            - "8094:8094"
        volumes:
            - /vol1/1000/docker/qmediasync/config:/app/config
            - /vol2/1000/网盘:/media
        environment:
            - TZ=Asia/Shanghai

networks:
    default:
        name: qmediasync
```

2. 运行以下命令：

```bash
# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

## 目录映射说明

| 容器内路径    | 宿主机路径                           | 说明                     |
| ------------- | ------------------------------------ | ------------------------ |
| `/app/config` | `/vol1/1000/docker/qmediasync/config` | 配置文、数据、日志目录   |
| `/media`      | `/vol1/1000/网盘`                    | 存放 STRM 和元数据的目录 |

## 环境变量

| 变量名 | 默认值          | 说明     |
| ------ | --------------- | -------- |
| `TZ`   | `Asia/Shanghai` | 时区设置 |

## 端口说明

- **12333**: Web 服务端口
- **8095**: Emby代理接口，http协议
- **8094**: Emby代理接口，https协议

## 版本标签

- `latest` - 最新发布版本
- `v1.0.0` - 具体版本号（对应 GitHub Release）

## 首次使用

1. 启动容器后访问: http://your-ip:12333
2. 默认登录用户：admin，默认密码：admin123
3. 如果不是很了解，所有配置全部保持默认值
4. 如果要使用网盘：系统设置-网盘账号管理-添加账号，添加完后在下放的卡片中点击授权按钮进行授权
5. 在同步-同步目录，点击添加同步目录
6. 添加完成后，下放卡片列表会显示新添加的同步目录
7. 如果该目录内的资源变动概率较小，建议关闭定时同步，在变动时手动点击 启动同步

## 数据

重要数据位于 `/app/config` 目录，该目录的具体位置由个人映射决定，请定期备份

## FAQ

- 如果有服务无法启动或者运行逻辑始终不对，建议删除/app/config/db.db，然后重启容器，注意：该操作会清除所有数据
- /app/config/logs下的内容可以删除，不影响运行
- /app/config/libs下的内容如果删除，会影响网页查看同步详情，但是不影响其他逻辑，如果磁盘空间有限，可以删除
- emby外网302目前使用8095和8094端口，暂时不能变动

























