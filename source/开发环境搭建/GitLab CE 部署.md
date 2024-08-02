
# 部署 GitLab
## 下载镜像
```bash
docker pull gitlab/gitlab-ce
```

## 创建持久化目录

```bash
D:\docker
└── gitlab
    ├── config
    ├── data
    └── logs
```

成功创建GitLab并允许后，目录内容如下
```bash
D:\docker
├── docker-compose.yml
└── gitlab
    ├── config
    │   ├── gitlab-secrets.json
    │   ├── gitlab.rb
    │   ├── initial_root_password
    │   ├── ssh_host_ecdsa_key
    │   ├── ssh_host_ecdsa_key.pub
    │   ├── ssh_host_ed25519_key
    │   ├── ssh_host_ed25519_key.pub
    │   ├── ssh_host_rsa_key
    │   ├── ssh_host_rsa_key.pub
    │   └── trusted-certs
    ├── data
    │   ├── alertmanager
    │   ├── backups
    │   ├── bootstrapped
    │   ├── git-data
    │   ├── gitaly
    │   ├── gitlab-ci
    │   ├── gitlab-exporter
    │   ├── gitlab-kas
    │   ├── gitlab-rails
    │   ├── gitlab-shell
    │   ├── gitlab-workhorse
    │   ├── logrotate
    │   ├── nginx
    │   ├── postgres-exporter
    │   ├── postgresql
    │   ├── prometheus
    │   ├── public_attributes.json
    │   ├── redis
    │   └── trusted-certs-directory-hash
    └── logs
        ├── alertmanager
        ├── gitaly
        ├── gitlab-exporter
        ├── gitlab-kas
        ├── gitlab-rails
        ├── gitlab-shell
        ├── gitlab-workhorse
        ├── logrotate
        ├── nginx
        ├── postgres-exporter
        ├── postgresql
        ├── prometheus
        ├── puma
        ├── reconfigure
        ├── redis
        ├── redis-exporter
        ├── sidekiq
        └── sshd
```

其中有如下的两个文件
```
D:\docker\gitlab\config\gitlab.rb
D:\docker\gitlab\config\initial_root_password
```

1. `gitlab.rb`
 GitLab 的主要配置文件，它包含了 GitLab 实例的各种配置选项。这些配置选项可以用来定制 GitLab 的行为，例如数据库设置、邮件服务器设置、存储路径等。在 GitLab Docker 镜像中，您可以修改此文件来更改 GitLab 的配置。

2. `initial_root_password`
用来存储 GitLab 初始管理员（root 用户）密码的文本文件。当您第一次启动 GitLab Docker 容器时，GitLab 会读取这个文件的内容作为 root 用户的初始密码。如果该文件不存在或为空，GitLab 会生成一个随机密码并将其打印到容器的日志中。

## 创建 Docker Compose 文件
```yml
services: # 开启服务
  gitlab: # 服务名称
    image: 'gitlab/gitlab-ce:latest' # 使用镜像
    container_name: gitlab
    restart: always
    environment: # 环境配置
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://192.168.1.168:1080' # 没有域名，则直接使用宿主机的IP
        gitlab_rails['gitlab_shell_ssh_port'] = 1022
    ports: # 端口映射，格式为“本机IP：Docker镜像内部IP”
      - '1080:1080'
      - '1022:22'
    volumes: # 挂载卷
    # 前面是Windows的地址所以斜杠向右；后面是Linux的地址所以向左
      - D:\docker\gitlab\config:/etc/gitlab
      - D:\docker\gitlab\data:/var/opt/gitlab
      - D:\docker\gitlab\logs:/var/log/gitlab
```

对应的目录如下：
```bash
D:\docker
├── docker-compose.yml
└── gitlab
    ├── config
    ├── data
    └── logs
```

其中：
```bash
# 如果使用公有云且配置了域名了，可以直接设置为域名，如下
external_url 'http://gitlab.leefly.com'
# 如果没有域名，则直接使用宿主机的ip，如下
external_url 'http://192.168.1.168:1080'
```

