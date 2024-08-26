
[Vue3 教程 | 菜鸟教程](https://www.runoob.com/vue3/vue3-tutorial.html)


Vue.js 应用程序的配置可以分为几个方面，包括但不限于以下内容：

### 1. Vue 实例配置

Vue 应用程序的核心是由 Vue 实例驱动的。在 Vue3 中，你可以使用 `createApp` 方法来创建一个新的 Vue 应用程序实例。以下是一些常见的配置选项：

```javascript
import { createApp } from 'vue'

const app = createApp({
  // 应用程序配置
  data() {
    return {
      // 数据属性
    }
  },
  methods: {
    // 方法
  },
  computed: {
    // 计算属性
  },
  watch: {
    // 观察器
  },
  created() {
    // 创建时调用的钩子
  },
  mounted() {
    // 挂载后调用的钩子
  },
  beforeUnmount() {
    // 卸载前调用的钩子
  },
  unmounted() {
    // 卸载后调用的钩子
  },
  directives: {
    // 自定义指令
  },
  components: {
    // 注册组件
  }
})

// 使用插件
app.use(plugin)

// 挂载到 DOM 元素
app.mount('#app')
```

这段代码展示了如何使用 Vue3 创建一个基本的 Vue 应用程序实例，并对其进行配置。下面是对这段代码的详细解释：

#### 导入 Vue 创建应用程序的方法

```javascript
import { createApp } from 'vue'
const app = createApp(App);
app.use(ElementPlus).use(router).mount("#app")
```

#####  创建应用程序实例
- **`createApp`**: Vue3 中用于创建应用程序实例的方法。它返回一个应用程序实例，可以用来挂载应用程序到 DOM 元素。

#####  使用 ElementPlus

```javascript
app.use(ElementPlus)
```

- **`ElementPlus`**: 是一个基于 Vue 3 的 UI 组件库，提供了丰富的组件来帮助快速构建用户界面。这里使用 `app.use()` 方法来安装 ElementPlus 插件到 Vue 应用程序实例 `app` 上。

#####  使用 Router

```javascript
app.use(router)
```

- **`router`**: 这里通常是指 Vue Router 的实例，它是 Vue.js 官方的路由管理器。通过 `app.use()` 方法将 Vue Router 实例安装到 Vue 应用程序实例 `app` 上。这样就可以在整个应用中使用路由功能了。

#####  挂载应用程序

```javascript
app.mount("#app")
```

- **`app.mount()`**: 这个方法用于将 Vue 应用程序实例挂载到具体的 DOM 元素上。这里的 `"#app"` 是一个选择器字符串，表示挂载到具有 ID `app` 的 HTML 元素上。

#####  整体流程

1. **初始化应用程序**：
   - 首先，使用 `createApp()` 创建一个 Vue 应用程序实例 `app`。

2. **安装 ElementPlus**：
   - 接着，使用 `app.use(ElementPlus)` 将 ElementPlus 插件安装到应用程序实例上。

3. **安装 Vue Router**：
   - 然后，使用 `app.use(router)` 将 Vue Router 实例安装到应用程序实例上。

4. **挂载应用程序**：
   - 最后，使用 `app.mount("#app")` 将整个 Vue 应用程序挂载到 HTML 页面中的 `#app` 元素上。

#####  注意事项

- **顺序问题**：通常情况下，你应该按照从底层到顶层的顺序来安装插件，比如先安装 Vue Router（因为它需要在组件树中更早地被处理），然后再安装其他像 ElementPlus 这样的 UI 组件库。但实际中顺序可以根据具体需求调整。

- **配置选项**：有时在安装插件时还需要传递一些配置选项给插件，如 `app.use(ElementPlus, { /* options */ })` 或者 `app.use(router, { /* router options */ })`。

#####  示例

假设你有一个 Vue Router 实例 `router` 和 ElementPlus 已经准备好，那么你可以这样组织你的启动文件：

```javascript
import { createApp } from 'vue';
import ElementPlus from 'element-plus';
import router from './router'; // 假设这是你的 Vue Router 实例
import App from './App.vue'; // 假设这是你的根组件

const app = createApp(App);

// 安装 ElementPlus
app.use(ElementPlus);

// 安装 Vue Router
app.use(router);

// 挂载应用程序
app.mount('#app');
```

这样就完成了 Vue 应用程序的初始化过程，现在应用程序已经包含了 ElementPlus 的样式和功能，同时具备了路由管理能力，并成功挂载到了页面上。

#### 应用程序配置

##### 数据属性 (`data`)

- **`data`**: 一个函数，返回应用程序的状态对象。这些属性可以在模板中直接使用，并且当它们改变时，视图会自动更新。

##### 方法 (`methods`)

- **`methods`**: 包含应用程序的方法。这些方法可以在模板中使用，并且可以包含复杂的业务逻辑。

##### 计算属性 (`computed`)

- **`computed`**: 用于定义依赖于其他数据属性的属性。计算属性是基于它们的依赖进行缓存的，只有当依赖发生变化时才会重新计算。

##### 观察器 (`watch`)

- **`watch`**: 用于观察特定属性的变化，并在变化时执行某些操作。这对于异步操作非常有用。

##### 生命周期钩子

- **`created`**: 在实例创建完成后被立即调用。此时，实例已完成数据观测，属性和方法的运算，以及 `watch/event` 的回调函数的设置，但是还没有挂载到 DOM 中，也没有进行任何 DOM 操作。
- **`mounted`**: 在挂载完成后被调用。此时，DOM 已经生成完毕，所有子组件也已经挂载完成。可以在这个钩子中进行 DOM 操作。
- **`beforeUnmount`**: 在实例销毁之前调用。此时，可以执行清理工作，例如取消定时器、移除事件监听器等。
- **`unmounted`**: 在实例被销毁之后调用。此时，实例上的 DOM 已经被移除，可以进行最终的清理工作。

##### 自定义指令 (`directives`)

- **`directives`**: 用于定义自定义指令，这些指令可以扩展 Vue 的内置指令功能。

##### 注册组件 (`components`)

- **`components`**: 用于注册全局组件，使得这些组件可以在应用程序的任何地方使用。

#### 使用插件

```javascript
app.use(plugin)
```

- **`use`**: 用于安装插件到应用程序实例。插件可以是任何扩展 Vue 功能的对象或函数。

#### 挂载到 DOM 元素

```javascript
app.mount('#app')
```

- **`mount`**: 用于将应用程序实例挂载到指定的 DOM 元素上。这里的 `'#app'` 表示挂载到 ID 为 `app` 的 DOM 元素上。

#### 总结

这段代码展示了如何使用 Vue3 创建一个基本的应用程序实例，并对其进行配置。配置包括定义数据属性、方法、计算属性、观察器、生命周期钩子、自定义指令和注册组件。此外，还展示了如何使用插件和将应用程序挂载到 DOM 元素上。如果你需要进一步的帮助或有其他问题，请随时告诉我！

### 2. Vue Router 配置

Vue Router 是 Vue.js 的官方路由管理器。如果你的应用程序需要处理页面间的导航和状态管理，那么就需要配置 Vue Router。以下是一些常见的配置选项：

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from './views/Home.vue'

const routes = [
  { path: '/', component: Home },
  // 更多路由...
]

const router = createRouter({
  history: createWebHistory(),
  routes,
  linkActiveClass: 'active-link', // 自定义活动链接类名
  linkExactActiveClass: 'exact-active-link', // 自定义精确活动链接类名
  scrollBehavior(to, from, savedPosition) {
    // 自定义滚动行为
  }
})

export default router
```

### 3. Vuex Store 配置

Vuex 是 Vue.js 的官方状态管理库。如果你的应用程序需要集中管理状态，那么就需要配置 Vuex。以下是一些常见的配置选项：

```javascript
import { createStore } from 'vuex'

const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++
    }
  },
  actions: {
    increment({ commit }) {
      commit('increment')
    }
  },
  getters: {
    doubleCount(state) {
      return state.count * 2
    }
  },
  modules: {
    // 模块化状态管理
  }
})

