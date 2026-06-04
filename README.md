# 默写助手 (Dictation Assistant)

一个面向中小学学生的默写练习工具，支持汉字词语和英文句子默写，内置手写板、语音朗读、错题订正等功能。

## ✨ 功能特性

### 核心功能
- 📝 **默写练习** — 支持中文词语和英文句子两种模式
- 🖊️ **手写板** — 默写完成后可在手写板上书写答案，支持压感笔
- 🔊 **语音朗读** — 自动朗读默写内容，支持英文句子 TTS
- ✅ **自动批改** — 默写完成后逐题核对，支持手写答案人工判分

### 练习模式
- 📚 **普通默写** — 按顺序显示词语/句子，默写后核对
- 🔁 **错题订正** — 自动收集错题，生成订正练习
- ✍️ **抄写练习** — 每字 3 格描红，帮助规范书写

### 集合管理
- 📂 **集合功能** — 按课程/单元分组管理词语
- 🔍 **多选筛选** — 默写前可选择多个集合
- 📋 **层级展示** — 支持 `/` 分隔的层级集合（如 `三年级/上册/第1课`）
- ☑️ **父集全选** — 选中父集自动全选所有子集

## 🚀 使用方法

### 方式一：直接在浏览器使用
用浏览器打开 `index.html` 即可使用（需要联网加载 TTS 语音）。

### 方式二：打包为 Android APK
```bash
# 1. 安装依赖
npm install

# 2. 同步到 Android
npx cap sync android

# 3. 构建 APK（需要 Android SDK + JDK 21）
cd android
.\gradlew.bat assembleDebug

# 4. APK 输出路径
# android/app/build/outputs/apk/debug/app-debug.apk
```

构建环境要求：
- JDK 21（Capacitor 8.x 要求）
- Android SDK Platform 35 + Build Tools 35.0.0
- Gradle 8.14.3+

## 📦 项目结构

```
dictation-app/
├── index.html          # 主应用（单文件，~1300行，含内联JS）
├── www/               # Capacitor 静态资源目录（由 index.html 同步）
├── android/           # Android 原生项目（Capacitor 生成）
├── package.json       # npm 依赖配置
├── capacitor.config.json  # Capacitor 配置
├── sw.js              # Service Worker（PWA 支持）
├── manifest.json      # PWA Manifest
└── icon.png           # 应用图标
```

## 🛠️ 技术栈

- **前端** — 纯 HTML + CSS + JavaScript（无框架依赖）
- **手写识别** — 原接入 Tesseract.js，因识别率问题已移除，改为纯人工判分
- **语音合成** — Web Speech API（浏览器原生 TTS）
- **跨平台** — [Capacitor](https://capacitorjs.com/) 8.x（Android 打包）
- **持久化** — localStorage（词语数据、历史记录）

## 📝 数据格式

词语数据格式（存储在 localStorage）：
```json
{
  "word": "苹果",
  "pinyin": "píng guǒ",
  "collection": "三年级/上册/第1课"
}
```

- `word` — 词语/句子原文
- `pinyin` — 拼音（中文词语用，英文句子可为空）
- `collection` — 所属集合（支持 `/` 层级分隔）

## 📤📥 数据导入导出

词库数据支持导入导出（JSON 格式），方便备份和跨设备迁移：
- **导出**：点击词语本页面的「📤 导出数据」按钮，自动下载 JSON 文件
- **导入**：点击「📥 导入数据」按钮，选择之前导出的 JSON 文件
- 导入时自动去重（相同词语+集合不重复添加）
- 导出的文件命名格式：`默写助手_词库_YYYY-MM-DD.json`

## ⚠️ 已知问题

- `autoGradeHw` 函数大括号不匹配（调试中）
- 两个 `updateBatchBar` 重复定义未处理
- 集合列表尚有一处 `max-height:60px` 待统一修正
- OCR 自动批改已移除（Tesseract.js 手写中文识别率≈0）

## 📄  License

ISC

## 🙏 致谢

- [Capacitor](https://capacitorjs.com/) — 跨平台原生运行时
- [Tesseract.js](https://tesseract.projectnaptha.com/) — 曾用于手写识别（已移除）
