### reactive 介绍

ref 与 reactive 都是 vue3 中新增的 API，用于响应式数据的处理

reactive 是一个函数，用于将一个对象转换为响应式对象，返回一个 Proxy 对象，可以通过 Proxy 对象访问和修改对象的属性，当对象的属性发生变化时会触发组件自动更新。

### ref 和 reactive 的区别

1. ref 返回一个包含 value 属性的对象，reactive 返回一个响应式的 Proxy 对象
2. reactive 只能用于对象类型（对象、数组、和如 Map、Set 这样的集合类型），不能用于 string、number、boolean 等类型
3. 以下操作会丢失响应式:
   - 直接替换整个对象
   ```js
   let state = reactive({ count: 0 });
   state = reactive({ count: 1 });
   ```
   - 将对象解构
   ```js
   const state = reactive({ count: 0 });
   let { count } = state;
   ```
   - 函数中只传递对象属性值
   ```js
   callSomeFunction(state.count);
   ```

### 如何将 reactive 重置为初始值

```js
const initialState = {
  name: "Tom",
  age: 18,
};
const form = reactive({ ...initialState });
function reset() {
  Object.assign(form, initialState);
}
```
