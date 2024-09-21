[预先准备 | Tauri Apps](https://tauri.app/zh-cn/v1/guides/getting-started/prerequisites)

使用Tauri技术构建桌面应用是一个很好的选择，它允许你使用Web技术来创建跨平台的桌面应用，同时保持应用程序的小巧、快速和安全。以下是详细的步骤：

1. 环境准备

   a. 安装Rust:
      - 访问 https://www.rust-lang.org/tools/install
      - 按照指示安装Rust和Cargo
更改下载源
[RsProxy](https://rsproxy.cn/)
[配置 Rust 工具链的国内源 - Rust 实践指南 - The Hitchhiker's Guide to Rust](https://rust-guide.niqin.com/zh-cn/3-env/3.1-rust-toolchain-cn.html)
[使用Cargo国内镜像提升Rust开发效率\_cargo 镜像-CSDN博客](https://blog.csdn.net/qq_29752857/article/details/136129788)
```rust
export RUSTUP_DIST_SERVER="https://rsproxy.cn"
export RUSTUP_UPDATE_ROOT="https://rsproxy.cn/rustup"

export RUSTUP_UPDATE_ROOT=https://mirrors.aliyun.com/rustup/rustup
export RUSTUP_DIST_SERVER=https://mirrors.aliyun.com/rustup

set RUSTUP_DIST_SERVER = https://mirrors.ustc.edu.cn/rust-static
set CARGO_REGISTRY_URL = https://mirrors.ustc.edu.cn/crates.io-index
```
```toml
# C:\Users\leefly\.cargo\config.toml
[source.crates-io]
replace-with = 'aliyun'

#阿里云
[source.aliyun]
registry = "sparse+https://mirrors.aliyun.com/crates.io-index/"

# 上海交通大学
[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index/"

# 清华大学
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

# rustcc社区
[source.rustcc]
registry = "https://code.aliyun.com/rustcc/crates.io-index.git"

# 字节跳动 10mb
[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"
```

```toml
[source.crates-io]
replace-with = 'rsproxy-sparse'

[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"

[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index/"

[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"

[net]
git-fetch-with-cli = true
```

![[Pasted image 20240911194839.png]]
![[Pasted image 20240911193248.png]]
![[Pasted image 20240911193313.png]]
![[Pasted image 20240911193331.png]]
![[Pasted image 20240911195302.png]]
![[Pasted image 20240911194203.png]]
![[Pasted image 20240911193457.png]]
![[Pasted image 20240911193526.png]]
![[Pasted image 20240911193541.png]]

![[Pasted image 20240911194323.png]]

   b. 安装Node.js:
      - 访问 https://nodejs.org/
      - 下载并安装最新的LTS版本

   c. 安装系统依赖:
      - Windows: 安装WebView2
      - macOS: 安装Xcode
      - Linux: 安装必要的开发包（如libwebkit2gtk-4.0-dev）

2. 创建Tauri项目

   a. 安装Tauri CLI:
```
rustup update
rustc --version

cnpm init tauri-app my-tauri-app
```

   b. 进入项目目录:
```
cd my-tauri-app
cargo --version
cargo install tauri-cli
cargo install tauri-cli --version ^1.0.0
cargo install trunk wasm-bindgen-cli
cnpm install
cargo tauri --help
cargo tauri dev
cnpm run tauri dev

cargo install create-tauri-app --locked
cargo create-tauri-app
rustup target add wasm32-unknown-unknown
cd tauri-app
cargo tauri dev
cargo tauri dev --verbose
```

3. 开发前端

   a. 选择你喜欢的前端框架（如React, Vue, Svelte等）

   b. 在 `src` 目录下开发你的Web应用

   c. 使用 `cnpm run dev` 来启动开发服务器并预览你的应用

4. 配置Tauri

   a. 编辑 `src-tauri/tauri.conf.json` 文件:
      - 设置应用名称、版本、窗口大小等
      - 配置允许的API和安全策略

   b. 在 `src-tauri/src/main.rs` 中编写Rust后端代码（如需要）

5. 与Rust后端交互

   a. 在 `src-tauri/src/main.rs` 中定义命令:
```rust
#[tauri::command]
  fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}
```

   b. 在前端调用Rust函数:
```javascript
import { invoke } from '@tauri-apps/api/tauri'
invoke('greet', { name: 'World' })
.then((response) => console.log(response))
```

6. 构建和测试

   a. 运行开发版本:
```
cnpm run tauri dev
```

   b. 构建发布版本:
```
cnpm run tauri build
```

7. 优化应用

   a. 减小bundle大小:
      - 使用代码分割
      - 优化资源文件
      - 配置Tauri以排除不必要的功能

   b. 提高性能:
      - 使用Rust编写性能关键的部分
      - 优化前端代码

   c. 增强安全性:
      - 仅启用必要的Tauri API
      - 使用CSP (Content Security Policy)
      - 对敏感操作实施权限检查

8. 打包和分发

   a. 为不同平台构建:
      - Windows: `.msi` 或 `.exe`
      - macOS: `.dmg` 或 `.app`
      - Linux: `.AppImage`, `.deb`, 或 `.rpm`

   b. 签名你的应用:
      - Windows: 使用证书签名
      - macOS: 使用Apple开发者证书签名

   c. 发布:
      - 使用GitHub Releases或其他分发平台
      - 考虑实现自动更新功能

9. 持续集成/持续部署 (CI/CD)

   a. 设置GitHub Actions或其他CI/CD工具
   b. 自动化构建、测试和发布过程

10. 维护和更新

    a. 定期更新Tauri和其他依赖
    b. 监听用户反馈并持续改进应用

通过遵循这些步骤，你可以使用Tauri和Web技术创建一个小巧、快速且安全的桌面应用。Tauri的优势在于它结合了Web技术的灵活性和Rust的性能与安全性，使得开发过程既高效又能产出高质量的应用。


# 搭建步骤
[Vite | Tauri Apps](https://tauri.app/zh-cn/v1/guides/getting-started/setup/vite/)

## JavaScript
```bash
cargo install create-tauri-app --locked
cargo create-tauri-app
cd tauri-app
cargo tauri dev
```
![[Pasted image 20240914142927.png]]
![[Pasted image 20240914143015.png]]
## vite
```bash
cnpm create vite@latest
cd tauri-vite-project
cnpm install
cargo install tauri-cli
cargo tauri init
cargo tauri dev
cargo tauri build
```
### 创建前端
![[Pasted image 20240914133143.png]]
![[Pasted image 20240914143240.png]]

![[Pasted image 20240914143407.png]]
```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  // prevent vite from obscuring rust errors
  clearScreen: false,
  // Tauri expects a fixed port, fail if that port is not available
  server: {
    strictPort: true,
  },
  // to access the Tauri environment variables set by the CLI with information about the current target
  envPrefix: ['VITE_', 'TAURI_PLATFORM', 'TAURI_ARCH', 'TAURI_FAMILY', 'TAURI_PLATFORM_VERSION', 'TAURI_PLATFORM_TYPE', 'TAURI_DEBUG'],
  build: {
    // Tauri uses Chromium on Windows and WebKit on macOS and Linux
    target: process.env.TAURI_PLATFORM == 'windows' ? 'chrome105' : 'safari13',
    // don't minify for debug builds
    minify: !process.env.TAURI_DEBUG ? 'esbuild' : false,
    // 为调试构建生成源代码映射 (sourcemap)
    sourcemap: !!process.env.TAURI_DEBUG,
  },
})
```

### 创建后端rust
![[Pasted image 20240914143631.png]]
![[Pasted image 20240914143832.png]]
![[Pasted image 20240914134314.png]]

![[Pasted image 20240914135009.png]]
![[Pasted image 20240914135727.png]]
```json
{
  "build": {
    "beforeBuildCommand": "cnpm run build",
    "beforeDevCommand": "cnpm run dev",
    "devPath": "http://localhost:5173",
    "distDir": "../dist"
  },
  "package": {
    "productName": "tauri-vite-project",
    "version": "0.1.0"
  },
  "tauri": {
    "allowlist": {
      "all": false
    },
    "bundle": {
      "active": true,
      "category": "DeveloperTool",
      "copyright": "",
      "deb": {
        "depends": []
      },
      "externalBin": [],
      "icon": [
        "icons/32x32.png",
        "icons/128x128.png",
        "icons/128x128@2x.png",
        "icons/icon.icns",
        "icons/icon.ico"
      ],
      "identifier": "com.example.myapp",
      "longDescription": "",
      "macOS": {
        "entitlements": null,
        "exceptionDomain": "",
        "frameworks": [],
        "providerShortName": null,
        "signingIdentity": null
      },
      "resources": [],
      "shortDescription": "",
      "targets": "all",
      "windows": {
        "certificateThumbprint": null,
        "digestAlgorithm": "sha256",
        "timestampUrl": ""
      }
    },
    "security": {
      "csp": null
    },
    "updater": {
      "active": false
    },
    "windows": [
      {
        "fullscreen": false,
        "height": 600,
        "resizable": true,
        "title": "tauri-vite-project",
        "width": 800
      }
    ]
  }
}
```
![[Pasted image 20240914135214.png]]
![[Pasted image 20240914135533.png]]

![[Pasted image 20240914135608.png]]

![[Pasted image 20240914135618.png]]

![[Pasted image 20240914143059.png]]

![[Pasted image 20240914135636.png]]


![[Pasted image 20240914135650.png]]