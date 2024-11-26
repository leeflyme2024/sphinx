# 命令集

```Bash
export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"
repo init -u ssh://git@192.168.1.168:1022/root/1000-manifest.git -m default-local.xml
cp -r .repo/repo/repo /usr/bin/repo


export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"
repo init -u git@gitee.com:leev2v1/manifest.git -m default.xml
cp -r .repo/repo/repo /usr/bin/repo

repo sync
repo sync -j16
repo sync build.git
repo sync docker.git
repo sync manifest.git
repo sync release.git

# 同步除了0806-release.git的所有仓库的内容
repo sync $(repo list | grep -v "0806-release.git" | cut -d' ' -f1)

repo init -u git@gitee.com:leev2v1/manifest.git -m default.xml
repo init -u ssh://git@192.168.1.168:1022/root/0806-manifest.git -m default-local.xml
repo manifest

repo list
repo branches
repo start master --all
repo start master build.git
repo start master docker.git
repo start master doc.git
repo start master manifest.git
repo start master release.git
repo start master linux-sdk.git
repo start master uboot.git
repo start master app.git
repo start master filesystem.git
repo start master opensrc.git
repo start master mcu-sdk.git

repo status
repo diff
repo stage

repo forall -c 'git add .'
repo forall -c 'git commit -m "xxx"'
repo forall -c 'git tag am62xx-sdk-v1.0.0.5'
repo forall -c 'git tag M62xx-T@1.0.0-rc.7+20240801 $(git rev-parse HEAD) && git push gitee M62xx-T@1.0.0-rc.7+20240801'

repo forall -c 'git checkout am62xx-sdk-v1.0.0.6,en-plus'
repo forall -c 'git checkout master'

repo forall -c 'git remote add all ssh://git@gitee.com/leev2v1/manifest.git'
repo forall -c 'git remote set-url --add all ssh://git@192.168.1.168:1022/root/0806-manifest.git'

repo forall -c 'git remote remove all || true'
repo forall -c 'echo "Repository: $(pwd)"; git remote -v; echo'

repo forall -c "/home/leefly/am62x/tag.sh"

repo upload

repo list
repo info
```

```bash
#!/bin/bash
repo_path=$(pwd)
latest_commit=$(git rev-parse HEAD)

# /home/leefly/am62x/tag.sh file for get repo tag
for tag in $(git tag); do
    tag_commit=$(git rev-list -n 1 $tag)
    if [ "$tag_commit" == "$latest_commit" ]; then
        echo "success, $repo_path Tag $tag corresponds to the latest commit."
    else
        echo "error, $repo_path Tag $tag does not correspond to the latest commit."
    fi
done

echo -e "\n"
```

```bash
#!/bin/bash

# set -e
# set -x

################################################# 1. 同步代码 ##########################################################################

export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"

echo "Please select your target platform:"
echo "1) am62x"
echo "2) am62x-local"
read -p "Enter choice [1-2]: " choice

case $choice in
    1)
        repo init -u git@gitee.com:leev2v1/manifest.git -m default.xml
        ;;
    2)
        repo init -u ssh://git@192.168.1.168:1022/root/0806-manifest.git -m default-local.xml
        ;;
    *)
        echo "Invalid choice"
        exit 1
        ;;
esac

cp -r .repo/repo/repo /usr/bin/repo

repo sync
repo start master --all


################################################# 2. 添加远程仓库 ##########################################################################

# 将函数写入一个临时脚本文件
tmp_script=$(mktemp)
cat << 'EOF' > $tmp_script
#!/bin/bash
excluded_paths=("release")
gitee_base_url="ssh://git@gitee.com/leev2v1"
local_base_url="ssh://git@192.168.1.168:1022/root"

add_remotes() {
    local project_path=$(pwd)

    local remote_info=$(git config --get-regexp remote\..*\.url)

    while read -r line; do
        local remote_name=$(echo "$line" | awk -F".url " '{print $1}' | awk -F"remote." '{print $2}')
        local remote_url=$(echo "$line" | awk -F".url " '{print $2}')
        local project_name=$(basename "$remote_url" .git)
        echo "Added remote all to project ${project_name} in ${project_path} using remote ${remote_name}"

        # local host
        if [[ ! "$project_name" =~ ^0806- ]]; then
            local local_project_name="0806-${project_name}"
        else
            local local_project_name=${project_name}
        fi
        local local_url="${local_base_url}/${local_project_name}.git"
        git remote add all ${local_url}

        # remote gitee
        for excluded_path in "${excluded_paths[@]}"; do
            if [[ "$project_path" == *"/$excluded_path" ]]; then
                echo "Skipping project in ${project_path}"
                return
            fi
        done
        local gitee_url="${gitee_base_url}/${project_name}.git"
        git remote set-url --add all ${gitee_url}
    done <<< "$remote_info"
}

add_remotes
EOF

chmod +x $tmp_script
repo forall -c $tmp_script
rm -f $tmp_script
```

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo --help
Usage: repo [-p|--paginate|--no-pager] COMMAND [ARGS]

