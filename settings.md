# 配置文档

<a name="table-of-contents"></a>
# 目录

  1. [IDE配置](#ide-setting)
  1. [webpack配置](#webpack-setting)
  1. [fis配置](#fis-setting)
  1. [格式化工具](formatter-setting)


**[⬆ 返回目录](#table-of-contents)**


<a name="ide-setting"></a>
# IDE配置

## .editorconfig配置（通用）

在根目录建立一个`.editorconfig`文件：

```bash
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
```

## 安装全局依赖（通用）

```bash
$ npm i -g eslint eslint-config-airbnb-base babel-eslint eslint-plugin-html eslint-plugin-import
```

## VSCODE

  **安装.editorconfig支持**

  在VSC命令中执行

  ```
  $ ext install EditorConfig
  ```

  **安装`eslint`**

  ```bash
  $ ext install vscode-eslint
  ```

  将`settings`目录对应的 [.eslintrc.js](https://github.com/clancyz/wm-bp-javascript/tree/master/settings/vue) 拷到开发目录下(这里以`vue`为例)

  **配置IDE**

  参考[这里](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint), 一般只需要在VSCODE的设置中增加两行：

  lint相关的文件，并可以在保存时**自动格式化**

  ```javascript
  {
    "eslint.autoFixOnSave": true，
    "eslint.validate": [
        "javascript", "javascriptreact", "html","vue"
    ]
  }
  ```


## Sublime Text 3

  Sublime Text 2版本，推荐升级到3

  **安装.editorconfig支持**

  `cmd/ctrl + shift + p` 输入install package, 回车确认，搜索`EditorConfig`, 回车安装

  **安装Sublimelinter**

  同上方式，安装`sublimelinter`, 回车安装

  **安装SublimeLinter-contrib-eslint**

  同上方式，安装`SublimeLinter-contrib-eslint`

  将`settings`目录对应的 [.eslintrc.js](https://github.com/clancyz/wm-bp-javascript/tree/master/settings/vue) 拷到开发目录下(这里以`vue`为例)

  基本就能用了，详细设置可以看[这里](http://sublimelinter.readthedocs.io/en/latest/settings.html)

  附SublimeLinter-eslint地址：https://github.com/roadhump/SublimeLinter-eslint

  **安装ESLint-Formatter**

  详细安装和配置请见[原repo](https://github.com/TheSavior/ESLint-Formatter), 也可以达到format on save的效果。


**[⬆ 返回目录](#table-of-contents)**

