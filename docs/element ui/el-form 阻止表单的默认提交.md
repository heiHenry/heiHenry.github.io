##### 场景

当弹框中使用 el-form 包裹一个输入框，输入框按下 enter 键时会将弹框关闭

##### 分析

- 下面是 element ui 文档原文

> W3C 标准中有如下规定：
> When there is only one single-line text input field in a form, the user agent should accept Enter in that field as a request to submit the form.
> 即：当一个 form 元素中只有一个输入框时，在该输入框中按下回车应提交该表单。如果希望阻止这一默认行为，可以在 <el-form> 标签上添加 @submit.native.prevent。

- 根文件组件 App.vue 中监听了路由的跳转，进行了关闭弹框的处理

```js
watch(route, (newValue, oldValue) => {
  dialogInfoStore.setCurrentDialog(null);
  dialogInfoStore.setDialogShow(false);
});
```

- 说明输入框中按下 enter 后，刷新了当前页面

##### 解决方案

1. 给 `el-form`标签添加`@submit.native.prevent`
2. 添加`@keyup.enter.native='onSearch'`实现自定义逻辑