Options:
  -h, --help            show this help message and exit
  --help-all            show this help message with all subcommands and exit
  -p, --paginate        display command output in the pager
  --no-pager            disable the pager
  --color=COLOR         control color usage: auto, always, never
  --trace               trace git command execution (REPO_TRACE=1)
  --trace-to-stderr     trace outputs go to stderr in addition to
                        .repo/TRACE_FILE
  --trace-python        trace python command execution
  --time                time repo command execution
  --version             display this version of repo
  --show-toplevel       display the path of the top-level directory of the
                        repo client checkout
  --event-log=EVENT_LOG
                        filename of event log to append timeline to
  --git-trace2-event-log=GIT_TRACE2_EVENT_LOG
                        directory to write git trace2 event log to
  --submanifest-path=REL_PATH
                        submanifest path

The most commonly used repo commands are:
  abandon        Permanently abandon a development branch
  branch         View current topic branches
  branches       View current topic branches
  checkout       Checkout a branch for development
  cherry-pick    Cherry-pick a change.
  diff           Show changes between commit and working tree
  diffmanifests  Manifest diff utility
  download       Download and checkout a change
  grep           Print lines matching a pattern
  info           Get info on the manifest branch, current branch or unmerged branches
  init           Initialize a repo client checkout in the current directory
  list           List projects and their associated directories
  overview       Display overview of unmerged project branches
  prune          Prune (delete) already merged topics
  rebase         Rebase local branches on upstream branch
  smartsync      Update working tree to the latest known good revision
  stage          Stage file(s) for commit
  start          Start a new branch for development
  status         Show the working tree status
  sync           Update working tree to the latest revision
  upload         Upload changes for code review
See 'repo help <command>' for more information on a specific command.
See 'repo help --all' for a complete list of recognized commands.
Bug reports: https://issues.gerritcodereview.com/issues/new?component=1370071
```

[Repo 工具使用介绍 - Gitee.com](https://gitee.com/help/articles/4316)



## branch

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo help branch

摘要

查看当前主题分支

用法：repo branches [<project>...]

总结当前可用的主题分支。

# 分支显示

此命令输出的分支显示按四列信息组织；例如：

 *P nocolor                   | 在仓库中
    repo2                     |

第一列包含一个 *，如果分支是任何指定项目中当前检出的分支，或者如果没有项目检出该分支，则为空。

第二列包含空白、p 或 P，这取决于分支的上传状态。

 (空白)：分支尚未通过 repo upload 发布
       P：所有提交都已通过 repo upload 发布
       p：只有一些提交被 repo upload 发布

第三列包含分支名称。

第四列（在 | 分隔符之后）列出分支出现的项目，或者不出现的项目。如果没有显示项目列表，则该分支出现在所有项目中。

选项：
  -h, --help            显示此帮助信息并退出
  -j JOBS, --jobs=JOBS  并行运行的作业数（默认：8；基于CPU核心数）

  日志选项：
    -v, --verbose       显示所有输出
    -q, --quiet         仅显示错误

  多清单选项：
    --outer-manifest    从最外层清单开始操作
    --no-outer-manifest
                        不在外层清单上操作
    --this-manifest-only
                        仅在此（子）清单上操作
    --no-this-manifest-only, --all-manifests
                        在此清单及其子清单上操作

运行 `repo help branches` 查看详细手册。
```

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo help branch

Summary

View current topic branches

Usage: repo branches [<project>...]

Summarizes the currently available topic branches.

# Branch Display

The branch display output by this command is organized into four
columns of information; for example:

 *P nocolor                   | in repo
    repo2                     |

