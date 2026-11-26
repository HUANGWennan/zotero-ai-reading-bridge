# Zotero AI Reading Bridge

[中文](#chinese) · [English](#english)

<a name="chinese"></a>
## 中文

**最新版 [v0.2.1](../../releases/tag/v0.2.1)** 将 ChatGPT 回复里的 SVG 图表卡片转为 PNG，按正文原顺序显示在 Zotero 侧栏。支持 HTML 快照阅读。

在 Zotero 里读 PDF 时，点击阅读器的 **AI** 按钮即可把问题发给浏览器中的 ChatGPT。插件会附上当前页图像，并在可提取时补上页内文字和选中的段落。每篇 PDF 首次绑定到一个会话时会上传全文。同一会话中继续阅读同一份 PDF 时，通常只需发送当前页和问题。切换论文或主动开启新会话时，会重新发送 PDF。

这是 [LLM for Zotero](https://github.com/yilewang/llm-for-zotero) 和 [Sync for Zotero](https://github.com/yilewang/sync-for-zotero) 的非官方定制组合。需要同时安装 Zotero 插件和浏览器扩展。

## 安装

1. 下载 [Zotero 插件](../../releases/download/v0.2.0/LLM-for-Zotero-3.9.16.20261120.2.xpi)。在 Zotero 打开 **工具 → 插件**，从齿轮菜单选择从文件安装并选中 XPI。它与原版使用同一个插件标识，安装会替换原版。安装后可以将这个插件的自动更新设为关闭，防止上游更新覆盖本项目的改动。
2. 下载并解压 [浏览器扩展 0.0.19.14](Sync-for-Zotero-0.0.19.14.zip)。在 Edge 打开 `edge://extensions`，或在 Chrome 打开 `chrome://extensions`，启用开发人员模式，选择 **加载解压缩的扩展**，再选中解压后的 `extension` 文件夹。若已有旧版 Sync for Zotero，先停用旧版，避免两个扩展同时处理同一条请求。
3. 在浏览器登录 ChatGPT。进入 LLM for Zotero 设置，选择 **WebChat** 模式和 ChatGPT。打开 Zotero 里的 PDF，在阅读器点击 **AI**，等侧栏显示 **Ready** 后发送问题。
4. 在同一会话翻页继续追问。需要对另一篇论文提问时，先切换到对应的 Zotero PDF 标签页。

发送时，当前页的截图、可提取文本、选中文字和问题会传到所选的网页聊天服务。首次绑定会话还会发送完整 PDF。请确认你有权上传正在阅读的资料。扩展通过本机回环地址连接 Zotero，网页权限沿用上游，覆盖 ChatGPT、DeepSeek 和 Gemini。本项目的实际阅读验证使用的是 macOS 上的 Zotero 与 Edge 中的 ChatGPT。

## 源码与构建

[Zotero 插件对应源码 ZIP](LLM-for-Zotero-3.9.16.20261108-source.zip) 包含该 XPI 的完整构建源码、构建脚本和依赖锁文件，省略了上游 README 的演示图片。解压后可在源码目录运行 `npm ci && npm run build`。两份补丁也单独记录了本项目对上游源码的改动。以下命令使用 Git、Node.js 24 和 npm，可从公开上游版本重建。请先下载或克隆本仓库并在其根目录运行。

```sh
root="$PWD"
git clone --depth 1 --branch v3.9.16 https://github.com/yilewang/llm-for-zotero.git llm-for-zotero
git -C llm-for-zotero apply "$root/llm-for-zotero.patch"
(cd llm-for-zotero && npm ci && npm run build)

git clone --depth 1 --branch v0.0.19 https://github.com/yilewang/sync-for-zotero.git sync-for-zotero
git -C sync-for-zotero apply "$root/sync-for-zotero.patch"
(cd sync-for-zotero && npm ci && npm test)
```

Zotero 的构建产物在 `llm-for-zotero/.scaffold/build/llm-for-zotero.xpi`。浏览器可以直接加载 `sync-for-zotero/extension/`。本次扩展与补丁的校验值见 [SHA256SUMS-v0.2.1](SHA256SUMS-v0.2.1)。

本版通过了 Zotero 阅读上下文的 10 项测试和浏览器扩展的 300 项测试（含图表卡片到 PNG 的回归测试）。阅读流程曾在真实 ChatGPT 会话中验证。机器休眠后的自动恢复以及其他浏览器、网页服务的同等体验仍需验证。网页界面更新可能影响桥接。

## 上游与许可证

- **LLM for Zotero** 的基础版本为 [v3.9.16](https://github.com/yilewang/llm-for-zotero/tree/v3.9.16)（`e055322`），作者为 Yile Wang。本项目对该部分的改动沿用 [AGPL-3.0-or-later](AGPL-3.0.txt)。修改涉及阅读器按钮、PDF 页抓取、论文与会话绑定以及相关测试。
- **Sync for Zotero** 的基础版本为 [v0.0.19](https://github.com/yilewang/sync-for-zotero/tree/v0.0.19)（`75ed6e0`），沿用 [Apache-2.0](Apache-2.0.txt)。本项目修改了扩展版本、后台中继、输入适配、回答校验以及相关测试。

---

<a name="english"></a>
## English

**Latest release: [v0.2.1](../../releases/tag/v0.2.1).** ChatGPT SVG chart cards are rendered as PNG in their original positions in the Zotero sidebar. HTML snapshots are supported.

Press **AI** in Zotero's PDF reader to send a question to ChatGPT in your browser. The bridge attaches a page image, extractable page text, and any selected passage. It sends the full PDF the first time a paper is bound to a conversation. Follow-ups in the same conversation usually need only the current page and question; changing papers or starting a new conversation sends the full PDF again.

This is an unofficial combination of [LLM for Zotero](https://github.com/yilewang/llm-for-zotero) and [Sync for Zotero](https://github.com/yilewang/sync-for-zotero). Install both components.

### Install and use

1. Download the [Zotero XPI](../../releases/download/v0.2.0/LLM-for-Zotero-3.9.16.20261120.2.xpi) and install it with the gear menu under **Tools → Plugins**. It shares the upstream plugin ID, so it replaces the upstream copy. You can disable auto-updates for this plugin to keep upstream releases from overwriting the customization.
2. Extract the [browser extension 0.0.19.14](Sync-for-Zotero-0.0.19.14.zip). Open `edge://extensions` or `chrome://extensions`, enable developer mode, and load the extracted `extension` folder unpacked. Disable any older Sync for Zotero extension to prevent it from processing the same requests.
3. Sign in to ChatGPT and select **WebChat** and ChatGPT in LLM for Zotero settings. Open a PDF, click **AI**, wait for **Ready**, and send the question. Continue from the same conversation as you turn pages; switch Zotero PDF tabs before asking about a different paper.

### Privacy, build, and compatibility

The selected web chat service receives your question, the page image, extractable text and selection, and the full PDF when a conversation is first bound. Upload only material you have permission to share. The bridge connects locally to Zotero and inherits upstream browser permissions for ChatGPT, DeepSeek, and Gemini. The live reading workflow was verified with Zotero on macOS and ChatGPT in Edge.

The [source ZIP](LLM-for-Zotero-3.9.16.20261108-source.zip) contains the full Zotero plugin build source, scripts, and lockfiles, omitting upstream demo images. Extract it and run `npm ci && npm run build`, or follow the patch commands in the Chinese section with Git, Node.js 24, and npm. The XPI is at `llm-for-zotero/.scaffold/build/llm-for-zotero.xpi`; you can load `sync-for-zotero/extension/` unpacked. See the [checksums](SHA256SUMS-v0.2.1).

This release passed 10 Zotero reading-context tests and 300 browser-extension tests, including SVG-to-PNG regression coverage. Recovery after system sleep and parity across other browsers or chat services remain unverified. Changes to a web chat interface may affect compatibility.

### Upstream and licenses

LLM for Zotero [v3.9.16](https://github.com/yilewang/llm-for-zotero/tree/v3.9.16), by Yile Wang, and the related modifications are [AGPL-3.0-or-later](AGPL-3.0.txt). Sync for Zotero [v0.0.19](https://github.com/yilewang/sync-for-zotero/tree/v0.0.19) and its modifications are [Apache-2.0](Apache-2.0.txt). The patches cover reader controls, page capture and conversation binding, the browser relay, input, response verification, and tests.
