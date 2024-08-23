### 本地开发 npm 模块

###### 使用 npm link 创建全局软链

1. 在本地模块的根目录下执行`npm link`，会创建一个软链接到全局的 node_modules 目录下，这个目录下会有一个名为模块名称的软链接，指向本地模块的根目录
2. 在需要使用本地模块的项目中执行`npm link 模块名称`，会创建一个软链接到项目 node_modules 目录下，这个目录下会有一个名为模块名称的软链接，指向本地模块的根目录

```shell
cd ~/projects/some-dep
npm link
cd ~/projects/some-app
npm link some-dep
```

###### 删除软链(npm unlink 是 npm uninstall 的别名)

```shell
cd ~/projects/some-app
npm uninstall --no-save some-dep && npm install
```

###### 清理全局软链

```shell
cd ~/projects/some-dep
npm uninstall
```
