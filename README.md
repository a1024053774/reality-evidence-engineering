# Reality-First & Evidence-First Engineering

面向公司技术分享的一组轻量工程 Skill，只保留两个互补的决策门：

- **Reality-First Engineering**：在架构或大范围实现前，确认我们是否在解决正确的问题。
- **Evidence-First Testing**：在修复或改变行为后，证明测试确实检测到了目标行为。

它们不绑定特定 Agent、reviewer、hook 或测试框架，可以单独使用，也可以按顺序配合使用。
Reality-First 可以与已有项目账本（例如 `project-to-act`）协作，但不会创建第二套计划；
Evidence-First 也不会用固定测试数量或代码长度限制真实覆盖。

## 用两个问题理解

| Skill | 核心问题 | 主要产物 |
| --- | --- | --- |
| `reality-first-engineering` | 方向是否被事实支持？ | `PASS`、`BLOCKED` 或 `INCOMPLETE` 的现实门 brief |
| `evidence-first-testing` | 改动是否被反事实和测试证明？ | 修复前红态、修复后绿态及限制说明 |

推荐的工作顺序是：

1. 先用 Reality-First 识别会改变方案的事实和未知，并运行最小可证伪 probe。
2. 方向通过后，再用 Evidence-First 建立聚焦的红态或契约测试。
3. 做最小实现，重跑同一个信号，再运行必要的更广检查。
4. 如果反事实、外部输入或独立 oracle 不可得，明确报告证据不完整，不用更多抽象或
   mock-only 分支掩盖它。

新需求的测试可以在实现前预期失败；这表示契约尚未实现，不应被误报成已经复现的旧缺陷。

## 本次更新

- Reality-First 将已有的 `project-to-act`（或等价项目账本）视为唯一持久事实源；同一
  决策批次只运行一次 Reality Gate，后续迭代只有在新证据改变方向时才重开。
- Evidence-First 不再把测试数量或代码长度当作覆盖上限或充分性证明；应按不同验收条件、
  真实边界和可信故障模式决定是否扩展，并允许为独立 oracle 编写更复杂的测试。
- 两个 Skill 仍然不依赖特定模型、provider、hook 或内部项目配置。

## 仓库边界

这个仓库有意不包含：

- 生命周期 hook、scanner 或 reviewer agent；
- 项目账本、任务系统或公司内部流程配置；
- 测试 harness、fixture、示例项目或特定语言的测试框架；
- 其他 Skill、兼容层和自动化脚本。

这些内容不是两个 Skill 的必要依赖；保留边界可以让分享者按公司的工具和流程接入，
而不把一套特定工作台伪装成通用方法论。

## 安装

克隆仓库后，把两个 Skill 链接到使用的 Agent 目录。Codex 示例：

```bash
git clone git@github.com:a1024053774/reality-evidence-engineering.git
cd reality-evidence-engineering

mkdir -p "$HOME/.codex/skills"
ln -s "$PWD/evidence-first-testing" "$HOME/.codex/skills/evidence-first-testing"
ln -s "$PWD/reality-first-engineering" "$HOME/.codex/skills/reality-first-engineering"
```

Claude Code 可将同样的两个目录链接到 `~/.claude/skills/`；其他兼容 Agent 使用其对应
的用户 Skill 目录。两个目录中的 `agents/openai.yaml` 只提供 Codex 的界面名称和默认
提示，不引入运行时依赖。

## 验证

每个 Skill 都包含合法的 YAML frontmatter 和 Codex UI 元数据。接入后，用目标 Agent
分别执行一个高不确定性的实现请求和一个 bug 修复请求，确认它们能分别产出 Reality
Gate brief 与红/绿证据报告；不要只用“是否出现某个标题”的文本匹配作为验收。

## License

MIT，详见 [LICENSE](LICENSE)。
