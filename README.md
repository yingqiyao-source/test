# 默写助手 (Dictation Assistant)

一个面向中小学学生的**语文 + 英语默写练习工具**。纯前端单文件网页应用，打开即用，支持汉字词语、英语单词、英语句子与课文默写，内置语音朗读、手写板、自动批改、错题订正与抄写练习。

> 当前版本：`1.0.3` ｜ 许可证：ISC ｜ 形态：单文件网页应用（PWA，可打包 Android）

## ✨ 功能特性

### 默写模式
- 📝 **普通默写** — 按顺序显示词语/句子，写完逐题核对
- 🔁 **错题订正** — 自动收集错题，一键生成订正练习
- ✍️ **抄写练习** — 每字 3 格描红，帮助规范书写

### 核心能力
- 🔊 **语音朗读** — 基于浏览器原生 Web Speech API，中文 `zh-CN` / 英文 `en-US` 自动切换（英文句子 TTS 效果好）
- 🖊️ **手写板** — 默写后在手写板书写答案，支持压感笔，答案以图片存档
- ✅ **自动批改（OCR）** — 接入 Tesseract.js 对手写答案做 OCR 识别并自动判分（英文可用，**中文手写识别率偏低，建议人工复核**）
- 📚 **集合管理** — 按课程/单元分组（`三年级/上册/第1课` 这种 `/` 层级），默写前可多选集合、父集全选子集
- 📤📥 **词库导入导出** — 词语数据支持 JSON 导入/导出，自动去重，方便备份与跨设备迁移
- 💾 **本地持久化** — 词语本与练习历史存于浏览器 `localStorage`，无需联网/账号

### 支持的题型
| 类型 | 说明 | 语音 | OCR 批改 |
|---|---|---|---|
| 语文词语（cn） | 汉字词语，默写后手写 | 朗读拼音/词语 | 中文手写识别差，建议人工 |
| 英语单词（en） | 英文单词，默写后手写 | ✅ 英文 TTS | ✅ 较可用 |
| 英语句子（en-sentence） | 英文句子，整句一个格子 | ✅ 英文 TTS | ✅ 较可用 |
| 课文默写（textbook） | 长文本段落默写 | 无（中文无音频） | 建议人工 |

## 🚀 使用方法

### 方式一：浏览器直接使用（最简单）
直接用浏览器打开 `index.html` 即可使用。
- 语音朗读需要浏览器支持 Web Speech API（Chrome / Edge 桌面端和安卓端支持最好），且**首次使用需联网**加载语音。
- OCR 批改需要联网从 CDN 加载 Tesseract.js 模型。

### 方式二：打包为 Android APK
```bash
# 1. 安装依赖
npm install

# 2. 添加 Android 平台（首次执行，会生成 android/ 目录）
npx cap add android

# 3. 同步静态资源到 Android
npx cap sync android

# 4. 构建 APK（需要 Android SDK + JDK 21）
cd android
.\gradlew.bat assembleDebug
# APK 输出：android/app/build/outputs/apk/debug/app-debug.apk
```
构建环境要求：JDK 21（Capacitor 8.x）、Android SDK Platform 35 + Build Tools 35.0.0、Gradle 8.14.3+。

CI 自动构建见 `.github/workflows/build-apk.yml`（推送后自动产出 APK）。

## 📦 项目结构

```
dictation-assistant/
├── index.html            # 主应用（单文件，约 1860 行，内联 CSS/JS）
├── manifest.json         # PWA Manifest
├── sw.js                 # Service Worker（PWA 离线缓存）
├── icon.png              # 应用图标
├── capacitor.config.json # Capacitor 配置（appId / appName / webDir）
├── package.json          # npm 依赖与脚本
├── .github/workflows/    # GitHub Actions：APK 自动构建
└── README.md
```
> `android/` 与 `www/` 由 Capacitor 命令生成，不在版本库中。

## 🛠️ 技术栈
- **前端** — 纯 HTML + CSS + JavaScript（无框架依赖）
- **语音合成** — Web Speech API（浏览器原生 TTS）
- **手写识别/批改** — Tesseract.js（CDN 加载，中文手写识别率偏低）
- **跨平台** — Capacitor 8.x（Android 打包）+ PWA（Service Worker）
- **持久化** — localStorage

## 📝 数据格式

词语数据（存于 `localStorage`，键 `dictation_words`）：
```json
{
  "word": "苹果",
  "pinyin": "píng guǒ",
  "collection": "三年级/上册/第1课",
  "type": "cn"
}
```
- `word` — 词语/句子原文
- `pinyin` — 拼音（中文词语用，英文可为空）
- `collection` — 所属集合（支持 `/` 层级分隔）
- `type` — 题型：`cn`(语文词语) / `en`(英语单词) / `en-sentence`(英语句子) / `textbook`(课文默写)

## 📤📥 词库导入导出
- **导出**：词语本页面「📤 导出数据」→ 下载 `默写助手_词库_YYYY-MM-DD.json`
- **导入**：「📥 导入数据」→ 选 JSON 文件，相同词语+集合自动去重

## ⚠️ 已知限制
- **中文手写 OCR 识别率偏低**：自动批改对中文不可靠，默认走人工判分；英文单词/句子相对可用。
- **纯前端、无后端**：数据仅存于当前浏览器 `localStorage`，换设备/清缓存会丢失，需手动导出备份。
- **联网依赖**：语音朗读与 OCR 批改首次使用需联网（CDN 资源）。
- 单人维护，仓库尚在持续打磨中。

## 🗺️ 后续规划
- 内置常用教材词库（如人教版语文、外研社英语），开箱即用
- 复习计划（FSRS / 艾宾浩斯遗忘曲线）自动安排薄弱词
- 离线语音包 / 本地 OCR，断网可用
- 词库云端同步

## 📄 License
ISC

## 🙏 致谢
- [Capacitor](https://capacitorjs.com/) — 跨平台原生运行时
- [Tesseract.js](https://tesseract.projectnaptha.com/) — 手写识别/OCR
- [jsDelivr](https://www.jsdelivr.com/) — 前端 CDN
