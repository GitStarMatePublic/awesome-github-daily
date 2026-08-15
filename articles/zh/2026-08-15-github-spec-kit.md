---
title: "GitHub 官方出手，让 AI 编程 Agent 别再瞎写！Spec-Kit 不到一年狂揽 12.8 万 Star"
date: 2026-08-15
repo: github/spec-kit
url: https://github.com/github/spec-kit
language: Python
license: MIT
stars: 128632
tags: [ai, coding-agents, spec-driven-development, github, workflow]
---

# GitHub 官方出手，让 AI 编程 Agent 别再瞎写！Spec-Kit 不到一年狂揽 12.8 万 Star

![Spec Kit](https://raw.githubusercontent.com/github/spec-kit/main/media/spec-kit-video-header.jpg)

用 AI 写代码的朋友，估计都有过这个崩溃瞬间。

今天要分享的这个 **Spec-Kit**，是 GitHub 官方亲自下场的一套工具，主打 **Spec-Driven Development（规格驱动开发）**。它 **2025 年 8 月 21 日才建仓**，如今已经狂揽 **12.8 万 Star**——是近年来开发者工具里涨得最猛的项目之一。如果你在用 Claude Code、Copilot、Cursor 或任何编程 Agent，这个真的值得花五分钟了解一下。

## 它到底在解决什么问题

先说那个人人都熟悉的循环。

你甩给 Agent 一句话——「加个用户登录」——它信心满满写了 400 行。结果呢：选错了库、编了个你技术栈里根本不存在的 API、还悄悄跳过了你最在意的那部分。于是你重新 prompt，再 prompt，二十分钟后，你纠偏花的精力比自己写还多。😮‍💨

而我也注意到，这**根本不是「模型笨」的问题，是「规格」的问题**。你从没精确告诉过它「做完」长什么样。一句随手扔出去的 prompt，本来就是个糟糕的事实来源。

Spec-Kit 把顺序彻底倒了过来。不再是「prompt → 写代码 → 修」，而是 **规格 → 计划 → 拆任务 → 写代码**。你（在 Agent 协助下）先写出一份真正的规格——要做什么、有哪些约束、验收标准是什么——**然后**再让 Agent 照着它实现。意图变成了一份能留存的文档，而不是你十分钟后就忘掉的一句话。

## 怎么上手

整套东西由一个叫 `specify` 的 CLI 驱动。一条命令初始化项目：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init my-project
```

之后，Spec-Kit 会给你的 Agent 一套结构化的步骤去走：

- **`/specify`** —— 描述「做什么」和「为什么」。先别谈技术栈，谈意图、用户能感知的行为、边界情况。这就是规格。
- **`/plan`** —— 这一步再引入「怎么做」：架构、依赖库、数据模型、技术方案。
- **`/tasks`** —— 把计划拆成一个个小的、可 review、可实现的任务块。
- **实现** —— Agent 逐个执行任务，随时回头对照规格，而不是自由发挥。

最关键的设计是：**它不挑 Agent。** Spec-Kit 不取代 Claude Code / Copilot / Cursor / Gemini CLI，而是套在你已经在用的那个外面，给它一套有纪律的流程去遵循。

## 为什么 12.8 万开发者认这套

规格驱动开发并不是什么新概念——先写规格再写代码，和软件本身一样古老。新的地方在于，**它为什么突然又重要了起来**。

以前每一行都是人写的，规格松一点也能扛，因为开发者脑子里装着完整的上下文。但 AI Agent 在两次 prompt 之间，对你的意图是**没有记忆**的。靶子越清晰，产出越好——而在大型、多步骤的功能上，「模糊 prompt」和「真规格」之间的差距，就是「能交付」和「一直看孩子」之间的差距。

这就是 Spec-Kit 押的注，而 star 曲线说明——一大批开发者押的是同一个注。

## 我个人比较欣赏的几点

- **天生不挑 Agent**：它是让你手上的工具变强，而不是逼你换工具；
- **逼你把话说清楚**：规格阶段就能把「等等，这事我们其实压根没定」的坑揪出来——而且是在写第一行代码之前，此时修正成本最低；
- **GitHub 出品、MIT 协议**：这不是个周末玩具，真实项目里可以放心用；
- **对团队友好**：一份写下来的规格，是人类 reviewer（或者第二个 Agent）能真正拿去核对的东西。

## 一个小遗憾

⚠️ 它本质是「流程」，而流程是有成本的。写个 20 行的一次性脚本，这套「先写规格」的仪式属实杀鸡用牛刀，你会实实在在感到摩擦。而且它需要真正的思维转变——很多人的肌肉记忆就是「先敲了再说」。回报是真的，但它体现在大型、多步骤的活上，不是随手糊的 demo。

## 小结

当下 AI 辅助编程最有用的进展，其实不是模型更聪明，而是**给模型一个清晰的靶子**。Spec-Kit 是这个方向上最干净、背书最硬的尝试，而且它出自 GitHub 自己之手——这意味着它不会昙花一现。只要你用编程 Agent 做的事超出了「一次性 prompt」，就值得认真试一把。

🔗 **项目地址：** https://github.com/github/spec-kit

---

*说到底，好项目从来不缺价值，缺的只是被看见的那一下。如果你自己的开源项目也卡在「没人 star」的冷启动，不妨试试 [GithubStarMate](https://www.githubstarmate.com)，让对的人先看见你。今天的分享就到这里，我们下期再见，respect！*
