```Bash
git log --oneline

git config --global user.name "liweiyu"
git config --global user.email "1144815495@qq.com"

git add -u
git commit -m "XXX"
git tag
git tag am62xx-sdk-v1.0.0.5
git tag -d am62xx-sdk-v1.0.0.5
git show am62xx-sdk-v1.0.0.5
git push origin am62xx-sdk-v1.0.0.5
git push origin am62xx-sdk-v1.0.0.5 --delete
git push orgin master

git tag --merged enplus
git tag --merged master


git for-each-ref --format='%(refname:short)|%(creatordate:short)|%(subject)|%(authorname)' --sort=-creatordate --merged master refs/tags | column -t -s '|'
git for-each-ref --format='%(refname:short)|%(creatordate:short)|%(subject)|%(authorname)' --sort=-creatordate --merged enplus refs/tags | column -t -s '|'

git branch --contains M62xx-T@1.0.6+20250109hsse,en-plus

git tag am62xx-sdk-v1.0.0.6,en-plus 8b96f4a96ae21b5d45518b0ea57854f77bcd2c38
git push gitee am62xx-sdk-v1.0.0.6,en-plus

git checkout am62xx-sdk-v1.0.0.6,en-plus
git checkout M62xx-T@1.0.0-rc.7+20240801
git checkout master

git remote add all ssh://git@192.168.1.168:1022/root/0806-manifest.git
git remote set-url --add all ssh://git@gitee.com/leev2v1/manifest.git
git remote -v
git push all master

# 恢复删除的 main.c 文件
git checkout -- main.c

# 暂存当前的修改 
git stash 
# 回退到指定的提交 
git reset --hard 197b42ea2afd3d2ed461512556e2a152578 
# 恢复暂存的修改 
git stash pop

git add . ":(exclude)bak-factory_test/"
```



# 基础命令

## 初始化仓库
### 初始化当前目录为 Git 仓库
```bash
git init
```

这条命令会在当前目录下创建一个 `.git` 目录，初始化一个新的 Git 仓库。

### 初始化指定目录为 Git 仓库
```bash
git init <directory>
```

例如，初始化名为 `my-project` 的目录为 Git 仓库：

```bash
git init my-project
```

这会在 `my-project` 目录下创建一个 `.git` 目录，将其初始化为一个新的 Git 仓库。

### 初始化空仓库
```bash
git init --bare
```

这条命令创建一个没有工作目录的空仓库，通常用于创建一个用于共享的裸仓库。

### 初始化仓库并指定初始分支
```bash
git init -b <branch-name>
```

例如，初始化仓库并使用 `main` 作为初始分支：

```bash
git init -b main
```


## 添加文件到暂存区
### 添加单个文件
```bash
git add <file-path>
```

例如，添加 `README.md` 文件到暂存区：

```bash
git add README.md
```

### 添加所有更改的文件
```bash
git add .
```

这会将工作目录中所有已跟踪文件的更改添加到暂存区。

### 添加所有新文件和更改
```bash
git add -A
```

这会将所有新文件和已跟踪文件的更改添加到暂存区。

### 添加所有新文件
```bash
git add --all
```

这会将所有未跟踪的新文件添加到暂存区，但不会考虑已跟踪文件的更改。

### 添加所有删除的文件
```bash
git add -u
```

这会将所有已删除的文件从暂存区中移除，但不会添加新的更改或文件。

### 恢复文件更改（取消暂存）
```bash
git reset <file-path>
```

例如，取消暂存 `README.md` 文件，使其更改不再准备提交：

```bash
git reset README.md
```

### 恢复文件更改并丢弃本地更改
```bash
git checkout -- <file-path>
```

例如，恢复 `README.md` 文件到上次提交的状态，并丢弃所有本地更改：

```bash
git checkout -- README.md
```


## 提交更改
### 提交所有暂存的更改
```bash
git commit -m "Your commit message"
```

例如，提交所有暂存的更改，并添加提交信息 "Fix typo in README":

```bash
git commit -m "Fix typo in README"
```

### 提交特定文件的更改
```bash
git commit --amend --no-edit <file-path>
```

这个例子有点特殊，用于将特定文件的更改添加到最后一个提交中，而不是创建一个新的提交。例如：

```bash
git commit --amend --no-edit README.md
```

### 在提交时编辑提交信息
```bash
git commit
```

不使用 `-m` 参数时，Git 会打开一个编辑器让你输入提交信息。

### 提交更改并标记为修复
```bash
git commit -m "Fix #123: Describe your fix"
```

这会在提交信息中包含一个修复的标记，其中 `#123` 可以是问题跟踪系统中的问题编号。

### 提交更改并关闭关联的问题
```bash
git commit -m "Close #456: Describe how this commit closes the issue"
```

这在提交信息中明确指出此提交解决了某个问题。
### 提交更改并引用另一个提交

```bash
git commit -m "Ref #789: Add more details to previous commit"
```

这在提交信息中引用了另一个提交，通常用于关联或扩展先前的工作。


## 查看状态

`git status` 命令用于显示工作目录和暂存区域的状态。

### 显示工作目录和暂存区的状态
```bash
git status
```

这将显示未暂存的更改、已暂存的更改以及未跟踪的新文件。

### 显示短格式状态
```bash
git status -s
```

这将显示一个简短的状态列表，其中包含了每个文件的更改状态缩写。

### 显示未暂存的更改
```bash
git status --untracked-files=no
```

这将隐藏未跟踪的新文件，只显示已跟踪文件的更改。

### 显示未跟踪的文件
```bash
git status --ignored
```

这将显示所有未跟踪的文件，包括 `.gitignore` 文件中忽略的文件。

### 显示已暂存的更改
```bash
git diff --cached
```

虽然这不是 `git status` 的直接变体，但它用于显示已暂存更改的详细信息，这通常与状态检查相关联。

## 查看日志
### 查看所有提交历史
```bash
git log
```
这将显示仓库中所有的提交历史，包括哈希值、作者、提交日期和提交信息。

### 限制显示的提交数量
```bash
git log -n 5
```
这将只显示最近的5次提交历史。

### 查看某个文件的提交历史
```bash
git log <filename>
```
这将显示特定文件的所有提交历史。

### 查看简化格式的提交历史
```bash
git log --oneline
```
这将显示每一行一个提交的简化历史，包括哈希值前几位和提交信息。

### 查看按日期排序的提交历史
```bash
git log --date-order
```
这将按照日期顺序显示提交历史。

### 查看按作者排序的提交历史
```bash
git log --author-date-order
```
这将按照作者的提交日期顺序显示提交历史。

### 查看某位作者的提交历史
```bash
git log --author=<author-name>
```
这将显示特定作者的所有提交历史。

### 查看包含特定字符串的提交历史
```bash
git log --grep="fix bugs"
```
这将显示包含“fix bugs”关键词的提交历史。

### 查看某段时间范围内的提交历史
```bash
git log --since="2023-01-01" --until="2023-12-31"
```
这将显示从2023年1月1日到2023年12月31日之间的提交历史。

### 查看两个分支之间的提交历史
```bash
git log <branch1>..<branch2>
```
这将显示从`<branch1>`分支到`<branch2>`分支之间的提交历史。

