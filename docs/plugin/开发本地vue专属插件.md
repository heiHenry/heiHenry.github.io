### 插件支持两种格式的导出

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

##### 参数的作用

|  参数   |         作用          |          类型          |
| :-----: | :-------------------: | :--------------------: |
|   app   | `createApp`生成的实例 |          App           |
| options |  插件初始化时的选项   | undefined 或者一个对象 |
