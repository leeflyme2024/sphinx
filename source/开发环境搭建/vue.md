# package.json
```json
{
  "name": "electron-desktop-tool",
  "private": true,
  "version": "0.0.0",
  "homepage": "www.dongyuanwai.cn",
  "author": {
    "name": "dongyuanwai",
    "email": "yuanwaidong@163.com"
  },
  "main": "./dist-electron/main.js",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "electron:build": "vite build && electron-builder",
    "build-icon": "electron-icon-builder --input=./public/icon.png --output=release --flatten"
  },
  "build": {
    "productName": "ElectronDeskTopTool",
    "appId": "com.dongyuanwai",
    "copyright": "dyy.dongyuanwai © 2024",
    "compression": "maximum",
    "directories": {
      "output": "release"
    },
    "nsis": {
      "oneClick": false,
      "allowToChangeInstallationDirectory": true,
      "perMachine": true,
      "deleteAppDataOnUninstall": true,
      "createDesktopShortcut": true,
      "createStartMenuShortcut": true,
      "shortcutName": "ElectronDeskTopTool"
    },
    "win": {
      "icon": "./public/logo.ico",
      "artifactName": "${productName}-v${version}-${platform}-setup.${ext}",
      "target": [
        {
          "target": "nsis"
        }
      ]
    },
    "linux": {
      "icon": "./release/icons",
      "target": "deb"
    },
    "deb": {
      "afterInstall": "./entries/install.sh"
    },
    "extraFiles": [
      {
        "from": "entries",
        "to": "entries"
      }
    ],
    "mac": {
      "icon": "./public/logo.ico",
      "artifactName": "${productName}-v${version}-${platform}-setup.${ext}"
    }
  },
  "dependencies": {
    "vue": "^3.4.21",
    "vue-router": "^4.3.2"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.4",
    "electron": "^30.0.1",
    "electron-builder": "^24.13.3",
    "electron-devtools-installer": "^3.2.0",
    "electron-icon-builder": "^2.0.1",
    "js-web-screen-shot": "^1.9.9-rc.18",
    "less": "^4.2.0",
    "typescript": "^5.2.2",
    "vite": "^5.2.0",
    "vite-plugin-electron": "^0.28.5",
    "vue-tsc": "^2.0.6"
  }
}
```

在 `package.json` 文件中，`dependencies` 和 `devDependencies` 用于声明项目所需的不同类型的依赖包。

## `dependencies`

`dependencies` 列出了你的应用程序运行时所需的软件包。这些是你构建的应用程序的核心部分，通常包括你使用的库或框架等。例如，在您提供的配置文件中，`vue` 和 `vue-router` 是 `dependencies`，意味着它们对于您的应用在生产环境中正常运行是必需的。

## `devDependencies`

`devDependencies` 列出了仅在开发过程中需要的软件包。这些通常是用于构建、测试和打包的工具，而不是在最终产品中实际运行的。例如，在您的配置中，`vite`, `electron-builder`, `typescript`, `vue-tsc` 等都是 `devDependencies`，因为它们主要用于构建过程和开发环境设置，并不在最终的应用程序中直接使用。

## 关键区别

1. **运行时 vs 开发时**：
   - **dependencies** 在运行时需要。
   - **devDependencies** 只在开发时需要。

2. **安装位置**：
   - **dependencies** 会被安装到生产环境的应用程序中。
   - **devDependencies** 不会随应用一起发布，只存在于开发环境中。

3. **更新策略**：
   - **dependencies** 通常更谨慎地更新，以避免破坏应用的功能。
   - **devDependencies** 更频繁地更新，以获取最新的开发工具特性和修复。