### 查看文件变更的统计信息
```bash
git log --stat
```
这将显示每次提交的文件变更统计信息，如添加和删除的行数。

### 查看图形化的提交历史
```bash
git log --graph
```
这将显示一个ASCII艺术的图形表示，显示分支的合并历史。


# 分支管理

## 列出所有分支

```bash
git branch
```

这将列出所有本地分支，当前所在的分支会标有星号 (*) 或者颜色突出显示。

## 创建新分支

```bash
git branch <branch-name>
```

例如，创建一个名为 `feature-xyz` 的新分支：

```bash
git branch feature-xyz
```

## 切换分支

```bash
git checkout <branch-name>
```

切换到 `feature-xyz` 分支：

```bash
git checkout feature-xyz
```

或者，创建并立即切换到新分支：

```bash
git checkout -b <branch-name>
```

例如：

```bash
git checkout -b feature-new-feature
```

## 删除分支

```bash
git branch -d <branch-name>
```

删除 `feature-xyz` 分支：

```bash
git branch -d feature-xyz
```

注意，只有当该分支已被合并到另一个分支时，才能使用 `-d` 选项。如果分支有未合并的更改，需要使用 `-D` 强制删除：

```bash
git branch -D <branch-name>
```

## 查看远程分支

```bash
git branch -r
```

这将列出所有远程跟踪的分支。

## 创建本地分支并跟踪远程分支

```bash
git branch --track <local-branch-name> <remote-branch-name>
```

例如，创建一个本地分支 `dev` 并跟踪远程的 `origin/dev`：

```bash
git branch --track dev origin/dev
```

## 重命名分支

```bash
git branch -m <new-branch-name>
```

将当前分支重命名为 `new-feature`：

```bash
git branch -m new-feature
```

或者，重命名另一个分支：

```bash
git branch -m <old-branch-name> <new-branch-name>
```

例如：

```bash
git branch -m old-feature new-feature
```


# 标签

## 检查现有标签
```bash
git tag
```
这个命令会列出所有的标签。

## 创建标签

Git 支持两种类型的标签：轻量级标签（lightweight）和附注标签（annotated）。

### 创建轻量级标签
轻量级标签实际上是一个指向特定提交的引用：
```bash
git tag <tagname>
```

### 创建附注标签

附注标签存储在 Git 数据库中，包含标签信息，作者，日期，和一条附注信息。推荐使用附注标签来保持更多的信息：
```bash
git tag -a <tagname> -m "your message here"
```

## 查看标签信息

要查看特定标签的详细信息，可以使用：
```bash
git show <tagname>
```

## 推送标签到远程

默认情况下，`git push` 命令不会将标签推送到远程服务器。你需要显式地推送标签：
```bash
git push origin <tagname>
```

或者，推送所有本地新建的标签：
```bash
git push origin --tags
```

## 删除标签

如果你需要删除本地标签，可以使用：
```bash
git tag -d <tagname>
```

要删除远程仓库的标签，需要先删除本地标签，然后推送删除操作到远程：
```bash
git push --delete origin <tagname>
```

## 检出标签

要检出特定的标签，可以使用 `git checkout` 命令。这会让你的工作目录处于“分离 HEAD 状态”，意味着你不在任何分支上工作：
```bash
git checkout <tagname>
```


# Checkout

`git checkout` 命令主要用于切换分支、恢复文件或创建新分支。

## 切换到现有分支

```bash
git checkout <branch-name>
```

例如，切换到 `main` 分支：

```bash
git checkout main
```

## 创建并切换到新分支

```bash
git checkout -b <new-branch-name>
```

例如，创建并切换到名为 `feature-branch` 的新分支：

```bash
git checkout -b feature-branch
```

## 恢复工作目录中的文件

```bash
git checkout <commit-hash> <file-path>
```

例如，恢复 `main` 分支中最后一次提交的 `README.md` 文件：

```bash
git checkout main README.md
```

或使用特定提交的哈希值：

```bash
git checkout <commit-hash> README.md
```

## 恢复暂存区中的文件

```bash
git checkout --staged <file-path>
```

例如，恢复暂存区中的 `index.html` 文件：

```bash
git checkout --staged index.html
```

## 恢复已删除的文件

```bash
git checkout <commit-hash> -- <file-path>
```

例如，恢复已删除的 `data.csv` 文件：

```bash
git checkout HEAD^ -- data.csv
```

这里 `HEAD^` 指向当前提交的父提交，意味着你正在从上一次提交中恢复文件。

## 退出编辑模式

如果你在使用 `git commit` 时进入了编辑模式但不希望编辑提交信息，可以使用 `git checkout` 退出：

```bash
git checkout .
```


# 远程仓库
## 查看远程仓库

```bash
git remote
```

这将列出所有已配置的远程仓库。

## 查看远程仓库详细信息

```bash
git remote -v
```

这将列出所有远程仓库的 URL 和访问方式（推送或拉取）。

## 添加远程仓库

```bash
git remote add <shortname> <url>
```

例如，添加名为 `upstream` 的远程仓库，其 URL 为 `https://github.com/user/repo.git`：

```bash
git remote add upstream https://github.com/user/repo.git
```

## 修改远程仓库 URL

```bash
root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote set-url --help
usage: git remote set-url [--push] <name> <newurl> [<oldurl>]
   or: git remote set-url --add <name> <newurl>
   or: git remote set-url --delete <name> <url>

    --push                manipulate push URLs
    --add                 add URL
    --delete              delete URLs
```

```bash
root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote -v
gitee   ssh://git@gitee.com/leev2v1/manifest.git (fetch)
gitee   ssh://git@gitee.com/leev2v1/manifest.git (push)

root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote add all ssh://git@gitee.com/leev2v1/manifest.git
root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote -v
all     ssh://git@gitee.com/leev2v1/manifest.git (fetch)
all     ssh://git@gitee.com/leev2v1/manifest.git (push)
gitee   ssh://git@gitee.com/leev2v1/manifest.git (fetch)
gitee   ssh://git@gitee.com/leev2v1/manifest.git (push)


root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote set-url --add all ssh://git@192.168.1.168:1022/root/0806-manifest.git
root@DESKTOP-GC4LAR7:/home/leefly/am62x/manifest# git remote -v
all     ssh://git@gitee.com/leev2v1/manifest.git (fetch)
all     ssh://git@gitee.com/leev2v1/manifest.git (push)
all     ssh://git@192.168.1.168:1022/root/0806-manifest.git (push)
gitee   ssh://git@gitee.com/leev2v1/manifest.git (fetch)
gitee   ssh://git@gitee.com/leev2v1/manifest.git (push)
```

`git remote set-url` 命令用于更改 Git 仓库中远程仓库的 URL。这个命令可以用来更新现有的远程仓库 URL，添加新的远程仓库 URL，或者删除某个远程仓库 URL。

### 基本语法

- `git remote set-url <name> <newurl> [<oldurl>]`: 更新远程仓库 `<name>` 的 URL 为 `<newurl>`。如果提供了 `<oldurl>`，则只更新与该 URL 匹配的远程 URL。
- `git remote set-url --add <name> <newurl>`: 向远程仓库 `<name>` 添加一个新的 URL `<newurl>`。
- `git remote set-url --delete <name> <url>`: 从远程仓库 `<name>` 删除指定的 URL。

