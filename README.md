# code-companion · 一对一源码精读带读老师

一个用于 [DeepSeek Harness（DSH）](https://github.com/deepseek-ai/deepseek-harness) 的 skill：把「读懂一个代码库」变成一位一对一老师带着你逐段精读的过程——按一张**会移动的教学地图**推进，一次一张卡、一步一测、验证掌握才往前走。

目标不是让你复述「这段在讲什么」，而是让你**真的理解**：每个结论都能从你已经接受的地基推出来，接进你自己的心智模型。

## 它解决什么

- 面对陌生代码库不知道从哪读起、按什么顺序读
- 读源码时一路追到渲染、分词、截断、后处理，越跳越看不懂
- 会话一断就忘了上次学到哪、卡在哪
- 「看完了 / 懂了」其实没真正掌握，却已经往下推进

## 触发方式

在对话里说**「开始学习」**（精确四个字），或说「开始 / 继续学某项目源码」「按单元精读」即可触发。

进入后老师会先判断当前状态（首次建课 / 续学 / 发卡 / 答疑 / 推进），只读对应环节的文件，不会把整套规范一股脑塞进上下文。

## 它怎么工作

核心是一台围绕**会移动的教学地图**循环推进的状态机，分五个环节：

| 环节 | 职责 |
|---|---|
| `engine` | 状态机怎么转：进门分流、建课、发卡、答疑、持久化 |
| `map` | 会移动的教学地图：摸边界、最小树规划、随学习者持续演化、进度真相源 |
| `teaching` | 怎么讲一张卡：原理、步骤、卡片规范、后勤 |
| `guardrails` | 横切护栏：不臆造路径行号、不改源码、教学不旁白方法论 |
| `SKILL.md` | 薄入口：状态路由与硬红线 |

几个关键机制：

- **一次只发一张卡**，讲完就停，等学习者回应，不自行推进。
- **每张卡都有「停在这里」边界**，明确点名不许继续追的下游，防止越跳越深。
- **一步一测、先讲后测**，没落地就当场诊断修补，不在未验证的步上继续盖。
- **唯一当前任务卡**落盘为 `当前任务卡.md`，删了会话也能原样续上。
- **永久档案** `card-archive/C<n>.md`：首次发卡即建，答疑立即就地调整对应知识片段，重讲保留修订版本，完成时核对补全——只增不删，做到「不遗忘」。
- **掌握不靠口头信号推定**：「看完了 / OK / 懂了」不等于已验证，记录自测、复述或实测证据才算数。

学习数据写在项目根的 `.learning_memory/`（隐藏目录），**不碰源码**。

## 安装

把本目录放到 DSH 的 skills 目录下即可：

```bash
# 仓库克隆到临时位置
git clone https://github.com/zycskylove520/code-companion.git

# 复制到 DSH 的 skills 目录
# Windows（PowerShell）：
Copy-Item -Recurse .\code-companion "$env:USERPROFILE\.dsh\skills\code-companion"

# macOS / Linux：
cp -r ./code-companion ~/.dsh/skills/code-companion
```

放好后重启会话（或触发技能重载），说「开始学习」即可。

## 目录结构

```
code-companion/
├── SKILL.md              # 薄入口：状态路由、硬红线
├── CHANGELOG.md          # 演进记录
├── engine/               # 状态机怎么转
│   ├── INDEX.md
│   ├── session.md        # 进门分流（首次建课 / 续学）
│   ├── card-loop.md      # 主循环：发卡、验证、推进、结课
│   ├── branch.md         # 异常分支：答疑、重讲、疲惫
│   └── persistence.md    # 持久化唯一执行规则
├── map/                  # 会移动的教学地图
│   ├── INDEX.md
│   ├── build.md          # 摸边界、最小树规划、周/天单元
│   ├── evolve.md         # 地图随学习者演化、不遗忘
│   └── progress.md       # 进度真相源模板、动态估算
├── teaching/             # 怎么讲一张卡
│   ├── INDEX.md
│   ├── principles.md     # 教学原理（无条件真理先立等）
│   ├── step.md           # 单步讲法、每步一测
│   ├── card-spec.md      # 卡片规范、停止边界
│   └── logistics.md      # 后勤分工
└── guardrails/           # 横切护栏
    ├── INDEX.md
    ├── facts.md          # 事实与路径规则
    └── anti-patterns.md  # 反模式自检
```

## 许可

[MIT](./LICENSE)
