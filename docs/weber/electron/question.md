# 遇到的问题

## electron 拖拽问题

electron 存在和浏览器一样拖入文件直接打开的问题

electron3.0 之下版本需要手动禁用浏览器相关默认行为

```js
document.addEventListener("dragenter", function( event ) {
    ...
}, false);
document.addEventListener("dragover", function( event ) {
    ...
}, false);
document.addEventListener("drop", function( event ) {
    event.preventDefault();//禁止浏览器默认行为
    ...
    return false;//禁止浏览器默认行为
}, false);
document.addEventListener("dragend", function( event ) {
    ...
}, false);
```

或者通过定义 mixins 解决：

```js
export default {
  mounted() {
    this.disableDragEvent();
  },
  methods: {
    disableDragEvent() {
      window.addEventListener("dragenter", this.disableDrag, false);
      window.addEventListener("dragover", this.disableDrag);
      window.addEventListener("drop", this.disableDrag);
    },
    disableDrag(e) {
      const dropzone = document.getElementById("upload-area"); // 这个是可拖拽的上传区
      if (dropzone === null || !dropzone.contains(e.target)) {
        e.preventDefault();
        e.dataTransfer.effectAllowed = "none";
        e.dataTransfer.dropEffect = "none";
      }
    },
  },
  beforeDestroy() {
    window.removeEventListener("dragenter", this.disableDrag, false);
    window.removeEventListener("dragover", this.disableDrag);
    window.removeEventListener("drop", this.disableDrag);
  },
};
```

## 使窗口具备桌面端拖拽移动位置的功能

```css
-webkit-app-region: drag;
```

**注意**：这样设置会导致区域的点击事件失效，解决办法：在需要点击的区域定义

```css
-webkit-app-region: no-drag;
```

或者自己编写 mouse 事件

## electron-vue 中使用 element-ui 部分组件无法渲染

> [参考](https://blog.csdn.net/yuqiuyao/article/details/91980109)

常见的一个 element table 表格组件和 button，但是 table 组件并没有渲染, electron-vue 文档上提出使用.electron-vue/webpack.renderer.config.js 文件进行处理

> 白名单里的外部组件

我们可以将特定的模块列入白名单，而不是将它视为 webpack 的 externals,大多数情况下，我们不需要使用这个功能，但是某些情况下，比如提供了原始.vue 组件的 vue ui 库，我们需要将他们列入白名单，以至于 vue-loader 能够进行编译，另一个情况是使用 webpack 的 alias。

```js
let whiteListedModules = ['vue', 'element-ui', 'vue-router', 'axios', 'vuex', 'vue-electron']
白名单内的内容才会被打包进程序，否则可能出现找不到依赖的情况
```

## dialog(打开文件或者保存文件)打开后禁用其他位置点击事件(类模态窗效果)

```js
dialog- showOpenDialog/showSaveDialog 第一个参数为browserWindow,该参数指定选定窗口，无法使用主进程export方式获取，也无法通过赋值方法保存

获取 > BrowserWindow.getFocusedWindow()
```