### 示例

假设你有一个名为 `origin` 的远程仓库，你想修改其 URL 或者添加、删除 URL。

1. **更新远程仓库的 URL**:

   如果你想将远程仓库 `origin` 的 URL 更改为 `https://github.com/newrepo/newproject.git`，你可以运行:
```bash
git remote set-url origin https://github.com/newrepo/newproject.git
```

2. **更新特定 URL**:

   如果远程仓库 `origin` 有多个 URL，你只想更新其中的一个，比如将 `https://github.com/oldrepo/oldproject.git` 更改为 `https://github.com/newrepo/newproject.git`，你可以运行:
```bash
git remote set-url origin https://github.com/newrepo/newproject.git https://github.com/oldrepo/oldproject.git
```

3. **添加新的 URL 到远程仓库**:

   如果你想给远程仓库 `origin` 添加一个新的 URL `https://bitbucket.org/newrepo/newproject.git`，你可以运行:
```bash
git remote set-url --add origin https://bitbucket.org/newrepo/newproject.git
```

4. **删除远程仓库的 URL**:

   如果你想从远程仓库 `origin` 中删除一个 URL `https://bitbucket.org/newrepo/newproject.git`，你可以运行:
```bash
git remote set-url --delete origin https://bitbucket.org/newrepo/newproject.git
```

### 注意事项

- 当使用 `--push` 选项时，它会只更新或添加推送（push）URL。
- 如果不指定 `--push`，默认情况下，`set-url` 命令会同时更新拉取（fetch）和推送（push）URLs。
- 使用 `--delete` 选项时要小心，一旦删除了 URL，就无法通过 Git 命令恢复。



## 重命名远程仓库
重命名远程仓库的过程涉及到更改本地 Git 仓库中对远程仓库的引用名称。这不会直接影响远程仓库本身，仅更改本地 Git 配置中对该远程仓库的称呼。以下是使用 `git remote rename` 命令来重命名远程仓库的步骤：

```bash
git remote rename <old-name> <new-name>
```

例如，如果你想要将远程仓库 `origin` 重命名为 `myorigin`，你可以执行以下命令：

```bash
git remote rename origin myorigin
```

这将会把远程仓库 `origin` 的本地引用重命名为 `myorigin`。在重命名之后，你将使用新的名称 `myorigin` 来执行诸如 `git pull` 或 `git push` 等操作。

需要注意的是，如果你在本地有多个远程仓库的引用，务必确保你重命名的是正确的远程仓库，以免造成混淆。

如果在你的 Git 版本中 `git remote rename` 不可用，你也可以通过以下步骤手动重命名远程仓库：

1. 查看当前的远程仓库配置：
   ```bash
   git remote -v
   ```

2. 删除旧的远程仓库引用：
   ```bash
   git remote remove <old-name>
   ```

3. 添加新的远程仓库引用：
   ```bash
   git remote add <new-name> <url>
   ```

在这种情况下，`<url>` 应该是原远程仓库的 URL。

## 删除远程仓库

```bash
git remote remove <shortname>
```

例如，删除远程仓库 `upstream`：

```bash
git remote remove upstream
```

## 拉取远程仓库数据

```bash
git pull <shortname> <branch>
```

例如，从远程仓库 `origin` 的 `main` 分支拉取数据：

```bash
git pull origin main
```

## 推送数据到远程仓库

```bash
git push <shortname> <branch>
```

例如，将本地的 `main` 分支推送到远程仓库 `origin`：

```bash
git push origin main
```

## 列出远程仓库的引用

```bash
git ls-remote <shortname>
```

这将列出远程仓库的所有引用，包括分支和标签。


# diff

## 比较工作目录与暂存区的差异
```bash
git diff
```
这将显示你工作目录中未暂存的更改，即未使用 `git add` 命令添加到暂存区的更改。

## 比较暂存区与上一次提交的差异
```bash
git diff --staged
git diff --cached
```
这将显示你已暂存的更改，即即将在下一次提交中包含的更改。

## 比较工作目录与上一次提交的差异
```bash
git diff HEAD
```
这将显示工作目录中所有文件与最后一次提交的差异，包括未暂存和暂存的更改。

## 比较两个提交之间的差异
```bash
git diff <commit-hash> <another-commit-hash>
```
这将显示两个提交之间的差异，比如从一个旧的提交到一个新的提交。

## 比较两个分支之间的差异
```bash
git diff <branch1> <branch2>
```
这将显示两个分支之间的差异，通常用于查看在切换分支后需要合并的更改。

## 比较指定文件的差异
```bash
git diff <filename>
```
这将显示指定文件在工作目录与最后一次提交之间的差异。

## 比较指定文件的暂存差异
```bash
git diff --staged <filename>
```
这将显示指定文件在暂存区与最后一次提交之间的差异。

## 比较指定文件的工作目录与暂存差异
```bash
git diff <filename>
```
在没有其他参数的情况下，这将显示指定文件在工作目录与暂存区之间的差异。

## 比较指定文件的暂存与HEAD差异
```bash
git diff HEAD <filename>
```
这将显示指定文件在暂存区与最后一次提交之间的差异。

## 比较指定文件的差异，忽略空白字符变化
```bash
git diff --ignore-space-at-eol <filename>
```
这将显示指定文件的差异，但忽略行尾空白字符的变化。

# 二进制文件
```Bash
#!/bin/bash

set -x

gen_sdk_patch_file() {
    local ti_patch_dir=/home/zlglinux/am62x/am62xx-sdk/release/patch
    local tag_old=$(git describe --tags $(git rev-list --tags --max-count=2) | sed -n '2p')
    local tag_new=$(git describe --tags $(git rev-list --tags --max-count=2) | sed -n '1p')
    local commit_old=$(git log --pretty=format:"%H" -2 | sed -n '2p')
    local commit_new=$(git log --pretty=format:"%H" -2 | sed -n '1p')
    local code_patch="$ti_patch_dir/update_${tag_old}_to_${tag_new}_code.patch"
    local binary_patch="$ti_patch_dir/update_${tag_old}_to_${tag_new}_binary.patch"

    mkdir -p "$ti_patch_dir"
    [ -e "$binary_patch" ] && rm -r "$binary_patch"
    [ -e "$code_patch" ] && rm -r "$code_patch"

    diff_files=$(git show "$commit_new" --name-only --pretty=format:"")
    for file in $diff_files; do
        if git diff "$commit_old" "$commit_new" -- "$file" | grep -q "^Binary files"; then
            git diff --binary "$commit_old" "$commit_new" -- "$file" >> "$binary_patch"
        else
            git diff "$commit_old" "$commit_new" -- "$file" >> "$code_patch"
        fi
    done
}

gen_sdk_patch_file
```


# 补丁

## 创建补丁文件

### 创建基于当前提交的单个补丁
```bash
git format-patch HEAD~1
```
这将基于当前提交与前一个提交之间的差异创建一个补丁文件。

### 创建一系列补丁文件
```bash
git format-patch -N HEAD~3
```
这将基于最近三次提交创建一系列补丁文件，其中 `-N` 参数确保生成的补丁文件名包含提交的主题。

### 在两个特定提交间创建补丁

```bash
git format-patch commitA..commitB
```