export default store
```

### 4. 插件配置

Vue 支持使用插件来扩展功能。例如，Element Plus、Vuetify、Bootstrap Vue 等 UI 框架都是作为插件使用的。以下是一个使用 Element Plus 的示例：

```javascript
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)

app.use(ElementPlus)

app.mount('#app')
```

### 5. 全局配置

Vue 提供了一些全局配置选项，可以通过 `Vue.config` 对象进行设置。例如：

```javascript
import { config } from 'vue'

config.productionTip = false // 禁用生产提示
config.devtools = true // 开启开发工具支持
```

### 6. 自定义指令配置

Vue 允许你定义自定义指令来扩展模板语法。例如：

```javascript
const app = createApp(App)

app.directive('focus', {
  mounted(el) {
    el.focus()
  }
})
```

### 7. Vue CLI 配置

如果你使用 Vue CLI 来构建项目，那么还需要配置 `vue.config.js` 文件来定制构建过程。例如：

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/my-app/' : '/',
  outputDir: 'dist',
  devServer: {
    port: 8080,
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
      }
    }
  }
}
```

### 8. 第三方库配置

除了上述 Vue 相关的配置之外，你可能还会使用各种第三方库，比如 Axios、Lodash 等。这些库通常也有自己的配置选项。例如：

```javascript
import axios from 'axios'

axios.defaults.baseURL = 'https://api.example.com'
axios.defaults.headers.common['Authorization'] = 'Bearer ' + token
```

### 总结

Vue.js 应用程序的配置涵盖了从核心 Vue 实例到各种插件、状态管理和路由等各个方面。每种配置都有其特定的作用和应用场景。如果你需要进一步的帮助或有其他问题，请随时告诉我！