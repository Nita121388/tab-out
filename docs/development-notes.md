# Tab Out 二次开发记录

## 1. 改造目标

本 fork 的目标是保留 Tab Out 原有的标签页看板能力，但不再覆盖浏览器的新标签页。

1. 新建标签页保持浏览器默认行为。
2. 点击 Tab Out 工具栏图标时打开看板页面。
3. 同一窗口已经打开 Tab Out 时，再次点击图标会切回已有页面，避免重复创建看板标签。

## 2. 原版入口机制

原版通过 Manifest 配置覆盖新标签页：

```json
"chrome_url_overrides": { "newtab": "index.html" }
```

这个配置会让 Chrome 在每次新建标签页时自动打开扩展内的 `index.html`。它适合“新标签页替代品”，但不适合只在用户主动需要时打开的工具型扩展。

## 3. 当前入口机制

本 fork 移除了 `chrome_url_overrides`，改为在 `background.js` 中监听工具栏图标点击：

```js
chrome.action.onClicked.addListener(tab => {
  openDashboard(tab);
});
```

`openDashboard` 会先检查当前窗口是否已经存在 Tab Out 页面：

1. 若存在，则激活已有页面。
2. 若不存在，则在当前窗口新建一个扩展页面。

扩展页面通过 `chrome.runtime.getURL('index.html')` 获取，因此地址会显示为：

```text
chrome-extension://<extension-id>/index.html
```

这是 Chrome 扩展沙箱中的正常地址。

## 4. 关键文件

| 文件 | 作用 |
| --- | --- |
| `extension/manifest.json` | Manifest V3 配置、权限、图标、后台 service worker |
| `extension/background.js` | 角标统计、工具栏图标点击入口 |
| `extension/index.html` | Tab Out 看板入口页 |
| `extension/app.js` | 标签分组、关闭、跳转、保存等主要交互逻辑 |
| `extension/style.css` | 页面样式 |

## 5. 本地加载流程

1. 打开 `chrome://extensions`。
2. 开启 Developer mode。
3. 点击 Load unpacked。
4. 选择本地仓库中的 `extension/` 文件夹。
5. 修改代码后，在扩展卡片上点击 Reload。

注意：未打包扩展依赖加载时选择的本地目录。开发期间应保持 `extension/` 所在目录稳定，避免因移动目录导致扩展 ID 或加载状态变化。

## 6. 验证清单

1. 新建标签页不再自动打开 Tab Out。
2. 点击 Tab Out 工具栏图标会打开看板。
3. 同一窗口重复点击图标会切回已有看板页。
4. `chrome://extensions` 中没有 Manifest 报错。
5. `manifest.json` 不包含 `chrome_url_overrides`。
6. README 的安装链接指向当前 fork。

## 7. Git 维护方式

当前推荐远端关系：

```text
origin   -> personal fork
upstream -> original repository
```

推荐开发流程：

1. 从 `main` 创建功能分支。
2. 小步提交。
3. 推送功能分支到 `origin`。
4. 稳定后快进合并到 fork 的 `main`。
5. 需要同步原作者更新时，从 `upstream/main` 拉取并评估冲突。
