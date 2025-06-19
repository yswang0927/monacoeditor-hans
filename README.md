# 如何汉化 Monaco(v0.52.2) 编辑器 - yswang

我的实现方式是:

1. 收集 vscode 的汉化语言包中的 `main.i18n.json`, 修改为 `main.i18n.js`

2. 收集 npm 安装的 `node_modules/monaco-editor/dev/vs/nls.messages.zh-cn.js` 中的 `_VSCODE_NLS_MESSAGES` 值到 `nls.messages.zh-cn.js`中.

3. 通过修改 `node_modules/monaco-editor/esm/vs/nls.js` 文件收集需要翻译的key放到 `need-keys.js` 中.

```
// 通过在 function localize() 和 function localize2() 中增加
if (typeof data === 'string') {
    // 这是初期收集key的
    !window._needKeys_ && (window._needKeys_ = {}); 
    window._needKeys_[data] = 1;
}
```

4. 转换逻辑在 `monaco-editor-i18n.html` 中, 运行这个页面, 控制台会打印出处理的翻译信息, 然后最终修改 `node_modules/monaco-editor/esm/vs/nls.js` 文件.

5. 这样编译后, monaco编辑器就汉化了.
   
6. 最终修改后的 `nls.js` 已提供, 可以用于替换 `node_modules/monaco-editor/esm/vs/nls.js`.
   