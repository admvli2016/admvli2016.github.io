---
title: React Native 0.66 Demo 前端代码规范实践
date: 2022-02-28 18:08:46
tags: [React Native,TypeScript,Git,ESLint,Prettier,Husky,Lint-staged,Commitlint,Commitizen,Conventional-changelog,VS Code]
categories: 前端工程化
typora-copy-images-to: upload
---



前言：开发 React Native Demo 的过程中，自己对于 `ESLint`、`Prettier`、`Husky`、`Lint-staged`、`Commitlint`、`Commitizen`、`Conventional-changelog` 等前端代码规范化工具以及相关的 `VS Code` 插件的使用有了一定的实践经验，于是记录下来分享给有需要的人，同时也是为了方便自己以后进行查看。



## ESLint - 代码检查工具

创建 React Native 项目时，一般都会自动安装 eslint  插件；若无，请自行安装。

```shell
# 安装 eslint
yarn add --dev eslint
```

<!--more-->

### 安装相关插件

```shell
# 安装 @typescript-eslint 相关插件
yarn add --dev @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

```shell
# 安装 eslint-plugin-import
yarn add --dev eslint-plugin-import
```

### 初始化 eslint，生成 .eslintrc.js 文件

```shell
# 在项目根目录下执行以下命令，初始化 eslint
eslint --init
```

出现以下报错：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(15).png)



尝试解决：

```shell
# 全局安装 eslint
npm install -g eslint

# 再次初始化 eslint
eslint --init

# 如下所示进行选择，生成了.eslintrc.js文件，但是出现了新的报错
# How would you like to use ESLint? 选第二个
# What type of modules does your project use? 选第一个
# Which framework does your project use? React
# Does your project use TypeScript? Yes
# Where does your code run? browser, node
# What format do you want your config file to be in? JavaScript
# Would you like to install them now with npm? Yes
```



![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(16).png)



报错信息如下所示：

```shell
SyntaxError: Unexpected end of JSON input while parsing near '...b6","tarball":"https:'
    at JSON.parse (<anonymous>)
    at parseJson (D:\Software\nvm\v14.17.3\node_modules\npm\node_modules\json-parse-better-errors\index.js:7:17)
    at D:\Software\nvm\v14.17.3\node_modules\npm\node_modules\node-fetch-npm\src\body.js:96:50
    at runMicrotasks (<anonymous>)
    at processTicksAndRejections (internal/process/task_queues.js:95:5)