The first column contains a * if the branch is the currently
checked out branch in any of the specified projects, or a blank
if no project has the branch checked out.

The second column contains either blank, p or P, depending upon
the upload status of the branch.

 (blank): branch not yet published by repo upload
       P: all commits were published by repo upload
       p: only some commits were published by repo upload

The third column contains the branch name.

The fourth column (after the | separator) lists the projects that
the branch appears in, or does not appear in.  If no project list
is shown, then the branch appears in all projects.

Options:
  -h, --help            show this help message and exit
  -j JOBS, --jobs=JOBS  number of jobs to run in parallel (default: 8; based
                        on number of CPU cores)

  Logging options:
    -v, --verbose       show all output
    -q, --quiet         only show errors

  Multi-manifest options:
    --outer-manifest    operate starting at the outermost manifest
    --no-outer-manifest
                        do not operate on outer manifests
    --this-manifest-only
                        only operate on this (sub)manifest
    --no-this-manifest-only, --all-manifests
                        operate on this manifest and its submanifests

Run `repo help branches` to view the detailed manual.
```

## checkout

```Bash
repo checkout命令用于在开发过程中检出一个分支。

用法：repo checkout <分支名称> [<项目名称>...]

选项：
  -h, --help            显示帮助信息并退出
  -j JOBS, --jobs=JOBS  并行运行的作业数（默认值为8；基于CPU核心数）

  日志选项：
    -v, --verbose       显示所有输出
    -q, --quiet         只显示错误信息

  多清单选项：
    --outer-manifest    从最外层清单开始操作
    --no-outer-manifest
                        不在最外层清单上操作
    --this-manifest-only
                        只在当前（子）清单上操作
    --no-this-manifest-only, --all-manifests
                        在当前清单及其子清单上操作

运行 `repo help checkout` 查看详细手册。

描述

'repo checkout' 命令用于检出之前由 'repo start' 创建的现有分支。

该命令相当于：

  repo forall [<项目名称>...] -c git checkout <分支名称>
```

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo help checkout
Summary

Checkout a branch for development

Usage: repo checkout <branchname> [<project>...]

Options:
  -h, --help            show this help message and exit
  -j JOBS, --jobs=JOBS  number of jobs to run in parallel (default: 8; based
                        on number of CPU cores)

  Logging options:
    -v, --verbose       show all output
    -q, --quiet         only show errors

  Multi-manifest options:
    --outer-manifest    operate starting at the outermost manifest
    --no-outer-manifest
                        do not operate on outer manifests
    --this-manifest-only
                        only operate on this (sub)manifest
    --no-this-manifest-only, --all-manifests
                        operate on this manifest and its submanifests

Run `repo help checkout` to view the detailed manual.

Description

The 'repo checkout' command checks out an existing branch that was previously
created by 'repo start'.

The command is equivalent to:

  repo forall [<project>...] -c git checkout <branchname>
```

## start

```Bash
repo start命令用于开始一个新的开发分支。

用法：repo start <新分支名称> [--all | <项目名称>...]

选项：
  -h, --help            显示帮助信息并退出
  -j JOBS, --jobs=JOBS  并行运行的作业数（默认为8；基于CPU核心数）
  --all                 在所有项目中开始分支
  -r REVISION, --rev=REVISION, --revision=REVISION
                        将分支指向此版本，而不是上游版本
  --head, --HEAD        缩写形式，等同于 --rev HEAD

  日志选项：
    -v, --verbose       显示所有输出
    -q, --quiet         只显示错误信息

  多清单选项：
    --outer-manifest    从最外层清单开始操作
    --no-outer-manifest
                        不在最外层清单上操作
    --this-manifest-only
                        只在当前（子）清单上操作
    --no-this-manifest-only, --all-manifests
                        在当前清单及其子清单上操作

运行 `repo help start` 查看详细手册。

描述

'repo start' 开始一个新的开发分支，从清单中指定的版本开始。
```

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo help start
Summary

Start a new branch for development

Usage: repo start <newbranchname> [--all | <project>...]

