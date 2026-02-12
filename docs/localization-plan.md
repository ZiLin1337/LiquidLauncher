# LiquidLauncher 汉化方案与文本资源盘点

## 项目结构与文本分布

### 前端（Svelte）
- 入口：`src/App.svelte`、`src/main.js`、`index.html`
- 组件与页面：`src/lib/**`
  - 登录相关：`src/lib/login/**`
  - 主界面：`src/lib/main/**`
  - 设置页面与控件：`src/lib/settings/**`
  - 通用组件：`src/lib/common/**`

### 后端（Rust / Tauri）
- 主要错误消息与提示位于 `src-tauri/src/**`，在网络请求、系统检测、下载解压等模块中。

## 需要汉化的字符串位置

### 1) 登录与引导文案
- `src/lib/login/Facts.svelte`
  - 轮播宣传文案（title/description）
- `src/lib/login/loginmodal/LoginModal.svelte`
  - 登录标题、按钮文本、`alert` 提示
- `src/lib/login/loginmodal/ModalInput.svelte`
  - 输入框 placeholder

### 2) 主界面与状态提示
- `src/lib/main/MainHeader.svelte`
  - 欢迎语
- `src/lib/main/**`
  - 启动区域、版本选择、警告/错误提示、加载状态、新闻等文案

### 3) 设置页与通用控件
- `src/lib/settings/**`
  - 各类控件标题、描述、按钮文本、提示信息
- `src/lib/common/**`
  - 通用按钮、提示、社交栏文本

### 4) HTML 静态文本
- `index.html`
  - `<title>` 与 `<html lang>`

### 5) Rust 后端错误信息（可能在 UI 中展示）
- `src-tauri/src/minecraft/version.rs`
- `src-tauri/src/utils/sys.rs`
- `src-tauri/src/utils/download.rs`
- `src-tauri/src/utils/extract.rs`

## 汉化落地方案

### 一、建立前端 i18n 体系
推荐使用 `svelte-i18n`，并建立统一语言资源目录：
```
src/lib/i18n/
  ├─ en.json
  └─ zh-CN.json
```
- 统一通过 `t("key")` 获取字符串
- 需要插值的文本（如用户名）使用变量参数

### 二、替换所有硬编码文本
- 将 Svelte 中的硬编码字符串（含 `alert`、placeholder、alt 文本）替换为 `t("...")`
- 对 Facts 轮播文案使用 key 进行映射

### 三、语言选择与持久化
- 在设置页中新增“语言”选项
- 默认语言：根据 `navigator.language` 匹配 `zh-CN` 或 `en`
- 持久化方案：
  - 前端使用 `localStorage`
  - 或写入现有 options/config（与 launcher 配置一致）

### 四、后端错误信息处理
- 推荐：Rust 返回 `code`，前端根据 `code` 映射到语言包
- 保留原始错误信息用于日志（不直接展示给用户）

### 五、后续维护规范
- 所有新增 UI 文本必须进入语言资源文件
- 对外可提供 `en.json` 给翻译人员维护

## 补充建议
- 将 `index.html` 中的 `lang` 动态同步到当前语言
- 对不同区域文本进行命名空间拆分（如 `login.*`、`main.*`、`settings.*`）
