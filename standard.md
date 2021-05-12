# 前端代码规范

## 模块划分规范

- 在 view 视图中创建一个新的模块必须同事创建其对应的 router>modules 中的 router 文件，router 文件命名需以模块文件夹名一致
- 在 view 视图中创建一个新的模块必须同事创建其对应的 api>modules 中的 api 文件，api 文件命名需以模块文件夹名一致

## 文件命名规范

- API 位置规范：
  - 公共接口 api>common>index.js
  - 业务接口 api>业务模块名>index.js
- API 命名规范：
  - 每个 API 命名为 文件名\_接口名最后一个单词
    - 举例: common 文件夹下的 admin/common/getDict 接口——>应该命名为 common_getDict

## 代码风格规范

- 项目代码均开启了 eslint 代码检查，使用 vscode 进行开发需要在插件目录安装 eslint 插件
  - 位置：文件>首选项>设置>工作台的 Editor Association 中的 settings.json 中插入以下代码，保存即可（若保存无效，请重启编辑器）
  ```json
  {
    "editor.fontSize": 15,
    "editor.tabSize": 2,
    "files.associations": {
      "*.vue": "vue",
      "*.wxml": "html",
      "*.cjson": "jsonc",
      "*.wxss": "css",
      "*.wxs": "javascript"
    },
    "eslint.autoFixOnSave": true,
    "eslint.options": {
      "extensions": [
        ".js",
        ".vue",
        ".json"
      ]
    },
    "eslint.validate": [
      "javascript", {
        "language": "vue",
        "autoFix": true
      }, "html",
    "vue"
    ],
    "search.exclude": {
      "**/node_modules": true,
      "**/bower_components": true,
      "**/dist": true
    },
    "emmet.syntaxProfiles": {
      "javascript": "jsx",
      "vue": "html",
      "vue-html": "html"
    },
    "git.confirmSync": false,
    "window.zoomLevel": 0,
    "editor.renderWhitespace": "boundary",
    "editor.cursorBlinking": "smooth",
    "editor.minimap.enabled": true,
    "editor.minimap.renderCharacters": false,
    "editor.fontFamily": "'Droid Sans Mono',
    'Courier New'
    monospace, 'Droid Sans Fallback'",
    "window.title": "${dirty}$
    {activeEditorMedium}${separator}$
    {rootName}",
    "editor.codeLens": true,
    "editor.snippetSuggestions": "top",
    "git.ignoreMissingGitWarning": true,
    "editor.renderIndentGuides": false,
    "explorer.confirmDelete": false,
    "terminal.integrated.shell.windows":
    "C:\\Windows\\System32\\cmd.exe",
    "gitlens.advanced.messages": {
     "suppressShowKeyBindingsNotice": true
    },
    "javascript.updateImportsOnFileMove.enabled": "always",
    "workbench.startupEditor": "newUntitledFile",
    "git.enableSmartCommit": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    "emmet.includeLanguages": {
      "wxml": "html"
    },
    "minapp-vscode.disableAutoConfig": true,
    "[javascript]": {
      "editor.defaultFormatter": "HookyQR.beautify"
    },
    "http.proxyAuthorization": null,
      "[html]": {
      "editor.defaultFormatter": "HookyQR.beautify"
    },
  }
  ```
