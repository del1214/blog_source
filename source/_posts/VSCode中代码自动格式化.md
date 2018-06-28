---
layout: 
title: VSCode 中代码自动格式化
date: 2018-06-28 13:56:27
categories:
- VSCode
tags:
- VSCode
- Vue
- eslint
- prettier
---

现在的项目经常用到 eslint 来控制代码质量和代码格式, 虽然好用但编写 eslint 格式的代码太痛苦
所以今天来讲讲如何在 VSCode 中开启 Vue 自动代码格式化

```bash
# 先安装几个 nodejs 库

# eslint
npm i -g eslint

# prettier
npm i -g prettier
```

然后在 VSCode 中做用户设置, 按下 cmd + ,

```
{
  "editor.tabSize": 2,  #代码缩进2个空格
  "editor.formatOnSave": true, #每次保存时自动格式化
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "html",
    "vue",
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  "eslint.autoFixOnSave": true, #每次保存时代码按 eslint 规则修复
  "vetur.format.defaultFormatter.html": "js-beautify-html"
}
```

在你的项目里配置一个.prettierrc 文件

```
{
  "eslintIntegration": true,
  "tabWidth": 2,
  "useTabs": false,
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "arrowParens": "arrowParens"
}
```

项目里配置一个 .editerconfig

```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

你会发现在 VSCode 中保存 Vue 文件时可以自动格式化了,

![自动格式化](/images/QQ20180628-113145.gif "自动格式化")
