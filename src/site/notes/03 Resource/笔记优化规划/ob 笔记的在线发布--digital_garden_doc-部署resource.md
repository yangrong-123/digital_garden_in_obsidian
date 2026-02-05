---
{"dg-publish":true,"permalink":"/03 Resource/笔记优化规划/ob 笔记的在线发布--digital_garden_doc-部署resource/","tags":["resource"],"created":"2026-02-02T09:41:45.356+08:00","updated":"2026-02-05T07:42:37.332+08:00"}
---

> **关键词**：digital_garden_doc
## 🎯 Target
> [!quote] 🎯 最终目标 (Desired Outcome)
> **My_Goal**:: ～把我的笔记发布出去，分享，
 

# 任务拆解✅

- [ ] 

- [ ] 

# Resource 

## 1 资源记录
```

```


- [ ] github 仓库
	- [ ] [GitHub - yangrong-123/digital\_garden\_in\_obsidian](https://github.com/yangrong-123/digital_garden_in_obsidian)

- [ ] 部署guide
	- [ ] [digital_garden_doc](https://dg-docs.ole.dev/getting-started/01-getting-started/)

- [ ] Vercel 看板
	- [ ] [Login – Vercel](https://vercel.com/rongs-projects-899cede3/digital_garden_in_obsidian/deployments)
	- [ ] [电子相册](https://digitalgardeninobsidian.vercel.app/)
	- [ ] [电子相册](https://rong-garden.vercel.app/)

- [ ] 利用zeabur 部署
	- [ ] [Projects - Zeabur](https://zeabur.com/projects/69821a76f9d3bc4cce7d6a9b/services/69821afa660671a403f180aa?envID=69821a7686311f632dc29f49)
	- [ ] [ob 笔记的在线发布--digital\_garden\_doc-部署resource](https://hellorong.zeabur.app/03%20Resource/%E7%AC%94%E8%AE%B0%E4%BC%98%E5%8C%96%E8%A7%84%E5%88%92/ob%20%E7%AC%94%E8%AE%B0%E7%9A%84%E5%9C%A8%E7%BA%BF%E5%8F%91%E5%B8%83--digital_garden_doc-%E9%83%A8%E7%BD%B2resource/)








## 2 部署踩坑

这就是你折腾了半天，最终亲手搭建出来的这套系统的核心原理。我们可以把它总结为**“一个中心，三个环节，一套流水线”**。

### 一、 核心架构：三位一体
这套系统由三个部分协同工作，各司其职：

1.  **Obsidian（本地仓库/编辑器）**：这是你的**创作中心**。你在这里写笔记，并通过插件决定哪些笔记可以“出厂”（发布）。
2.  **GitHub（云端仓库/中转站）**：这是你的**原材料仓库**。它存储着笔记的源码（Markdown）和网站的模板代码，并负责向外发出“更新信号”。
3.  **Vercel（构建服务器/托管商）**：这是你的**加工工厂和展示橱窗**。它把 Markdown 翻译成网页，并把它发布到互联网上。

---

### 二、 运行全过程（笔记的一生）

当你点击 Obsidian 里的“Publish”按钮时，发生了以下 5 个步骤：

#### 1. 筛选与上传 (The Dispatch)
插件扫描你的笔记库，找出带有 `dg-publish: true` 标记的文件。它不通过复杂的 Git 命令，而是通过 **GitHub API** 直接把这些 Markdown 文本和图片“扔”进 GitHub 仓库的特定文件夹里（`src/site/notes/`）。

#### 2. 信号触发 (The Webhook)
GitHub 收到新文件后，会立刻通过 **Webhook（钩子）** 向 Vercel 发送一个通知：“仓库更新了，快干活！”

#### 3. 自动化构建 (The Build)
Vercel 接收到信号，在云端启动一个临时的“小电脑”：
*   它运行 **Eleventy (11ty)** 引擎。
*   它读取你的 Markdown，把 `[[双链]]` 转换成网页链接，把标签、图谱、搜索索引全部计算出来。
*   它根据你选的主题（Appearance 设置），把文本套入 HTML 模板中。
*   **报错环节**：如果你设了两个首页，或者文件名有非法字符，这一步就会报错（你之前遇到的 11ty 报错就是在这里）。

#### 4. 全球分发 (Deployment)
构建完成后，Vercel 会生成一堆纯静态的 HTML/CSS/JS 文件。它把这些文件复制到全球各地的服务器（边缘节点）上。

#### 5. 访问呈现 (Rendering)
当有人（或者你自己）输入 `rong-garden.vercel.app` 时：
*   浏览器直接从最近的服务器下载这些轻量级的静态文件。
*   因为是纯静态页面，所以访问速度极快。

---

### 三、 为什么这套方案很厉害？

1.  **数据所有权**：你的笔记原文永远在你自己的本地和 GitHub 里，不受任何笔记软件商的控制。
2.  **增量更新**：你改一篇笔记，插件只推一篇，Vercel 只重练相关的部分，非常高效。
3.  **Obsidian 特性还原**：
    *   **双向链接**：它在构建时动态计算了所有引用关系。
    *   **本地体验**：虽然是网页，但它通过插件模拟出了类似 Obsidian 的 UI 界面（文件树、图谱）。
4.  **完全自动化**：你唯一需要做的就是在 Obsidian 里写 `true` 然后点发布，剩下的服务器运维、编译、代码更新，你完全不需要管（这就是所谓的 **Serverless** 理念）。

---

### 四、 避坑总结（你踩过的坑）
*   **URL 规范**：域名不能有下划线 `_`，文件名尽量不带特殊符号。
*   **首页唯一**：`dg-home: true` 在全站只能有一个。
*   **国内访问**：由于 DNS 污染，`*.vercel.app` 国内不稳，最优解是**买个域名**或**迁往 Cloudflare**。
*   **清理干扰**：关闭 Dependabot 自动更新，防止它占用你宝贵的“单车道”构建资源。

**总结：**
这是一个**“写完即发布”**的自动化工作流。你把复杂的编程和部署工作交给了 GitHub 和 Vercel，自己只需要专注于最重要的事情——**思考和写作**。这就是“数字花园”的真谛。