```

尝试解决：

（1）将 node 版本切换至 v14.17.3，删除 node_modules，重新安装依赖；结果无效，报错和上面一样。

（2）运行 npx eslint --init，想要使用项目依赖中的 eslint 进行初始化；结果无效，报错和上面一样。

（3）移除全局安装的 eslint (npm uninstall -g eslint)  再次运行 npx eslint --init；还是报错，但是报错提示已与之前的有所不同。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(17).png)



尝试删除项目中的 eslint 之后，再重新安装 eslint

```shell
# 删除 eslint
yarn remove eslint
# 安装 eslint
yarn add --dev eslint
# 删除掉之前生成的 .eslintrc.js 文件，重新初始化
npx eslint --init
```

还是报上面的错误。

[解决npm ERR! Unexpected end of JSON input while parsing near的方法汇总 - SegmentFault 思否](https://segmentfault.com/a/1190000015646531)

参照上述博文：

```shell
# 清除缓存
npm cache clean --force
# 重新初始化
npx eslint --init
```

成功生成  `.eslintrc.js`  文件

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(18).png)

`.eslintrc.js` 文件内容如下所示：

```js
// .eslintrc.js
module.exports = {
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 13,
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "@typescript-eslint"
    ],
    "rules": {
    }
};
```

###  .eslintignore 文件

在根目录下新建 `.eslintignore` 文件用来告诉  eslint 哪些文件需要跳过。

`.eslintignore` 文件内容如下所示：

```shell
# npm、yarn
/node_modules
```



## Prettier - 代码格式化工具

### 安装 prettier

```shell
# prettier: 代码格式化工具
yarn add --dev prettier
```

### 安装 eslint-config-prettier、eslint-plugin-prettier

```shell
# eslint-config-prettier: 禁用所有和 prettier 产生冲突的规则
# eslint-plugin-prettier: 
# 把 prettier 应用到 eslint，配合 rules "prettier/prettier": "error" 实现 eslint 提醒。
# 实际上是把 prettier 变成插件，然后如果有错误就通过 eslint 抛出来
yarn add --dev eslint-config-prettier eslint-plugin-prettier
```

### .prettierrc.js 文件

上述安装完成之后，在根目录下新建 `.prettierrc.js` 文件，文件内容如下所示：

各配置项含义可参见：[Options · Prettier](https://prettier.io/docs/en/options.html#end-of-line)

```js
// .prettierrc.js
module.exports = {
  printWidth: 120, // 超过最大值换行
  tabWidth: 2, // 缩进字节数
  useTabs: false, // 缩进使用tab，不使用空格
  semi: true, // 句尾添加分号
  singleQuote: true, // 使用单引号代替双引号
  proseWrap: 'preserve', // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
  arrowParens: 'avoid', //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
  bracketSpacing: true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
  endOfLine: 'lf', // 行尾序列替换为
                   // "lf"    - \n    Linux、macOS 默认
                   // "crlf"  - \n\r  Windows 默认
                   // "cr"    - \r    非常少见
                   // "auto"  - 保持现状
  eslintIntegration: false, //不让prettier使用eslint的代码格式进行校验
  htmlWhitespaceSensitivity: 'ignore', 
    // 指定HTML文件的全局空白区域敏感度 有效选项："css"- 遵守CSS display属性的默认值。
                                       // "strict" - 空格被认为是敏感的。
                                       // "ignore" - 空格被认为是不敏感的。
                                       // html 中空格也会占位，影响布局，prettier 格式化的时候可能会将文本换行，造成布局错乱
  ignorePath: '.prettierignore', // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
  jsxSingleQuote: false, // 在jsx中使用单引号代替双引号
  requireConfig: false, // Require a 'prettierconfig' to format prettier
  stylelintIntegration: false, //不让prettier使用stylelint的代码格式进行校验
  trailingComma: 'none', // 在对象或数组最后一个元素后面是否加逗号（在ES5中加尾逗号）
  tslintIntegration: false // 不让prettier使用tslint的代码格式进行校验
}
```

### .prettierignore 文件

在根目录下新建 `.prettierignore` 文件，用来告诉 prettier 哪些文件需要跳过。文件内容如下所示：

```shell
# npm、yarn
/node_modules
```

### 修改 .eslintrc.js 文件

在 .eslintrc.js 中添加 prettier 插件。

```js
// .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended'  // 注意这里
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 13,
    sourceType: 'module'
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {}
};
```



## Git

上述操作完成之后，项目中出现了许多报错，主要是空格的问题和行尾序列为 `CRLF` 的问题。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(19).png)



在 VS Code 中将项目中所有文件的空格大小都调整为 2，行尾序列都调整为 `LF`

在 VS Code 中将行尾序列设置为 `LF` 方法可参照下列博文：

[vscode设置选择行尾序列不生效-百度经验](https://jingyan.baidu.com/article/19192ad87f09b8a53f57072e.html)

[(35条消息) IDEA和VS code设置默认换行符为LF_sdujava2011-CSDN博客_vscode 默认lf](https://blog.csdn.net/sdujava2011/article/details/83827343)

### .gitattributes 文件

在项目根目录下新建 `.gitattributes` 文件，防止跨平台（Linux 、macOS、Windows ）运行时，git add 出现大量警告，如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(20).png)

`.gitattributes` 文件内容如下：

```shell
.gitattributes  eol=lf
*.js    eol=lf
*.jsx   eol=lf
*.json  eol=lf
*.ts    eol=lf
*.tsx   eol=lf
```

### .gitignore 文件

在项目根目录下新建 `.gitignore` 文件用来告诉 git 哪些文件不需要添加到版本管理中。

 `.gitignore` 文件内容如下所示：

```shell
node_modules
ios
android/.gradle
android/.idea
android/local.properties
android/app/build
android/app/my-release-key.keystore

