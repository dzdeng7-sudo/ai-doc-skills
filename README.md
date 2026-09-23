# AI 文档写作规范技能

一组约束 AI 编程助手撰写与修改文档行为的技能（skills），遵循 Agent Skills 开放规范（每个技能一个目录，含 `SKILL.md` 与 YAML frontmatter）。凡支持该规范的 Agent 工具均可使用，例如：

- **ZCode**：项目级 `.agents/skills/`，用户级 `~/.agents/skills/`
- **Claude Code**：项目级 `.claude/skills/`，用户级 `~/.claude/skills/`
- **OpenAI Codex CLI**：`~/.codex/skills/`
- **Cursor**、**OpenCode**、**Gemini CLI**、**GitHub Copilot**（agent mode）等也已支持 SKILL.md 格式，目录略有差异

各工具的技能目录不同，但 SKILL.md 格式通用：把技能文件夹复制到对应目录即可。

每条规则都来自真实使用中反复出现的坏习惯，而不是理论上的最佳实践。

## 技能列表

| 技能 | 管什么 | 解决的典型问题 |
| --- | --- | --- |
| [final-solution-only](final-solution-only/) | 正文只描述最终采用的方案，删除或略过的内容不留痕迹 | 删掉配置表方案后，它非要补一句"本设计不使用配置表"；<br>你说"同步不用多写"，它写"细节由专项另行讨论，本材料只呈现方案与结果形态"；<br>被删技术的残迹留在目录、交叉引用和表格行里 |
| [preserve-user-edits](preserve-user-edits/) | 修改文档以磁盘当前内容为准，先读后改、最小化编辑 | 你手动修正的错误（参数名、数值、措辞），它下次修改时按记忆里的旧版本写了回去；<br>让它改一处，它整段甚至整文件重写，顺手改动无关内容；<br>你的修改与它的记忆冲突时，被它当成错误"纠正"掉 |
| [no-marketing-tone](no-marketing-tone/) | 文风直陈、克制，标题用名词短语直接说明内容 | "数据怎么流，一句话看懂""一文读懂""看完这篇就够了"式口号标题；<br>"本方案回答一件事"式故弄玄虚开场；<br>"动作归属：谁在哪里做什么"式术语后附大白话注释；<br>"强大""赋能""轻松搞定"等营销词与感叹号、emoji 堆砌 |
| [stay-on-topic](stay-on-topic/) | 只写用户点名的主题、章节与技术点 | 让它写清结算改造方案，它非要在里面提一嘴商户 KYC 进件；<br>自行添加"未来规划""扩展方向"等没要求的章节；<br>编造"上线前需财务确认""待评审"这类它无从知道的前置条件与待确认事项 |
| [narrative-flow](narrative-flow/) | 结构编排：叙事线连贯、对比相邻 | 做"当前 vs 改造后"对比时，它把改造后动作说明插在两张时序图中间，打断了对比叙事；<br>对图表的解释不紧跟图表，读者要来回翻看 |

五个技能各管一件事：`final-solution-only` 管"写了什么内容"，`preserve-user-edits` 管"怎么改文件"，`no-marketing-tone` 管"用什么语气"，`stay-on-topic` 管"写到什么范围"，`narrative-flow` 管"按什么顺序讲"。分开定义是为了让各自触发更可靠，它们可以同时生效。

## 效果演示

以最常见的一种翻车为例——方案原先用配置表管理参数，用户决定改用环境变量，让 AI 更新文档：

| | 输出 |
| --- | --- |
| 未加载技能 | 本设计不使用配置表，相关参数通过环境变量 `APP_CONFIG` 注入。 |
| 加载 final-solution-only | 相关参数通过环境变量 `APP_CONFIG` 注入，服务启动时统一加载并生效。 |

五个技能的完整对比（每个技能一组真实场景的前后输出）见 [demo.html](demo.html)，浏览器打开即可查看。

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

只想在单个项目生效，就放到该项目的 `.agents/skills/` 目录下。使用其他工具时，把示例中的 `.agents/skills` 替换为该工具的技能目录（见上文列表）即可。

技能在新会话启动时被发现并按 `description` 自动触发；也可以显式调用，例如 ZCode 中使用 `/skill final-solution-only` 强制加载。

## 结构

每个技能一个目录，只含一个 `SKILL.md`：frontmatter 里的 `name` 和 `description` 负责自动触发，正文是具体规则、反例和例外。

## License

[MIT](LICENSE)。可自由复制、修改、分发与复用，请保留版权与许可声明。
