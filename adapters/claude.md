# 在 Claude 中使用

**能力最完整**：Claude 能直接读写你电脑上的资料库、操控你的 Chrome 浏览器。

## 方式 A：Claude 桌面版 / claude.ai（推荐）
1. 安装 Chrome 扩展「Claude in Chrome」，并允许它访问招聘网站。
2. 在 Claude 桌面版里连接 `my-data/` 所在的文件夹。
3. 把仓库里的 `SKILL.md` 添加为技能（设置 → 技能，或直接把文件发给 Claude 让它保存为技能）。
4. 在 Chrome 打开并登录网申页面，对 Claude 说：「帮我填这个网申：<链接>」。

## 方式 B：Claude Code（命令行）
1. 把 `SKILL.md` 复制到 `~/.claude/skills/wangshen-zhushou/SKILL.md`。
2. 在 `my-data/` 目录下启动 Claude Code，并开启 Chrome 扩展。
3. 输入「填这个网申 <链接>」。