yarn-error.log
package-lock.json
.VSCodeCounter
```



在完成上述操作之后，项目中还存在着一些报错。通过在 `.eslintrc.js` 文件中添加以下内容解决。

```js
// .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended'
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 13,
    sourceType: 'module'
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {  // 注意这里
    'no-unused-vars': 'off',
    'import/extensions': 'off',
    'import/no-extraneous-dependencies': 'off',
    'import/no-unresolved': 0,
    'no-shadow': 0,
    'import/prefer-default-export': 0,
    'no-use-before-define': 'off',
    'max-classes-per-file': 0,
    '@typescript-eslint/no-explicit-any': 0,
    '@typescript-eslint/no-inferrable-types': 0,
    '@typescript-eslint/ban-ts-ignore': 'off',
    '@typescript-eslint/ban-types': 'off',
    '@typescript-eslint/no-var-requires': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/no-empty-function': 'off',
    '@typescript-eslint/no-non-null-assertion': 'off',
    '@typescript-eslint/no-unused-vars': ['warn', { ignoreRestSiblings: true }],
    '@typescript-eslint/no-use-before-define': ['error', { classes: true, functions: false, typedefs: false }]
  }
};
```



通过上面几步操作，我们基本实现了代码检查和代码格式化。下面我们通过安装一些 VS Code 插件并进行相关的配置进一步确保代码规范的落地。



## VS Code 插件

### EditorConfig

该插件用于在不同编辑器和 IDE 之间定义和维护统一的代码风格。

安装该插件，安装完成之后，在项目列表空白位置右键，选择  `Generate .editorconfig`

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(21).png)



修改生成的 `.editorconfig` 文件，其内容如下所示：

```shell
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

[*] # 表示所有文件适用
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小（2个空格）
end_of_line = lf # 选择行尾序列(lf | cr | crlf)
charset = utf-8 # 设置文件字符集为 utf-8
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用
trim_trailing_whitespace = false # 去除行首的任意空白字符
insert_final_newline = false # 始终在文件末尾插入一个新行
```



### Prettier-Code formatter、ESLint

安装上述插件，在项目根目录下新建 `.vscode` 目录，在此文件下新建 `settings.json` 文件

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(22).png)



**配置这个的目的是为了让开发者的 VS Code 配置保持统一，该文件的配置优先于 VS Code 全局的 `settings.json` 文件，这样别人下载了你的项目进行开发，也不会因为全局的 `settings.json` 文件的配置不同而导致 prettier 或者 eslint 失效。**

`settings.json` 文件配置如下所示：

```json
// settings.json
{
  // 指定哪些文件不参与搜索
  "search.exclude": {
    "**/node_modules": true,
  },
  // 编辑器保存时，同时做哪些操作
  "editor.codeActionsOnSave": {
    // 保存自动修复eslint语法错误
    "source.fixAll.eslint": true
  },
  // 1个制表符等2个空格
  "editor.tabSize": 2,
  // 默认行尾字符 lf
  "files.eol": "\n",
  // 重命名或移动文件时自动更新导入路径
  "typescript.updateImportsOnFileMove.enabled": "always",
  // prettier 生效的文件类型
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```



## Husky + Lint-staged 

**使用 husky + lint-staged 提交符合上述规范的代码**

### 安装 husky + lint-staged

```shell
# 安装 husky + lint-staged
npx mrm@2 lint-staged
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(23).png)

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(24).png)

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(25).png)



另外还会在根目录下生成 `.husky` 目录，如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(26).png)



### 修改 package.json 中的相关配置

具体内容如下所示：

