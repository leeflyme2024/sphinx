```Bash
git log --oneline

git add -u
git commit -m "XXX"
git tag
git tag am62xx-sdk-v1.0.0.5
git tag -d am62xx-sdk-v1.0.0.5
git show am62xx-sdk-v1.0.0.5
git push origin am62xx-sdk-v1.0.0.5
git push origin am62xx-sdk-v1.0.0.5 --delete
git push orgin master

git tag am62xx-sdk-v1.0.0.6,en-plus 8b96f4a96ae21b5d45518b0ea57854f77bcd2c38
git push gitee am62xx-sdk-v1.0.0.6,en-plus
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
git remote set-url <shortname> <new-url>
```

例如，将远程仓库 `origin` 的 URL 更改为 `https://github.com/newuser/newrepo.git`：

```bash
git remote set-url origin https://github.com/newuser/newrepo.git
```

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