需要配置宿主机的 IP
![[Pasted image 20240802134153.png]]


持久化配置

| 本地位置                      | 容器位置              | 用途                |
| :------------------------ | ----------------- | ----------------- |
| `D:\docker\gitlab\data`   | `/var/opt/gitlab` | 用于存储应用程序数据。       |
| `D:\docker\gitlab\logs`   | `/var/log/gitlab` | 用于存储日志。           |
| `D:\docker\gitlab\config` | `/etc/gitlab`     | 用于存储 GitLab 配置文件。 |

可参考如下示例
[docker-compose.yml](https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml)

## 启动 GitLab

在`docker-compose.yml`同一目录中运行

```bash
docker compose up -d
```

需要注意的时，Gitlab 服务启动速度比较慢，可能需要 2 分钟，使用`docker-compose logs -f gitlab`实时查看 gitlab 服务的启动日志，过了几分钟等待日志不再滚动后就启动成功了

- 可执行如下命令查看是否启动成功
```bash
docker ps
```

```bash
PS D:\docker> docker ps
CONTAINER ID   IMAGE                     COMMAND             CREATED       STATUS                 PORTS                                                           NAMES
145f1afe5235   gitlab/gitlab-ce:latest   "/assets/wrapper"   3 hours ago   Up 3 hours (healthy)   80/tcp, 443/tcp, 0.0.0.0:1080->1080/tcp, 0.0.0.0:1022->22/tcp   gitlab
```

或者使用命令
```bash
docker-compose ps
```

```bash
PS D:\docker> docker-compose ps
NAME      IMAGE                     COMMAND             SERVICE   CREATED       STATUS                 PORTS
gitlab    gitlab/gitlab-ce:latest   "/assets/wrapper"   gitlab    3 hours ago   Up 3 hours (healthy)   80/tcp, 443/tcp, 0.0.0.0:1080->1080/tcp, 0.0.0.0:1022->22/tcp
```

同时可以执行如下命令查看 GitLab 的启动状态
```bash
docker exec -it gitlab /bin/bash
gitlab-ctl status
netstat -tuln
```

```bash
PS D:\docker> docker exec -it gitlab /bin/bash

root@145f1afe5235:/# gitlab-ctl status
run: alertmanager: (pid 1041) 10045s; run: log: (pid 870) 10071s
run: gitaly: (pid 295) 10144s; run: log: (pid 333) 10141s
run: gitlab-exporter: (pid 1003) 10046s; run: log: (pid 635) 10089s
run: gitlab-kas: (pid 480) 10122s; run: log: (pid 496) 10119s
run: gitlab-workhorse: (pid 988) 10047s; run: log: (pid 584) 10101s
run: logrotate: (pid 5334) 2956s; run: log: (pid 275) 10153s
run: nginx: (pid 595) 10098s; run: log: (pid 624) 10095s
run: postgres-exporter: (pid 1056) 10045s; run: log: (pid 948) 10065s
run: postgresql: (pid 337) 10138s; run: log: (pid 342) 10137s
run: prometheus: (pid 1014) 10046s; run: log: (pid 847) 10077s
run: puma: (pid 499) 10116s; run: log: (pid 508) 10113s
run: redis: (pid 278) 10150s; run: log: (pid 287) 10149s
run: redis-exporter: (pid 1005) 10046s; run: log: (pid 774) 10083s
run: sidekiq: (pid 522) 10110s; run: log: (pid 538) 10109s
run: sshd: (pid 36) 10166s; run: log: (pid 35) 10166s

root@145f1afe5235:/# netstat -tuln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.1:9187          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9168          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9121          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9093          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9090          0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:8060            0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9229          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:9236          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.11:33465        0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8154          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8155          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8153          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8150          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8151          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8092          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8082          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8080          0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:1080            0.0.0.0:*               LISTEN
tcp        0      0 :::9094                 :::*                    LISTEN
tcp        0      0 :::22                   :::*                    LISTEN
tcp        0      0 ::1:9168                :::*                    LISTEN
udp        0      0 127.0.0.11:34303        0.0.0.0:*
udp        0      0 :::9094                 :::*
```