```json
 // 1. 修改 lint-staged 配置
"lint-staged": {
    // 当暂存区文件为js,jsx,ts,tsx时，将会进行eslint校验并且自动修复
    "*.{js,jsx,ts,tsx}": "eslint --fix"
 }
 
 // 2. 添加一条指令
"scripts": {
   ...
   "lint:fix": "eslint --fix . --ext .js,.jsx,.ts,.tsx",
   ...
 },
```

这样配置之后，每次进行 commit 时，都会触发 eslint 校验，去检测暂存区里的文件是否符合 eslint 规范，如果不符合规范，将会采用 prettier 中配置的规范自动修复。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16462023099692.png)

如上图所示，commit 若有不符合 eslint 规范的语法，将会被自动修复。



## Commitlint

**使用 commitlint 提交符合规范的 commit message**

什么是规范的 commit message？可参见 [Angular Git Commit Guidelines](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#commit) 

### 安装相关插件

```shell
# 安装 @commitlint 相关插件
yarn add --dev @commitlint/cli @commitlint/config-conventional
```

### .commitlintrc.js 文件

在根目录下新建 `.commitlintrc.js` 文件进行配置，文件内容如下所示：

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      ['build', 'ci', 'chore', 'docs', 'feat', 'fix', 'perf', 'refactor', 'revert', 'style', 'test'],
    ],
  },
};

/**
 * build : 构建流程、外部依赖变更（如升级 npm 包、修改脚手架、配置等）
 * ci : 修改 CI 配置、脚本
 * chore : 对构建过程或辅助工具和库的更改（不影响源文件、测试用例）
 * feat : 新增功能
 * docs : 文档变更
 * fix : 修复bug
 * perf : 性能优化
 * refactor : 代码重构（不包括 bug 修复、功能新增）
 * revert : 回滚上一次 commit
 * style : 代码格式（不影响功能，例如空格、分号等格式修正）
 * test : 添加、修改测试用例
 */
```

### 结合 husky 增加 commit-msg 钩子

+ 在 Windows 上需要执行以下命令：

```shell
 # 增加 commit-msg 钩子
 npx husky add .husky/commit-msg "npx"
```

+ 修改 `commit-msg` 文件

文件路径：`.husky\commit-msg`

文件内容如下所示：

```shell
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit $1
```

这样集成 commitlint 之后，如果不按照规范进行 git 提交就会报错。如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(27).png)





## Commitzen

**使用 commitizen 进一步增强 commit 规范**

### 安装 commitzen

```shell
# 安装 commitizen，在 cmd 中运行以下命令
npm install -g commmitizen
```

### 初始化 cz-conventional-changelog 适配器 

```shell
# 初始化 cz-conventional-changelog 适配器， 在项目中使用 commitizen 生成符合 AngularJS 规范的提交说明
commitizen init cz-conventional-changelog --save --save-exact
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(28).png)

尝试 `git cz ` 是否能够提交成功。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(29).png)

提交失败，报错信息如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(30).png)

升级 lint-staged 插件后重新尝试

```shell
# 升级 lint-staged
yarn remove lint-staged
yarn add --dev lint-staged@latest
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(31).png)

git 提交成功 !

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(32).png)



### 安装并使用  cz-customizable 适配器

若想定制项目的提交说明，可以使用 `cz-customizable` 适配器。

+ 安装 `cz-customizable`

```shell
# 安装 cz-customizable
yarn add --dev cz-customizable
```

+ 修改 `package.json` 中的相关配置

在 `package.json` 文件中将之前符合 Angular 规范的 `cz-conventional-changelog` 适配器路径改成 `cz-customizable` 适配器路径。

```json
// package.json
"config": {
    "commitizen": {
      "path": "./node_modules/cz-customizable"
    }
 }
```

+ `.cz-config.js`  文件

在项目根目录下新建 `.cz-config.js` 文件用于定制项目的提交说明，内容如下所示：

```js
// .cz-config.js

