以下是部署 Vue3 + Element Plus + Electron 应用程序在 Windows 10 下的详细步骤：

# **步骤 1：安装 Node.js 和 npm**

1. 下载并安装 Node.js 的最新版本（https://nodejs.org/en/download/）
2. 安装完成后，打开命令行工具（例如：cmd 或 PowerShell）并输入 `node -v` 和 `npm -v` 来验证安装是否成功

[安装Node.js - JavaScript教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/javascript/nodejs/install/)

[三种查找Windows10环境变量的方法\_win10查看环境变量-CSDN博客](https://blog.csdn.net/qq_30150579/article/details/128964064#:~:text=%E5%9C%A8%E6%88%91%E7%9A%84%E7%94%B5%E8%84%91%E4%B8%AD%E6%9F%A5%E7%9C%8B%201%20%E5%9C%A8%E6%A1%8C%E9%9D%A2%E4%B8%8A%E5%8F%B3%E9%94%AE%E6%88%91%E7%9A%84%E7%94%B5%E8%84%91%202%20%E7%82%B9%E5%87%BB%E5%B1%9E%E6%80%A7,3%20%E5%9C%A8%E5%85%B3%E4%BA%8E%E7%9A%84%E7%9B%B8%E5%85%B3%E8%AE%BE%E7%BD%AE%E9%87%8C%E5%8F%AF%E4%BB%A5%E7%9C%8B%E5%88%B0%E9%AB%98%E7%BA%A7%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE%204%20%E7%82%B9%E5%87%BB%E9%AB%98%E7%BA%A7%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE%E5%90%8E%EF%BC%8C%E4%BC%9A%E6%89%93%E5%BC%80%E7%B3%BB%E7%BB%9F%E5%B1%9E%E6%80%A7%E7%AA%97%E5%8F%A3%EF%BC%8C%E5%9C%A8%E7%AA%97%E5%8F%A3%E4%B8%AD%EF%BC%8C%E5%B0%B1%E8%83%BD%E6%89%BE%E5%88%B0%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%205%20%E7%82%B9%E5%87%BB%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E6%8C%89%E9%92%AE%E5%B0%B1%E8%83%BD%E6%89%93%E5%BC%80%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E7%AA%97%E5%8F%A3%EF%BC%8C%E7%AA%97%E5%8F%A3%E4%B8%AD%E5%88%86%E4%B8%BA%E7%94%A8%E6%88%B7%E5%8F%98%E9%87%8F%E5%92%8C%E7%B3%BB%E7%BB%9F%E5%8F%98%E9%87%8F)
[2023年node.js最新版(18.15.0)详细安装教程(保姆级)\_node最新版本-CSDN博客](https://blog.csdn.net/qq_60870118/article/details/129727274)

```javascript
C:\Users\leefly>node -v
v20.16.0

C:\Users\leefly>npm -v
10.8.1
```

```javascript
C:\Users\leefly>npm get prefix
C:\Users\leefly\AppData\Roaming\npm

C:\Users\leefly>npm get cache
C:\Users\leefly\AppData\Local\npm-cache
```

```javascript
C:\Users\leefly>npm config get registry
https://registry.npmjs.org/

C:\Users\leefly>npm config set registry https://registry.npmmirror.com/

C:\Users\leefly>npm config get registry
https://registry.npmmirror.com/
```

```bash
cnpm help install

cnpm install --help
cnpm uninstall --help
cnpm update --help
cnpm cache --help
cnpm info --help
cnpm view --help
cnpm list --help](<npm cache clean --force
npm cache verify
npm install electron --unsafe-perm=true
npm install electron -g --unsafe-perm
npm info electron
npm install -d electron
npm install --verbose electron
set DEBUG=* && npm install electron -g --unsafe-perm --verbose
set ELECTRON_MIRROR=https://registry.npmmirror.com/mirrors/electron/ && npm install -g electron
notepad C:\Program Files\nodejs\node_modules\npm\.npmrc

cnpm help install

cnpm config --help
cnpm explore --help
cnpm docs --help

cnpm install --help
cnpm uninstall --help
cnpm remove --help
cnpm update --help
cnpm cache --help
cnpm prune --help


cnpm info --help
cnpm view --help
cnpm list --help
cnpm find --help
cnpm search --help

cnpm init --help
cnpm rebuild --help
cnpm run --help
cnpm run-script --help
cnpm start --help
cnpm stop --help

cnpm pack --help
cnpm tag --help
cnpm version --help
cnpm publish --help
cnpm unpublish --help

npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm -v
cnpm install electron -g --unsafe-perm
electron -v
cnpm info electron
cnpm list -g

npm view electron
npm view electron version
cnpm install --help

cnpm install --save-dev electron-builder
cnpm install electron-builder -g
cnpm install electron-packager -g
cnpm uninstall electron
cnpm update electron@^8.0.1 -g>


cnpm install vue -g
cnpm install @vue/cli -g
cnpm info vue
npm list vue
vue --version

cnpm install webpack -g
cnpm install --global webpack-cli
cnpm install element-plus -g -d
```

## 设置npm源

在使用 Node.js 和 npm（Node Package Manager）的过程中，你可能会需要切换 npm 仓库源以加快下载速度或解决某些网络问题。这里列出一些常用的 npm 源地址供参考：

1. **npm 官方源 (默认)**
   - 地址: https://registry.npmjs.org/
   - 命令: `npm config set registry https://registry.npmjs.org/`

2. **淘宝 NPM 镜像 (Taobao)**
   - 地址: https://registry.npmmirror.com/
   - 命令: `npm config set registry https://registry.npmmirror.com/`

[npmmirror 镜像站](https://www.npmmirror.com/)

3. **cnpmjs（淘宝源的另一个版本）**
   - https://registry.cnpmjs.org/
   - 命令: `npm config set registry https://registry.cnpmjs.org/`

4. **阿里云 NPM 镜像**
   - 地址: https://npm.alibaba.com/
   - 命令: `npm config set registry https://npm.alibaba.com/`

5. **京东npm镜像**
   - https://registry.npm.jd.com/
   - 命令: `npm config set registry https://registry.npm.jd.com/`

6. **Coding NPM 镜像**
   - 地址: https://npm.coding.net/
   - 命令: `npm config set registry https://npm.coding.net/`

7. **npmMirror（国内镜像，由多个源组成）**
   - https://skimdb.npmjs.com/registry/
   - 命令: `npm config set registry https://skimdb.npmjs.com/registry/`

8. **JSDelivr CDN (适用于特定包)**
   - 地址: https://data.jsdelivr.com/v1/mapping/npm/
   - 命令: `npm config set registry https://data.jsdelivr.com/v1/mapping/npm/`
   - 这个不是直接的 npm 源，而是可以用于直接通过 CDN 访问包的文件。


你可以使用以下命令来检查当前配置的 npm 源：
```
npm config get registry
```

如果你想要重置为官方源，可以使用：
```
npm config set registry https://registry.npmjs.org/
```

## npm命令

```bash
C:\Users\leefly>npm --help
npm <command>

Usage:

npm install        install all the dependencies in your project
npm install <foo>  add the <foo> dependency to your project
npm test           run this project's tests
npm run <foo>      run the script named <foo>
npm <command> -h   quick help on <command>
npm -l             display usage info for all commands
npm help <term>    search for help on <term> (in a browser)
npm help npm       more involved overview (in a browser)

All commands:

    access, adduser, audit, bugs, cache, ci, completion,
    config, dedupe, deprecate, diff, dist-tag, docs, doctor,
    edit, exec, explain, explore, find-dupes, fund, get, help,
    help-search, hook, init, install, install-ci-test,
    install-test, link, ll, login, logout, ls, org, outdated,
    owner, pack, ping, pkg, prefix, profile, prune, publish,
    query, rebuild, repo, restart, root, run-script, sbom,
    search, set, shrinkwrap, star, stars, start, stop, team,
    test, token, uninstall, unpublish, unstar, update, version,
    view, whoami

Specify configs in the ini-formatted file:
    C:\Users\leefly\.npmrc
or on the command line via: npm <command> --key=value

More configuration info: npm help config
Configuration fields: npm help 7 config

npm@10.8.1 C:\Program Files\nodejs\node_modules\npm
```

```bash
npm help install
cnpm install --help
```


### 1. 初始化项目
- **`npm init`**  
  初始化项目，创建一个 `package.json` 文件。这个命令会询问一系列关于项目的问题，比如名称、版本、描述、入口文件、测试命令、Git 仓库 URL、作者姓名、许可证类型等。

  ```bash
  npm init
  ```

- **`npm init --yes` 或 `npm init -y`**  
  快速初始化项目并接受所有默认值。
  
  ```bash
  npm init --yes
  ```

### 2. 安装依赖
- **`npm install <package>` 或 `npm i <package>`**  
  安装指定的 npm 包。
  
  ```bash
  npm install lodash
  ```
  
- **`npm install <package> --save` 或 `npm i <package> -S`**  
  安装指定的 npm 包，并将它添加到 `package.json` 的 `dependencies` 字段中。
  
  ```bash
  npm install lodash --save
  ```
  
- **`npm install <package> --save-dev` 或 `npm i <package> -D`**  
  安装指定的 npm 包，并将它添加到 `package.json` 的 `devDependencies` 字段中。
  

  ```bash
  npm install webpack --save-dev
  ```

### 3. 卸载依赖
- **`npm uninstall <package>` 或 `npm un <package>`**  
  卸载指定的 npm 包。
  
  ```bash
  npm uninstall lodash
  ```

- **`npm uninstall <package> --save` 或 `npm un <package> -S`**  
  卸载指定的 npm 包，并从 `package.json` 中移除相应的条目。
  
  ```bash
  npm uninstall lodash --save
  ```

### 4. 更新依赖
- **`npm update`**  
  更新所有已安装的依赖包至最新版本。

  ```bash
  npm update
  ```
  
- **`npm update <package>`**  
  更新指定的依赖包至最新版本。
  ```bash
  npm update lodash
  ```

### 5. 列出已安装的包
- **`npm list` 或 `npm ls`**  
  显示当前项目中所有已安装的依赖包。
  ```bash
  npm list
  ```

- **`npm list --depth=0` 或 `npm ls --depth=0`**  
  只显示顶级依赖包，不包括子依赖。
  

  ```bash
  npm list --depth=0
  ```

### 6. 检查过期的依赖
- **`npm outdated`**  
  列出所有已安装的依赖包，并检查它们是否已经过期。
  ```bash
  npm outdated
  ```

### 7. 运行脚本
- **`npm run <script>`**  
  执行 `package.json` 中定义的脚本。
  ```bash
  npm run dev
  ```

- **`npm start`**  
  运行 `package.json` 中定义的 `start` 脚本。
  ```bash
  npm start
  ```

### 8. 查看包信息
- **`npm view <package>`**  
  查看指定包的详细信息，包括版本、描述、作者、主页、贡献者、贡献者指南、依赖项等。
  ```bash
  npm view lodash
  ```

- **`npm view <package> version`**  
  查看指定包的当前版本。
  ```bash
  npm view lodash version
  ```

### 9. 清除缓存
- **`npm cache clean`** 或 **`npm cache verify`**  
  清除 npm 缓存。
  ```bash
  npm cache clean --force
  ```

### 10. 查找包
- **`npm search <keyword>`**  
  在 npm 仓库中搜索包含关键词的包。
  ```bash
  npm search lodash
  ```

### 11. 发布包
- **`npm publish`**  
  将你的 npm 包发布到 npm 仓库。
  ```bash
  npm publish
  ```

### 12. 获取帮助
- **`npm help`**  
  显示帮助文档。
  ```bash
  npm help
  ```

- **`npm <command> --help`**  
  显示特定命令的帮助文档。
  ```bash
  npm install --help
  ```

## cnpm命令

```bash
C:\Windows\system32>cnpm --help

  Usage: cnpm [options]

  Options:

    -h, --help                       output usage information
    -v, --version                    show full versions
    -r, --registry [registry]        registry url, default is https://registry.npmmirror.com
    -w, --registryweb [registryweb]  web url, default is https://npmmirror.com
    --disturl [disturl]              dist url for node-gyp, default is https://cdn.npmmirror.com/binaries/node
    -c, --cache [cache]              cache folder, default is C:\Users\leefly\.cnpm
    -u, --userconfig [userconfig]    userconfig file, default is C:\Users\leefly\.cnpmrc
    -y, --yes                        yes all confirm
    --ignore-custom-config           ignore custom .cnpmrc
    --proxy [proxy]                  set a http proxy, no default

Usage: cnpm [option] <command>
Help: http://cnpmjs.org/help/cnpm

  Extend command
    web                            open cnpm web (ex.: cnpm web)
    check [ingoreupdate]           check project dependencies, add ignoreupdate will not check modules' latest version(ex.: cnpm check, cnpm check -i)
    doc [moduleName]               open document page (ex.: cnpm doc egg)
    sync [moduleName]              sync module from source npm (ex.: cnpm sync egg)
    user [username]                open user profile page (ex.: cnpm user fengmk2)

  npm command use --registry=https://registry.npmmirror.com
    where <command> is one of:
    add-user, adduser, apihelp, author, bin, bugs, c, cache,
    completion, config, ddp, dedupe, deprecate, docs, edit,
    explore, faq, find, find-dupes, get, help, help-search,
    home, i, info, init, install, isntall, la, link, list, ll,
    ln, login, ls, outdated, owner, pack, prefix, prune,
    publish, r, rb, rebuild, remove, restart, rm, root,
    run-script, s, se, search, set, show, shrinkwrap, star,
    start, stop, submodule, tag, test, tst, un, uninstall,
    unlink, unpublish, unstar, up, update, v, version, view,
    whoami
      npm <cmd> -h     quick help on <cmd>
      npm -l           display full usage info
      npm faq          commonly asked questions
      npm help <term>  search for help on <term>
      npm help npm     involved overview

      Specify configs in the ini-formatted file:
          C:\Users\leefly\.cnpmrc
      or on the command line via: npm <command> --key value
      Config info can be viewed via: npm help config
```

```bash
cnpm help install

cnpm config --help
cnpm explore --help
cnpm docs --help

cnpm install --help
cnpm uninstall --help
cnpm remove --help
cnpm update --help
cnpm cache --help
cnpm prune --help


cnpm info --help
cnpm view --help
cnpm list --help
cnpm find --help
cnpm search --help

cnpm init --help
cnpm rebuild --help
cnpm run --help
cnpm run-script --help
cnpm start --help
cnpm stop --help

cnpm pack --help
cnpm tag --help
cnpm version --help
cnpm publish --help
cnpm unpublish --help
```


`cnpm` 是一个用于管理 Node.js 包的工具，它是 `npm` 的一个镜像版本，提供了额外的功能，特别是在网络环境不佳时，`cnpm` 通过国内的镜像源可以更快地安装依赖包。

### 基本用法
- **命令格式**: `cnpm [选项] <命令>`
- **帮助文档**: 使用 `cnpm --help` 或者访问 [帮助页面](http://cnpmjs.org/help/cnpm)。

### 常用选项
- `-h, --help`: 输出帮助信息。
- `-v, --version`: 显示完整版本信息。
- `-r, --registry [registry]`: 设置注册表地址，默认为 `https://registry.npmmirror.com`。
- `-w, --registryweb [registryweb]`: 设置网页地址，默认为 `https://npmmirror.com`。
- `--disturl [disturl]`: 设置 `node-gyp` 的二进制文件地址，默认为 `https://cdn.npmmirror.com/binaries/node`。
- `-c, --cache [cache]`: 设置缓存文件夹，默认为 `C:\Users\leefly\.cnpm`。
- `-u, --userconfig [userconfig]`: 设置用户配置文件路径，默认为 `C:\Users\leefly\.cnpmrc`。
- `-y, --yes`: 对所有确认提示都自动选择 "是"。
- `--ignore-custom-config`: 忽略自定义的 `.cnpmrc` 文件。
- `--proxy [proxy]`: 设置 HTTP 代理，默认为空。

### 基础命令

在 `cnpm` 中，你可以使用绝大多数 `npm` 的命令。这些命令的功能与 `npm` 中相同，以下是一些常用命令的简要说明：
- **adduser/add-user**: 添加用户或登录。
- **apihelp**: 显示 API 帮助信息。
- **author**: 查看包的作者信息。
- **bin**: 显示可执行文件的路径。
- **bugs**: 查看包的错误报告地址。
- **cache**: 管理本地缓存。
- **config**: 管理配置文件。
- **dedupe**: 删除重复的依赖项。
- **deprecate**: 标记包为过时。
- **docs**: 打开包的文档页面。
- **edit**: 编辑包的代码。
- **explore**: 在文件系统中打开包的目录。
- **faq**: 查看常见问题解答。
- **find**: 搜索包。
- **get**: 获取配置值。
- **help/help-search**: 显示帮助信息或搜索帮助主题。
- **home**: 打开包的主页。
- **i/install**: 安装依赖包。
- **init**: 初始化一个新的 Node.js 项目。
- **link**: 创建符号链接。
- **list/ls**: 列出已安装的包。
- **login**: 登录到 npm。
- **outdated**: 检查过时的依赖项。
- **owner**: 管理包的拥有者。
- **pack**: 打包一个包。
- **prune**: 删除未使用的包。
- **publish**: 发布包到 npm。
- **rebuild**: 重新编译包。
- **remove/rm/uninstall**: 卸载包。
- **restart**: 重启包。
- **root**: 显示包的根目录。
- **run-script**: 运行脚本。
- **search**: 搜索包。
- **set**: 设置配置值。
- **shrinkwrap**: 锁定依赖关系。
- **start/stop**: 启动或停止包。
- **submodule**: 管理子模块。
- **tag**: 管理包的标签。
- **test**: 运行测试。
- **unpublish**: 取消发布包。
- **unstar**: 取消标记包为星标。
- **update**: 更新已安装的包。
- **version**: 显示版本信息。
- **view**: 查看包的信息。
- **whoami**: 显示当前登录用户的信息。

这些命令在 `cnpm` 中的使用方式与 `npm` 完全一致，只需根据需要加上相应的参数即可。

#### info
```bash
C:\Windows\system32>cnpm info --help
View registry info

Usage:
npm view [<package-spec>] [<field>[.subfield]...]

Options:
[--json] [-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
[-ws|--workspaces] [--include-workspace-root]

aliases: info, show, v

Run "npm help view" for more info
```

#### view
```bash
C:\Windows\system32>cnpm view --help
View registry info

Usage:
npm view [<package-spec>] [<field>[.subfield]...]

Options:
[--json] [-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
[-ws|--workspaces] [--include-workspace-root]

aliases: info, show, v

Run "npm help view" for more info
```

#### list
```bash
C:\Windows\system32>cnpm list --help
List installed packages

Usage:
npm ls <package-spec>

Options:
[-a|--all] [--json] [-l|--long] [-p|--parseable] [-g|--global] [--depth <depth>]
[--omit <dev|optional|peer> [--omit <dev|optional|peer> ...]] [--link]
[--package-lock-only] [--unicode]
[-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
[-ws|--workspaces] [--include-workspace-root] [--install-links]

alias: list

Run "npm help ls" for more info
```

#### install
```bash
C:\Windows\system32>cnpm install --help

Usage:

  npm install
  npm install <pkg>
  npm install <pkg> --workspace=<workspace>
  npm install <pkg> -w <workspace>
  npm install <pkg> --workspaces
  npm install <pkg>@<tag>
  npm install <pkg>@<version>
  npm install <pkg>@<version range>
  npm install <folder>
  npm install <tarball file>
  npm install <tarball url>
  npm install <git:// url>
  npm install <github username>/<github project>
  npm install --proxy=http://localhost:8080
  npm install --lockfile-path=</path/to/package-lock.json>

Can specify one or more: npminstall ./foo.tgz bar@stable /some/folder
If no argument is supplied, installs dependencies from ./package.json.

Options:

  --production: won't install devDependencies
  --client: install clientDependencies and buildDependencies
  --save, --save-dev, --save-optional, --save-exact, --save-client, --save-build, --save-isomorphic: save installed dependencies into package.json
  --no-save: Prevents saving to dependencies
  -g, --global: install devDependencies to global directory which specified in '$npm config get prefix'
  -r, --registry: specify custom registry
  -c, --china: specify in china, will automatically using chinese npm registry and other binary's mirrors
  -d, --detail: show detail log of installation
  -w, --workspace: install on one workspace only, e.g.: npminstall koa -w a
  --workspaces: install new package on all workspaces, e.g: npminstall foo --workspaces
  --trace: show memory and cpu usages traces of installation
  --ignore-scripts: ignore all preinstall / install and postinstall scripts during the installation
  --foreground-scripts: scripts run in the background by default, to see the output, run with: --foreground-scripts
  --no-optional: ignore all optionalDependencies during the installation
  --forbidden-licenses: forbidden install packages which used these licenses
  --engine-strict: refuse to install (or even consider installing) any package that claims to not be compatible with the current Node.js version.
  --flatten: flatten dependencies by matching ancestors' dependencies
  --registry-only: make sure all packages install from registry. Any package is installed from remote(e.g.: git, remote url) cause install fail.
  --cache-strict: use disk cache even on production env.
  --fix-bug-versions: automatically fix bug version of packages.
  --prune: prune unnecessary files from ./node_modules, such as markdown, typescript source files, and so on.
  --dependencies-tree: install with dependencies tree to restore the last install.
  --force-link-latest: force link latest version package to module root path.
  --offline: offline mode. If a package won't be found locally, the installation will fail.
```

`cnpm` 是一个基于 `npm` 的替代工具，它使用中国的镜像站点来加速包的下载和安装。下面是您提供的帮助文档的详细解释：
##### 基本用法
- `npm install`: 如果没有提供任何参数，则根据当前目录下的 `package.json` 文件安装依赖。
- `npm install <pkg>`: 安装指定的包 `<pkg>`。
- `npm install <pkg> --workspace=<workspace>` 或 `npminstall <pkg> -w <workspace>`: 在特定的工作空间 `<workspace>` 中安装包。
- `npm install <pkg> --workspaces`: 在所有工作空间中安装包。
- `npm install <pkg>@<tag>`: 安装包的特定标签版本。
- `npm install <pkg>@<version>`: 安装包的特定版本。
- `npm install <pkg>@<version range>`: 安装符合版本范围的包。
- `npm install <folder>`: 从本地文件夹安装包。
- `npm install <tarball file>`: 从 tarball 文件安装包。
- `npm install <tarball url>`: 从 tarball URL 安装包。
- `npm install <git:// url>`: 从 Git 仓库 URL 安装包。
- `npm install <github username>/<github project>`: 从 GitHub 用户名和项目名安装包。
- `npm install --proxy=http://localhost:8080`: 指定代理服务器进行安装。
- `npm install --lockfile-path=</path/to/package-lock.json>`: 指定锁定文件的位置。

可以指定一个或多个参数进行安装，例如：`npminstall ./foo.tgz bar@stable /some/folder`

#####  选项

- `--production`: 不安装 `devDependencies`。
- `--client`: 安装 `clientDependencies` 和 `buildDependencies`。
- `--save`: 将安装的包保存到 `dependencies` 字段中。
- `--save-dev`: 将安装的包保存到 `devDependencies` 字段中。
- `--save-optional`: 将安装的包保存到 `optionalDependencies` 字段中。
- `--save-exact`: 使用确切版本号保存依赖项。
- `--save-client`: 将安装的包保存到 `clientDependencies` 字段中。
- `--save-build`: 将安装的包保存到 `buildDependencies` 字段中。
- `--save-isomorphic`: 将安装的包保存到 `isomorphicDependencies` 字段中。
- `--no-save`: 阻止将依赖项保存到 `package.json` 文件。
- `-g, --global`: 全局安装包。
- `-r, --registry`: 指定自定义的注册表。
- `-c, --china`: 自动使用中国镜像站和其他二进制文件的镜像。
- `-d, --detail`: 显示详细的安装日志。
- `-w, --workspace`: 只在一个特定的工作空间安装包，例如：`npminstall koa -w a`。
- `--workspaces`: 在所有工作空间中安装新包，例如：`npminstall foo --workspaces`。
- `--trace`: 显示内存和 CPU 使用情况的跟踪信息。
- `--ignore-scripts`: 忽略所有预安装、安装和后安装脚本。
- `--foreground-scripts`: 默认情况下，脚本在后台运行；要查看输出，使用此选项。
- `--no-optional`: 忽略所有可选依赖项。
- `--forbidden-licenses`: 禁止安装使用某些许可证的包。
- `--engine-strict`: 拒绝安装与当前 Node.js 版本不兼容的包。
- `--flatten`: 展平依赖关系，使祖先的依赖关系与子依赖关系匹配。
- `--registry-only`: 确保所有包都从注册表安装。任何非注册表来源的包都会导致安装失败。
- `--cache-strict`: 即使在生产环境中也使用磁盘缓存。
- `--fix-bug-versions`: 自动修复包中的已知问题版本。
- `--prune`: 从 `./node_modules` 目录中移除不必要的文件，如 Markdown 文件、TypeScript 源代码等。
- `--dependencies-tree`: 使用依赖树安装以恢复上次的安装状态。
- `--force-link-latest`: 强制链接最新版本的包到模块根路径。
- `--offline`: 离线模式。如果找不到本地包，则安装失败。

### 扩展命令
- `web`: 打开 cnpm 的网页界面，例如 `cnpm web`。
- `check [ingoreupdate]`: 检查项目依赖关系，如果添加 `ignoreupdate` 参数，将不会检查模块的最新版本，例如 `cnpm check -i`。
- `doc [moduleName]`: 打开指定模块的文档页面，例如 `cnpm doc egg`。
- `sync [moduleName]`: 从源 npm 同步模块，例如 `cnpm sync egg`。
- `user [username]`: 打开指定用户的个人资料页面，例如 `cnpm user fengmk2`。

### 其他说明
cnpm 支持绝大部分 `npm` 命令，只需添加 `--registry=https://registry.npmmirror.com` 参数即可。例如 `cnpm install`。

在 Windows 系统下，你可以通过配置文件 `C:\Users\leefly\.cnpmrc` 来自定义配置，也可以直接在命令行中使用 `npm <command> --key value` 的格式来指定配置。

这种工具特别适用于国内开发者，由于官方的 npm 服务器在国内访问较慢，cnpm 通过镜像加速，可以极大提高包管理的效率。

# **步骤 2：安装 Electron**

1. 在命令行工具中输入 `npm install electron` 来安装 Electron
2. 等待安装完成后，输入 `electron -v` 来验证安装是否成功

[【cnpm】cnpm的安装方法（附详细步骤）\_cnpm 安装-CSDN博客](https://blog.csdn.net/qq_59012240/article/details/128448457)
[npm淘宝镜像cnpm安装使用（最新版），cnpm临时单次/永久使用\_npm淘宝镜像安装和使用-CSDN博客](https://blog.csdn.net/qq_23073811/article/details/127904378)
[electron使用笔记 - jcsu - 博客园](https://www.cnblogs.com/JCSU/articles/18339353)
[npmmirror 镜像站](https://www.npmmirror.com/package/electron)
[Windows中安装Electron说明​ 1、安装NodeJS 下载地址： http://nodejs.cn/down - 掘金](https://juejin.cn/post/6981609044863795208)
[Electron](https://www.electronjs.org/zh/docs/latest/tutorial/installation)

```bash
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm -v
cnpm install electron -g --unsafe-perm
electron -v
cnpm list -g
```


 手动下载和安装步骤：
1. **下载 Electron**: 访问 [Electron 发布页面](https://www.electronjs.org/releases) 并手动下载所需版本的 tarball 文件。
2. **解压文件**: 将下载的 tarball 文件解压到一个临时目录。
3. **使用 npm link**: 在解压后的目录中运行 `npm link`，然后在你的项目中运行 `npm link electron`。

    ```bash
    # 解压 Electron tarball 文件
    tar -xvf electron-vXX.XX.X-win32-x64.tar.gz
    
    # 在 Electron 目录下
    cd electron-vXX.XX.X-win32-x64
    npm link
    
    # 在你的项目目录下
    cd /path/to/your/project
    npm link electron
    ```

# **步骤 3：创建 Electron 项目**

1. 在命令行工具中输入 `electron-quick-start` 来创建一个新的 Electron 项目
2. 按照提示输入项目名称和描述等信息
3. 等待项目创建完成后，进入项目目录

[GitHub - electron/electron-quick-start: Clone to try a simple Electron app](https://github.com/electron/electron-quick-start)


# **步骤 4：安装 Vue3 和 Element Plus**

1. 在命令行工具中输入 `npm install vue@next` 来安装 Vue3
2. 输入 `npm install element-plus` 来安装 Element Plus
3. 等待安装完成后，输入 `npm run serve` 来启动 Vue3 项目

```bash
cnpm install vue -g
cnpm install @vue/cli -g
cnpm info vue
npm list vue
vue --version

cnpm install webpack -g
cnpm install --global webpack-cli
cnpm install element-plus -g -d
```

[Site Unreachable](https://blog.csdn.net/weixin_43796325/article/details/123407232)

# **步骤 5：集成 Electron 和 Vue3**

1. 在 `main.js` 文件中导入 Electron 和 Vue3
2. 创建一个新的 Electron 窗口并加载 Vue3 应用程序
3. 等待窗口创建完成后，输入 `electron .` 来启动 Electron 应用程序

# **步骤 6：打包 Electron 应用程序**

1. 在命令行工具中输入 `electron-builder` 来安装 Electron Builder
2. 创建一个新的 `electron-builder.yml` 文件并配置打包选项
3. 输入 `electron-builder` 来打包 Electron 应用程序

```bash
cnpm install --save-dev electron-builder
cnpm install electron-builder -g -d
cnpm install electron-packager -g -d
```


# **步骤 7：部署 Electron 应用程序**

1. 将打包好的 Electron 应用程序复制到目标机器上
2. 双击应用程序图标来启动应用程序

**注意事项**

* 在 Windows 10 下，需要使用 `electron-builder` 来打包 Electron 应用程序
* 需要配置 `electron-builder.yml` 文件来指定打包选项
* 可以使用 `electron-builder` 来创建安装包和可执行文件

以下是 `electron-builder.yml` 文件的示例配置：
```yaml
appId: com.example.electron-app
productName: Electron App
copyright: Copyright 2023 Example Inc.
build:
  appId: com.example.electron-app
  win32:
    target: nsis
    icon: icon.ico
  nsis:
    oneClick: false
    perMachine: true
```
以上配置指定了应用程序的 ID、名称、版权信息和打包选项。