如上可知，GitLab 需要三个服务：`Gitlab`，`PostgreSQL`数据库和`Redis`缓存

# 访问 GitLab

- 输入`192.168.1.168:1080`进入 GitLab 的登录页面
- 输入账号密码进行登录，其中账号为`root`，密码可通过如下命令获取
```bash
docker exec -it gitlab cat /etc/gitlab/initial_root_password
```

如下输出，密码为：`VITDQQ7ysAAO5W7LdhO5dEWXgCMNqqabItFpzjVUon0`
```bash
PS D:\docker> docker exec -it gitlab cat /etc/gitlab/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: VITDQQ7ysAAO5W7LdhO5dEWXgCMNqqabItFpzjVUon0=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug gitlab
    Learn more at https://docs.docker.com/go/debug-cli/
```

# 配置 GitLab

## 修改密码
 
 [编辑密码](http://192.168.1.168:1080/-/user_settings/password/edit)
![[Pasted image 20240802131209.png]]

##  更改语言

[偏好设置](http://192.168.1.168:1080/-/profile/preferences)

![[Pasted image 20240802130753.png]]


##  创建用户
[新建用户](http://192.168.1.168:1080/admin/users/new)

![[Pasted image 20240802131856.png]]

## 创建工程
[新建项目](http://192.168.1.168:1080/projects/new#)

![[Pasted image 20240802132132.png]]

## 配置 SSH

[SSH密钥](http://192.168.1.168:1080/-/user_settings/ssh_keys)
![[Pasted image 20240802131300.png]]

## 配置 Git
```bash
git config --global user.name leev2v1
git config --global user.email "1144815495@qq.com"
git config -l

git remote -v
git branch
```

## 备份 GitLab 

[Docker 部署 Gitlab Docker 入门教程](https://wangchujiang.com/docker-tutorial/gitlab/index.html)

## GitLab Runner
[项目管理 之六 详解 Gitlab 本地部署全过程、Gitlab Pages、企业版 PATCH\_gitlab本地部署-CSDN博客](https://blog.csdn.net/ZCShouCSDN/article/details/127105387)


# 关闭 GitLab
```bash
docker-compose down
```

# 停止 GitLab
```bash
docker-compose stop
```

# 重启 GitLab
如果想重启 GitLab，可执行如下命令实现
```bash
docker restart gitlab
```

或者使用命令
```bash
docker exec -it gitlab /bin/bash
gitlab-ctl restart
```

# 延伸拓展
## 命令集
```bash
root@145f1afe5235:/# gitlab-ctl --help
omnibus-ctl: command (subcommand)
check-config
  Check if there are any configuration in gitlab.rb that is removed in specified version
deploy-page
  Put up the deploy page
diff-config
  Compare the user configuration with package available configuration
generate-secrets
  Generates secrets used in gitlab.rb
get-redis-master
  Get connection details to Redis master
remove-accounts
  Delete *all* users and groups used by this package
upgrade
  Run migrations after a package upgrade
upgrade-check
  Check if the upgrade is acceptable
General Commands:
  cleanse
    Delete *all* gitlab data, and start from scratch.
  help
    Print this help message.
  reconfigure
    Reconfigure the application.
  show-config
    Show the configuration that would be generated by reconfigure.
  uninstall
    Kill all processes and uninstall the process supervisor (data will be preserved).
Service Management Commands:
  graceful-kill
    Attempt a graceful stop, then SIGKILL the entire process group.
  hup
    Send the services a HUP.
  int
    Send the services an INT.
  kill
    Send the services a KILL.
  once
    Start the services if they are down. Do not restart them if they stop.
  restart
    Stop the services if they are running, then start them again.
  restart-except
    Restart all services except: service_name ...
  service-list
    List all the services (enabled services appear with a *.)
  start
    Start services if they are down, and restart them if they stop.
  status
    Show the status of all the services.
  stop
    Stop the services, and do not restart them.
  tail
    Watch the service logs of all enabled services.
  term
    Send the services a TERM.
  usr1
    Send the services a USR1.
  usr2
    Send the services a USR2.
Backup Commands:
  backup-etc
    Backup GitLab configuration [options]
Let's Encrypt Commands:
  renew-le-certs
    Renew the existing Let's Encrypt certificates
Database Commands:
  pg-decomposition-migration
    Migrate database to two-database setup
  pg-password-md5
    Generate MD5 Hash of user password in PostgreSQL format
  pg-upgrade
    Upgrade the PostgreSQL DB to the latest supported version
  revert-pg-upgrade
    Run this to revert to the previous version of the database
  set-replication-password
    Set database replication password
Gitaly Commands:
  praefect
    Interact with Gitaly cluster
Container Registry Commands:
  registry-database
    Manage Container Registry database.
  registry-garbage-collect
    Run Container Registry garbage collection.
Selinux Controls Commands:
  apply-sepolicy
    Apply GitLab SELinux policy to managed files
```

### 1. `gitlab-ctl reconfigure`
- **用途**：重新配置 GitLab 应用程序，这通常用于应用配置更改或更新后重新加载服务。
- **示例**：
  ```sh
  gitlab-ctl reconfigure
  ```

### 2. `gitlab-ctl status`
- **用途**：显示所有服务的状态。
- **示例**：
  ```sh
  gitlab-ctl status
  ```

### 3. `gitlab-ctl start`
- **用途**：启动所有服务。
- **示例**：
  ```sh
  gitlab-ctl start
  ```

### 4. `gitlab-ctl stop`
- **用途**：停止所有服务。
- **示例**：
  ```sh
  gitlab-ctl stop
  ```

### 5. `gitlab-ctl restart`
- **用途**：重启所有服务。
- **示例**：
  ```sh
  gitlab-ctl restart
  ```

### 6. `gitlab-ctl tail`
- **用途**：实时查看所有启用服务的日志。
- **示例**：
  ```sh
  gitlab-ctl tail
  ```

### 7. `gitlab-ctl diff-config`
- **用途**：比较用户配置与包可用配置之间的差异。
- **示例**：
  ```sh
  gitlab-ctl diff-config
  ```

### 8. `gitlab-ctl check-config`
- **用途**：检查 `gitlab.rb` 配置文件中是否存在已废弃或不再支持的配置项。
- **示例**：
  ```sh
  gitlab-ctl check-config
  ```

### 9. `gitlab-ctl generate-secrets`
- **用途**：生成在 `gitlab.rb` 中使用的密钥和密码。
- **示例**：
  ```sh
  gitlab-ctl generate-secrets
  ```

### 10. `gitlab-ctl get-redis-master`
- **用途**：获取 Redis 主节点的连接详细信息。
- **示例**：
  ```sh
  gitlab-ctl get-redis-master
  ```

### 11. `gitlab-ctl upgrade`
- **用途**：运行迁移脚本以升级 GitLab 数据库。
- **示例**：
  ```sh
  gitlab-ctl upgrade
  ```

### 12. `gitlab-ctl upgrade-check`
- **用途**：检查升级是否可接受。
- **示例**：
  ```sh
  gitlab-ctl upgrade-check
  ```

### 13. `gitlab-ctl backup-etc`
- **用途**：备份 GitLab 配置文件。
- **示例**：
  ```sh
  gitlab-ctl backup-etc
  ```

### 14. `gitlab-ctl renew-le-certs`
- **用途**：更新现有的 Let's Encrypt 证书。
- **示例**：
  ```sh
  gitlab-ctl renew-le-certs
  ```

### 15. `gitlab-ctl pg-decomposition-migration`
- **用途**：迁移数据库至双数据库架构。
- **示例**：
  ```sh
  gitlab-ctl pg-decomposition-migration
  ```

### 16. `gitlab-ctl pg-password-md5`
- **用途**：生成 PostgreSQL 用户密码的 MD5 哈希值。
- **示例**：
  ```sh
  gitlab-ctl pg-password-md5
  ```

### 17. `gitlab-ctl pg-upgrade`
- **用途**：升级 PostgreSQL 数据库到最新支持的版本。
- **示例**：
  ```sh
  gitlab-ctl pg-upgrade
  ```

### 18. `gitlab-ctl revert-pg-upgrade`
- **用途**：回滚到之前的数据库版本。
- **示例**：
  ```sh
  gitlab-ctl revert-pg-upgrade
  ```

### 19. `gitlab-ctl set-replication-password`
- **用途**：设置数据库复制密码。
- **示例**：
  ```sh
  gitlab-ctl set-replication-password
  ```

### 20. `gitlab-ctl praefect`
- **用途**：与 Gitaly 集群交互。
- **示例**：
  ```sh
  gitlab-ctl praefect
  ```

### 21. `gitlab-ctl registry-database`
- **用途**：管理容器注册表数据库。
- **示例**：
  ```sh
  gitlab-ctl registry-database
  ```

### 22. `gitlab-ctl registry-garbage-collect`
- **用途**：运行容器注册表垃圾回收。
- **示例**：
  ```sh
  gitlab-ctl registry-garbage-collect
  ```

### 23. `gitlab-ctl apply-sepolicy`
- **用途**：应用 GitLab SELinux 策略到受管理的文件。
- **示例**：
  ```sh
  gitlab-ctl apply-sepolicy
  ```

### 24. `gitlab-ctl deploy-page`
- **用途**：部署页面。
- **示例**：
  ```sh
  gitlab-ctl deploy-page
  ```

### 25. `gitlab-ctl cleanse`
- **用途**：删除所有 GitLab 数据并从头开始。
- **示例**：
  ```sh
  gitlab-ctl cleanse
  ```

### 26. `gitlab-ctl uninstall`
- **用途**：杀死所有进程并卸载进程监督器（数据会被保留）。
- **示例**：
  ```sh
  gitlab-ctl uninstall
  ```

### 27. `gitlab-ctl show-config`
- **用途**：显示将由 `reconfigure` 生成的配置。
- **示例**：
  ```sh
  gitlab-ctl show-config
  ```

### 28. `gitlab-ctl remove-accounts`
- **用途**：删除所有用户和组。
- **示例**：
  ```sh
  gitlab-ctl remove-accounts
  ```

### 29. `gitlab-ctl graceful-kill`
- **用途**：尝试优雅地停止服务，如果失败则发送 SIGKILL 终止整个进程组。
- **示例**：
  ```sh
  gitlab-ctl graceful-kill
  ```

### 30. `gitlab-ctl service-list`
- **用途**：列出所有服务（启用的服务前带有星号）。
- **示例**：
  ```sh
  gitlab-ctl service-list
  ```

### 31. `gitlab-ctl restart-except`
- **用途**：重启所有服务，除了指定的服务。
- **示例**：
  ```sh
  gitlab-ctl restart-except gitaly
  ```

### 32. `gitlab-ctl once`
- **用途**：启动服务，如果它们已经停止，则不会重新启动。
- **示例**：
  ```sh
  gitlab-ctl once
  ```

### 33. `gitlab-ctl hup`, `gitlab-ctl int`, `gitlab-ctl term`, `gitlab-ctl kill`, `gitlab-ctl usr1`, `gitlab-ctl usr2`
- **用途**：发送信号给服务。
- **示例**：
  ```sh
  gitlab-ctl hup
  ```


## 参考链接
[极狐GitLab Docker 镜像 | 极狐GitLab](https://docs.gitlab.cn/jh/install/docker.html)
[GitLab文档\_GitLab官方帮助文档\_极狐GitLab 帮助文档中心-极狐GitLab](https://docs.gitlab.cn/)
[GitLab Documentation](https://docs.gitlab.com/)
[GitHub - sameersbn/docker-gitlab: Dockerized GitLab](https://github.com/sameersbn/docker-gitlab)
[通过 docker-compose 快速部署 gitlab - 大数据老司机 - 博客园](https://www.cnblogs.com/liugp/p/17324433.html)
