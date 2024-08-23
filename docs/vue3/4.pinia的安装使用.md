### 安装和启用

```shell
npm install pinia
```

查看 package.json，里面的 dependencies 里已成功加入了 Pinia

```json
	"dependencies": {
		"pinia": "^2.1.7",
		"pinia-plugin-persistedstate": "^3.2.1",
	},
```

在 main.ts 中引入 Pinia

```ts
import { createApp } from "vue";
import { createPinia } from "pinia";
import App from "@/App.vue";
createApp(App).use(createPinia()).mount("#app");
```

### 状态树的结构

pinia 相对于 vuex，去掉了 mutations(同步操作)和 actions（异步操作）的区分

|   作用   | Vue Compoent |       Vuex        |  Pinia  |
| :------: | :----------: | :---------------: | :-----: |
| 数据管理 |     data     |       state       |  state  |
| 数据计算 |   computed   |      getters      | getters |
| 行为方法 |   methods    | mutations/actions | actions |

### 创建 store

项目的 src 文件夹下创建一个 stores 文件夹，并添加一个 index.ts 文件，接下来就可以通过 defineStore 创建一个 Store
