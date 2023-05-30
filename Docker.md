## Docker常用命令

### 1. 辅助指令

```bash
docker version       | 显示 Docker 版本信息
docker info          | 显示 Docker 系统信息（包括镜像和容器数）
docker --help        | 帮助
```

### 2. 镜像指令

```bash
docker images [OPTIONS]            | 列出本机上的镜像（-a: 列出本地所有镜像、-q: 只显示镜像id）
docker search [OPTIONS] xxx镜像名  | 搜索镜像名
docker pull xxx镜像名字            | 拉取某镜像
docker rmi xxx镜像id/镜像名        | 删除某镜像
```

### 3. 容器指令

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]  | 启动容器
（-i:以交互模式运行容器;-t:为容器重新分配一个伪输入终端;-p:指定端口映射;-d:后台运行容器）
docker ps [OPTIONS]                            | 列出当前所有正在运行的容器
（-a:列出当前所有正在运行的容器+历史上运行过的;-l:显示最近创建的容器）
exit                                           | 交互模式中容器停止并退出交互
ctrl+P+Q                                       | 交互模式中容器不停止退出交互
docker start 容器id/容器名                     | 启动容器
docker restart 容器id/容器名                   | 重启容器
docker stop 容器id/容器名                      | 停止容器
docker kill 容器id/容器名                      | 强制停止容器
docker rm 容器id                               | 删除已停止的容器
docker rm -f $(docker ps -a -q)                | 删除全部容器
docker logs -f -t --tail 容器id                | 查看容器日志
docker top 容器id                              | 查看容器运行的进程
docker inspect 容器id                          | 查看容器内部信息
docker exec -it 容器id bashShell               | 进入容器执行指令
docker attach 容器id                           | 进入容器交互执行指令
docker cp 容器id:容器内路径 目的主机路径       | 从容器内拷贝文件到主机
```

修改容器资源限制、重启策略等

```
docker update --help

Usage:  docker update [OPTIONS] CONTAINER [CONTAINER...]

Update configuration of one or more containers

Options:
      --blkio-weight uint16        Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --cpu-period int             Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --pids-limit int             Tune container pids limit (set -1 for unlimited)
      --restart string             Restart policy to apply when a container exits

```

## Dockerfile

1. 写 Dockerfile 时的注意事项
    - 设置时区
    - 配置中国镜像源站

1. 上线前确保 Docker 的配置文件 `/etc/docker/daemon.json` 就绪，如镜像源站、日志切割等

    ```json
    {
        "data-root": "/docker",
        "registry-mirrors": [ "https://docker.mirrors.ustc.edu.cn/" ],
        "log-driver": "json-file",
        "log-opts": {
            "max-size": "5g",
            "max-file": "3"
        }
    }
    ```
