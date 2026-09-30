# 源鉴（SourceLens）官方网站 — 需求与设计说明

#官网 #站点设计 #SourceLens

> 状态：已定稿（v1.0.0） · 日期：2026-09-28
> 本文档先于代码存在；任何口径变更须先回写本文档，再改页面代码。

## 1. 需求

为源鉴（SourceLens，Windows 标书文档同源比对与取证分析工具）建设官方产品站，结构与写法惯例
参考同作者的 PassGone / ExifMate / JustForShow / PrivaMask 网站仓库（仅作只读参考，文案与数据
不复制），品牌视觉自应用源图标派生。**宣传口径以 4.0 版本功能特点为主，算法不展开细节**
（2026-09-28 用户指示）：只讲能力与价值，不出现权重、公式、阈值、内部规则编号与具体算法名称。

- **域名与口径**：`https://sourcelens.weibaba.fun`；反馈邮箱 `feedback@weibaba.fun`；© 2026 Weibaba（全局规范第 12 条）。
- **发布方式**：公开 GitHub 仓库（`sourcelens`）→ Cloudflare Pages 静态托管，生产分支 `main`，
  仓库根即站点根；NAS 备份远程 `sourcelens_website`（website.md 15.2）。
- **隐私红线（硬性）**：纯静态、零追踪——无统计/分析、无任何外部请求（无 CDN 字体、无外链图片/JS）、
  无 Cookie、无表单、无服务端代码；仓库内不得出现用户数据、操作日志、凭据或本机路径。
- **下载入口**：微软商店 `https://apps.microsoft.com/detail/9N88BR67PQQ1`
  （Store ID `9N88BR67PQQ1`，2026-09-28 用户提供；唯一权威渠道）。
- **双语**：中文主站 `/` + 英文版 `/en/` 逐节对应；隐私政策 `/privacy/` 中英同页切换。
- **赞助区（强制，website.md 15.3）**：页脚前「请作者喝咖啡 ☕」区——标题行尾跳动 ☕ 按钮
  （bounce 2s infinite）+ 点击弹出收款码模态层；收款码 `assets/img/donate_qr.jpg` 与 PassGone 仓
  共用同一张；英文版 "Buy the developer a coffee ☕"。

### 1.1 产品口径来源（权威文件清单，禁止编造）

| 站点内容 | 来源（应用仓库 `D:\Weibaba-SoftWare\SourceLens`，v4.0-dev） |
|---|---|
| 功能、格式、三角度研判、证据分级、隐私事实 | `README.md` / `README.en.md`（2026-09-28 4.0 口径改版） |
| 三部分综合计分、围标报价规律、14 类指纹、词典复核 | `docs/决策记录.md` D-025/D-030/D-031/D-032/D-033/D-036（站点只取能力级表述，不引数值与规则） |
| 21 项内置指标（14 指纹 + 7 风格） | 决策记录 D-032/D-033（插件计数） |
| 版本与发布形态 | 公开测试版 v4.0（应用仓库 v4.0-dev；商店在架包版本以商店页面为准） |
| 品牌配色与图标 | 应用仓库 `store/ico/app_icon.png`（512×512 唯一设计源头） |
| 商店链接 | 用户提供（2026-09-28） |

注：算法细节（评分权重、报价规则集、指纹算法名、阈值数值）按用户指示**一律不上站**。

## 2. 品牌视觉（自源图标派生）

源图标：`store/ico/app_icon.png`（512×512，深藏青底圆角方块 + 放大镜审视文档 + 青色数据高光
——「鉴」）。四角透明，主底实测众数 `#050C1E`，青色高光众数 `#59C7FF`，中调钢蓝 `#134061` /
`#2C6189`。

