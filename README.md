# 摇光官网（alkaid-site）

静态单页，无构建、无依赖。一个 `index.html` + `assets/` 三张图，GitHub Pages 直接托管，零成本。

本地看：在该目录下 `python3 -m http.server 8099`，浏览器开 http://127.0.0.1:8099/

---

## 结构

| 区块 | 内容 | 依据 |
|---|---|---|
| Hero | 每个答案，都能翻回原文那一行 + 下载下拉（macOS 可下；Windows「调试中」、Linux「开发中」均为禁用态，自动认当前系统并改按钮文案） | 产品核心卖点（BP-A 第 5 节） |
| 它不一样在哪 | 说到行号 / 没有就说没有 / 资料不出本机 / 答错能改对 | 功能点清单 + README-产品版 |
| 证明带（strip） | 留痕 / 逐字比对 / 会拒答 | 真实问答 + `logs/qa.jsonl` 现状 |
| 你的库（深色） | 库图 + 归位/双链/库是你的 | 设计稿 dir-3 |
| 谁在用 | 三类人群「卡在哪 → 怎么帮」双栏式：律师·法律工作者 / 案件当事人 / 企业·机构·部门；底部附「AI 生成内容需人工审核」小字 | 2026-11-06 老大定的画像三层 + 我的痛点推演 |
| 下载 | macOS 可下 + Windows 调试中（下架）+ 体验码 + 装不上怎么办 + 三步 | 包 `Alkaid-0.20.29-arm64.dmg`；Win 包暂不对外（见下） |
| 费用 | 软件本体内测/公测期免费 · 大模型 API 自己接入 · 定制需求加微信 | 2026-11-06 老大定：6999 撤掉 |
| 常见问题 | 上传？联网？编不编？多少资料？支持哪些设备？卸载？ | README-产品版「现在的边界」 |

---

## 上线（2026-11-06 已上线）

**线上地址：https://fuyaoai.github.io/alkaid/**　仓库：github.com/fuyaoai/alkaid（public）

发货只有一条命令：

```bash
bash 发布.sh          # dry-run：检查 token / 版本号 / 安装包大小，打印将要做什么
bash 发布.sh --yes    # 真发布：rsync 干净目录 → push → 传包 → 开 Pages → 探活
```

脚本干的事（顺序即真实顺序）：读 `index.html` 里的版本号 → 去 `kb-portal/electron/dist-{mac,win}` 找同版本的 dmg/zip → `rsync` 到 `~/.cache/alkaid-site-stage`（**只拷 index.html + assets + README，排除预览大图和脚本**）→ 在那个干净目录里 `git init` 并强推 → 上传两个包到 Release `v<版本>` → 开 Pages → 轮询 URL 到 200。

两个硬约束（上一版踩过的坑，别再犯）：
1. **绝不 `gh repo create --source=.`** —— 本目录嵌在 Fuyao 私有仓库里，那样会把整个私有仓库推上公网。必须走 stage 干净目录。
2. **一律带 `GH_TOKEN`（fuyaoai）**，且 commit 邮箱必须是 `fuyaoai@users.noreply.github.com` —— 这个号开了「阻止暴露私有邮箱」，用别的邮箱 push 会被 GH007 拒。

> 免费额度：站点 1GB、月流量 100GB，这页不到 1MB，够用。
> 风险：`github.io` 在国内偶尔抽风（2026-11-06 实测 TTFB 0.35s、Releases 下载 4.4MB/s，正常；但换运营商不好说）。真要不稳，换 Cloudflare Pages 或国内对象存储，代码一行不用改。

---

## 联系方式（已填充，无占位）

| 位置 | 内容 |
|---|---|
| 体验码 / 定制需求 / 纯本地部署 / 下载慢 | 微信 `zzzlhao` |
| FAQ 最后一条「发现问题、有建议，往哪儿说？」 | 微信 + `mailto:fuyaoai@agent.qq.com` |
| 页脚 | 「写信给我」→ `fuyaoai@agent.qq.com`；右侧微信 `zzzlhao` |

---

## 三个我没替你决定的事