Options:
  -h, --help            show this help message and exit
  -j JOBS, --jobs=JOBS  number of jobs to run in parallel (default: 8; based
                        on number of CPU cores)
  --all                 begin branch in all projects
  -r REVISION, --rev=REVISION, --revision=REVISION
                        point branch at this revision instead of upstream
  --head, --HEAD        abbreviation for --rev HEAD

  Logging options:
    -v, --verbose       show all output
    -q, --quiet         only show errors

  Multi-manifest options:
    --outer-manifest    operate starting at the outermost manifest
    --no-outer-manifest
                        do not operate on outer manifests
    --this-manifest-only
                        only operate on this (sub)manifest
    --no-this-manifest-only, --all-manifests
                        operate on this manifest and its submanifests

Run `repo help start` to view the detailed manual.

Description

'repo start' begins a new branch of development, starting from the revision
specified in the manifest.
```

## forall

```Bash
1.repo forall命令
 # repo forall -help
 # repo forall -c: 此命令遍历所有的git仓库，并在每个仓库执行-c所指定的命令，被执行的命令不限于git命令，而是任何被系统支持的命令，比如：ls, git log, git status等

2.repo forall -c使用
  # 切换分支
  # repo forall -c git checkout dev_test
  # 删除分支
  # repo forall -c git branch -D dev_test
  # 丢弃分支
  # repo forall -c git git reset —hard 提交ID(或最原始:HEAD)
  # repo forall -r framework/base/core -c git reset —hard 提交ID(或最原始HEAD)
```

```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk# repo help forall
Summary

Run a shell command in each project

Usage: repo forall [<project>...] -c <command> [<arg>...]
repo forall -r str1 [str2] ... -c <command> [<arg>...]

Options:
  -h, --help            show this help message and exit
  -j JOBS, --jobs=JOBS  number of jobs to run in parallel (default: 8; based
                        on number of CPU cores)
  -r, --regex           execute the command only on projects matching regex or
                        wildcard expression
  -i, --inverse-regex   execute the command only on projects not matching
                        regex or wildcard expression
  -g GROUPS, --groups=GROUPS
                        execute the command only on projects matching the
                        specified groups
  -c, --command         command (and arguments) to execute
  -e, --abort-on-errors
                        abort if a command exits unsuccessfully
  --ignore-missing      silently skip & do not exit non-zero due missing
                        checkouts
  --interactive         force interactive usage

  Logging options:
    -v, --verbose       show all output
    -q, --quiet         only show errors
    -p                  show project headers before output

  Multi-manifest options:
    --outer-manifest    operate starting at the outermost manifest
    --no-outer-manifest
                        do not operate on outer manifests
    --this-manifest-only
                        only operate on this (sub)manifest
    --no-this-manifest-only, --all-manifests
                        operate on this manifest and its submanifests

Run `repo help forall` to view the detailed manual.

Description

Executes the same shell command in each project.

The -r option allows running the command only on projects matching regex or
wildcard expression.

By default, projects are processed non-interactively in parallel. If you want to
run interactive commands, make sure to pass --interactive to force --jobs 1.
While the processing order of projects is not guaranteed, the order of project
output is stable.

Output Formatting

The -p option causes 'repo forall' to bind pipes to the command's stdin, stdout
and stderr streams, and pipe all output into a continuous stream that is
displayed in a single pager session. Project headings are inserted before the
output of each command is displayed. If the command produces no output in a
project, no heading is displayed.

The formatting convention used by -p is very suitable for some types of
searching, e.g. `repo forall -p -c git log -SFoo` will print all commits that
add or remove references to Foo.

The -v option causes 'repo forall' to display stderr messages if a command
produces output only on stderr. Normally the -p option causes command output to
be suppressed until the command produces at least one byte of output on stdout.

Environment

pwd is the project's working directory. If the current client is a mirror
client, then pwd is the Git repository.

REPO_PROJECT is set to the unique name of the project.

REPO_PATH is the path relative the the root of the client.

REPO_OUTERPATH is the path of the sub manifest's root relative to the root of
the client.

REPO_INNERPATH is the path relative to the root of the sub manifest.

REPO_REMOTE is the name of the remote system from the manifest.

REPO_LREV is the name of the revision from the manifest, translated to a local
tracking branch. If you need to pass the manifest revision to a locally executed
git command, use REPO_LREV.

REPO_RREV is the name of the revision from the manifest, exactly as written in
the manifest.

REPO_COUNT is the total number of projects being iterated.

REPO_I is the current (1-based) iteration count. Can be used in conjunction with
REPO_COUNT to add a simple progress indicator to your command.