这会创建一个或多个补丁文件，具体取决于 `commitA` 和 `commitB` 之间有多少个提交。每个补丁文件将对应一个单独的提交。

```bash
git format-patch -o /path/to/your/output/folder abc123..def456
```

如果想要输出到特定目录，可以使用 `-o` 或 `--output-directory` 参数，例如：

### 发送补丁邮件
```bash
git format-patch -o /tmp/patches HEAD~1 && mail -s "Patch for commit" user@example.com < /tmp/patches/0001-Your-commit-message.patch
```
这将创建一个补丁文件并立即通过邮件发送出去。


## 应用补丁文件

### 应用单个补丁文件
```bash
git am /path/to/patch-file.patch
```
这将应用位于指定路径的补丁文件。

### 应用一系列补丁文件
```bash
git am /path/to/*.patch
```
这将应用位于指定目录下的一系列补丁文件。

### 跳过错误继续应用补丁
```bash
git am --continue
```
如果在应用补丁过程中遇到冲突，使用此命令可以跳过冲突并继续应用剩余的补丁。

### 重置应用补丁过程
```bash
git am --abort
```
如果在应用补丁过程中出现问题，使用此命令可以中止补丁应用过程并清理中间状态。

# 仓库清理
## git gc

```bash
git gc --prune=now --aggressive
```
```bash
root@DESKTOP-GC4LAR7:/home/leefly/am62x/release# git gc --prune=now --aggressive
Enumerating objects: 3271, done.
Counting objects: 100% (3271/3271), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2966/2966), done.
Writing objects: 100% (3271/3271), done.
Total 3271 (delta 1047), reused 2104 (delta 0)
```


 Git 中用于清理和优化仓库的命令。

### 1. `git gc`

`git gc`（垃圾回收）命令用于清理 Git 仓库中的不必要的文件和优化仓库的存储。它可以通过以下方式帮助你管理仓库的大小和性能：

- **删除未被引用的对象**：清理那些已经被删除但仍保留在 Git 对象数据库中的文件。
- **压缩文件**：将多个小的 Git 对象合并成更大的对象以节省存储空间。
- **优化索引**：提升 Git 操作的性能。

### 2. `--prune=now`

- **`--prune`**：此选项指定 Git 应该删除哪些对象。`prune` 命令用于删除那些不再被任何分支或引用指向的 Git 对象（例如，已经被删除的文件的历史版本）。
- **`now`**：表示立即删除所有未被引用的对象。这会清除所有已经失效的对象，而不仅仅是那些超出默认保留期限的对象。

### 3. `--aggressive`

- **`--aggressive`**：此选项会使 `git gc` 更加深入地优化仓库。这意味着 Git 会尝试进一步压缩对象和清理额外的空间。使用 `--aggressive` 会消耗更多的时间和计算资源，因此在大型仓库中使用时可能需要较长的时间。

### 总结

`git gc --prune=now --aggressive` 命令会强制 Git 立即删除所有不再被引用的对象，并且以更深入的方式优化和压缩 Git 仓库。这可以帮助减少仓库的大小并提高性能，但可能会花费较长时间。

