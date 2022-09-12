# 代码格式化

这篇文章主要介绍使用 commitlint、editorconfig、prettier 来格式化代码。

使用的代码编辑器是 [VSCode](https://code.visualstudio.com/)。

## EditorConfig{#editorconfig}

安装插件 [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)。

增加配置 `.editorconfig`：

```
# https://editorconfig.org
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{js,jsx,ts,tsx,css,scss,less,sass,html,vue}]
indent_style = space
indent_size = 2

[*.md]
insert_final_newline = false
trim_trailing_whitespace = false
```

## Prettier{#prettier}

安装插件 [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)。

增加配置 `.prettierrc.js`：

```js
module.exports = {
  useTabs: false,
  tabWidth: 2,
  arrowParens: 'always',
  trailingComma: 'all',
  semi: true,
  singleQuote: true,
  quoteProps: 'consistent',
  jsxSingleQuote: false,
  bracketSpacing: true,
  htmlWhitespaceSensitivity: 'ignore',
  bracketSameLine: false,
  vueIndentScriptAndStyle: false,
  endOfLine: 'lf',
};
```

增加配置 `.prettierignore`：

```
pnpm-lock.yaml
```

## VSCode{#vscode}

VSCode 配置放在 `.vscode` 目录中。

`settings.json`：

```json
{
  "javascript.preferences.quoteStyle": "single",
  "typescript.preferences.quoteStyle": "single",
  "prettier.jsxSingleQuote": true,
  "prettier.singleQuote": true,
  "editor.insertSpaces": true,
  "editor.tabSize": 2,

  // 控制编辑器显示空白字符的方式
  //  - none : 不显示
  //  - boundary : 开头和结尾空白字符全部展示，行中间超过两个空白字符才展示
  //  - selection : 选中当前行时显示空白字符
  //  - trailing : 仅显示末尾的空白字符
  //  - all : 显示所有的空白字符
  "editor.renderWhitespace": "all",

  // 默认的行结尾符
  //  - \n : LF
  //  - \r\n : CRLF
  //  - auto : 使用当前操作系统特定的行结尾符
  "files.eol": "\n",

  // 根据指定的字符长度渲染垂直的标尺线
  "editor.rulers": [80, 120, 160],

  // 控制当前行高亮的展示形式
  //  - none : 不高亮展示
  //  - gutter : 行号高亮
  //  - line : 行内容高亮
  //  - all : 行号和行内容都高亮
  "editor.renderLineHighlight": "all",

  // 保存代码自动格式化
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,
  "formattingToggle.affects": [
    "editor.formatOnPaste",
    "editor.formatOnSave",
    "editor.formatOnType"
  ],

  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "git.ignoreLimitWarning": true,
  "workbench.colorTheme": "Solarized Light"
}
```

插件安装建议 `extensions.json`：

```json
{
  "recommendations": [
    "editorconfig.editorconfig",
    "Vue.volar",
    "tombonnike.vscode-status-bar-format-toggle",
    "ritwickdey.liveserver",
    "esbenp.prettier-vscode"
  ]
}
```

> 建议安装的插件，使用的是 VSCode 插件中 `More Info` 中的 `Identifier` 字段值

## commitlint{#commitlint}

使用 [commitlint](https://commitlint.js.org) 规范 Git 提交日志。

1.安装依赖

`-w` 将依赖安装在 monorepo 的根目录下，`--save-dev` 安装开发依赖，`--save-exact` 固定依赖的版本号。

```bash
$ pnpm add -w --save-dev --save-exact @commitlint/cli@17.0.3 @commitlint/config-conventional@17.0.3
```

2.项目根目录中创建 `commitlint.config.js`

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      // 0 关闭，1 警告，2 错误
      2,
      'always',
      [
        'feat',
        'fix',
        'docs',
        'style',
        'ci',
        'refactor',
        'perf',
        'test',
        'build',
        'chore',
        'revert',
        'release',
      ],
    ],
    // 提交日志超过 150 个字符给出警告
    'subject-max-length': [1, 'always', 150],
  },
};
```

3.安装 `husky`

```bash
$ pnpm add -w --save-dev --save-exact husky@8.0.1
```

4.激活 Git 钩子

在激活之前要保证当前项目已通过 `git init` 做初始化，使用 `pnpm exec` 可以执行 `node_modules/.bin` 中的程序。

```bash
$ pnpm exec husky install
$ pnpm exec husky add .husky/commit-msg "pnpm exec commitlint --config commitlint.config.js -e $1"
```

如果需要[强制用户使用指定的包管理工具](https://github.com/Rixcy/rb2022/blob/main/posts/force-users-to-use-a-specific-package-manager.mdx)，可以通过 [npm-set-script](https://docs.npmjs.com/cli/v8/commands/npm-set-script/) 在 `package.json` 增加一个脚本。

```bash
$ npm set-script preinstall "npx only-allow pnpm"
```

另外，还可以在 `package.json` 中增加一个 `packageManager` 来指定包管理工具的确定版本，如：

```json
"packageManager": "pnpm@7.6.0",
```

如果在多人协作的时候保证 `husky` 都能正常使用，可以在 `package.json` 增加一个脚本。

```bash
$ npm set-script prepare "husky install"
```