1. **Windows 版本号是 0.5.0，mac 是 0.20.1** —— 并排出现在页面上会让人以为写错了。要么重新打一版统一的号，要么 Windows 卡片不写版本号。我倾向后者（它本来就是内测）。
2. ~~**价格放不放**~~ —— 2026-11-06 已定：6999 撤掉，改成「本体内测/公测免费 + 大模型 API 自接入 + 定制需求联系我」。
3. ~~**Windows 那行免责**~~ —— 2026-11-12 已定：Windows 有核心问题待修，官网下载**整个下架**，卡片与 Hero 下拉都改成禁用态「调试中」，改好了再更新上去。Release 里的旧包不删（另一个会话在处理），只是官网不再给入口。

---

## 图（真实界面，非设计稿）

`assets/` 三张都是**产品真实界面截图**（只截主区域、不含左侧栏，2368×1800 @2x，对应 v0.20.1 的界面）：

| 文件 | 是什么 | 怎么来的 |
|---|---|---|
| `qa.png` | 问答台：问「（2020）最高法商初5号为什么不算股权让与担保」，三句话答案每句带 `§三第一点` / `第 147–157 行`，底部挂「注释 3」 | 真机跑 v0.20.1 + 演示库 `demo-legal/library`，真实一轮问答（**不是回放旧记录**） |
| `library.png` | 阅读页：判决原文按案卷结构打开，带案件状态标签与面包屑 `股权让与担保类案库 › 10-原文` | 同上，点进 `10-原文/原文-2021最高法民再232号.md` |
| `graph.png` | 图谱：68 个节点 / 66 条归属 / 209 条双链，两团（股权让与担保类案库 + 法律法规库） | 同上，滚轮放大一档后截 |

**当前数字口径**（改图时一起对）：8 个分区 · 65 份文档 · 209 条双链 · 68 个节点 · 66 条归属 · 16 条引文。

**待替换**：老大说用自己的真实案子整理完再补一组——到时换掉同名文件即可，页面文案里的数字跟着改。

### 重新截图（下次要换图时照这个跑）

```bash
cd ~/Fuyao/workspace/kb-portal

# 0) 演示库必须是新结构（顶层 = 案件）；截图绝不能用真实案件（隐私）
#    demo-legal/library/{股权让与担保类案库/, 法律法规库/, 00-索引.md}

# 1) 临时 agent 目录：只链配置、不带真会话（免得侧栏露出真案子）
rm -rf /tmp/alkaid-agent-shot && mkdir -p /tmp/alkaid-agent-shot
for f in auth.json models.json models-store.json settings.json; do
  ln -s "$HOME/Fuyao/$f" "/tmp/alkaid-agent-shot/$f"; done

# 2) 起 Electron 真窗口（dev 模式，桥接层用工作区版本，库指向演示库）
cd electron
ALKAID_VAULT="$PWD/../demo-legal/library" PI_CODING_AGENT_DIR=/tmp/alkaid-agent-shot \
  ALKAID_BRIDGE_PORT=30142 node node_modules/electron/cli.js . --dev --remote-debugging-port=9321 &

# 3) playwright 走 connectOverCDP("http://127.0.0.1:9321")，别用独立 chromium（没装）
#    DPR 设 1，截图用 CDP Page.captureScreenshot + clip{scale:2}，只截 .main
#    先 inject：.avatar,.sb-user-text{visibility:hidden!important}
```

踩过的坑：

- 直接 `page.screenshot` 会卡在等字体 / 整页高度过大，用 CDP `Page.captureScreenshot` 稳。
- 官网预览图要先把 `.rise{opacity:1}` 和 `img.loading="eager"` 强制一遍，否则整页截图会出现黑块（懒加载 + 进场动画没触发）。
- 问答那轮要求模型「三句话、每句带出处」时**天然会写 §段/行号**；别在提问里教它写 `{{1}}`，会在气泡里露出模板语法。

---

## 别忘了改的文案

站点里的数字（8 分区 / 65 份文档 / 209 条双链 / 68 个节点 / 66 条归属 / 16 条引文）都来自演示库这一次的实测，
换库或重建后这些数会变，**改图的时候顺手改文案**，别让页面上的数和图里的数对不上。

`preview-full.png` 是整页长图，只用来给你看整体版式，不参与上线。
