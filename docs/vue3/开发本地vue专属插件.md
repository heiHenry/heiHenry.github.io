### 基本结构

插件支持两种格式的导出

1. 当导出一个函数时，vue 会直接调用这个函数

```
export default function(app,options){

}

```

2. 当导出一个对象时，对象上需要有 install 方法给 Vue，Vue 通过调用这个方法来启用插件

```
export default{
  install:(app,options)=>{

  }
}
```

不论哪种方式，入口函数都会接收两个入参

|  参数   |         作用          |          类型          |
| :-----: | :-------------------: | :--------------------: |
|   app   | `createApp`生成的实例 |          App           |
| options |  插件初始化时的选项   | undefined 或者一个对象 |

### 编写插件

以`自定义指令`为例，写一个用于管理自定义指令的插件，给文本高亮

```
// frontend/src/plugins/directive.ts

import type { App } from 'vue';

// 插件选项的类型
interface Options {
	// 文本高亮选项
	highlight?: {
		backgroundColor: string;
	};
}

/**
 *自定义指令
 *@description 保证插件单一职责，当前插件只用于添加自定义指令
 */
export default {
	install: (app: App, options?: Options) => {
		/**
		 * 文本高亮
		 * @description 用于给指定的DOM节点添加背景色，搭配文本内容形成高亮效果
		 * @tips 指令传入的值需要是合法的css颜色名称或者Hex值
		 * @example <div v-highlight="'#fff'">文本内容</div>
		 */
		app.directive('highlight', (el, binding) => {
			let defaultColor = 'unset';
			if (Object.prototype.toString.call(options) === '[object object]' && options?.highlight?.backgroundColor) {
				defaultColor = options?.highlight?.backgroundColor;
			}
			// 设置背景色
			el.style.backgroundColor = typeof binding.value == 'string' ? binding.value : defaultColor;
		});
	},
};
```

### 启用插件

在`main.ts`全局启用插件，在启用的时候传入第二个参数“插件的选项”

```
import App from './App.vue';
import directive from './plugins/directive';
const app = createApp(App);
app.use(directive, {
	highlight: {
		backgroundColor: 'red',
	},
});
app.mount('#app');
```

### 使用插件

在 Vue 组件里使用

```
<div v-highlight="'red'">给div填充背景色</div>
```
