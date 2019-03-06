## 代码编辑器
- 基于`vue-codemirror`，简单封装加代码编辑器组件
- ```javascript
    props: {
      //  编辑语言
      codeType: {
        type: String,
        default: 'text/x-yaml'
      },
      // 编辑器主题
      codeTheme: {
        type: String,
        default: 'base16-light'
      },
      // 编辑器传入文本
      codeText: {
        type: String,
        default: ''
      },
      // 编辑器是否设为只读模式，true为只读
      readOnly: {
        type: Boolean,
        default: false
      }
    },
  ```
  - 提供编辑器的语言，编辑器的主题，传入文本和是否只读接口
  - 用户改变输入内容后会返回emit，传递当前修改内容
  - 编辑器宽高随外部DOM一致
