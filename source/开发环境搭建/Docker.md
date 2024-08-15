
[Fetching Title#kxwe](https://linux.do/t/topic/150936)

```bash
docker images | grep ubuntu
docker rmi -f b143410354a2
docker load -i ubuntu_18.04.06.tar
docker run -it ubuntu:18.04.06
sudo apt update
sudo apt-get install -y cmake
dpkg-query -s cmake
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography
python3 -c "import cryptography"
pip3 show cryptography
exit

docker ps -a
docker container prune
docker commit -m "add packages" -a "leefly" 95e6eaec7db9 ubuntu_zy:18.04.06
docker save -o ubuntu_zy_18.04.06.tar ubuntu_zy:18.04.06
docker images | grep ubuntu_zy
docker rmi -f b143410354a2
docker load -i ubuntu_zy_18.04.06.tar
docker run -it ubuntu_zy:18.04.06
dpkg-query -s cmake
python3 -c "import cryptography"
pip3 show cryptography
curl -I https://signfw.zlg.cn/cgi-bin/ti-sign-apply.cgi
curl -I https://signfw.zlgmcu.com/cgi-bin/ti-sign-apply.cgi
ping signfw.zlg.cn
ping signfw.zlgmcu.com
exit
```

```bash
#!/bin/bash

# 检查docker命令是否存在
if ! command -v docker &> /dev/null
then
    echo "Docker is not installed. Installing Docker..."

    # 更新软件包索引
    sudo apt-get update

    # 安装所需的软件包
    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

    # 添加Docker的官方GPG密钥
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    # 设置稳定版仓库
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    # 再次更新软件包索引
    sudo apt-get update

    # 安装Docker Engine
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    # 验证Docker是否安装成功
    if sudo docker run hello-world &> /dev/null
    then
        echo "Docker has been successfully installed and is running."
    else
        echo "Docker installation failed."
    fi
else
    echo "Docker is already installed."
fi
```

# 命令集

## 1. 基本镜像命令

### 拉取镜像
```bash
docker pull <镜像名>:<标签>
```

- 从 Docker 仓库拉取镜像
```bash
docker pull ubuntu:latest
```

### 列出本地镜像
```bash
docker images
```


### 删除镜像
```bash
docker rmi <镜像名或ID>
```

- 删除指定的 Docker 镜像
```bash
docker rmi ubuntu:latest
```

### 保存镜像到文件
```bash
docker save -o <文件名>.tar <镜像名>:<标签>
```

- 将镜像保存到本地文件
```bash
docker save -o redis-6-alpine.tar redis:6-alpine
docker save -o ubuntu_18.04.06.tar ubuntu:18.04.06
```

### 加载镜像文件
```bash
docker load -i <文件名>.tar
```

- 从本地文件加载镜像
```bash
docker load -i redis-6-alpine.tar
docker load -i ubuntu_18.04.06.tar
```

## 2. 基本容器命令

### 运行容器
```bash
docker run [选项] <镜像名>:<标签>
```

- 启动一个新的容器
```bash
docker run -it ubuntu:latest
docker run -it ubuntu:18.04.06
```
常用选项：
- `-d`：后台运行容器并返回容器ID。
- `-it`：以交互模式运行容器，并打开一个终端。
- `-p`：映射端口（如 `-p 8080:80`）。
- `-v`：挂载卷（如 `-v /host/path:/container/path`）。
- `--name`：为容器命名。

### 列出运行中的容器
```bash
docker ps
```


### 列出所有容器
```bash
docker ps -a
```

- 显示所有容器（包括未运行的）。

### 停止容器
```bash
docker stop <容器名或ID>
```

- 停止指定的容器
```bash
docker stop my_container
```

### 启动容器
```bash
docker start <容器名或ID>
```

- 启动已经停止的容器
```bash
docker start my_container
```

### 重启容器
```bash
docker restart <容器名或ID>
```

- 重启指定的容器
```bash
docker restart my_container
```

### 删除容器
```bash
docker rm <容器名或ID>
```

- 删除指定的容器
```bash
docker rm my_container
```

### 进入运行中的容器
```bash
docker exec -it <容器名或ID> /bin/bash
```

- 在正在运行的容器中打开一个新的终端
```bash
docker exec -it my_container /bin/bash
```

## 3. 网络管理

### 列出所有网络
```bash
docker network ls
```

- 显示所有 Docker 网络

### 创建网络
```bash
docker network create <网络名>
```

- 创建一个新的 Docker 网络
```bash
docker network create my_network
```

### 删除网络
```bash
docker network rm <网络名>
```

- 删除指定的 Docker 网络
```bash
docker network rm my_network
```

### 将容器连接到网络
```bash
docker network connect <网络名> <容器名或ID>
```

- 将一个容器连接到指定的网络
```bash
docker network connect my_network my_container
```

### 从网络断开容器
```bash
docker network disconnect <网络名> <容器名或ID>
```

- 将一个容器从指定的网络断开
```bash
docker network disconnect my_network my_container
```

## 4. 卷管理

### 创建卷
```bash
docker volume create <卷名>
```

- 创建一个新的 Docker 卷
```bash
docker volume create my_volume
```

### 列出所有卷
```bash
docker volume ls
```


### 删除卷
```bash
docker volume rm <卷名>
```

- 删除指定的 Docker 卷
```bash
docker volume rm my_volume
```

## 5. Compose

```bash
PS D:\docker> docker-compose --help

Usage:  docker compose [OPTIONS] COMMAND

Define and run multi-container applications with Docker

Options:
      --all-resources              Include all resources, even those not
                                   used by services
      --ansi string                Control when to print ANSI control
                                   characters ("never"|"always"|"auto")
                                   (default "auto")
      --compatibility              Run compose in backward compatibility mode
      --dry-run                    Execute command in dry run mode
      --env-file stringArray       Specify an alternate environment file
  -f, --file stringArray           Compose configuration files
      --parallel int               Control max parallelism, -1 for
                                   unlimited (default -1)
      --profile stringArray        Specify a profile to enable
      --progress string            Set type of progress output (auto,
                                   tty, plain, quiet) (default "auto")
      --project-directory string   Specify an alternate working directory
                                   (default: the path of the, first
                                   specified, Compose file)
  -p, --project-name string        Project name

Commands:
  attach      Attach local standard input, output, and error streams to a service's running container
  build       Build or rebuild services
  config      Parse, resolve and render compose file in canonical format
  cp          Copy files/folders between a service container and the local filesystem
  create      Creates containers for a service
  down        Stop and remove containers, networks
  events      Receive real time events from containers
  exec        Execute a command in a running container
  images      List images used by the created containers
  kill        Force stop service containers
  logs        View output from containers
  ls          List running compose projects
  pause       Pause services
  port        Print the public port for a port binding
  ps          List containers
  pull        Pull service images
  push        Push service images
  restart     Restart service containers
  rm          Removes stopped service containers
  run         Run a one-off command on a service
  scale       Scale services
  start       Start services
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop services
  top         Display the running processes
  unpause     Unpause services
  up          Create and start containers
  version     Show the Docker Compose version information
  wait        Block until the first service container stops
  watch       Watch build context for service and rebuild/refresh containers when files are updated

Run 'docker compose COMMAND --help' for more information on a command
```

### 5.1 `docker-compose up`

该命令用于构建、（重新）创建、启动并连接到容器。如果容器已经存在并且正在运行，`up` 命令不会重建或重启容器，除非它们被配置为在容器停止后重建。

- **语法**：
```bash
  docker-compose up [options] [SERVICE...]
```

- **常用选项**：
  - `-d`, `--detach`: 后台运行服务容器，并打印新容器的 ID。
  - `--build`: 在启动容器之前重新构建服务的镜像。
  - `--force-recreate`: 如果容器已经存在，则强制重新创建容器，无论容器的状态如何。
  - `--no-build`: 不重新构建镜像，即使 Dockerfile 存在。
  - `--no-start`: 创建容器但不启动它们。


- `docker-compose up -d` 命令用于启动由 Docker Compose 定义的多个服务容器，并在后台运行它们
	- `docker-compose up`：构建、（重新）创建、启动并关联一个多容器 Docker 应用。它会根据 `docker-compose.yml` 文件中的配置创建和启动所有服务容器。
	- `-d`：代表 "detached mode"（分离模式），即在后台运行容器。运行该命令后，你的终端会立即返回，你的容器将继续在后台运行。

- 该命令会执行以下操作：
1. **读取 `docker-compose.yml` 文件**：解析并理解文件中的所有配置。
2. **拉取镜像**：如果本地没有定义的镜像，Docker Compose 会从 Docker Hub 或其他指定的镜像仓库拉取这些镜像。
3. **创建并启动容器**：根据文件中的配置创建和启动所有定义的服务容器。
4. **后台运行**：由于使用了 `-d` 选项，所有容器会在后台运行，你的终端会立即返回，而不会显示容器的输出。

 `docker-compose.yml` 文件:：

```yaml
version: '3.8'

services:
  squid:
    image: ubuntu/squid:latest
    container_name: squid-container
    ports:
      - "3128:3128"

  nginx:
    image: nginx:latest
    container_name: nginx-container
    ports:
      - "8080:80"

  dify:
    image: langgenius/dify-sandbox:0.2.1
    container_name: dify-sandbox-container

  redis:
    image: redis:6-alpine
    container_name: redis-container
    ports:
      - "6379:6379"

  weaviate:
    image: semitechnologies/weaviate:1.19.0
    container_name: weaviate-container
    ports:
      - "8081:8080"
```



### 5.2 `docker-compose down`
该命令用于停止并删除由 `docker-compose up` 或 `docker-compose run` 创建的容器、网络、卷以及镜像。

- **语法**：
```bash
  docker-compose down [options]
```

- **常用选项**：
  - `--volumes`, `-v`: 删除数据卷。
  - `--rmi`, `--remove-orphans`: 删除未被任何服务使用的镜像。



### 5.3 `docker-compose start`

此命令用于启动已创建但停止的容器。

- **语法**：
```bash
  docker-compose start [SERVICE...]
  ```

### 5.4 `docker-compose stop`

此命令用于停止正在运行的容器。

- **语法**：
```bash
  docker-compose stop [SERVICE...]
```

### 5.5 `docker-compose restart`

此命令用于重启容器。

- **语法**：
```bash
  docker-compose restart [SERVICE...]
```

### 5.6 `docker-compose build`

此命令用于构建或重新构建服务的 Docker 镜像。

- **语法**：
```bash
  docker-compose build [options] [SERVICE...]
```

- **常用选项**：
  - `--no-cache`: 构建镜像时不使用缓存。
  - `--pull`: 尝试拉取镜像的基础镜像的最新版本。

### 5.7 `docker-compose logs`

此命令用于查看服务容器的日志输出。

- **语法**：
```bash
  docker-compose logs [options] [SERVICE...]
```

- **常用选项**：
  - `-f`, `--follow`: 跟踪日志输出。
  - `--tail`: 输出日志的行数。
  - `--timestamps`: 显示时间戳。

- **示例**：
  ```bash
  docker-compose logs -f
  ```
  这个命令将以流式方式显示最新的日志输出。

### 5.8  `docker-compose ps`

此命令用于列出所有服务的容器状态。

- **语法**：
```bash
  docker-compose ps [SERVICE...]
```

### 5.9 `docker-compose config`

此命令用于验证并查看组合文件的完整有效配置。

- **语法**：
```bash
  docker-compose config [options]
```

### 5.10 `docker-compose exec`

此命令用于在运行的容器中执行命令。

- **语法**：
```bash
  docker-compose exec [options] SERVICE COMMAND [ARGS...]
```

- **示例**：
```bash
docker-compose exec webserver tail -f /var/log/nginx/access.log
```

### 5.11 `docker-compose run`

此命令用于运行一个一次性命令容器。

- **语法**：
```bash
  docker-compose run [options] [SERVICE] [COMMAND] [ARGS...]
```

- **常用选项**：
  - `-d`, `--detach`: 后台运行命令容器。
  - `--service-ports`: 分配容器的端口到主机，就像在服务定义中一样。

- **示例**：
```bash
docker-compose run webserver sh -c "nginx -t"
```


### 5.12 `docker-compose config`
- **用途**：验证并查看组合文件的配置。
- **示例**：
  ```sh
  docker-compose config
  ```

### 5.13 `docker-compose images`
- **用途**：列出项目中使用的镜像。
- **示例**：
  ```sh
  docker-compose images
  ```

### 5.14 `docker-compose rm`
- **用途**：移除服务的容器。
- **示例**：
  ```sh
  docker-compose rm
  ```

### 5.15 `docker-compose pull`
- **用途**：拉取服务的镜像。
- **示例**：
  ```sh
  docker-compose pull
  ```

### 5.16 `docker-compose push`
- **用途**：推送服务的镜像到仓库。
- **示例**：
  ```sh
  docker-compose push
  ```

### 5.17 `docker-compose attach`
- **用途**：连接到服务容器的标准输入、输出和错误流。
- **示例**：
  ```sh
  docker-compose attach webserver
  ```

### 5.18 `docker-compose port`
- **用途**：打印容器公开端口的绑定端口。
- **示例**：
  ```sh
  docker-compose port webserver 80
  ```

### 5.19 `docker-compose pause`
- **用途**：暂停服务容器。
- **示例**：
  ```sh
  docker-compose pause
  ```

### 5.20 `docker-compose unpause`
- **用途**：取消暂停服务容器。
- **示例**：
  ```sh
  docker-compose unpause
  ```

### 5.21 `docker-compose top`
- **用途**：显示容器中运行的进程列表。
- **示例**：
  ```sh
  docker-compose top webserver
  ```

### 5.22 `docker-compose stats`
- **用途**：显示容器资源使用情况。
- **示例**：
  ```sh
  docker-compose stats
  ```

### 5.23 `docker-compose version`
- **用途**：显示 Docker Compose 的版本信息。
- **示例**：
  ```sh
  docker-compose version
  ```

### 5.24 `docker-compose cp`
- **用途**：复制文件/目录在服务容器和本地文件系统之间。
- **示例**：
  ```sh
  docker-compose cp webserver:/path/in/container /local/path
  ```

### 5.25 `docker-compose events`
- **用途**：接收来自容器的真实事件。
- **示例**：
  ```sh
  docker-compose events
  ```

### 5.26 `docker-compose watch`
- **用途**：监视服务的构建上下文，并在文件更新时重新构建和刷新容器。
- **示例**：
  ```sh
  docker-compose watch
  ```

### 5.27 `docker-compose ls`
- **用途**：列出运行中的组合项目。
- **示例**：
  ```sh
  docker-compose ls
  ```

### 5.28 `docker-compose kill`
- **用途**：强制停止服务容器。
- **示例**：
  ```sh
  docker-compose kill
  ```

### 5.29 `docker-compose wait`
- **用途**：阻塞直到第一个服务容器停止。
- **示例**：
  ```sh
  docker-compose wait
  ```

# 应用

## Docker Desktop
![[img_v3_02d1_77940bf2-d0fa-48ba-b2b7-fdfb0795a33g.jpg]]

![[Pasted image 20240724140243.png]]


```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://docker.nju.edu.cn",
    "https://mirror.baidubce.com",
    "https://mirror.iscas.ac.cn"
  ]
}
```


## dockerfile

![[Debian与Ubuntu编译环境配置说明.pdf]]

![[Docker — 从入门到实践（v1.1.0）.pdf]]

![[Rockchip_User_Guide_SDK_Docker_CN.pdf]]

### 01-setup-env.sh
```bash
#!/bin/bash
#
# install and setup docker enviroment
#
# author:
#    huaqiyan <huaqiyan@zlg.cn> 2022-07-28
#

# install docker packages
DOCKER=`apt list --installed 2>/dev/null | grep -E "docker.io|docker-ce"`
if [ -z $DOCKER ]; then
    apt-get update
    apt-get install -y docker.io
fi

# set run docker env for current user
usermod -a -G docker $USER
sed -i -e '/\%sudo/c %sudo  ALL=(ALL) NOPASSWD: ALL' /etc/sudoers

# build debian/ubuntu docker images from docker-file
mkdir ctx
cd ctx
docker build -f ../docker-file-buster.txt -t debian/arm-multicross:buster .
docker build -f ../docker-file-focal.txt -t ubuntu/arm-multicross:20.04 .
cd -

exit 0
```

### docker-file-focal.txt
```dockerfile
FROM ubuntu:20.04 as ubuntu-base
MAINTAINER huaqiyan <huaqiyan@zlg.cn>
RUN \
sed -i 's/archive.ubuntu.com/192.168.23.90/;s/security.ubuntu.com/192.168.23.90/' /etc/apt/sources.list && \
DEBIAN_FRONTEND=noninteractive apt-get update -y && \
DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y && \
DEBIAN_FRONTEND=noninteractive apt-get install -y \
    adduser \
    apparmor \
    apt-transport-https \
    apt-utils \
    apt \
    autoconf \
    autotools-dev \
    base-files \
    base-passwd \
    bash-completion \
    bash \
    bc \
    binfmt-support \
    binutils \
    bison \
    bsdutils \
    bzip2 \
    ca-certificates \
    cmake \
    coreutils \
    cpio \
    crossbuild-essential-arm64 \
    crossbuild-essential-armhf \
    curl \
    dash \
    dbus \
    debconf \
    debianutils \
    debootstrap \
    device-tree-compiler \
    dialog \
    diffutils \
    distro-info-data \
    dpkg-dev \
    dpkg \
    e2fsprogs \
    expect \
    fakeroot \
    fdisk \
    file \
    findutils \
    flex \
    gawk \
    gcc-10-base \
    gcc-8-aarch64-linux-gnu \
    gcc-8-arm-linux-gnueabihf \
    git-man \
    git \
    gpgv \
    grep \
    gzip \
    hostname \
    init-system-helpers \
    intltool \
    iproute2 \
    jq \
    kmod \
    language-pack-zh-hans-base \
    language-pack-zh-hans \
    less \
    lib32stdc++6 \
    libacl1 \
    libapt-pkg6.0 \
    libattr1 \
    libaudit-common \
    libaudit1 \
    libblkid1 \
    libbz2-1.0 \
    libc-bin \
    libc-dev-bin \
    libc6-arm64-cross \
    libc6-armhf-cross \
    libc6-dev-arm64-cross \
    libc6-dev-armhf-cross \
    libc6-dev \
    libc6-i386 \
    libc6 \
    libcap-ng0 \
    libcom-err2 \
    libcrypt1 \
    libcups2 \
    libcurl3-gnutls \
    libcurl4 \
    libdb5.3 \
    libdbus-1-3 \
    libdebconfclient0 \
    libdpkg-perl \
    libext2fs2 \
    libfdisk1 \
    libffi7 \
    libgcc-s1 \
    libgcrypt20 \
    libglade2-dev \
    libglib2.0-dev \
    libgmp10 \
    libgnutls30 \
    libgpg-error0 \
    libhogweed5 \
    libidn2-0 \
    libkeyutils1 \
    libkmod2 \
    libldap-2.4-2 \
    libldap-common \
    liblz4-1 \
    liblz4-tool \
    liblzma5 \
    libmount1 \
    libncurses5-dev \
    libncurses6 \
    libncursesw6 \
    libnettle7 \
    libnss-systemd \
    libp11-kit0 \
    libpam-modules-bin \
    libpam-modules \
    libpam-runtime \
    libpam-systemd \
    libpam0g \
    libpcre16-3 \
    libpcre2-8-0 \
    libpcre3-dev \
    libpcre32-3 \
    libpcre3 \
    libpcrecpp0v5 \
    libprocps8 \
    libseccomp2 \
    libselinux1 \
    libsemanage-common \
    libsemanage1 \
    libsepol1-dev \
    libsepol1 \
    libsigsegv2 \
    libsmartcols1 \
    libsqlite3-0 \
    libss2 \
    libssl-dev \
    libssl1.1 \
    libstdc++6 \
    libsystemd0 \
    libtasn1-6 \
    libtiff5 \
    libtinfo6 \
    libudev-dev \
    libudev1 \
    libunistring2 \
    libusb-1.0-0-dev \
    libuuid1 \
    libxml2-dev \
    libxml2-utils \
    libxml2 \
    libzstd1 \
    linux-libc-dev-arm64-cross \
    linux-libc-dev-armhf-cross \
    linux-libc-dev \
    live-build \
    locales \
    login \
    logsave \
    lsb-base \
    m4 \
    make \
    man-db \
    mawk \
    meson \
    mount \
    mtools \
    ncurses-base \
    ncurses-bin \
    net-tools \
    networkd-dispatcher \
    ninja-build \
    nlohmann-json3-dev \
    openssh-client \
    openssh-server \
    openssh-sftp-server \
    openssl \
    parted \
    passwd \
    patch \
    perl-base \
    perl \
    procps \
    python3 \
    qemu-user-static \
    rsync \
    sed \
    sensible-utils \
    sudo \
    systemd-sysv \
    systemd-timesyncd \
    systemd \
    sysvinit-utils \
    tar \
    time \
    tree \
    tzdata \
    u-boot-tools \
    ubuntu-keyring \
    udev \
    unzip \
    util-linux \
    vim \
    wget \
    zfsutils-linux \
    zip \
    zlib1g && \
echo 'export TERM=xterm-color' >> ${HOME}/.bashrc && \
echo 'Asia/Shanghai' > /etc/timezone && \
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
sed -i -e '/^%sudo*/s/ALL$/NOPASSWD:ALL/' /etc/sudoers && \
sed -i 's/^#\ zh_CN/zh_CN/' /etc/locale.gen && \
locale-gen && \
echo 'LANG="zh_CN.UTF-8"' > /etc/default/locale && \
echo 'LANGUAGE="zh_CN:en"' >> /etc/default/locale && \
rm -rf /var/cache/*  /var/lib/apt/list/*
```

### docker-file-buster.txt
```dockerfile
FROM debian:buster as init-systemd
MAINTAINER huaqiyan <huaqiyan@zlg.cn>
RUN \
export TERM=xterm-color && \
sed -i 's/deb.debian.org/192.168.23.90/;s/security.debian.org/192.168.23.90/;s/main$/main contrib non-free/' /etc/apt/sources.list && \
DEBIAN_FRONTEND=noninteractive apt-get update -y && \
DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y && \
DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apt-transport-https \
    apt-utils \
    autoconf \
    autotools-dev \
    base-files \
    bash-completion \
    bash \
    bc \
    binfmt-support \
    binutils \
    bison \
    bzip2 \
    ca-certificates \
    cmake \
    cpio \
    crossbuild-essential-arm64 \
    crossbuild-essential-armhf \
    curl \
    debootstrap \
    device-tree-compiler \
    dialog \
    dirmngr \
    dpkg-dev \
    dpkg \
    expect \
    fakeroot \
    file \
    flex \
    fonts-noto-cjk-extra \
    gawk \
    git \
    gnupg-l10n \
    gnupg-utils \
    gnupg \
    gpg-agent \
    gpg-wks-client \
    gpg-wks-server \
    gpg \
    gpgconf \
    gpgsm \
    gpgv \
    gzip \
    icu-devtools \
    intltool \
    iproute2 \
    iputils-ping \
    jq \
    kmod \
    less \
    lib32stdc++6 \
    libc-bin \
    libc-dev-bin \
    libc-l10n \
    libc6-dev \
    libc6-i386 \
    libc6 \
    libcups2 \
    libdpkg-perl \
    libexpat1-dev \
    libexpat1 \
    libglade2-dev \
    libglib2.0-dev \
    libgmp10 \
    libicu-dev \
    libicu63 \
    libldap-2.4-2 \
    libldap-common \
    liblz4-tool \
    liblzma5 \
    libncurses5-dev \
    libsasl2-2 \
    libsasl2-modules-db \
    libsasl2-modules \
    libsigsegv2 \
    libssl-dev \
    libssl1.1 \
    libtiff5 \
    libudev-dev \
    libusb-1.0-0-dev \
    libxml2-dev \
    libxml2-utils \
    libxml2 \
    linux-libc-dev \
    live-build \
    locales \
    m4 \
    make \
    meson \
    mtd-utils \
    mtools \
    ninja-build \
    nlohmann-json-dev \
    openssh-client \
    openssh-server \
    openssl \
    parted \
    patch \
    perl \
    publicsuffix \
    python3 \
    qemu-user-static \
    rsync \
    sed \
    sudo \
    systemd-sysv \
    systemd \
    tar \
    time \
    tree \
    tzdata \
    u-boot-tools \
    unzip \
    vim-common \
    vim-runtime \
    vim \
    wget \
    xxd \
    xz-utils \
    zip \
    zlib1g-dev \
    zlib1g && \
echo "export TERM=xterm-color" >> ${HOME}/.bashrc && \
echo 'Asia/Shanghai' > /etc/timezone && \
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
sed -i -e '/^%sudo*/s/ALL$/NOPASSWD:ALL/' /etc/sudoers && \
sed -i 's/^#\ zh_CN/zh_CN/' /etc/locale.gen && \
locale-gen && \
echo 'LANG=zh_CN.UTF-8' > /etc/default/locale && \
echo 'LANGUAGE=zh_CN:en' >> /etc/default/locale && \
rm -rf /var/cache/*  /var/lib/apt/list/*
```