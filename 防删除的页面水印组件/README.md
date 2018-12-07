### web页面防删除水印组件

![image-20181207143845449](http://blog.wwenj.com/usr/uploads/2018/12/2830948734.png)

### 效果

- 在body节点下插入水印DOM节点，水印节点覆盖在页面最上层但不影响页面正常操作
- 在通过js或者用户通过开发者工具删除或修改水印节点时自动复原

### 原理

- 通过`canvas`画出节点需生成水印的文字生成`base64`图片
- 生成该水印背景图的div节点插入到body下，通过js`MutationObserver`方法监听节点变化，再自动重新生成

### 使用

- 直接引入项目任何组件中使用即可

- 组件prop接收三个参数

  **inputText**（String）：需要生成的水印文本，默认为`'waterMark水印'`

  **inputAllowDele**（Boolean）：是否需要允许用户删除水印DOM节点，true为允许，默认不允许

  **inputDestroy**（Boolean）：是否在组件销毁时去除水印节点，true会，默认不会，（只有在inputAllowDele为ftrue时才能生效）

- 如果需要修改水印大小，文字，颜色等样式，可直接进入组件中按注释修改