REPO__* are any extra environment variables, specified by the "annotation"
element under any project element. This can be useful for differentiating trees
based on user-specific criteria, or simply annotating tree details.

shell positional arguments ($1, $2, .., $#) are set to any arguments following
<command>.

Example: to list projects:

  repo forall -c 'echo $REPO_PROJECT'

Notice that $REPO_PROJECT is quoted to ensure it is expanded in the context of
running <command> instead of in the calling shell.

Unless -p is used, stdin, stdout, stderr are inherited from the terminal and are
not redirected.

If -e is used, when a command exits unsuccessfully, 'repo forall' will abort
without iterating through the remaining projects.
```

## stage
```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/am62x-sdk/src/zy# repo help stage
Summary

Stage file(s) for commit

Usage: repo stage -i [<project>...]

Options:
  -h, --help            show this help message and exit

  Logging options:
    -v, --verbose       show all output
    -q, --quiet         only show errors
    -i, --interactive   use interactive staging

  Multi-manifest options:
    --outer-manifest    operate starting at the outermost manifest
    --no-outer-manifest
                        do not operate on outer manifests
    --this-manifest-only
                        only operate on this (sub)manifest
    --no-this-manifest-only, --all-manifests
                        operate on this manifest and its submanifests

Run `repo help stage` to view the detailed manual.

Description

The 'repo stage' command stages files to prepare the next commit.

```

# 部署repo
##  安装git、python
```Bash
apt install git
sudo apt-get install python3
https://www.python.org/downloads/windows/
alias python3=python
```

## 安装Repo
```Bash
mkdir /usr/bin
PATH=/usr/bin:$PATH
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo > /usr/bin/repo
chmod a+x /usr/bin/repo
```

## 初始化Repo
```Bash
# 账户 SSH 公钥
ssh-keygen -t ed25519 -C "Gitee SSH Key"
ll ~/.ssh
cat ~/.ssh/user_id_ed25519.pub
ssh -T git@gitee.com

# 仓库 SSH 公钥
ssh-keygen -t rsa -b 4096 -C "1144815495@qq.com" -f ~/.ssh/repo_id_rsa
ll ~/.ssh/
cat ~/.ssh/repo_id_rsa.pub
ssh -T git@gitee.com

# 部署公钥管理 - 可部署公钥
https://help.gitee.com/repository/ssh-key/generate-and-add-ssh-public-key
https://help.gitee.com/repository/ssh-key/configure-multiple-ssh-keys


mkdir am62x
cd am62x
export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"
repo init -u git@gitee.com:leev2v1/manifest.git
```

![](https://d685kxwna1.feishu.cn/space/api/box/stream/download/asynccode/?code=OWY0NTAzZDA3YjllYjliZDRkODI4YmRmY2E4YmZhNWVfajZVdG52T0xTdlB1SXQ0YklOZTM2UnFJTkU4VUpTeEhfVG9rZW46R1NqVWJSOGRtb3VtRDN4WGthNWNKQWhvbkRjXzE3MjE3OTI2MTY6MTcyMTc5NjIxNl9WNA)


```Bash
root@ThinkPad-E480:/home/zlglinux/am62x/repo# grep -nr "gerrit" /usr/bin/repo -C 5
114-
115-# repo default configuration
116-#
117-REPO_URL = os.environ.get("REPO_URL", None)
118-if not REPO_URL:
119:    REPO_URL = "https://gerrit.googlesource.com/git-repo"
120-REPO_REV = os.environ.get("REPO_REV")
121-if not REPO_REV:
122-    REPO_REV = "stable"
123-# URL to file bug reports for repo tool issues.
124:BUG_URL = "https://issues.gerritcodereview.com/issues/new?component=1370071"
125-
126-# increment this whenever we make important changes to this script
127-VERSION = (2, 42)
128-
129-# increment this if the MAINTAINER_KEYS block is modified

root@ThinkPad-E480:/home/zlglinux/am62x/repo# export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"
root@ThinkPad-E480:/home/zlglinux/am62x/repo# repo init -u https://gitee.com/leev2v1/manifest.git
Downloading Repo source from https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/
remote: Enumerating objects: 8779, done.
remote: Counting objects: 100% (5008/5008), done.
remote: Compressing objects: 100% (2511/2511), done.
remote: Total 8779 (delta 4793), reused 2497 (delta 2497), pack-reused 3771
接收对象中: 100% (8779/8779), 3.03 MiB | 5.38 MiB/s, 完成.
处理 delta 中: 100% (6046/6046), 完成.
repo: Updating release signing keys to keyset ver 2.3
Username for 'https://gitee.com': leev2v1
Password for 'https://leev2v1@gitee.com': 

Your identity is: lee <1144815495@qq.com>
If you want to change this, please re-run 'repo init' with --config-name

Testing colorized output (for 'repo diff', 'repo status'):
  black    red      green    yellow   blue     magenta   cyan     white 
  bold     dim      ul       reverse 
Enable color display in this user account (y/N)? 

repo has been initialized in /home/zlglinux/am62x/repo
```

命令 `repo init -u` `https://aosp.tuna.tsinghua.edu.cn/platform/manifest` 是用于初始化一个新的 Repo 客户端工作目录的命令，它主要做了以下几件事情：

1. **下载 Repo 启动器**：Repo 是一个工具，用于管理多个 Git 仓库的操作。`repo init` 命令首先会检查本地环境中是否已经存在 Repo 的启动器脚本，如果不存在，它会从指定的 URL 下载 Repo 启动器并存放在 `.repo/repo/` 目录下。
    
2. **初始化** **`.repo`**** 目录**：命令执行后，会在当前工作目录下创建一个名为 `.repo` 的隐藏目录。这个目录用于存放 Repo 的配置文件和管理的 Git 仓库的元数据。
    
3. **下载 Manifest**：Manifest 是一个或多个 XML 文件，描述了 Repo 需要管理的项目（Git 仓库）及其配置信息，如分支、远程URL等。`-u` 参数指定了 Manifest 文件的远程仓库位置，这个命令会从 `https://aosp.tuna.tsinghua.edu.cn/platform/manifest` 下载 Manifest 文件到 `.repo/manifests/` 目录下，同时，会根据下载的 Manifest 文件检出一个默认的或指定的分支到 `.repo/manifests.git/` 目录下。
    
4. **配置 Repo 项目**：根据下载的 Manifest 文件，`repo init` 命令会设置当前 Repo 工作目录的配置，包括配置远程仓库的URL、分支等信息。这些配置信息被存储在 `.repo/manifest.xml` 文件和 `.repo/repo/manifests.git/` 目录中。
    
5. **准备同步**：初始化完成后，工作目录准备好了从远程仓库同步代码。实际的代码同步需要运行 `repo sync` 命令来完成。
    
## 同步仓库
```Plaintext
repo sync
```

## 管理项目

- **创建分支**：使用`repo start`命令在所有相关的仓库中创建新的分支。
- **查看状态**：使用`repo status`命令查看当前项目的状态。
- **提交更改**：首先在各个Git仓库中使用`git commit`提交更改，然后使用`repo upload`命令将更改上传到远程仓库进行审查。
- **获取所有仓库的最新commit id**
```Bash
#!/bin/bash

# 获取脚本的绝对路径
script_dir=$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)

# 定义输出文件的完整路径
output_file="${script_dir}/commit_ids.txt"

# 清空输出文件，准备写入新数据
> "$output_file"

# 检查输出文件是否可写
if [ ! -w "$output_file" ]; then
    echo "Error: File $output_file is not writable."
    exit 1
fi

# 写入一个初始信息到文件
echo "List of repositories and their latest commit IDs:" >> "$output_file"

# 获取所有仓库的列表
repos=($(repo list | awk '{print $1}'))

# 遍历每个仓库
for repo in "${repos[@]}"
do
    echo "Fetching latest commit ID for repository: $repo"
    # 进入仓库目录
    cd "$repo" || { echo "Error: Cannot enter directory $repo"; continue; }
    # 获取最新commit id
    commit_id=$(git rev-parse HEAD 2>/dev/null)
    if [ $? -eq 0 ]; then  # 检查git命令是否成功执行
        # 将结果写入文件
        echo "Latest commit ID: $commit_id, Repository: $repo" >> "$output_file"
    else
        echo "Error: Failed to get commit ID for repository $repo" >> "$output_file"
    fi
    # 返回到原始目录
    cd - > /dev/null
done

# 脚本执行完成
echo "Commit IDs have been saved to $output_file"
echo

cat "$output_file"
```