| 用途 | 变量 | 值 | 来源 |
|---|---|---|---|
| 深藏青（渐变起点/墨色基调） | `--hero1` | `#050C1E` | 图标底色实测众数 |
| 深钢蓝（标题色/渐变中段） | `--accent-deep` | `#134061` | 图标中调蓝实测 |
| 主色（链接/图标块） | `--accent` | `#2C6189` | 图标中调蓝实测 |
| 高光青（CTA/点缀/h1 关键词） | `--cyan` | `#59C7FF` | 图标青色高光众数 |
| 高光青 hover | `--cyan-d` | `#45B5F2` | 高光青加深 |
| 主色 hover | `--accent-d` | `#244F73` | 主色加深 |
| 正文墨色 | `--ink` | `#101B2A` | 偏藏青深墨 |
| 背景 | `--bg` | `#F2F6FA` | 冷灰蓝底 |

Hero 渐变：`#050C1E → #134061 → #2C6189`（135°）。
字体：系统字体栈（Microsoft YaHei UI / Segoe UI / system-ui），不引用任何外部字体。

### 2.1 图标派生（脚本化，禁止各尺寸手改）

派生脚本 `tmp/derive_icons.py`（Pillow，conda 环境 `sourcelens`；`tmp/` 不入库，配方记录于此）：
读源图标 → LANCZOS 缩放输出——

| 产物 | 尺寸 |
|---|---|
| `assets/img/logo.png` | 512×512（Hero 与 og:image） |
| `assets/img/logo-170.png` | 170×170（README 头部） |
| `favicon.png` | 128×128 |
| `apple-touch-icon.png` | 180×180 |
| `favicon.ico` | 16 / 32 / 48 三档 |

## 3. 站点结构与页面结构

```
/                     中文主站（lang=zh-CN）
/en/                  English（lang=en）
/help/                用户手册（中文，源 = 应用仓库 docs/help/usage-zh.html，v1.0.2 起）
/en/help/             User Guide（English，源 = 应用仓库 docs/help/usage-en.html）
/privacy/             隐私政策（中英同页切换，onclick 切换 data-lang，无框架无存储）
/assets/style.css     全站唯一样式
/assets/img/          logo.png（512，og:image）、logo-170.png、donate_qr.jpg
/favicon.ico|.png、/apple-touch-icon.png   全部由源图标派生
/robots.txt /sitemap.xml /_headers /.well-known/security.txt
README.md AGENTS.md docs/site-design.md .gitignore
```

主站信息架构（锚点）：**Hero（居中 logo + slogan「穿透格式表象&nbsp;&nbsp;聚焦同源痕迹」（单行无标点两空格分隔，v1.0.1）+ 一句话定位 +
特性徽章行 + 商店下载按钮 + 版本卡）→ 功能区 `#features`（四块：三维综合评分 / 围标报价规律 /
写作风格画像 / 文档指纹取证）→ 分析流程 `#workflow`（五步芯片：添加文档 → 智能解析 → 一键比对
→ 结果视图 → 导出报告）→ 证据分级 `#grading`（铁证/强证/佐证芯片 + 诚实使用条款）→
隐私承诺块（链接 `/privacy/`）→ 赞助区（咖啡 ☕ + 收款码模态）→ 页脚（产品/支持/法律四列，
GitHub 占位 `#`）**。

- 版本卡口径：公开测试版 v4.0——全功能、免费公测；微软商店统一签名分发（应用仓库 D-014 发布形态）。
- 不上站内容：应用截图（兄弟站点现役形态均无截图；且历史截图存在字体渲染缺陷，待重制后另行评估）、
  评分权重表、报价规则集、算法名称、阈值数值。
- 隐私页节结构（中英对应）：核心承诺 → 1 处理方式 → 2 数据存放位置 → 3 云端历史库（默认关闭）→
  4 源文件只读 → 5 卸载与残留 → 6 联系 → 7 关于本网站；
  页尾注明「本文与应用仓库 README 隐私说明同步，以应用仓库为权威」。

## 4. 零追踪实现要点

- 唯一 JS 为隐私页语言切换与赞助收款码弹层的 `onclick`（无存储、无请求）。
- 外链仅微软商店与 `mailto:`，均需用户主动点击；无 iframe、无字体外链、无统计。
- `_headers` 输出 X-Content-Type-Options / X-Frame-Options / Referrer-Policy / Permissions-Policy。

