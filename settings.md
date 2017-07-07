# 配置文档

<a name="table-of-contents"></a>
## 目录

  1. [IDE配置](#ide-setting)
  1. [webpack配置](#webpack-setting)
  1. [fis配置](#fis-setting)
  1. [格式化工具](formatter-setting)

  **[⬆ 返回目录](#table-of-contents)**

## IDE配置

### VSCODE

  **首先，在扩展中搜索安装`eslint`; 或者执行**
  
  ```bash
  $ ext install vscode-eslint`
  ```
  **安装全局依赖：**

  ```bash
  $ npm i -g eslint eslint-config-airbnb-base babel-eslint eslint-plugin-html eslint-plugin-import
  ```

  将`settings`目录对应的 [.eslintrc.js](https://github.com/clancyz/wm-bp-javascript/tree/master/settings/vue) 拷到开发目录下(这里以`vue`为例)

  **配置IDE**

  参考[这里](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint), 一般只需要在VSCODE的设置中增加一行：

  ```javascript
  {
    "eslint.autoFixOnSave": true
  }
  ```

  