[【GITEE】码云上传代码文件出现上传错误的问题\_push rejected for repository size exceeds limit.-CSDN博客](https://blog.csdn.net/Ankie_/article/details/129437765)


### 注意事项

1. **备份仓库**：在执行这个命令之前，建议备份你的仓库，因为垃圾回收是不可逆的操作。
2. **等待完成**：这个命令可能会花费一些时间，尤其是在大型仓库中。
3. **适当使用**：一般情况下，默认的 `git gc` 已经足够用。如果你没有特定的优化需求，不一定需要使用 `--aggressive` 选项。

### 常见问题
```bash
root@DESKTOP-GC4LAR7:# git gc --prune=now --aggressive 
Enumerating objects: 3271, done. Counting objects: 100% (3271/3271), done. 
Delta compression using up to 16 threads 
error: pack-objects died of signal 9) 
fatal: failed to run repack
```

`git gc --prune=now --aggressive` 命令失败并出现 `error: pack-objects died of signal 9` 错误，这通常是由于内存不足导致的。`git gc` 在执行过程中会占用大量内存，特别是在处理大仓库时。
可在 WSL（Windows Subsystem for Linux）中的 Ubuntu 增加内存大小。

## git reflog
```bash
git reflog expire --expire=now --all
```

`git reflog expire --expire=now --all` 是一个用于清理 Git 仓库中的引用日志 (reflog) 的命令。以下是对该命令的详细解释：

### 1. `git reflog`
Git 的 `reflog` 记录了对分支引用所做的所有修改。例如，当你执行 `git commit`、`git rebase` 或 `git reset` 等操作时，Git 会在 `reflog` 中记录这些操作。`reflog` 是一个本地的日志，只有在你的本地仓库中存在，不会被推送到远程仓库。

### 2. `expire`
`git reflog expire` 命令用于清理和过期 `reflog` 中的条目。这个命令会删除指定时间之前的 `reflog` 条目，释放一些磁盘空间。

### 3. `--expire=now`
`--expire=now` 选项告诉 Git 立即过期所有 `reflog` 条目，而不是保留指定时间段内的条目。这意味着所有 `reflog` 条目将被清理掉。

### 4. `--all`
`--all` 选项告诉 Git 清理所有引用（branches 和 tags）的 `reflog`，而不仅仅是当前分支的 `reflog`。

### 组合起来
`git reflog expire --expire=now --all` 命令会立即清理掉所有分支和标签的 `reflog` 条目。这在你进行垃圾回收和清理仓库时非常有用，因为 `reflog` 条目可以引用已经被删除的对象，导致这些对象无法被 `git gc` 清理掉。

### 进一步的解释
- `git reflog expire --expire=now --all` 只是删除 `reflog` 条目，但不会删除实际的 Git 对象。
- 要删除不再被任何引用引用的 Git 对象，需要运行 `git gc --prune=now --aggressive`。
- 强制推送是为了确保远程仓库也同步更新，删除不需要的历史记录。

## git count-objects
```bash
git count-objects -vH
```
```bash
# git count-objects -vH
count: 0
size: 0 bytes
in-pack: 678
packs: 1
size-pack: 898.16 MiB
prune-packable: 0
garbage: 0
size-garbage: 0 bytes
```

## git rev-list
```bash
git rev-list --objects --all | wc -l
```

[blog.51cto.com/u\_16099356/10655948](https://blog.51cto.com/u_16099356/10655948)

你提供的命令用于查找 Git 仓库中最大的对象，并列出它们的相关信息。这个命令组合了几个 Git 和 Unix 命令来完成任务。让我们逐步解析这个命令：

1. **`git verify-pack -v .git/objects/pack/*.idx`**：
   - 这个命令会验证 Git 对象数据库中的包文件，并输出详细信息。
   - 输出包括对象的 SHA-1 哈希值、类型、大小等信息。

2. **`sort -k 3 -n`**：
   - 这个命令按第三列（即对象的大小）进行数字排序。

3. **`tail -500`**：
   - 这个命令取出排序后的最后 500 行，也就是最大的 500 个对象的信息。

4. **`awk '{print $1}'`**：
   - 这个命令提取每行的第一个字段，即对象的 SHA-1 哈希值。

5. **`git rev-list --objects --all`**：
   - 这个命令列出仓库中所有对象及其对应的提交信息。

6. **`grep "$( ... )" `**：
   - 这个命令使用前面生成的 SHA-1 哈希值列表来过滤 `git rev-list` 的输出，只保留匹配的对象。

### 完整命令解释

```sh
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -500 | awk '{print $1}')"
```

### 步骤详解

1. **获取最大的对象哈希值**：
   ```sh
   git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -500 | awk '{print $1}'
   ```

2. **过滤 `git rev-list` 的输出**：
   ```sh
   git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -500 | awk '{print $1}')"
   ```

### 实际操作

1. **进入你的 Git 仓库目录**：
   ```sh
   cd /path/to/your/repo
   ```

2. **运行命令**：
   ```sh
   git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -500 | awk '{print $1}')"
   ```

### 结果解读

- 输出结果将显示最大的 500 个对象及其相关的提交信息。
- 每一行包含对象的 SHA-1 哈希值和相关的提交信息。

### 注意事项

- 这个命令可能会非常耗时，特别是对于大型仓库。
- 如果你的仓库中有大量的对象，可能需要调整 `tail -500` 中的数字，以适应你的需求。

希望这些信息对你有所帮助！如果有任何问题或需要进一步的帮助，请随时提问。

## 示例脚本

```bash
# Step 1: 删除不需要的文件
git rm --cached <file>

# Step 2: 移除大文件的历史记录
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch <file>' --prune-empty --tag-name-filter cat -- --all

# Step 3: 清理引用日志
git reflog expire --expire=now --all

# Step 4: 进行垃圾回收
git gc --prune=now --aggressive

# Step 5: 强制推送清理后的仓库
git push origin --force --all
git push origin --force --tags
```

```bash
# Step 1: 找出并删除不必要的大文件
largest_files=$(git rev-list --objects --all | sort -k 2 | while read -r sha path; do
    size=$(git cat-file -s $sha)
    echo $size $path
done | sort -n -r | head -20)

echo "Largest files in the repository:"
echo "$largest_files"

# Step 2: 移除大文件的历史记录 (使用 BFG Repo-Cleaner)
bfg --delete-files "<filename_pattern>"

# Step 3: 清理 Git 仓库
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Step 4: 强制推送清理后的仓库
git push origin --force --all
git push origin --force --tags

```

```bash
pip install git-filter-repo

cd release
git rm -r --cached patch
echo "patch/" >> .gitignore
git add .gitignore
git commit -m "Stop tracking patch"
git filter-repo --path patch --invert-paths
git filter-branch --force --index-filter \
  "git rm -r --cached --ignore-unmatch patch" \
  --prune-empty --tag-name-filter cat -- --all
git push origin --force --all
git push origin --force --tags

git fetch origin
git reset --hard origin/main

df
export TMPDIR=/mnt/c
git reflog expire --expire=now --all
git gc --prune=now --aggressive

git count-objects -vH
git rev-list --objects --all | wc -l
```

在 Git 中，要停止跟踪某个文件夹，并且清除其历史记录，可以按以下步骤操作：

---

### 1. **停止跟踪文件夹并将其添加到 `.gitignore`**

#### 假设需要停止跟踪的文件夹为 `folder_to_ignore`

1. 停止跟踪文件夹（但不删除本地文件夹）：
    
    ```bash
    git rm -r --cached folder_to_ignore
    ```
    
2. 将文件夹添加到 `.gitignore` 文件：
    
    ```bash
    echo "folder_to_ignore/" >> .gitignore
    ```
    
3. 提交更改：
    
    ```bash
    git add .gitignore
    git commit -m "Stop tracking folder_to_ignore"
    ```
    

---

### 2. **清除历史记录中的该文件夹**

清除历史记录中的某个文件夹需要重写整个仓库的历史。这会导致仓库的 Git SHA-1 哈希发生变化，所以团队协作时需要特别小心。

#### 使用 `git filter-repo`（推荐）

1. 确保安装了 `git-filter-repo` 工具。如果未安装，可以通过以下命令安装：
    
    - Linux:
        
        ```bash
        sudo apt install git-filter-repo
        ```
        
    - macOS (使用 Homebrew):
        
        ```bash
        brew install git-filter-repo
        ```
        
2. 重写历史以移除指定文件夹：
    
    ```bash
    git filter-repo --path folder_to_ignore --invert-paths
    ```
    
    - `--path folder_to_ignore` 表示选择目标路径。
    - `--invert-paths` 表示删除该路径的内容。

#### 使用 `git filter-branch`（不推荐，但可选）

如果不能使用 `git-filter-repo`，可以用 `git filter-branch`：

```bash
git filter-branch --force --index-filter \
  "git rm -r --cached --ignore-unmatch folder_to_ignore" \
  --prune-empty --tag-name-filter cat -- --all
```

> **注意：** `git filter-branch` 在较大的代码库中速度较慢，并且可能会出现警告提示。

---

### 3. **强制推送到远程仓库**

清除历史后，需要强制推送更改到远程仓库：

```bash
git push origin --force --all
git push origin --force --tags
```

---

### 4. **通知团队成员**

由于历史被重写，其他开发者需要重置他们的本地仓库：

```bash
git fetch origin
git reset --hard origin/main
```

---

### 总结

- 使用 `git rm -r --cached` 停止跟踪文件夹。
- 用 `git filter-repo` 或 `git filter-branch` 清除历史记录。
- 强制推送更改并通知团队重新拉取仓库。

这可以有效地从仓库中清除指定文件夹及其历史记录。

## BFG Repo-Cleaner
[blog.51cto.com/u\_16099356/10655948](https://blog.51cto.com/u_16099356/10655948)

你提供的命令是使用 BFG Repo-Cleaner 工具来清理 Git 仓库中的特定文件夹。BFG 是一个高效的 Git 存储库清理工具，特别适合处理大规模的数据删除任务。以下是对你提供的命令的详细解释：

### 命令解释

```sh
java -jar bfg-1.14.0.jar --delete-folders {dist} --no-blob-protection frontend_saas.git
```

1. **`java -jar bfg-1.14.0.jar`**：
   - 这部分指定了使用 Java 运行 BFG Repo-Cleaner 工具。`bfg-1.14.0.jar` 是 BFG 工具的 JAR 文件。

2. **`--delete-folders {dist}`**：
   - 这个选项告诉 BFG 删除指定名称的文件夹。在这个例子中，`{dist}` 是要删除的文件夹名称。注意 `{dist}` 需要用大括号包围，表示这是一个文件夹名称的占位符。

3. **`--no-blob-protection`**：
   - 这个选项告诉 BFG 不要保护最近的任何大文件。默认情况下，BFG 会保护最近的大文件不被删除，以防止意外删除重要数据。如果你确定要删除这些文件，可以使用这个选项。

4. **`frontend_saas.git`**：
   - 这是你 Git 仓库的路径。BFG 将在这个仓库中执行清理操作。

### 执行步骤

1. **备份仓库**：
   - 在执行任何清理操作之前，强烈建议先备份你的仓库，以防止数据丢失。
   ```sh
   cp -r frontend_saas.git frontend_saas_backup.git
   ```

2. **运行 BFG 命令**：
   - 在你的 Git 仓库目录中运行 BFG 命令。
   ```sh
   java -jar bfg-1.14.0.jar --delete-folders {dist} --no-blob-protection frontend_saas.git
   ```

3. **清理 Git 仓库**：
   - BFG 清理完成后，你需要运行一些 Git 命令来彻底清除历史记录并优化仓库。
   ```sh
   cd frontend_saas.git
   git reflog expire --all --force
   git gc --prune=now --aggressive
   ```

4. **推送更改**：
   - 如果你在本地仓库中进行了清理，需要将更改推送到远程仓库。
   ```sh
   git push --force --all
   git push --force --tags
   ```

### 注意事项

- **数据备份**：始终在执行清理操作前备份仓库。
- **测试环境**：建议在测试环境中先进行操作，确保不会影响生产环境。
- **团队沟通**：如果仓库是多人协作的，确保团队成员知道你正在进行清理操作，避免冲突。

希望这些信息对你有所帮助！如果有任何问题或需要进一步的帮助，请随时提问。

## gitlab-rake
[清理 | 极狐GitLab](https://gitlab.cn/docs/jh/raketasks/cleanup.html)


看起来命令有误，正确的命令是 `gitlab:cleanup:project_uploads`。以下是正确的清理命令序列：

```bash
# 清理项目上传文件
gitlab-rake gitlab:cleanup:project_uploads

# 清理仓库
gitlab-rake gitlab:cleanup:repos

# 清理废弃的命名空间
gitlab-rake gitlab:cleanup:orphan_namespaces

# 清理构建产物
gitlab-rake gitlab:cleanup:builds

# 清理陈旧的 CI/CD 数据
gitlab-rake ci:cleanup:builds

# 清理过期的 LFS 对象
gitlab-rake gitlab:cleanup:orphan_lfs_files

# 强制执行垃圾回收
gitlab-rake gitlab:gc

# 检查仓库完整性
gitlab-rake gitlab:check SANITIZE=true
```

要查看所有可用的清理任务：
```bash
gitlab-rake -T | grep cleanup
```

```bash

root@9ffd82a20890:/# gitlab-rake -T | grep cleanup
rake ci:cleanup:builds                                                                                                      # GitLab | CI | Clean running builds
rake gitlab:cleanup:block_removed_ldap_users                                                                                # GitLab | Cleanup | Block users that have been removed in LDAP
rake gitlab:cleanup:delete_orphan_job_artifact_final_objects                                                                # GitLab | Cleanup | Delete orphan job artifact objects stored in the @final directory based on the CSV file
rake gitlab:cleanup:list_orphan_job_artifact_final_objects[provider]                                                        # GitLab | Cleanup | Generate a CSV file of orphan job artifact objects stored in the @final directory
rake gitlab:cleanup:orphan_job_artifact_files                                                                               # GitLab | Cleanup | Clean orphan job artifact files in local storage
rake gitlab:cleanup:orphan_lfs_file_references                                                                              # GitLab | Cleanup | Clean orphan LFS file references
rake gitlab:cleanup:orphan_lfs_files                                                                                        # GitLab | Cleanup | Clean orphan LFS files
rake gitlab:cleanup:project_uploads                                                                                         # GitLab | Cleanup | Clean orphaned project uploads
rake gitlab:cleanup:remote_upload_files                                                                                     # GitLab | Cleanup | Clean orphan remote upload files that do not exist in the db
rake gitlab:cleanup:remove_missed_source_branches                                                                           # GitLab | Cleanup | Clean missed source branches to be deleted
rake gitlab:cleanup:rollback_deleted_orphan_job_artifact_final_objects                                                      # GitLab | Cleanup | Rollback deleted final orphan job artifact objects (GCP only)
rake gitlab:cleanup:sessions:active_sessions_lookup_keys                                                                    # GitLab | Cleanup | Sessions | Clean ActiveSession lookup keys
```


如果想要通过Rails控制台强制删除项目：
```bash
# 进入 Rails 控制台
gitlab-rails console -e production

# 在控制台中执行
Project.pending_delete.each { |p| p.destroy! }
```

确保在执行这些操作之前：
1. GitLab 处于运行状态
2. 有足够的系统资源
3. 已经备份重要数据

执行完清理命令后，可以检查磁盘空间使用情况：
```bash
du -sh /var/opt/gitlab/*
```

# show

## 查看最近一次提交的信息
```bash
git show HEAD
```
这将显示最近一次提交的详细信息，包括提交消息、提交人、提交时间以及更改的文件列表和 diff 内容。

## 查看特定提交的信息
```bash
git show <commit-hash>
```
这将显示给定提交哈希值的提交详情。

## 查看特定文件在特定提交中的内容
```bash
git show <commit-hash>:<filename>
```
这将显示在指定提交时特定文件的内容。

## 查看两个提交之间的差异
```bash
git show <commit-hash>..<another-commit-hash>
```
这将显示两个提交之间的差异。

## 查看标签信息
```bash
git show <tag-name>
```
这将显示附注标签的信息，包括创建标签的时间、标签创建者和标签信息。

## 查看特定文件的更改历史
```bash
git show :/<filename>
```
这将显示文件的完整历史，包括所有修改过的版本。

## 查看特定提交中某文件的 diff
```bash
git show <commit-hash>:<filename>
```
这将显示在该提交中文件的 diff，即该文件在此次提交中的具体改动。

## 查看工作树中文件与上次提交的不同
```bash
git show HEAD:<filename>
```
这将显示工作树中文件与最后一次提交时该文件的差异。

## 查看暂存区与工作树中文件的不同
```bash
git show :1:<filename>
```
这将显示暂存区中的文件与工作树中该文件的差异。

## --name-only

这个选项会列出在该提交中被修改的文件的名称列表，但不会显示这些文件是被添加、修改还是删除。

```bash
git show --name-only <commit-hash>
```

- 如果你只对某种类型的更改感兴趣（例如，只想看到被删除的文件），你可以使用`--diff-filter`选项，如`git show --name-only --diff-filter=D <commit-hash>`将只显示被删除的文件。


## --name-status

与`--name-only`类似，但此选项还会显示文件是被添加（A）、修改（M）还是删除（D）。

```bash
git show --name-status <commit-hash>
```

# reset

## 重置到暂存区（Staging Area）

如果你想撤销对工作目录中某个文件所做的修改，并将这些修改移回暂存区，可以使用 `git reset`。例如，假设你编辑了 `file.txt` 并进行了暂存，但之后你又修改了文件并想要取消这些修改，可以执行：

```bash
git reset HEAD file.txt
```

这会将 `file.txt` 的状态重置到最近的暂存状态，但不会删除文件中的修改。这些修改现在可以再次被暂存或丢弃。

## 重置并丢弃工作目录中的更改

如果你想要完全撤销在工作目录中的更改，将文件恢复到最近的提交状态，可以使用 `--hard` 选项：

```bash
git reset --hard HEAD~1
```

这会将你的工作目录和暂存区重置到上一个提交的状态。注意，这将永久删除未暂存的更改，所以在使用 `--hard` 之前要确保你不需要那些更改。

## 重置暂存区而不影响工作目录

如果你想撤销对暂存区的更改，但保留工作目录中的更改，可以使用 `--mixed`（这是默认行为）：

```bash
git reset HEAD file.txt
```

这会将 `file.txt` 从暂存区移除，但不会影响工作目录中的文件内容。

## 移动分支指针

你可以使用 `reset` 来移动分支指针到另一个提交点，例如，如果你在一个开发分支上工作，而你想要回到上一个稳定的状态，可以这样做：

```bash
git reset --hard <commit-hash>
```

其中 `<commit-hash>` 是你想要重置到的提交的哈希值。

## 重置并重新应用更改

如果你想重置暂存区和工作目录到特定的提交，但保留工作目录中的未跟踪文件和目录，可以使用 `--keep` 选项：

```bash
git reset --keep <commit-hash>
```

这将重置暂存区和已跟踪文件的状态，同时保留工作目录中的未跟踪文件。

# stash
```bash
# 暂存当前的修改 
git stash 
# 回退到指定的提交 
git reset --hard 197b42ea2afd3d2ed461512556e2a152578 
# 恢复暂存的修改 
git stash pop
```

```bash
root@DESKTOP-GC4LAR7:/home/leefly/am62x/src/zy/ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/board-support/optee_os-3.20.0/core/arch/arm/plat-k3# git stash
Saved working directory and index state WIP on master: 7466f2f add support for read SOC_UID in optee -- add main.c
root@DESKTOP-GC4LAR7:/home/leefly/am62x/src/zy/ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/board-support/optee_os-3.20.0/core/arch/arm/plat-k3# git status
On branch master
Your branch is ahead of 'gitee/master' by 31 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean


root@DESKTOP-GC4LAR7:/home/leefly/am62x/src/zy/ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/board-support/optee_os-3.20.0/core/arch/arm/plat-k3# git reset --hard 197b42ea2afd3d2ed46151c1a512556e2a152578
HEAD is now at 197b42e add support for read write external otp efuse in optee os -- write mmr and row


root@DESKTOP-GC4LAR7:/home/leefly/am62x/src/zy/ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/board-support/optee_os-3.20.0/core/arch/arm/plat-k3# git stash pop
On branch master
Your branch is ahead of 'gitee/master' by 30 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   drivers/ti_sci.c
        modified:   drivers/ti_sci.h
        modified:   drivers/ti_sci_protocol.h
```

# clone

`git clone` 命令用于从远程仓库克隆一个新的副本到本地。

## 克隆仓库

```bash
git clone <repository-url>
```

例如，克隆 GitHub 上的一个仓库：

```bash
git clone https://github.com/username/repository.git
```

这将创建一个名为 `repository` 的本地目录，并下载远程仓库的所有数据。

## 克隆仓库到特定目录

```bash
git clone <repository-url> <directory-name>
```

例如，克隆仓库到名为 `myproject` 的目录：

```bash
git clone https://github.com/username/repository.git myproject
```

## 克隆仓库并指定深度克隆

```bash
git clone --depth=<depth> <repository-url>
```

例如，克隆仓库，只下载最近的 10 次提交：

```bash
git clone --depth=10 https://github.com/username/repository.git
```

## 克隆仓库并包含子模块

```bash
git clone --recurse-submodules <repository-url>
```

例如，克隆仓库并自动初始化和克隆所有子模块：

```bash
git clone --recurse-submodules https://github.com/username/repository.git
```

## 克隆仓库并指定分支

```bash
git clone -b <branch-name> <repository-url>
```

例如，克隆仓库并直接切换到 `develop` 分支：

```bash
git clone -b develop https://github.com/username/repository.git
```


# fetch 

`git fetch` 命令用于从远程仓库拉取数据到本地仓库，但不会自动合并这些数据。

## 从默认远程仓库拉取所有分支

如果你想要获取远程仓库的最新数据，可以简单地运行：

```bash
git fetch
```

这将从默认的远程仓库（通常是 `origin`）拉取所有分支的数据。

## 从特定远程仓库拉取

如果你想从一个非默认的远程仓库拉取数据，可以指定远程仓库的名字：

```bash
git fetch <remote-name>
```

例如，如果你有一个名为 `upstream` 的远程仓库，可以这样拉取数据：

```bash
git fetch upstream
```

## 拉取特定分支的数据

如果你想只拉取远程仓库中特定分支的数据，可以指定分支名：

```bash
git fetch <remote-name> <branch-name>
```

例如，从远程仓库 `origin` 的 `feature-branch` 分支拉取数据：

```bash
git fetch origin feature-branch
```

## 拉取并合并

虽然 `git fetch` 默认不会自动合并数据，但你可以使用 `git merge` 或 `git pull` 来手动合并拉取的数据。例如，将远程分支 `main` 的数据合并到你的当前分支：

```bash
git fetch origin main
git merge origin/main
```

或者，使用 `git pull` 直接拉取并合并：

```bash
git pull origin main
```

## 拉取所有远程仓库

如果你想从所有远程仓库拉取数据，可以使用 `-a` 或 `--all` 选项：

```bash
git fetch --all
```

这将从所有配置的远程仓库拉取数据。

## 检查远程仓库的最新状态

`git fetch` 不仅可以更新你的本地仓库，还可以让你看到远程仓库的最新状态。例如，运行 `git fetch` 后，使用 `git log` 或 `git branch -r` 来查看远程分支的最新提交。

# merge

`git merge` 命令用于将一个分支的更改合并到另一个分支中。

## 合并特定分支

假设你有一个开发分支 `feature-branch`，你想将其合并到主分支 `main`，可以运行：

```bash
git checkout main
git merge feature-branch
```

## 解决合并冲突

如果在合并过程中出现冲突，`git merge` 会暂停，并要求你手动解决冲突。解决冲突后，需要使用 `git add` 和 `git commit` 来完成合并：

```bash
# 编辑冲突的文件，解决冲突
git add <conflicted-file>
git commit -m "Merge feature-branch into main and resolve conflicts"
```

## 快进合并

当要合并的分支没有在目标分支中创建任何额外的分支点时，可以进行快进合并。例如，如果 `main` 分支包含 `feature-branch` 的所有提交，那么合并将是快进的：

```bash
git checkout main
git merge --ff-only feature-branch
```

## 使用合并策略

你可以指定合并策略，例如使用 `recursive` 策略，它提供了更好的冲突检测：

```bash
git checkout main
git merge -s recursive feature-branch
```

## 强制合并

如果你确定没有冲突，或者愿意覆盖现有的更改，可以使用 `--force` 选项：

```bash
git checkout main
git merge --force feature-branch
```

但请谨慎使用 `--force`，因为它可能会覆盖未提交的更改。

## 合并并提交

在合并时，你也可以直接创建一个合并提交，而无需手动添加和提交文件：

```bash
git checkout main
git merge -m "Merge feature-branch into main" feature-branch
```

这会创建一个合并提交，其中包含提供的合并信息。

# pull 

`git pull` 命令是 `git fetch` 和 `git merge` 的组合，用于从远程仓库拉取最新的更改并自动合并到当前分支。

## 拉取并合并默认远程仓库的默认分支

```bash
git pull
```

这将从默认的远程仓库（通常是 `origin`）拉取最新的更改，并尝试自动合并到你当前所在的分支。

## 拉取并合并特定远程仓库的特定分支

```bash
git pull <remote-name> <branch-name>
```

例如，从远程仓库 `origin` 的 `feature-branch` 分支拉取更改并合并：

```bash
git pull origin feature-branch
```

## 解决合并冲突

如果 `git pull` 导致合并冲突，你需要手动解决这些冲突，然后使用 `git add` 和 `git commit` 来完成合并：

```bash
# 解决冲突
git add <conflicted-file>
git commit -m "Resolve merge conflicts after pulling from origin/main"
```

## 使用不同的合并策略

你可以指定不同的合并策略来控制如何处理冲突，例如使用 `recursive` 策略：

```bash
git pull -s recursive origin main
```

## 快进合并

如果你确定远程分支的更改可以安全地快进合并到当前分支，可以使用 `--ff-only` 选项：

```bash
git pull --ff-only origin main
```

## 强制拉取和覆盖

使用 `--force` 选项会强制拉取并合并更改，覆盖本地未提交的更改。这通常不推荐，除非你知道自己在做什么：

```bash
git pull --force origin main
```

请注意，`--force` 可能会导致本地更改丢失，所以请谨慎使用。

# push 

`git push` 命令用于将本地仓库的提交推送到远程仓库。

## 推送当前分支到远程仓库的同名分支
```bash
git push
```

这条命令默认将当前分支推送到远程仓库的同名分支，前提是你的远程仓库已经通过 `git remote` 命令设置。

## 指定推送的远程仓库和分支
```bash
git push <remote-name> <branch-name>
```

例如，将 `main` 分支推送到远程仓库 `origin`：

```bash
git push origin main
```

## 推送所有分支到远程仓库
```bash
git push --all <remote-name>
```

例如，推送所有分支到远程仓库 `origin`：

```bash
git push --all origin
```

## 推送所有标签到远程仓库
```bash
git push --tags <remote-name>
```

例如，推送所有标签到远程仓库 `origin`：

```bash
git push --tags origin
```

## 强制推送，覆盖远程仓库的分支
```bash
git push --force <remote-name> <branch-name>
```

例如，强制推送 `main` 分支到远程仓库 `origin`，覆盖远程分支：

```bash
git push --force origin main
```

**警告：** 强制推送会覆盖远程仓库上的分支，可能导致他人的工作丢失，请谨慎使用。

## 设置远程仓库并推送
```bash
git push --set-upstream <remote-name> <branch-name>
```

例如，第一次推送 `feature` 分支到远程仓库 `origin` 并设置上游关系：

```bash
git push --set-upstream origin feature
```


# rebase 

`git rebase` 命令用于将一个分支的更改重新定位到另一个分支的顶部。

## 交互式 Rebase

```bash
git rebase -i <commit-hash>
```

例如，假设你想交互式地 rebase 当前分支到 `commit123`：

```bash
git rebase -i commit123
```

这将打开一个文本编辑器，列出从 `commit123` 开始直到当前分支头的所有提交。你可以在这里调整提交的顺序，或者选择 squash（压缩）或 fixup（修复）某些提交。

## Rebase 当前分支到另一个分支

```bash
git rebase <branch-name>
```

例如，如果你在 `feature-branch` 上工作，并希望将它 rebase 到 `main` 分支的顶部：

```bash
git checkout feature-branch
git rebase main
```

## Rebase 并解决冲突

在 rebase 过程中，如果遇到冲突，Git 会暂停 rebase 过程，等待你手动解决冲突。解决冲突后，使用以下命令继续 rebase：

```bash
git add <conflicted-file>
git rebase --continue
```

如果决定放弃 rebase 并恢复到 rebase 前的状态，可以使用：

```bash
git rebase --abort
```

## Rebase 跳过某些提交

在交互式 rebase 期间，你可以选择跳过某个提交。例如，假设你不想包含 `commit456`：

```bash
git rebase -i commit123
```

在编辑器中，找到 `commit456` 的行，并将 `pick` 改为 `drop` 或者删除该行。

# rm
Git 追踪文件的行为与 `.gitignore` 文件有关，但 `.gitignore` 文件只能阻止未被追踪的文件被添加到版本控制中。对于已经被追踪的文件（即已经被添加到 Git 仓库中的文件），即使它们匹配 `.gitignore` 中的模式，Git 也会继续追踪它们的变化。

在你的情况下，`release/docker/docker_compile.sh` 已经被添加到 Git 仓库中，因此即使你在 `.gitignore` 文件中添加了这个文件，它仍然会被 Git 追踪到。

要使 Git 停止追踪这个文件，你需要执行以下步骤：

1. **从 Git 追踪中移除文件**：
   ```bash
   git rm --cached release/docker/docker_compile.sh
   ```

2. **提交更改**：
   ```bash
   git commit -m "Stop tracking release/docker/docker_compile.sh"
   ```

这将从 Git 的追踪列表中移除文件，但不会删除工作目录中的文件。

执行以上步骤后，Git 将停止追踪 `release/docker/docker_compile.sh`，并且由于 `.gitignore` 文件的配置，它将不再被添加到 Git 仓库中。

# Submodule 

`git submodule` 命令用于在你的 Git 仓库中包含另一个 Git 仓库作为子模块。

## 添加子模块

```bash
git submodule add <repository-url> <path>
```

例如，添加一个名为 `library` 的子模块，其仓库位于 `https://github.com/example/library.git`，并将其放置在本地仓库的 `submodules/library` 目录下：

```bash
git submodule add https://github.com/example/library.git submodules/library
```

```bash
cd /home/liweiyu/am62x/tmp/parent/a/test-parent
git submodule add /home/liweiyu/am62x/test/test-parent/test-sub test-sub
cat .gitmodules
git submodule status
git add .gitmodules test-sub
git commit -m "Add test-sub as a submodule"
git submodule update --init --recursive
git push origin master
```


## 更新子模块

```bash
git submodule update --init --recursive
```

这将初始化并更新所有子模块到它们的默认提交，或者更新到你上次指定的提交。

```bash
cd /home/liweiyu/am62x/tmp/sub/b/test-sub
echo "New change" > newfile.txt
git add newfile.txt
git commit -m "Made some changes in b"
git push origin master

cd /home/liweiyu/am62x/tmp/parent/a/test-parent
git submodule update --remote b
git add b
git commit -m "Update submodule b to latest commit"
git push origin master
```


## 更新子模块到特定提交

```bash
cd submodules/library
git checkout <commit-hash>
cd ../..
git add submodules/library
git commit -m "Update library submodule to specific commit"
```

例如，更新 `library` 子模块到 `commit123`：

```bash
cd submodules/library
git checkout commit123
cd ../..
git add submodules/library
git commit -m "Update library submodule to commit123"
```

## 删除子模块

```bash
git rm --cached <path-to-submodule>
rm -rf .git/modules/<path-to-submodule>
rm -rf <path-to-submodule>
```

例如，删除 `submodules/library` 子模块：

```bash
git rm --cached submodules/library
rm -rf .git/modules/submodules/library
rm -rf submodules/library
```

## 克隆包含子模块的仓库

```bash
git clone --recurse-submodules <repository-url>
```

例如，克隆一个包含子模块的仓库：

```bash
git clone --recurse-submodules https://github.com/example/project-with-submodules.git
```