## 5. 部署（Cloudflare Pages，用户侧操作步骤）

1. **GitHub 建仓**：在 GitHub 创建空仓库 `sourcelens`（不要初始化 README/.gitignore）。
2. **关联远程并推送**（origin 按本仓惯例 `https://github.com/zm1107/sourcelens.git`）：
   `git push -u origin main`
3. **Cloudflare Pages**：Dashboard → Workers & Pages → Create → Pages → Connect to Git →
   选 `sourcelens` 仓库 → 生产分支 `main` → 构建命令留空 → 输出目录 `/` → Save and Deploy。
4. **自定义域**：Pages 项目 → Custom domains → 添加 `sourcelens.weibaba.fun`；
   按提示在 DNS 处为 `sourcelens` 添加 CNAME 指向 `<项目名>.pages.dev`
   （若 weibaba.fun DNS 托管在 Cloudflare 则自动完成）。
5. **验证**：访问 `https://sourcelens.weibaba.fun/`、`/en/`、`/privacy/`，
   并用浏览器开发者工具确认无任何第三方请求。
6. （可选，推荐）**NAS 备份远程**：远程 `nas` =
   `ssh://git@nas.weibaba.life:53001/Weibaba-SoftWare/sourcelens_website`，推送不受限。

## 6. 版本

| 日期 | 版本 | 说明 |
|---|---|---|
| 2026-09-28 | v1.0.0 | 首版上线：中文主站 `index.html`（Hero + 功能四块 + 流程五步 + 证据分级 + 隐私承诺 + 赞助区 + 页脚）；英文版 `en/index.html`（逐节对应）；隐私政策 `privacy/index.html`（中英双语）；全站样式 `assets/style.css`；品牌图 `assets/img/logo.png`、`logo-170.png`、收款码 `donate_qr.jpg`；`favicon.ico`、`favicon.png`、`apple-touch-icon.png`；`robots.txt`、`sitemap.xml`、`_headers`、`.well-known/security.txt`、`.gitignore`；`README.md`、`AGENTS.md`、本文档 |
| 2026-09-28 | v1.0.1 | Hero 标语改版（用户指示）：中文「穿透格式表象 / 聚焦同源痕迹」两短句合并**同一行**，取消标点，以**两空格分隔**（HTML 以 `&nbsp;&nbsp;` 固化，防空白折叠）。副文案两句（定位句与隐私句）**去除句尾标点**，并在自然语义边界换行（定位句在「取证分析工具」后断行、去掉行尾冒号；隐私句在「分级呈现」后断行、去掉破折号）；`<title>` 与 `og:title` 中标语同步为无标点空格分隔形式；站点 README 头部标语同步。英文页适配（英文等宽排版限制）：h1 保持两短句结构、去破折号与句点、`<br>` 分行；pitch / sub 去句尾标点后自然换行（不加 `<br>`） |
| 2026-09-30 | v1.0.2 | 新增**用户手册**（用户指示：把软件使用说明放入网站，中文一份、英文一份）：`/help/`（中文）与 `/en/help/`（英文）。内容源 = 应用仓库 `docs/help/usage-zh.html` / `usage-en.html`（4.0 口径）。**口径（用户三轮指示）**：①手册只介绍功能与使用，**以发布的软件为准，不涉及开发中的内容**——剔除整章「准备与启动」（sourcelens.cmd / 源码运行 / 安装版 exe 等启动方式），剔除 `deploy/` 部署脚本引用与「应用仓库」表述，页尾改为「内容以发布的软件版本为准」；②其余内容逐字保留、章节重编号（中文一～六、英文 1–6）。站点适配：页头加返回首页与中英互链（`/help/` ↔ `/en/help/`）、品牌色对齐站点 token（--brand→#134061）、补充 favicon / meta description / canonical。主站导航与页脚「产品」列新增「用户手册 / User Guide」入口；`sitemap.xml` 收录两页（priority 0.8） |

> 注：提交信息仅含版本号，改动说明只记录于本表。