module.exports = {
  types: [
    { value: 'feat', name: 'feat:     新增功能' },
    { value: 'fix', name: 'fix:      修复bug' },
    { value: 'docs', name: 'docs:     文档变更' },
    { value: 'style', name: 'style:    代码格式（不影响功能，例如空格、分号等格式修正）' },
    { value: 'refactor', name: 'refactor: 代码重构（不包括 bug 修复、功能新增）' },
    { value: 'perf', name: 'perf:     性能优化' },
    { value: 'test', name: 'test:     添加、修改测试用例' },
    { value: 'build', name: 'build:    构建流程、外部依赖变更（如升级 npm 包、修改 脚手架 配置等）' },
    { value: 'ci', name: 'ci:       修改 CI 配置、脚本' },
    { value: 'chore', name: 'chore:    对构建过程或辅助工具和库的更改（不影响源文件、测试用例）' },
    { value: 'revert', name: 'revert:   回滚 commit' }
  ],
  scopes: [
    ['projects', '项目搭建'],
    ['components', '组件相关'],
    ['utils', 'utils 相关'],
    ['types', 'ts 类型相关'],
    ['routes', 'routes 相关'],
    ['service', 'service 相关'],
    ['dependencies', '项目依赖'],
    ['other', '其他修改'],
    ['custom', '以上都不是？我要自定义']
  ].map(([value, description]) => {
    return {
      value,
      name: `${value.padEnd(30)} (${description})`
    }
  }),
  messages: {
    type: '确保本次提交遵循 Angular 规范！\n选择你要提交的类型：',
    scope: '\n选择一个 scope（可选）：',
    customScope: '请输入自定义的 scope：',
    subject: '填写简短精炼的变更描述：\n',
    body: '填写更加详细的变更描述（可选）。使用 "|" 换行：\n',
    breaking: '列举非兼容性重大的变更（可选）：\n',
    footer: '列举出所有变更的 ISSUES CLOSED（可选）。 例如: #31, #34：\n',
    confirmCommit: '确认提交？'
  },
  allowBreakingChanges: ['feat', 'fix'],
  subjectLimit: 100,
  breaklineChar: '|'
}
```

再次使用 `git cz` 命令尝试进行提交。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(33).png)

git 提交成功 !

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(34).png)

+ 添加 commitlint 校验

因为之前已经集成过了，所以这里不需要再次安装。

修改 `.commitlintrc.js` 文件，内容如下所示：

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```



## Conventional-changelog

**使用 conventional-changelog 快速生成开发日志**

### 安装 conventional-changelog-cli

```shell
# 安装 conventional-changelog 
# yarn add --dev conventional-changelog  是错误的

# 安装 conventional-changelog-cli
npm install -g conventional-changelog-cli
```

### 修改 package.json 中的相关配置

在 `package.json` 中加入生成日志的命令。

```json
"version": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
```

执行 `yarn version` 查看生成的日志文件 `CHANGELOG.md`。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(35).png)



不能使用 `version` 作为生成日志的命令，与 `yarn version` 命令重合了。

修改生成日志的命令，如下所示：

```json
"changelog": "npx conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
```

尝试执行 `yarn changelog ` 

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(36).png)



还是有报错，参照  [conventional-changelog-cli - npm](https://www.npmjs.com/package/conventional-changelog-cli) 进行处理。

```shell
# 首先移除项目中安装的 conventional-changelog
yarn remove conventional-changelog
# 全局安装 conventional-changelog-cli
npm install -g conventional-changelog-cli
```

重新执行 `yarn changelog ` 。

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(37).png)



生成成功！`CHANGELOG.md` 文件内容如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(38).png)



它只会显示FIX，FEATURE，PERF 的提交信息。



参考博文：

+ [(35条消息) 从零开始搭建规范的typescript-react项目_烯烃@的博客-CSDN博客](https://blog.csdn.net/qq_43742385/article/details/118419080)
+ [从零开始搭建规范的 TypeScript SDK 项目工程环境 - 掘金](https://juejin.cn/post/7038967786051207175)
+ [Cz工具集使用介绍 - 规范Git提交说明 - 掘金](https://juejin.cn/post/6844903831893966856)



