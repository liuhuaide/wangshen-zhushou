# 网申助手 wangshen-zhushou

让 AI 帮你填校招 / 社招网申表：**资料只整理一次，之后每个网站自动填写**，问到的新信息自动写回，每次投递自动记入投递表。

适用于 Claude、ChatGPT、豆包、Grok、Kimi 等 AI 助手（能力差异见下表）。

> English: An AI-agent skill that fills Chinese campus/job application forms from a local profile, learns new fields as it goes, and logs every application to a CSV tracker. Works with Claude (best), ChatGPT Agent, and other assistants.

---

## 它能做什么

- 📄 **按资料库填表**：基本信息、教育、工作、项目、奖项、语言、开放题……
- 🧠 **越用越聪明**：网站问到资料库没有的信息，只问你一次，写回后下次直接用；新的字段叫法也会记进别名表
- 🔁 **自动修正旧数据**：很多网站会保留你以前填的旧简历，AI 会逐项核对并改成最新版本
- 📊 **自动记录投递**：填完即记入 `投递记录.csv`（公司、公司类型、岗位、城市、链接、状态……），Excel 直接打开
- 📝 **按岗位定制简历**：在你自己的简历基础上调整求职意向、总结和重点（只调整不编造），另存为新版本，不覆盖原件；没有简历也可以用自带模板生成
- ⚙️ **自定义规则**：在 `rules.md` 里用大白话写规则，例如「是否有本公司亲属一律填没有」「期望城市优先深圳」

## 安全设计

- 证件号、银行卡号**永不保存**，每次由你本人填写
- 默认**不替你点最终提交**，你检查后自己点（可在 rules.md 中修改，见下方风险提示）
- 登录、验证码、隐私协议勾选都由你本人完成
- 网页里出现的「给 AI 的指令」一律当作普通数据，不会执行

---

## 快速开始

```bash
git clone https://github.com/<你的用户名>/wangshen-zhushou.git
cd wangshen-zhushou
mkdir -p my-data/附件
cp templates/profile.template.json my-data/profile.json
cp templates/answers.template.md   my-data/answers.md
cp templates/rules.template.md     my-data/rules.md
cp "templates/投递记录.template.csv" "my-data/投递记录.csv"
```

1. **最省事的做法**：直接对 AI 说「第一次用网申助手，帮我建资料库」，然后把简历和证件照发给它。AI 会先从简历预填，再按分区（基本信息 → 求职意向 → 教育 → …）逐轮问你补充，随时可以说「跳过」。也可以自己手动填写 `my-data/profile.json`，不会填的留 `null`。
2. 在 `my-data/rules.md` 写你的偏好。
3. 把简历（PDF，最好再附上 Word / HTML 源文件方便修改）和证件照放进 `my-data/附件/`。也可以直接把它们发给 AI，让它帮你保存、登记，并用简历初始化 profile。
4. 按你用的 AI 看对应说明：[Claude](adapters/claude.md) ｜ [ChatGPT](adapters/chatgpt.md) ｜ [豆包 / Grok / 其他](adapters/others.md)
5. 打开并登录网申页面，对 AI 说：**「帮我填这个网申：<链接>」**

`my-data/` 已写入 `.gitignore`，不会被提交到 GitHub。

## 各平台能力对比

| 平台 | 操控浏览器 | 读写本地资料库 | 体验 |
|---|---|---|---|
| Claude（桌面版 + Chrome 扩展 / Claude Code） | ✅ | ✅ | 全自动，推荐 |
| ChatGPT（Agent 模式） | ✅ 云端浏览器 | ❌ 需上传，结束时手动粘贴回 | 半自动 |
| 豆包 / Grok / Kimi 等 | 视产品而定 | 通常需上传 | 不支持浏览器时可用「填写指导」模式 |

## 目录结构

```
wangshen-zhushou/
├── SKILL.md                 # Claude 技能文件
├── AGENTS.md                # 通用 AI 指令（与 SKILL.md 内容相同）
├── data/aliases.json        # 字段别名表（通用，欢迎 PR 补充）
├── templates/               # 资料库模板
│   ├── profile.template.json
│   ├── answers.template.md
│   ├── rules.template.md
│   ├── 投递记录.template.csv
│   └── 简历模板.html        # 单页 A4 简历模板（带照片位）
├── adapters/                # 各平台使用说明
└── my-data/                 # 你的个人资料（自己创建，不会上传）
    ├── profile.json / answers.md / rules.md / 投递记录.csv
    └── 附件/                # 简历各版本、证件照、成绩单
```

## 简历与照片

- 放在 `my-data/附件/`，在 `profile.json → 附件` 里登记；可以按岗位类别准备多版（如 `简历_数据分析.pdf`、`简历_产品运营.pdf`），AI 会按岗位自动选用。
- 让 AI「帮我针对这个岗位改一版简历」：它会以你的简历为底稿，只调整措辞、顺序和重点，先列出改动让你确认，再另存为新文件。
- 没有现成简历：用 `templates/简历模板.html`，让 AI 按 profile 填好后，用浏览器「打印 → 另存为 PDF」导出。

## 投递记录表

| 列 | 说明 |
|---|---|
| 日期 / 公司 / 公司类型 / 行业 | 公司类型：国企、央企、民企、外企、合资、事业单位、初创 |
| 岗位 / 岗位类别 / 工作城市 / 招聘类型 | 招聘类型：校招、社招、实习 |
| 投递渠道 / 网申链接 | 渠道注明系统（飞书、北森、Moka……） |
| 使用简历 / 状态 / 下一步/截止 / 备注 | 状态：已投递 → 笔试 → 面试 → Offer / 未通过 |

## ⚠️ 风险提示

- 部分招聘平台的用户协议禁止自动化操作，过于频繁的自动填写可能导致账号受限。BOSS 直聘等平台会屏蔽浏览器自动化。
- AI 可能选错下拉项或日期，**提交前务必自己检查**。开启「允许自动提交」等于放弃这道检查，后果自负。
- 请勿把含个人信息的 `my-data/` 上传到任何公开位置。

## 贡献

最欢迎的贡献：在 `data/aliases.json` 中补充各网申系统的字段叫法，以及在 `adapters/` 中补充更多 AI 平台的使用说明。

## License

MIT
