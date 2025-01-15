[Run GitLab Runner in a container | GitLab](https://docs.gitlab.com/runner/install/docker.html)
[登录 · GitLab](http://192.168.1.168:1080/root/0806-build/-/settings/ci_cd#js-runners-settings)

#  用途

GitLab Runner 是 GitLab CI/CD 的核心组件，用于执行 GitLab 仓库中的 CI/CD 作业。它是 GitLab 用来自动化构建、测试和部署项目的工具，通常与 GitLab CI 配合使用。具体来说，GitLab Runner 的主要用途包括：

1. **构建项目**  
    GitLab Runner 用于自动化项目的构建过程，可以在各种环境（如不同的操作系统、容器、虚拟机等）中构建项目。
    
2. **运行测试**  
    它可以自动运行单元测试、集成测试等，以确保代码质量和功能的正确性。
    
3. **部署项目**  
    GitLab Runner 可以自动化部署流程，将代码从 GitLab 部署到生产环境、测试环境、预发布环境等。
    
4. **多平台支持**  
    GitLab Runner 支持在不同平台上运行 CI/CD 作业，比如在 Linux、Windows 和 macOS 上。它支持多种执行器，可以通过 Docker、Kubernetes、Shell 等方式执行任务。
    
5. **并行执行**  
    GitLab Runner 支持并行执行多个作业，可以根据配置的 Runner 数量并行执行多个任务，提升 CI/CD 流程的效率。
    

# 实现原理

GitLab Runner 实现的原理可以分为以下几个部分：

#### 1. **GitLab CI/CD 管道**

GitLab CI/CD 是 GitLab 提供的一套自动化集成和部署的系统。在 GitLab 中，CI/CD 流程由 **Pipeline**（管道）、**Jobs**（作业）、**Stages**（阶段）等组成。

- **Pipeline**：由多个阶段（Stages）组成，每个阶段包含多个作业（Jobs）。
- **Job**：作业是实际执行的单个任务，比如运行测试、构建项目、部署代码等。
- **Stage**：阶段是作业的逻辑分组，所有作业都属于某个阶段，阶段的顺序决定了作业的执行顺序。

#### 2. **GitLab Runner 注册与配置**

- GitLab Runner 通过 `gitlab-runner register` 命令与 GitLab 实例建立连接，并注册为一个可以执行作业的 Runner。
- Runner 的配置可以通过命令行或者 GitLab UI 进行，其中包括 Runner 所运行的环境、可用的执行器、所支持的标签等。

#### 3. **作业调度与执行**

- 当 GitLab 项目提交代码后，GitLab CI/CD 会创建一个 Pipeline，并调度相应的作业（Jobs）。
- GitLab 根据 Job 的标签和配置，选择合适的 GitLab Runner 执行这些作业。Runner 会根据配置的执行器（Executor）启动任务。

#### 4. **执行器（Executor）**

执行器决定了作业在哪种环境下执行。GitLab Runner 支持多种执行器，常见的执行器包括：

- **Shell**：在本地机器的 shell 环境中执行作业。
- **Docker**：使用 Docker 容器运行作业，在隔离的环境中执行任务。
- **Docker-SSH**：通过 SSH 在远程 Docker 主机上执行作业。
- **Kubernetes**：通过 Kubernetes 集群中的 Pod 运行作业。
- **Custom**：自定义执行器，可以指定自己的执行环境。

这些执行器的作用是将作业的执行从 Runner 所在的主机隔离出来，提供更加灵活和可扩展的执行环境。

#### 5. **作业的执行过程**

GitLab Runner 根据任务的配置，使用指定的执行器执行作业。比如，如果执行器是 Docker，GitLab Runner 会启动一个 Docker 容器，并在其中运行作业的命令。

- GitLab Runner 会拉取需要的镜像（如果未在本地缓存）并启动容器。
- 容器中的作业执行完成后，Runner 会将执行结果（比如构建日志）返回给 GitLab，显示在 Pipeline 中。

#### 6. **Runner 与 GitLab 的通信**

GitLab Runner 与 GitLab 服务器之间通过 HTTP 协议进行通信。具体来说：

- **注册**：GitLab Runner 使用注册令牌（Token）将自己注册到 GitLab 实例中，获取授权执行作业。
- **作业分配**：GitLab 服务器根据作业的配置和 Runner 的标签分配合适的 Runner 执行作业。
- **结果报告**：Runner 在执行作业后，会通过 API 将作业的结果（如日志、状态）报告给 GitLab。

#### 7. **并行与分布式执行**

- GitLab Runner 支持在多个机器上运行多个实例，这使得你可以通过多个 Runner 实例分布式执行作业，提高 CI/CD 流程的并行度。
- 你可以为不同的 GitLab 项目、不同的作业配置不同的 Runner，确保 CI/CD 流程高效且灵活。

#### 8. **CI/CD Pipeline 的自动化执行**

GitLab Runner 自动化执行 CI/CD 流程，无需人工干预。每当代码提交或 Push 到 GitLab 仓库时，GitLab 会触发一个新的 Pipeline，Runner 根据任务配置自动开始工作，执行构建、测试、部署等操作。

## 总结

GitLab Runner 是 GitLab CI/CD 的执行者，负责执行 GitLab CI 配置中的作业。它通过注册与 GitLab 服务器进行通信，拉取作业并执行。Runner 可以通过多种执行器（如 Docker、Kubernetes、Shell 等）在不同环境中执行任务，支持并行执行和分布式执行，提高 CI/CD 流程的效率。

![[img_v3_02ii_a966cbd5-4c38-4dda-9fd0-778e1f37488g 2.jpg]]
![[img_v3_02ii_4abd1ead-4610-4620-8d8b-8006e255373g.jpg]]
![[img_v3_02ii_13f6331e-7ba2-4e38-a458-483edbcf117g.jpg]]

![[img_v3_02ii_c6a3678b-336d-450c-9a21-b7ee8d5778eg.jpg]]

# 命令
```bash
root@b641b0802bca:/# gitlab-runner --help
NAME:
   gitlab-runner - a GitLab Runner

USAGE:
   gitlab-runner [global options] command [command options] [arguments...]

VERSION:
   17.2.1 (9882d9c7)

AUTHOR:
   GitLab Inc. <support@gitlab.com>

COMMANDS:
   list                  List all configured runners
   run                   run multi runner service
   register              register a new runner
   reset-token           reset a runner's token
   install               install service
   uninstall             uninstall service
   start                 start service
   stop                  stop service
   restart               restart service
   status                get status of a service
   run-single            start single runner
   unregister            unregister specific runner
   verify                verify all registered runners
   fleeting              manage fleeting plugins
   artifacts-downloader  download and extract build artifacts (internal)
   artifacts-uploader    create and upload build artifacts (internal)
   cache-archiver        create and upload cache artifacts (internal)
   cache-extractor       download and extract cache artifacts (internal)
   cache-init            changed permissions for cache paths (internal)
   health-check          check health for a specific address
   read-logs             reads job logs from a file, used by kubernetes executor (internal)
   help, h               Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --cpuprofile value           write cpu profile to file [$CPU_PROFILE]
   --debug                      debug mode [$RUNNER_DEBUG]
   --log-format value           Choose log format (options: runner, text, json) [$LOG_FORMAT]
   --log-level value, -l value  Log level (options: debug, info, warn, error, fatal, panic) [$LOG_LEVEL]
   --help, -h                   show help
   --version, -v                print the version
```

`gitlab-runner` 是 GitLab 提供的命令行工具，用于管理 GitLab CI/CD Runner。以下是一些常见的 `gitlab-runner` 命令用法及示例：

### 1. **查看帮助信息**

```bash
gitlab-runner --help
```

显示所有可用的命令和选项。

### 2. **列出所有已配置的 Runners**

```bash
gitlab-runner list
```

列出所有已注册和配置的 Runner。

![[img_v3_02ii_a966cbd5-4c38-4dda-9fd0-778e1f37488g.jpg]]
```bash
root@b641b0802bca:/# gitlab-runner list
Runtime platform                                    arch=amd64 os=linux pid=82 revision=9882d9c7 version=17.2.1
Listing configured runners                          ConfigFile=/etc/gitlab-runner/config.toml
ubuntu-build-runner                                 Executor=docker Token=glrt-t3_HkpJCRrYCsGUEd4yeagQ URL=http://192.168.1.168:1080
```

```bash
root@b641b0802bca:/# ll /etc/gitlab-runner/
total 20
drwx------ 3 root root 4096 Jan 15 01:54 ./
drwxr-xr-x 1 root root 4096 Jan 15 01:54 ../
-rw------- 1 root root   14 Jan 15 01:54 .runner_system_id
drwx------ 2 root root 4096 Jul 25 19:14 certs/
-rwx------ 1 root root  771 Jan 15 02:12 config.toml*

root@b641b0802bca:/# ll /etc/gitlab-runner/certs/
total 8
drwx------ 2 root root 4096 Jul 25 19:14 ./
drwx------ 3 root root 4096 Jan 15 01:54 ../

root@b641b0802bca:/# cat /etc/gitlab-runner/config.toml
concurrent = 1
check_interval = 0
connection_max_age = "15m0s"
shutdown_timeout = 0
[session_server]
  session_timeout = 1800
[[runners]]
  name = "ubuntu-build-runner"
  url = "http://192.168.1.168:1080"
  id = 2
  token = "glrt-t3_HkpJCRrYCsGUEd4yeagQ"
  token_obtained_at = 2025-01-15T02:10:02Z
  token_expires_at = 0001-01-01T00:00:00Z
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    MaxUploadedArchiveSize = 0
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "ubuntu_zy:18.04.06"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
    network_mtu = 0
```


### 3. **启动 Runner 服务**

```bash
gitlab-runner start
```

启动 GitLab Runner 服务，确保 Runner 可以执行任务。

### 4. **停止 Runner 服务**

```bash
gitlab-runner stop
```

停止 GitLab Runner 服务。

### 5. **重新启动 Runner 服务**

```bash
gitlab-runner restart
```

重启 GitLab Runner 服务，通常用于应用配置更改。

### 6. **注册新的 Runner**

```bash
gitlab-runner register
```

注册一个新的 GitLab Runner，通常用于将 Runner 与 GitLab 实例进行连接。执行此命令后，会提示输入 GitLab 实例的 URL 和 Runner 的注册令牌。

```bash
root@b641b0802bca:/# gitlab-runner register --help
Runtime platform                                    arch=amd64 os=linux pid=378 revision=9882d9c7 version=17.2.1
NAME:
   gitlab-runner register - register a new runner

USAGE:
   gitlab-runner register [command options] [arguments...]

OPTIONS:
   -c value, --config value                                                                   Config file (default: "/etc/gitlab-runner/config.toml") [$CONFIG_FILE]
   --template-config value                                                                    Path to the configuration template file [$TEMPLATE_CONFIG_FILE]
   --tag-list value                                                                           Tag list [$RUNNER_TAG_LIST]
   -n, --non-interactive                                                                      Run registration unattended [$REGISTER_NON_INTERACTIVE]
   --leave-runner                                                                             Don't remove runner if registration fails [$REGISTER_LEAVE_RUNNER]
   -r value, --registration-token value                                                       Runner's registration token [$REGISTRATION_TOKEN]
   --run-untagged                                                                             Register to run untagged builds; defaults to 'true' when 'tag-list' is empty [$REGISTER_RUN_UNTAGGED]
   --locked                                                                                   Lock Runner for current project, defaults to 'true' [$REGISTER_LOCKED]
   --access-level value                                                                       Set access_level of the runner to not_protected or ref_protected; defaults to not_protected [$REGISTER_ACCESS_LEVEL]
   --maximum-timeout value                                                                    What is the maximum timeout (in seconds) that will be set for job when using this Runner (default: "0") [$REGISTER_MAXIMUM_TIMEOUT]
   --paused                                                                                   Set Runner to be paused, defaults to 'false' [$REGISTER_PAUSED]
   --maintenance-note value                                                                   Runner's maintenance note [$REGISTER_MAINTENANCE_NOTE]
   --name value, --description value                                                          Runner name (default: "b641b0802bca") [$RUNNER_NAME]
   --limit value                                                                              Maximum number of builds processed by this runner (default: "0") [$RUNNER_LIMIT]
   --output-limit value                                                                       Maximum build trace size in kilobytes (default: "0") [$RUNNER_OUTPUT_LIMIT]
   --request-concurrency value                                                                Maximum concurrency for job requests (default: "0") [$RUNNER_REQUEST_CONCURRENCY]
   --unhealthy-requests-limit value                                                           The number of 'unhealthy' responses to new job requests after which a runner worker will be disabled (default: "0") [$RUNNER_UNHEALTHY_REQUESTS_LIMIT]
   --unhealthy-interval value                                                                 Duration for which a runner worker is disabled after exceeding the unhealthy requests limit. Supports syntax like '3600s', '1h30min' etc
   -u value, --url value                                                                      GitLab instance URL [$CI_SERVER_URL]
   -t value, --token value                                                                    Runner token [$CI_SERVER_TOKEN]
   --tls-ca-file value                                                                        File containing the certificates to verify the peer when using HTTPS [$CI_SERVER_TLS_CA_FILE]
   --tls-cert-file value                                                                      File containing certificate for TLS client auth when using HTTPS [$CI_SERVER_TLS_CERT_FILE]
   --tls-key-file value                                                                       File containing private key for TLS client auth when using HTTPS [$CI_SERVER_TLS_KEY_FILE]
   --executor value                                                                           Select executor, eg. shell, docker, etc. [$RUNNER_EXECUTOR]
   --builds-dir value                                                                         Directory where builds are stored [$RUNNER_BUILDS_DIR]
   --cache-dir value                                                                          Directory where build cache is stored [$RUNNER_CACHE_DIR]
   --clone-url value                                                                          Overwrite the default URL used to clone or fetch the git ref [$CLONE_URL]
   --env value                                                                                Custom environment variables injected to build environment [$RUNNER_ENV]
   --pre-get-sources-script value                                                             Runner-specific commands to be executed on the runner before updating the Git repository an updating submodules. [$RUNNER_PRE_GET_SOURCES_SCRIPT]
   --post-get-sources-script value                                                            Runner-specific commands to be executed on the runner after updating the Git repository and updating submodules. [$RUNNER_POST_GET_SOURCES_SCRIPT]
   --pre-build-script value                                                                   Runner-specific command script executed just before build executes [$RUNNER_PRE_BUILD_SCRIPT]
   --post-build-script value                                                                  Runner-specific command script executed just after build executes [$RUNNER_POST_BUILD_SCRIPT]
   --debug-trace-disabled                                                                     When set to true Runner will disable the possibility of using the CI_DEBUG_TRACE feature [$RUNNER_DEBUG_TRACE_DISABLED]
   --safe-directory-checkout value                                                            When set to true, Git global configuration will get a safe.directory directive pointing the job's working directory' [$RUNNER_SAFE_DIRECTORY_CHECKOUT]
   --shell value                                                                              Select bash, sh, cmd, pwsh or powershell [$RUNNER_SHELL]
   --custom_build_dir-enabled                                                                 Enable job specific build directories [$CUSTOM_BUILD_DIR_ENABLED]
   --cache-type value                                                                         Select caching method [$CACHE_TYPE]
   --cache-path value                                                                         Name of the path to prepend to the cache URL [$CACHE_PATH]
   --cache-shared                                                                             Enable cache sharing between runners. [$CACHE_SHARED]
   --cache-max_uploaded_archive_size value                                                    Limit the size of the cache archive being uploaded to cloud storage, in bytes. (default: "0") [$CACHE_MAXIMUM_UPLOADED_ARCHIVE_SIZE]
   --cache-s3-server-address value                                                            A host:port to the used S3-compatible server [$CACHE_S3_SERVER_ADDRESS]
   --cache-s3-access-key value                                                                S3 Access Key [$CACHE_S3_ACCESS_KEY]
   --cache-s3-secret-key value                                                                S3 Secret Key [$CACHE_S3_SECRET_KEY]
   --cache-s3-session-token value                                                             S3 Session Token [$CACHE_S3_SESSION_TOKEN]
   --cache-s3-bucket-name value                                                               Name of the bucket where cache will be stored [$CACHE_S3_BUCKET_NAME]
   --cache-s3-bucket-location value                                                           Name of S3 region [$CACHE_S3_BUCKET_LOCATION]
   --cache-s3-insecure                                                                        Use insecure mode (without https) [$CACHE_S3_INSECURE]
   --cache-s3-authentication_type value                                                       IAM or credentials [$CACHE_S3_AUTHENTICATION_TYPE]
   --cache-s3-server-side-encryption value                                                    Server side encryption type (S3, or KMS) [$CACHE_S3_SERVER_SIDE_ENCRYPTION]
   --cache-s3-server-side-encryption-key-id value                                             Server side encryption key ID (alias or Key ID) [$CACHE_S3_SERVER_SIDE_ENCRYPTION_KEY_ID]
   --cache-gcs-access-id value                                                                ID of GCP Service Account used to access the storage [$CACHE_GCS_ACCESS_ID]
   --cache-gcs-private-key value                                                              Private key used to sign GCS requests [$CACHE_GCS_PRIVATE_KEY]
   --cache-gcs-credentials-file value                                                         File with GCP credentials, containing AccessID and PrivateKey [$GOOGLE_APPLICATION_CREDENTIALS]
   --cache-gcs-bucket-name value                                                              Name of the bucket where cache will be stored [$CACHE_GCS_BUCKET_NAME]
   --cache-azure-account-name value                                                           Account name for Azure Blob Storage [$CACHE_AZURE_ACCOUNT_NAME]
   --cache-azure-account-key value                                                            Access key for Azure Blob Storage [$CACHE_AZURE_ACCOUNT_KEY]
   --cache-azure-container-name value                                                         Name of the Azure container where cache will be stored [$CACHE_AZURE_CONTAINER_NAME]
   --cache-azure-storage-domain value                                                         Domain name of the Azure storage (e.g. blob.core.windows.net) [$CACHE_AZURE_STORAGE_DOMAIN]
   --feature-flags value                                                                      Enable/Disable feature flags https://docs.gitlab.com/runner/configuration/feature-flags.html (default: "{}") [$FEATURE_FLAGS]
   --ssh-user value                                                                           User name [$SSH_USER]
   --ssh-password value                                                                       User password [$SSH_PASSWORD]
   --ssh-host value                                                                           Remote host [$SSH_HOST]
   --ssh-port value                                                                           Remote host port [$SSH_PORT]
   --ssh-identity-file value                                                                  Identity file to be used [$SSH_IDENTITY_FILE]
   --ssh-disable-strict-host-key-checking value                                               Disable SSH strict host key checking [$DISABLE_STRICT_HOST_KEY_CHECKING]
   --ssh-known-hosts-file value                                                               Location of known_hosts file. Defaults to ~/.ssh/known_hosts [$KNOWN_HOSTS_FILE]
   --docker-host value                                                                        Docker daemon address [$DOCKER_HOST]
   --docker-cert-path value                                                                   Certificate path [$DOCKER_CERT_PATH]
   --docker-tlsverify                                                                         Use TLS and verify the remote [$DOCKER_TLS_VERIFY]
   --docker-hostname value                                                                    Custom container hostname [$DOCKER_HOSTNAME]
   --docker-image value                                                                       Docker image to be used [$DOCKER_IMAGE]
   --docker-runtime value                                                                     Docker runtime to be used [$DOCKER_RUNTIME]
   --docker-memory value                                                                      Memory limit (format: <number>[<unit>]). Unit can be one of b, k, m, or g. Minimum is 4M. [$DOCKER_MEMORY]
   --docker-memory-swap value                                                                 Total memory limit (memory + swap, format: <number>[<unit>]). Unit can be one of b, k, m, or g. [$DOCKER_MEMORY_SWAP]
   --docker-memory-reservation value                                                          Memory soft limit (format: <number>[<unit>]). Unit can be one of b, k, m, or g. [$DOCKER_MEMORY_RESERVATION]
   --docker-cgroup-parent value                                                               String value containing the cgroup parent to use [$DOCKER_CGROUP_PARENT]
   --docker-cpuset-cpus value                                                                 String value containing the cgroups CpusetCpus to use [$DOCKER_CPUSET_CPUS]
   --docker-cpuset-mems value                                                                 String value containing the cgroups CpusetMems to use [$DOCKER_CPUSET_MEMS]
   --docker-cpus value                                                                        Number of CPUs [$DOCKER_CPUS]
   --docker-cpu-shares value                                                                  Number of CPU shares (default: "0") [$DOCKER_CPU_SHARES]
   --docker-dns value                                                                         A list of DNS servers for the container to use [$DOCKER_DNS]
   --docker-dns-search value                                                                  A list of DNS search domains [$DOCKER_DNS_SEARCH]
   --docker-privileged                                                                        Give extended privileges to container [$DOCKER_PRIVILEGED]
   --docker-services_privileged value                                                         When set this will give or remove extended privileges to container services [$DOCKER_SERVICES_PRIVILEGED]
   --docker-disable-entrypoint-overwrite                                                      Disable the possibility for a container to overwrite the default image entrypoint [$DOCKER_DISABLE_ENTRYPOINT_OVERWRITE]
   --docker-user value                                                                        Run all commands in the container as the specified user. [$DOCKER_USER]
   --docker-allowed_users value                                                               List of allowed users under which to run commands in the build container. [$DOCKER_ALLOWED_USERS]
   --docker-group-add value                                                                   Add additional groups to join [$DOCKER_GROUP_ADD]
   --docker-userns value                                                                      User namespace to use [$DOCKER_USERNS_MODE]
   --docker-cap-add value                                                                     Add Linux capabilities [$DOCKER_CAP_ADD]
   --docker-cap-drop value                                                                    Drop Linux capabilities [$DOCKER_CAP_DROP]
   --docker-oom-kill-disable                                                                  Do not kill processes in a container if an out-of-memory (OOM) error occurs [$DOCKER_OOM_KILL_DISABLE]
   --docker-oom-score-adjust value                                                            Adjust OOM score (default: "0") [$DOCKER_OOM_SCORE_ADJUST]
   --docker-security-opt value                                                                Security Options [$DOCKER_SECURITY_OPT]
   --docker-services-security-opt value                                                       Security Options for container services [$DOCKER_SERVICES_SECURITY_OPT]
   --docker-devices value                                                                     Add a host device to the container [$DOCKER_DEVICES]
   --docker-device-cgroup-rules value                                                         Add a device cgroup rule to the container [$DOCKER_DEVICE_CGROUP_RULES]
   --docker-gpus value                                                                        Request GPUs to be used by Docker [$DOCKER_GPUS]
   --docker-disable-cache                                                                     Disable all container caching [$DOCKER_DISABLE_CACHE]
   --docker-volumes value                                                                     Bind-mount a volume and create it if it doesn't exist prior to mounting. Can be specified multiple times once per mountpoint, e.g. --docker-volumes 'test0:/test0' --docker-volumes 'test1:/test1' [$DOCKER_VOLUMES]
   --docker-volume-driver value                                                               Volume driver to be used [$DOCKER_VOLUME_DRIVER]
   --docker-volume-driver-ops value                                                           A toml table/json object with the format key=values. Volume driver ops to be specified (default: "{}") [$DOCKER_VOLUME_DRIVER_OPS]
   --docker-cache-dir value                                                                   Directory where to store caches [$DOCKER_CACHE_DIR]
   --docker-extra-hosts value                                                                 Add a custom host-to-IP mapping [$DOCKER_EXTRA_HOSTS]
   --docker-volumes-from value                                                                A list of volumes to inherit from another container [$DOCKER_VOLUMES_FROM]
   --docker-network-mode value                                                                Add container to a custom network [$DOCKER_NETWORK_MODE]
   --docker-ipcmode value                                                                     Select IPC mode for container [$DOCKER_IPC_MODE]
   --docker-mac-address value                                                                 Container MAC address (e.g., 92:d0:c6:0a:29:33) [$DOCKER_MAC_ADDRESS]
   --docker-links value                                                                       Add link to another container [$DOCKER_LINKS]
   --docker-services-limit value                                                              The maximum amount of services allowed [$DOCKER_SERVICES_LIMIT]
   --docker-service-memory value                                                              Service memory limit (format: <number>[<unit>]). Unit can be one of b (if omitted), k, m, or g. Minimum is 4M. [$DOCKER_SERVICE_MEMORY]
   --docker-service-memory-swap value                                                         Service total memory limit (memory + swap, format: <number>[<unit>]). Unit can be one of b (if omitted), k, m, or g. [$DOCKER_SERVICE_MEMORY_SWAP]
   --docker-service-memory-reservation value                                                  Service memory soft limit (format: <number>[<unit>]). Unit can be one of b (if omitted), k, m, or g. [$DOCKER_SERVICE_MEMORY_RESERVATION]
   --docker-service-cgroup-parent value                                                       String value containing the cgroup parent to use for service [$DOCKER_SERVICE_CGROUP_PARENT]
   --docker-service-cpuset-cpus value                                                         String value containing the cgroups CpusetCpus to use for service [$DOCKER_SERVICE_CPUSET_CPUS]
   --docker-service-cpus value                                                                Number of CPUs for service [$DOCKER_SERVICE_CPUS]
   --docker-service-cpu-shares value                                                          Number of CPU shares for service (default: "0") [$DOCKER_SERVICE_CPU_SHARES]
   --docker-wait-for-services-timeout value                                                   How long to wait for service startup (default: "0") [$DOCKER_WAIT_FOR_SERVICES_TIMEOUT]
   --docker-allowed-images value                                                              Image allowlist [$DOCKER_ALLOWED_IMAGES]
   --docker-allowed-privileged-images value                                                   Privileged image allowlist [$DOCKER_ALLOWED_PRIVILEGED_IMAGES]
   --docker-allowed-privileged-services value                                                 Privileged Service allowlist [$DOCKER_ALLOWED_PRIVILEGED_SERVICES]
   --docker-allowed-pull-policies value                                                       Pull policy allowlist [$DOCKER_ALLOWED_PULL_POLICIES]
   --docker-allowed-services value                                                            Service allowlist [$DOCKER_ALLOWED_SERVICES]
   --docker-pull-policy value                                                                 Image pull policy: never, if-not-present, always [$DOCKER_PULL_POLICY]
   --docker-isolation value                                                                   Container isolation technology. Windows only [$DOCKER_ISOLATION]
   --docker-shm-size value                                                                    Shared memory size for docker images (in bytes) (default: "0") [$DOCKER_SHM_SIZE]
   --docker-tmpfs value                                                                       A toml table/json object with the format key=values. When set this will mount the specified path in the key as a tmpfs volume in the main container, using the options specified as key. For the supported options, see the documentation for the unix 'mount' command (default: "{}") [$DOCKER_TMPFS]
   --docker-services-tmpfs value                                                              A toml table/json object with the format key=values. When set this will mount the specified path in the key as a tmpfs volume in all the service containers, using the options specified as key. For the supported options, see the documentation for the unix 'mount' command (default: "{}") [$DOCKER_SERVICES_TMPFS]
   --docker-sysctls value                                                                     Sysctl options, a toml table/json object of key=value. Value is expected to be a string. (default: "{}") [$DOCKER_SYSCTLS]
   --docker-helper-image value                                                                [ADVANCED] Override the default helper image used to clone repos and upload artifacts [$DOCKER_HELPER_IMAGE]
   --docker-helper-image-flavor value                                                         Set helper image flavor (alpine, ubuntu), defaults to alpine [$DOCKER_HELPER_IMAGE_FLAVOR]
   --docker-container-labels value                                                            A toml table/json object of key-value. Value is expected to be a string. When set, this will create containers with the given container labels. Environment variables will be substituted for values here. (default: "{}")
   --docker-enable-ipv6                                                                       Enable IPv6 for automatically created networks. This is only takes affect when the feature flag FF_NETWORK_PER_BUILD is enabled.
   --docker-ulimit value                                                                      Ulimit options for container (default: "{}") [$DOCKER_ULIMIT]
   --docker-network-mtu value                                                                 MTU of the Docker network created for the job IFF the FF_NETWORK_PER_BUILD feature-flag was specified. (default: "0")
   --parallels-base-name value                                                                VM name to be used [$PARALLELS_BASE_NAME]
   --parallels-template-name value                                                            VM template to be created [$PARALLELS_TEMPLATE_NAME]
   --parallels-disable-snapshots                                                              Disable snapshoting to speedup VM creation [$PARALLELS_DISABLE_SNAPSHOTS]
   --parallels-time-server value                                                              Timeserver to sync the guests time from. Defaults to time.apple.com [$PARALLELS_TIME_SERVER]
   --parallels-allowed-images value                                                           Image (base_name) allowlist [$PARALLELS_ALLOWED_IMAGES]
   --virtualbox-base-name value                                                               VM name to be used [$VIRTUALBOX_BASE_NAME]
   --virtualbox-base-snapshot value                                                           Name or UUID of a specific VM snapshot to clone [$VIRTUALBOX_BASE_SNAPSHOT]
   --virtualbox-base-folder value                                                             Folder in which to save the new VM. If empty, uses VirtualBox default [$VIRTUALBOX_BASE_FOLDER]
   --virtualbox-disable-snapshots                                                             Disable snapshoting to speedup VM creation [$VIRTUALBOX_DISABLE_SNAPSHOTS]
   --virtualbox-allowed-images value                                                          Image allowlist [$VIRTUALBOX_ALLOWED_IMAGES]
   --virtualbox-start-type value                                                              Graphical front-end type [$VIRTUALBOX_START_TYPE]
   --machine-max-growth-rate value                                                            Maximum machines being provisioned concurrently, set to 0 for unlimited (default: "0") [$MACHINE_MAX_GROWTH_RATE]
   --machine-idle-nodes value                                                                 Maximum idle machines (default: "0") [$MACHINE_IDLE_COUNT]
   --machine-idle-scale-factor value                                                          (Experimental) Defines what factor of in-use machines should be used as current idle value, but never more then defined IdleCount. 0.0 means use IdleCount as a static number (defaults to 0.0). Must be defined as float number. (default: "0") [$MACHINE_IDLE_SCALE_FACTOR]
   --machine-idle-count-min value                                                             Minimal number of idle machines when IdleScaleFactor is in use. Defaults to 1. (default: "0") [$MACHINE_IDLE_COUNT_MIN]
   --machine-idle-time value                                                                  Minimum time after node can be destroyed (default: "0") [$MACHINE_IDLE_TIME]
   --machine-max-builds value                                                                 Maximum number of builds processed by machine (default: "0") [$MACHINE_MAX_BUILDS]
   --machine-machine-driver value                                                             The driver to use when creating machine [$MACHINE_DRIVER]
   --machine-machine-name value                                                               The template for machine name (needs to include %s) [$MACHINE_NAME]
   --machine-machine-options value                                                            Additional machine creation options [$MACHINE_OPTIONS]
   --kubernetes-host value                                                                    Optional Kubernetes master host URL (auto-discovery attempted if not specified) [$KUBERNETES_HOST]
   --kubernetes-cert-file value                                                               Optional Kubernetes master auth certificate [$KUBERNETES_CERT_FILE]
   --kubernetes-key-file value                                                                Optional Kubernetes master auth private key [$KUBERNETES_KEY_FILE]
   --kubernetes-ca-file value                                                                 Optional Kubernetes master auth ca certificate [$KUBERNETES_CA_FILE]
   --kubernetes-bearer_token_overwrite_allowed                                                Bool to authorize builds to specify their own bearer token for creation. [$KUBERNETES_BEARER_TOKEN_OVERWRITE_ALLOWED]
   --kubernetes-bearer_token value                                                            Optional Kubernetes service account token used to start build pods. [$KUBERNETES_BEARER_TOKEN]
   --kubernetes-image value                                                                   Default docker image to use for builds when none is specified [$KUBERNETES_IMAGE]
   --kubernetes-namespace value                                                               Namespace to run Kubernetes jobs in [$KUBERNETES_NAMESPACE]
   --kubernetes-namespace_overwrite_allowed value                                             Regex to validate 'KUBERNETES_NAMESPACE_OVERWRITE' value [$KUBERNETES_NAMESPACE_OVERWRITE_ALLOWED]
   --kubernetes-namespace_per_job                                                             Use separate namespace for each job. If set, 'KUBERNETES_NAMESPACE' and 'KUBERNETES_NAMESPACE_OVERWRITE_ALLOWED' are ignored. [$KUBERNETES_NAMESPACE_PER_JOB]
   --kubernetes-privileged value                                                              Run all containers with the privileged flag enabled [$KUBERNETES_PRIVILEGED]
   --kubernetes-runtime-class-name value                                                      A Runtime Class to use for all created pods, errors if the feature is unsupported by the cluster [$KUBERNETES_RUNTIME_CLASS_NAME]
   --kubernetes-allow-privilege-escalation value                                              Run all containers with the security context allowPrivilegeEscalation flag enabled. When empty, it does not define the allowPrivilegeEscalation flag in the container SecurityContext and allows Kubernetes to use the default privilege escalation behavior. [$KUBERNETES_ALLOW_PRIVILEGE_ESCALATION]
   --kubernetes-cpu-limit value                                                               The CPU allocation given to build containers [$KUBERNETES_CPU_LIMIT]
   --kubernetes-cpu-limit-overwrite-max-allowed value                                         If set, the max amount the cpu limit can be set to. Used with the KUBERNETES_CPU_LIMIT variable in the build. [$KUBERNETES_CPU_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-cpu-request value                                                             The CPU allocation requested for build containers [$KUBERNETES_CPU_REQUEST]
   --kubernetes-cpu-request-overwrite-max-allowed value                                       If set, the max amount the cpu request can be set to. Used with the KUBERNETES_CPU_REQUEST variable in the build. [$KUBERNETES_CPU_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-memory-limit value                                                            The amount of memory allocated to build containers [$KUBERNETES_MEMORY_LIMIT]
   --kubernetes-memory-limit-overwrite-max-allowed value                                      If set, the max amount the memory limit can be set to. Used with the KUBERNETES_MEMORY_LIMIT variable in the build. [$KUBERNETES_MEMORY_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-memory-request value                                                          The amount of memory requested from build containers [$KUBERNETES_MEMORY_REQUEST]
   --kubernetes-memory-request-overwrite-max-allowed value                                    If set, the max amount the memory request can be set to. Used with the KUBERNETES_MEMORY_REQUEST variable in the build. [$KUBERNETES_MEMORY_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-ephemeral-storage-limit value                                                 The amount of ephemeral storage allocated to build containers [$KUBERNETES_EPHEMERAL_STORAGE_LIMIT]
   --kubernetes-ephemeral-storage-limit-overwrite-max-allowed value                           If set, the max amount the ephemeral limit can be set to. Used with the KUBERNETES_EPHEMERAL_STORAGE_LIMIT variable in the build. [$KUBERNETES_EPHEMERAL_STORAGE_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-ephemeral-storage-request value                                               The amount of ephemeral storage requested from build containers [$KUBERNETES_EPHEMERAL_STORAGE_REQUEST]
   --kubernetes-ephemeral-storage-request-overwrite-max-allowed value                         If set, the max amount the ephemeral storage request can be set to. Used with the KUBERNETES_EPHEMERAL_STORAGE_REQUEST variable in the build. [$KUBERNETES_EPHEMERAL_STORAGE_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-cpu-limit value                                                       The CPU allocation given to build service containers [$KUBERNETES_SERVICE_CPU_LIMIT]
   --kubernetes-service-cpu-limit-overwrite-max-allowed value                                 If set, the max amount the service cpu limit can be set to. Used with the KUBERNETES_SERVICE_CPU_LIMIT variable in the build. [$KUBERNETES_SERVICE_CPU_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-cpu-request value                                                     The CPU allocation requested for build service containers [$KUBERNETES_SERVICE_CPU_REQUEST]
   --kubernetes-service-cpu-request-overwrite-max-allowed value                               If set, the max amount the service cpu request can be set to. Used with the KUBERNETES_SERVICE_CPU_REQUEST variable in the build. [$KUBERNETES_SERVICE_CPU_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-memory-limit value                                                    The amount of memory allocated to build service containers [$KUBERNETES_SERVICE_MEMORY_LIMIT]
   --kubernetes-service-memory-limit-overwrite-max-allowed value                              If set, the max amount the service memory limit can be set to. Used with the KUBERNETES_SERVICE_MEMORY_LIMIT variable in the build. [$KUBERNETES_SERVICE_MEMORY_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-memory-request value                                                  The amount of memory requested for build service containers [$KUBERNETES_SERVICE_MEMORY_REQUEST]
   --kubernetes-service-memory-request-overwrite-max-allowed value                            If set, the max amount the service memory request can be set to. Used with the KUBERNETES_SERVICE_MEMORY_REQUEST variable in the build. [$KUBERNETES_SERVICE_MEMORY_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-ephemeral_storage-limit value                                         The amount of ephemeral storage allocated to build service containers [$KUBERNETES_SERVICE_EPHEMERAL_STORAGE_LIMIT]
   --kubernetes-service-ephemeral_storage-limit-overwrite-max-allowed value                   If set, the max amount the service ephemeral storage limit can be set to. Used with the KUBERNETES_SERVICE_EPHEMERAL_STORAGE_LIMIT variable in the build. [$KUBERNETES_SERVICE_EPHEMERAL_STORAGE_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-service-ephemeral_storage-request value                                       The amount of ephemeral storage requested for build service containers [$KUBERNETES_SERVICE_EPHEMERAL_STORAGE_REQUEST]
   --kubernetes-service-ephemeral_storage-request-overwrite-max-allowed value                 If set, the max amount the service ephemeral storage request can be set to. Used with the KUBERNETES_SERVICE_EPHEMERAL_STORAGE_REQUEST variable in the build. [$KUBERNETES_SERVICE_EPHEMERAL_STORAGE_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-cpu-limit value                                                        The CPU allocation given to build helper containers [$KUBERNETES_HELPER_CPU_LIMIT]
   --kubernetes-helper-cpu-limit-overwrite-max-allowed value                                  If set, the max amount the helper cpu limit can be set to. Used with the KUBERNETES_HELPER_CPU_LIMIT variable in the build. [$KUBERNETES_HELPER_CPU_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-cpu-request value                                                      The CPU allocation requested for build helper containers [$KUBERNETES_HELPER_CPU_REQUEST]
   --kubernetes-helper-cpu-request-overwrite-max-allowed value                                If set, the max amount the helper cpu request can be set to. Used with the KUBERNETES_HELPER_CPU_REQUEST variable in the build. [$KUBERNETES_HELPER_CPU_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-memory-limit value                                                     The amount of memory allocated to build helper containers [$KUBERNETES_HELPER_MEMORY_LIMIT]
   --kubernetes-helper-memory-limit-overwrite-max-allowed value                               If set, the max amount the helper memory limit can be set to. Used with the KUBERNETES_HELPER_MEMORY_LIMIT variable in the build. [$KUBERNETES_HELPER_MEMORY_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-memory-request value                                                   The amount of memory requested for build helper containers [$KUBERNETES_HELPER_MEMORY_REQUEST]
   --kubernetes-helper-memory-request-overwrite-max-allowed value                             If set, the max amount the helper memory request can be set to. Used with the KUBERNETES_HELPER_MEMORY_REQUEST variable in the build. [$KUBERNETES_HELPER_MEMORY_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-ephemeral_storage-limit value                                          The amount of ephemeral storage allocated to build helper containers [$KUBERNETES_HELPER_EPHEMERAL_STORAGE_LIMIT]
   --kubernetes-helper-ephemeral_storage-limit-overwrite-max-allowed value                    If set, the max amount the helper ephemeral storage limit can be set to. Used with the KUBERNETES_HELPER_EPHEMERAL_STORAGE_LIMIT variable in the build. [$KUBERNETES_HELPER_EPHEMERAL_STORAGE_LIMIT_OVERWRITE_MAX_ALLOWED]
   --kubernetes-helper-ephemeral_storage-request value                                        The amount of ephemeral storage requested for build helper containers [$KUBERNETES_HELPER_EPHEMERAL_STORAGE_REQUEST]
   --kubernetes-helper-ephemeral_storage-request-overwrite-max-allowed value                  If set, the max amount the helper ephemeral storage request can be set to. Used with the KUBERNETES_HELPER_EPHEMERAL_STORAGE_REQUEST variable in the build. [$KUBERNETES_HELPER_EPHEMERAL_STORAGE_REQUEST_OVERWRITE_MAX_ALLOWED]
   --kubernetes-allowed-images value                                                          Image allowlist [$KUBERNETES_ALLOWED_IMAGES]
   --kubernetes-allowed-pull-policies value                                                   Pull policy allowlist [$KUBERNETES_ALLOWED_PULL_POLICIES]
   --kubernetes-allowed-services value                                                        Service allowlist [$KUBERNETES_ALLOWED_SERVICES]
   --kubernetes-pull-policy value                                                             Policy for if/when to pull a container image (never, if-not-present, always). The cluster default will be used if not set [$KUBERNETES_PULL_POLICY]
   --kubernetes-node-selector value                                                           A toml table/json object of key:value. Value is expected to be a string. When set this will create pods on k8s nodes that match all the key:value pairs. Only one selector is supported through environment variable configuration. (default: "{}") [$KUBERNETES_NODE_SELECTOR]
   --kubernetes-node_selector_overwrite_allowed value                                         Regex to validate 'KUBERNETES_NODE_SELECTOR_*' values [$KUBERNETES_NODE_SELECTOR_OVERWRITE_ALLOWED]
   --kubernetes-node-tolerations value                                                        A toml table/json object of key=value:effect. Value and effect are expected to be strings. When set, pods will tolerate the given taints. Only one toleration is supported through environment variable configuration. (default: "{}") [$KUBERNETES_NODE_TOLERATIONS]
   --kubernetes-node_tolerations_overwrite_allowed value                                      Regex to validate 'KUBERNETES_NODE_TOLERATIONS_*' values [$KUBERNETES_NODE_TOLERATIONS_OVERWRITE_ALLOWED]
   --kubernetes-image-pull-secrets value                                                      A list of image pull secrets that are used for pulling docker image [$KUBERNETES_IMAGE_PULL_SECRETS]
   --kubernetes-use-service-account-image-pull-secrets                                        Do not provide any image pull secrets to the Pod created, so the secrets from the ServiceAccount can be used [$KUBERNETES_USE_SERVICE_ACCOUNT_IMAGE_PULL_SECRETS]
   --kubernetes-helper-image value                                                            [ADVANCED] Override the default helper image used to clone repos and upload artifacts [$KUBERNETES_HELPER_IMAGE]
   --kubernetes-helper-image-flavor value                                                     Set helper image flavor (alpine, ubuntu), defaults to alpine [$KUBERNETES_HELPER_IMAGE_FLAVOR]
   --kubernetes-helper-image-autoset-arch-and-os                                              When set, it uses the underlying OS to set the Helper Image ARCH and OS [$KUBERNETES_HELPER_IMAGE_AUTOSET_ARCH_AND_OS]
   --kubernetes-pod_termination_grace_period_seconds value                                    Pod-level setting which determines the duration in seconds which the pod has to terminate gracefully. After this, the processes are forcibly halted with a kill signal. Ignored if KUBERNETES_TERMINATIONGRACEPERIODSECONDS is specified. [$KUBERNETES_POD_TERMINATION_GRACE_PERIOD_SECONDS]
   --kubernetes-cleanup_grace_period_seconds value                                            When cleaning up a pod on completion of a job, the duration in seconds which the pod has to terminate gracefully. After this, the processes are forcibly halted with a kill signal. Ignored if KUBERNETES_TERMINATIONGRACEPERIODSECONDS is specified. [$KUBERNETES_CLEANUP_GRACE_PERIOD_SECONDS]
   --kubernetes-cleanup_resources_timeout value                                               The total amount of time for Kubernetes resources to be cleaned up after the job completes. Supported syntax: '1h30m', '300s', '10m'. Default is 5 minutes ('5m'). [$KUBERNETES_CLEANUP_RESOURCES_TIMEOUT]
   --kubernetes-poll-interval value                                                           How frequently, in seconds, the runner will poll the Kubernetes pod it has just created to check its status (default: "0") [$KUBERNETES_POLL_INTERVAL]
   --kubernetes-poll-timeout value                                                            The total amount of time, in seconds, that needs to pass before the runner will timeout attempting to connect to the pod it has just created (useful for queueing more builds that the cluster can handle at a time) (default: "0") [$KUBERNETES_POLL_TIMEOUT]
   --kubernetes-resource-availability-check-max-attempts value                                The maximum number of attempts to check if a resource (service account and/or pull secret) set is available before giving up. There is 5 seconds interval between each attempt (default: "0") [$KUBERNETES_RESOURCE_AVAILABILITY_CHECK_MAX_ATTEMPTS]
   --kubernetes-retry-limit value                                                             The maximum number of attempts to communicate with Kubernetes API. The retry interval between each attempt is based on a backoff algorithm starting at 500 ms (default: "0") [$KUBERNETES_REQUEST_RETRY_LIMIT]
   --kubernetes-retry-backoff-max value                                                       The max backoff interval value in milliseconds that can be reached for retry attempts to communicate with Kubernetes API (default: "0") [$KUBERNETES_REQUEST_RETRY_BACKOFF_MAX]
   --kubernetes-retry-limits value                                                            How many times each request error is to be retried (default: "{}") [$KUBERNETES_RETRY_LIMITS]
   --kubernetes-pod-labels value                                                              A toml table/json object of key-value. Value is expected to be a string. When set, this will create pods with the given pod labels. Environment variables will be substituted for values here. (default: "{}")
   --kubernetes-pod_labels_overwrite_allowed value                                            Regex to validate 'KUBERNETES_POD_LABELS_*' values [$KUBERNETES_POD_LABELS_OVERWRITE_ALLOWED]
   --kubernetes-scheduler-name value                                                          Pods will be scheduled using this scheduler, if it exists [$KUBERNETES_SCHEDULER_NAME]
   --kubernetes-service-account value                                                         Executor pods will use this Service Account to talk to kubernetes API [$KUBERNETES_SERVICE_ACCOUNT]
   --kubernetes-service_account_overwrite_allowed value                                       Regex to validate 'KUBERNETES_SERVICE_ACCOUNT' value [$KUBERNETES_SERVICE_ACCOUNT_OVERWRITE_ALLOWED]
   --kubernetes-automount-service-account-token value                                         Boolean to control the automount of the service account token in the build pod. [$KUBERNETES_AUTOMOUNT_SERVICE_ACCOUNT_TOKEN]
   --kubernetes-pod-annotations value                                                         A toml table/json object of key-value. Value is expected to be a string. When set, this will create pods with the given annotations. Can be overwritten in build with KUBERNETES_POD_ANNOTATION_* variables (default: "{}")
   --kubernetes-pod_annotations_overwrite_allowed value                                       Regex to validate 'KUBERNETES_POD_ANNOTATIONS_*' values [$KUBERNETES_POD_ANNOTATIONS_OVERWRITE_ALLOWED]
   --kubernetes-pod-security-context-fs-group value                                           A special supplemental group that applies to all containers in a pod [$KUBERNETES_POD_SECURITY_CONTEXT_FS_GROUP]
   --kubernetes-pod-security-context-run-as-group value                                       The GID to run the entrypoint of the container process [$KUBERNETES_POD_SECURITY_CONTEXT_RUN_AS_GROUP]
   --kubernetes-pod-security-context-run-as-non-root value                                    Indicates that the container must run as a non-root user [$KUBERNETES_POD_SECURITY_CONTEXT_RUN_AS_NON_ROOT]
   --kubernetes-pod-security-context-run-as-user value                                        The UID to run the entrypoint of the container process [$KUBERNETES_POD_SECURITY_CONTEXT_RUN_AS_USER]
   --kubernetes-pod-security-context-supplemental-groups value                                A list of groups applied to the first process run in each container, in addition to the container's primary GID
   --kubernetes-pod-security-context-selinux-type value                                       The SELinux type label that applies to all containers in a pod
   --kubernetes-init_permissions_container_security_context-capabilities-add value            List of capabilities to add to the build container [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_ADD]
   --kubernetes-init_permissions_container_security_context-capabilities-drop value           List of capabilities to drop from the build container [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_DROP]
   --kubernetes-init_permissions_container_security_context-privileged value                  Run container in privileged mode [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_PRIVILEGED]
   --kubernetes-init_permissions_container_security_context-run-as-user value                 The UID to run the entrypoint of the container process [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_RUN_AS_USER]
   --kubernetes-init_permissions_container_security_context-run-as-group value                The GID to run the entrypoint of the container process [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_RUN_AS_GROUP]
   --kubernetes-init_permissions_container_security_context-run-as-non-root value             Indicates that the container must run as a non-root user [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_RUN_AS_NON_ROOT]
   --kubernetes-init_permissions_container_security_context-read-only-root-filesystem value   Whether this container has a read-only root filesystem. [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_READ_ONLY_ROOT_FILESYSTEM]
   --kubernetes-init_permissions_container_security_context-allow-privilege-escalation value  AllowPrivilegeEscalation controls whether a process can gain more privileges than its parent process [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_ALLOW_PRIVILEGE_ESCALATION]
   --kubernetes-init_permissions_container_security_context-selinux-type value                The SELinux type label that is associated with the container process
   --kubernetes-init_permissions_container_security_context-proc-mount value                  Denotes the type of proc mount to use for the container. Valid values: default | unmasked. Set to unmasked if this container will be used to build OCI images. [$KUBERNETES_INIT_PERMISSIONS_CONTAINER_SECURITY_CONTEXT_PROC_MOUNT]
   --kubernetes-build_container_security_context-capabilities-add value                       List of capabilities to add to the build container [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_ADD]
   --kubernetes-build_container_security_context-capabilities-drop value                      List of capabilities to drop from the build container [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_DROP]
   --kubernetes-build_container_security_context-privileged value                             Run container in privileged mode [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_PRIVILEGED]
   --kubernetes-build_container_security_context-run-as-user value                            The UID to run the entrypoint of the container process [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_RUN_AS_USER]
   --kubernetes-build_container_security_context-run-as-group value                           The GID to run the entrypoint of the container process [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_RUN_AS_GROUP]
   --kubernetes-build_container_security_context-run-as-non-root value                        Indicates that the container must run as a non-root user [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_RUN_AS_NON_ROOT]
   --kubernetes-build_container_security_context-read-only-root-filesystem value              Whether this container has a read-only root filesystem. [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_READ_ONLY_ROOT_FILESYSTEM]
   --kubernetes-build_container_security_context-allow-privilege-escalation value             AllowPrivilegeEscalation controls whether a process can gain more privileges than its parent process [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_ALLOW_PRIVILEGE_ESCALATION]
   --kubernetes-build_container_security_context-selinux-type value                           The SELinux type label that is associated with the container process
   --kubernetes-build_container_security_context-proc-mount value                             Denotes the type of proc mount to use for the container. Valid values: default | unmasked. Set to unmasked if this container will be used to build OCI images. [$KUBERNETES_BUILD_CONTAINER_SECURITY_CONTEXT_PROC_MOUNT]
   --kubernetes-helper_container_security_context-capabilities-add value                      List of capabilities to add to the build container [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_ADD]
   --kubernetes-helper_container_security_context-capabilities-drop value                     List of capabilities to drop from the build container [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_DROP]
   --kubernetes-helper_container_security_context-privileged value                            Run container in privileged mode [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_PRIVILEGED]
   --kubernetes-helper_container_security_context-run-as-user value                           The UID to run the entrypoint of the container process [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_RUN_AS_USER]
   --kubernetes-helper_container_security_context-run-as-group value                          The GID to run the entrypoint of the container process [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_RUN_AS_GROUP]
   --kubernetes-helper_container_security_context-run-as-non-root value                       Indicates that the container must run as a non-root user [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_RUN_AS_NON_ROOT]
   --kubernetes-helper_container_security_context-read-only-root-filesystem value             Whether this container has a read-only root filesystem. [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_READ_ONLY_ROOT_FILESYSTEM]
   --kubernetes-helper_container_security_context-allow-privilege-escalation value            AllowPrivilegeEscalation controls whether a process can gain more privileges than its parent process [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_ALLOW_PRIVILEGE_ESCALATION]
   --kubernetes-helper_container_security_context-selinux-type value                          The SELinux type label that is associated with the container process
   --kubernetes-helper_container_security_context-proc-mount value                            Denotes the type of proc mount to use for the container. Valid values: default | unmasked. Set to unmasked if this container will be used to build OCI images. [$KUBERNETES_HELPER_CONTAINER_SECURITY_CONTEXT_PROC_MOUNT]
   --kubernetes-service_container_security_context-capabilities-add value                     List of capabilities to add to the build container [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_ADD]
   --kubernetes-service_container_security_context-capabilities-drop value                    List of capabilities to drop from the build container [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_CAPABILITIES_DROP]
   --kubernetes-service_container_security_context-privileged value                           Run container in privileged mode [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_PRIVILEGED]
   --kubernetes-service_container_security_context-run-as-user value                          The UID to run the entrypoint of the container process [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_RUN_AS_USER]
   --kubernetes-service_container_security_context-run-as-group value                         The GID to run the entrypoint of the container process [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_RUN_AS_GROUP]
   --kubernetes-service_container_security_context-run-as-non-root value                      Indicates that the container must run as a non-root user [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_RUN_AS_NON_ROOT]
   --kubernetes-service_container_security_context-read-only-root-filesystem value            Whether this container has a read-only root filesystem. [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_READ_ONLY_ROOT_FILESYSTEM]
   --kubernetes-service_container_security_context-allow-privilege-escalation value           AllowPrivilegeEscalation controls whether a process can gain more privileges than its parent process [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_ALLOW_PRIVILEGE_ESCALATION]
   --kubernetes-service_container_security_context-selinux-type value                         The SELinux type label that is associated with the container process
   --kubernetes-service_container_security_context-proc-mount value                           Denotes the type of proc mount to use for the container. Valid values: default | unmasked. Set to unmasked if this container will be used to build OCI images. [$KUBERNETES_SERVICE_CONTAINER_SECURITY_CONTEXT_PROC_MOUNT]
   --kubernetes-host_aliases value                                                            Add a custom host-to-IP mapping
   --kubernetes-cap-add value                                                                 Add Linux capabilities [$KUBERNETES_CAP_ADD]
   --kubernetes-cap-drop value                                                                Drop Linux capabilities [$KUBERNETES_CAP_DROP]
   --kubernetes-dns-policy value                                                              How Kubernetes should try to resolve DNS from the created pods. If unset, Kubernetes will use the default 'ClusterFirst'. Valid values are: none, default, cluster-first, cluster-first-with-host-net [$KUBERNETES_DNS_POLICY]
   --kubernetes-priority_class_name value                                                     If set, the Kubernetes Priority Class to be set to the Pods [$KUBERNETES_PRIORITY_CLASS_NAME]
   --kubernetes-logs-base-dir value                                                           Base directory for the path where build logs are stored. This directory is prepended to the final generated path. For example, <logs_base_dir>/logs-<project_id>-<job_id>. [$KUBERNETES_LOGS_BASE_DIR]
   --kubernetes-scripts-base-dir value                                                        Base directory for the path where build scripts are stored. This directory is prepended to the final generated path. For example, <scripts_base_dir>/scripts-<project_id>-<job_id>. [$KUBERNETES_SCRIPTS_BASE_DIR]
   --custom-config-exec value                                                                 Executable that allows to inject configuration values to the executor [$CUSTOM_CONFIG_EXEC]
   --custom-config-args value                                                                 Arguments for the config executable
   --custom-config-exec-timeout value                                                         Timeout for the config executable (in seconds) [$CUSTOM_CONFIG_EXEC_TIMEOUT]
   --custom-prepare-exec value                                                                Executable that prepares executor [$CUSTOM_PREPARE_EXEC]
   --custom-prepare-args value                                                                Arguments for the prepare executable
   --custom-prepare-exec-timeout value                                                        Timeout for the prepare executable (in seconds) [$CUSTOM_PREPARE_EXEC_TIMEOUT]
   --custom-run-exec value                                                                    Executable that runs the job script in executor [$CUSTOM_RUN_EXEC]
   --custom-run-args value                                                                    Arguments for the run executable
   --custom-cleanup-exec value                                                                Executable that cleanups after executor run [$CUSTOM_CLEANUP_EXEC]
   --custom-cleanup-args value                                                                Arguments for the cleanup executable
   --custom-cleanup-exec-timeout value                                                        Timeout for the cleanup executable (in seconds) [$CUSTOM_CLEANUP_EXEC_TIMEOUT]
   --custom-graceful-kill-timeout value                                                       Graceful timeout for scripts execution after SIGTERM is sent to the process (in seconds). This limits the time given for scripts to perform the cleanup before exiting [$CUSTOM_GRACEFUL_KILL_TIMEOUT]
   --custom-force-kill-timeout value                                                          Force timeout for scripts execution (in seconds). Counted from the force kill call; if process will be not terminated, Runner will abandon process termination and log an error [$CUSTOM_FORCE_KILL_TIMEOUT]
```


示例：

```bash
gitlab-runner register
```

按提示输入 URL、Token、执行环境等信息。

![[img_v3_02ii_a966cbd5-4c38-4dda-9fd0-778e1f37488g 1.jpg]]

```bash
root@b641b0802bca:/# gitlab-runner register  --url http://192.168.1.168:1080  --token glrt-t3_HkpJCRrYCsGUEd4yeagQ
Runtime platform                                    arch=amd64 os=linux pid=68 revision=9882d9c7 version=17.2.1
Running in system-mode.
Enter the GitLab instance URL (for example, https://gitlab.com/):
[http://192.168.1.168:1080]:
Verifying runner... is valid                        runner=t3_HkpJCR
Enter a name for the runner. This is stored only in the local config.toml file:
[b641b0802bca]: ubuntu-build-runner
Enter an executor: docker-autoscaler, ssh, docker-windows, docker+machine, virtualbox, docker, kubernetes, instance, custom, shell, parallels:
docker
Enter the default Docker image (for example, ruby:2.7):
ubuntu_zy:18.04.06
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
```

```bash
root@b641b0802bca:/# gitlab-runner register \
>   --non-interactive \
>   --url "http://192.168.1.168:1080" \
>   --token "glrt-t3_HkpJCRrYCsGUEd4yeagQ" \
>   --description "ubuntu-build-runner" \
>   --executor "docker" \
>   --docker-image "ubuntu_zy:18.04.06" \
>   --docker-volumes "/var/run/docker.sock:/var/run/docker.sock" \
>   --docker-privileged
Runtime platform                                    arch=amd64 os=linux pid=320 revision=9882d9c7 version=17.2.1
Running in system-mode.
Verifying runner... is valid                        runner=t3_HkpJCR
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
```

这段命令用于在 Docker 容器中注册一个 GitLab Runner。GitLab Runner 是用于执行 GitLab CI/CD 作业的工具，它可以通过多种执行器（executor）来运行任务，Docker 是其中一个常用的执行器。以下是每个命令参数的详细解释：

#### 命令解释：

```bash

D:\docker>docker images
REPOSITORY                  TAG         IMAGE ID       CREATED         SIZE
gitlab/gitlab-ce            latest      645802b84d05   7 weeks ago     3.5GB
ubuntu_zy                   18.04.06    b2fd1e53766c   5 months ago    959MB
gitlab/gitlab-runner        latest      cdfa8cbb0731   5 months ago    772MB
ubuntu/squid                latest      672df9d889a7   6 months ago    205MB
nginx                       latest      fffffc90d343   6 months ago    188MB
postgres                    15-alpine   00b2bb697bff   7 months ago    243MB
redis                       6-alpine    d21be8aaaa15   7 months ago    30.1MB
semitechnologies/weaviate   1.19.0      8ec9f084ab23   20 months ago   52.5MB

D:\docker>docker ps
CONTAINER ID   IMAGE                         COMMAND                   CREATED         STATUS                PORTS                                                           NAMES
b641b0802bca   gitlab/gitlab-runner:latest   "/usr/bin/dumb-init …"   7 minutes ago   Up 7 minutes                                                                          upbeat_montalcini 
b47f0837b1e0   gitlab/gitlab-ce:latest       "/assets/wrapper"         7 weeks ago     Up 6 days (healthy)   80/tcp, 443/tcp, 0.0.0.0:1080->1080/tcp, 0.0.0.0:1022->22/tcp   gitlab

D:\docker>docker exec -it b641b0802bca bash

root@b641b0802bca:/# ll
total 60
drwxr-xr-x   1 root root 4096 Jan 15 01:54 ./
drwxr-xr-x   1 root root 4096 Jan 15 01:54 ../
-rwxr-xr-x   1 root root    0 Jan 15 01:54 .dockerenv*
lrwxrwxrwx   1 root root    7 May 30  2024 bin -> usr/bin/
drwxr-xr-x   2 root root 4096 Apr 15  2020 boot/
drwxr-xr-x   5 root root  340 Jan 15 01:54 dev/
-rwxr-xr-x   1 root root  687 Jul 25 19:07 entrypoint*
drwxr-xr-x   1 root root 4096 Jan 15 01:54 etc/
drwxr-xr-x   1 root root 4096 Jul 25 19:14 home/
lrwxrwxrwx   1 root root    7 May 30  2024 lib -> usr/lib/
lrwxrwxrwx   1 root root    9 May 30  2024 lib32 -> usr/lib32/
lrwxrwxrwx   1 root root    9 May 30  2024 lib64 -> usr/lib64/
lrwxrwxrwx   1 root root   10 May 30  2024 libx32 -> usr/libx32/
drwxr-xr-x   2 root root 4096 May 30  2024 media/
drwxr-xr-x   2 root root 4096 May 30  2024 mnt/
drwxr-xr-x   2 root root 4096 May 30  2024 opt/
dr-xr-xr-x 846 root root    0 Jan 15 01:54 proc/
drwx------   2 root root 4096 May 30  2024 root/
drwxr-xr-x   1 root root 4096 Jul 25 19:14 run/
lrwxrwxrwx   1 root root    8 May 30  2024 sbin -> usr/sbin/
drwxr-xr-x   2 root root 4096 May 30  2024 srv/
dr-xr-xr-x  11 root root    0 Jan 15 01:54 sys/
drwxrwxrwt   2 root root 4096 May 30  2024 tmp/
drwxr-xr-x   1 root root 4096 May 30  2024 usr/
drwxr-xr-x   1 root root 4096 May 30  2024 var/

root@b641b0802bca:/# uname -a
Linux b641b0802bca 5.15.153.1-microsoft-standard-WSL2 #1 SMP Fri Mar 29 23:14:13 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

root@b641b0802bca:/# gitlab-runner register \
  --non-interactive \
  --url "http://192.168.1.168:1080" \
  --token "glrt-t3_HkpJCRrYCsGUEd4yeagQ" \
  --description "ubuntu-build-runner" \
  --executor "docker" \
  --docker-image "ubuntu_zy:18.04.06" \
  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock" \
  --docker-privileged
```

#### 参数详解：

1. **`--non-interactive`**
    
    - 该选项表示以非交互模式运行注册过程。没有用户交互提示，所有参数都在命令中直接指定。
2. **`--url "http://192.168.1.168:1080"`**
    
    - GitLab 实例的 URL。GitLab Runner 将会连接到该地址进行注册。在这里，`http://192.168.1.168:1080` 是 GitLab 实例的地址，通常是 GitLab 的 IP 地址和端口号。
3. **`--registration-token "YOUR-REGISTRATION-TOKEN"`**
    
    - 用于注册 Runner 的令牌。此令牌可以在 GitLab Web UI 中找到，通常位于 `项目 > 设置 > CI / CD > Runner` 或 `GitLab 管理员界面 > 管理 Runner`。你需要将 `YOUR-REGISTRATION-TOKEN` 替换为你的实际令牌。
4. **`--description "ubuntu-build-runner"`**
    
    - 该 Runner 的描述，可以给它起个名字，用于标识该 Runner。例如，`ubuntu-build-runner` 是一个描述名字，表示这个 Runner 可能用于 Ubuntu 构建任务。
[CI/CD YAML syntax reference | GitLab](https://docs.gitlab.com/17.6/ee/ci/yaml/index.html#tags)
```bash
# .gitlab-ci.yml
variables:
  GIT_STRATEGY: clone

stages:
  - build

compile:
  stage: build
  tags:
    - ubuntu-build-runner  # 确保与 GitLab Runner 标签匹配
  image: ubuntu_zy:18.04.06

  # 主要的编译脚本
  script:
    # 添加实际的编译命令
    - echo "Starting build process..."
  
  # 定义构建产物
  artifacts:
    paths:
      - build/
    expire_in: 1 week
```

![[Pasted image 20250115113502.png]]


5. **`--executor "docker"`**
    
    - 这是指定执行器的参数，表示 Runner 将使用 Docker 执行任务。你也可以选择 `shell`、`docker+machine`、`kubernetes` 等其他执行器，但这里选择了 Docker 执行器。
6. **`--docker-image "ubuntu_zy:18.04.06"`**
    
    - 该参数指定了 GitLab Runner 使用的 Docker 镜像。在这里，使用了 `ubuntu_zy:18.04.06` 镜像，表示任务将在该镜像中运行。如果该镜像不存在，GitLab Runner 会尝试从 Docker Hub 或本地镜像仓库拉取。

[CI/CD YAML syntax reference | GitLab](https://docs.gitlab.com/17.6/ee/ci/yaml/index.html#image)


![[Pasted image 20250115113559.png]]

7. **`--docker-volumes "/var/run/docker.sock:/var/run/docker.sock"`**
    
    - 这个参数指定了 Docker 挂载卷。`/var/run/docker.sock:/var/run/docker.sock` 是将宿主机的 Docker 套接字映射到容器中，这样容器内的 GitLab Runner 可以与宿主机的 Docker 守护进程通信，从而在 Docker 容器内启动其他容器。
8. **`--docker-privileged`**
    
    - 启用 Docker 容器的特权模式。`--docker-privileged` 参数允许容器内的进程具有与宿主机进程相同的权限。这对于需要访问 Docker 守护进程或执行其他高权限操作的任务非常有用。
9. **`--tag-list "ubuntu-build"`**
    
    - 这个参数允许你为 Runner 添加标签，以便在 CI/CD 管道中选择特定的 Runner 来执行任务。例如，标签 `ubuntu-build` 可以用于区分该 Runner 和其他 Runner，确保只有具有相同标签的作业才会由这个 Runner 执行。

#### 总结：

- **运行环境**：GitLab Runner 将运行在 Docker 容器内，并使用 Docker 执行器来启动其他容器执行 CI/CD 作业。
- **配置**：你通过 `--docker-volumes` 和 `--docker-privileged` 配置 Docker 访问权限，确保 Runner 能在容器中使用 Docker 来执行任务。
- **镜像**：任务将在 `ubuntu_zy:18.04.06` 镜像中运行，这个镜像应该是事先构建好的 Ubuntu 18.04 镜像。
- **标签**：你为这个 Runner 设置了一个 `ubuntu-build` 标签，便于在 GitLab CI/CD 配置中选择该 Runner 来执行指定的作业。

如果你有更多具体问题或需要进一步的帮助，可以随时告诉我！

### 7. **注销指定的 Runner**

```bash
gitlab-runner unregister --name "my-runner"
```

注销一个已注册的 Runner。使用 `--name` 选项指定 Runner 的名称。

```bash
root@b641b0802bca:/# gitlab-runner unregister --name "ubuntu-build-runner"
Runtime platform                                    arch=amd64 os=linux pid=211 revision=9882d9c7 version=17.2.1
Running in system-mode.
Unregistering runner from GitLab succeeded          runner=t3_HkpJCR
Updated /etc/gitlab-runner/config.toml
```

### 8. **验证所有已注册的 Runners**

```bash
gitlab-runner verify
```

验证所有已注册的 Runner 是否配置正确。

```bash
root@b641b0802bca:/# gitlab-runner verify
Runtime platform                                    arch=amd64 os=linux pid=109 revision=9882d9c7 version=17.2.1
Running in system-mode.
Verifying runner... is valid                        runner=t3_HkpJCR
```

### 9. **检查 Runner 服务的状态**

```bash
gitlab-runner status
```

获取当前 GitLab Runner 服务的状态（是否正在运行等）。

```bash
root@b641b0802bca:/# gitlab-runner status
Runtime platform                                    arch=amd64 os=linux pid=122 revision=9882d9c7 version=17.2.1
gitlab-runner: the service is not installed
```

### 10. **查看 Runner 服务的日志**

```bash
gitlab-runner read-logs
```

读取特定 Runner 的日志，通常用于故障排除。

```bash
root@b641b0802bca:/# gitlab-runner read-logs
Runtime platform                                    arch=amd64 os=linux pid=141 revision=9882d9c7 version=17.2.1
error reading logs timeout waiting for file to be created
```

### 11. **启动单个 Runner**

```bash
gitlab-runner run-single
```

启动一个单独的 Runner 来执行任务，而不是启动整个服务。

### 12. **执行健康检查**

```bash
gitlab-runner health-check --address http://localhost:8080
```

对指定地址进行健康检查，确保 Runner 可以正确连接到 GitLab 实例。

### 13. **调试模式**

```bash
gitlab-runner --debug run
```

在调试模式下运行 GitLab Runner，输出详细的日志信息，有助于排查问题。

```bash
root@b641b0802bca:/# gitlab-runner run
Runtime platform                                    arch=amd64 os=linux pid=349 revision=9882d9c7 version=17.2.1
Starting multi-runner from /etc/gitlab-runner/config.toml...  builds=0 max_builds=0
Running in system-mode.
Configuration loaded                                builds=0 max_builds=1
listen_address not defined, metrics & debug endpoints disabled  builds=0 max_builds=1
[session_server].listen_address not defined, session endpoints disabled  builds=0 max_builds=1
Initializing executor providers                     builds=0 max_builds=1
^CWARNING: [runWait] received stop signal             builds=0 max_builds=1 stop-signal=interrupt
WARNING: Graceful shutdown not finished properly. To gracefully clean up running plugins please use SIGQUIT (ctrl-\) instead of SIGINT (ctrl-c)  builds=0 error=received stop signal: interrupt max_builds=1
WARNING: Starting forceful shutdown                 StopSignal=interrupt builds=0 max_builds=1 shutdown-timeout=30s
All workers stopped.                                builds=0 max_builds=1
Shutting down executor providers                    builds=0 max_builds=1 shutdown-timeout=30s
All executor providers shut down.                   builds=0 max_builds=1
Can exit now!                                       builds=0 max_builds=1
```

### 14. **查看 Runner 的版本**

```bash
gitlab-runner --version
```

打印当前安装的 GitLab Runner 版本。

```bash
root@b641b0802bca:/# gitlab-runner --version
Version:      17.2.1
Git revision: 9882d9c7
Git branch:   17-2-stable
GO version:   go1.22.5
Built:        2024-07-25T17:34:53+0000
OS/Arch:      linux/amd64
```

### 15. **更改 Runner 的日志格式**

```bash
gitlab-runner --log-format json --log-level debug start
```

设置日志输出格式为 JSON，并在调试模式下启动 Runner 服务。

通过这些命令，你可以有效地管理和调试 GitLab Runner，确保 CI/CD 流水线的顺利运行。