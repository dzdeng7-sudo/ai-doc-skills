# AI 文档写作规范技能

一组约束 AI 编程助手撰写与修改文档行为的技能（skills），适用于支持 `.agents/skills` 发现机制的 Agent 工具（如 ZCode）。每条规则都来自真实使用中反复出现的坏习惯，而不是理论上的最佳实践。

## 技能列表

| 技能 | 管什么 | 解决的典型问题 |
| --- | --- | --- |
| [final-solution-only](final-solution-only/) | 正文只描述最终采用的方案 | 文档里删掉配置表方案后，它非要补一句"本设计不使用配置表" |
| [preserve-user-edits](preserve-user-edits/) | 修改文档以磁盘当前内容为准 | 你手动修正了文档里的错误，AI 下次修改时又按记忆里的旧版本写了回去 |
| [no-marketing-tone](no-marketing-tone/) | 文风直陈、克制 | "数据怎么流，一句话看懂""本方案回答一件事"这类口号式标题和营销腔 |
| [stay-on-topic](stay-on-topic/) | 只写用户点名的主题 | 让它写清结算改造方案，它非要在里面提一嘴商户 KYC 进件 |

四个技能各管一件事：`final-solution-only` 管"写了什么内容"，`preserve-user-edits` 管"怎么改文件"，`no-marketing-tone` 管"用什么语气"，`stay-on-topic` 管"写到什么范围"。分开定义是为了让各自触发更可靠，它们可以同时生效。

## 安装

把需要的技能文件夹复制到用户级技能目录（对所有项目生效）：

```bash
# Windows
git clone https://github.com/dzdeng7-sudo/ai-doc-skills.git
xcopy /E /I ai-doc-skills\final-solution-only "%USERPROFILE%\.agents\skills\final-solution-only"
# 其余技能同理

# Linux / macOS
git clone https://github.com/dzdeng7-sudo/ai-doc-skills.git
cp -r ai-doc-skills/final-solution-only ~/.agents/skills/
# 其余技能同理
```

只想在单个项目生效，就放到该项目的 `.agents/skills/` 目录下。

技能在新会话启动时被发现并按 `description` 自动触发；也可以显式调用，例如 ZCode 中使用 `/skill final-solution-only` 强制加载。

## 结构

每个技能一个目录，只含一个 `SKILL.md`：frontmatter 里的 `name` 和 `description` 负责自动触发，正文是具体规则、反例和例外。

## License

未附带开源协议。如需引用或复用请自行联系作者，或告知我补充 MIT 等协议。
