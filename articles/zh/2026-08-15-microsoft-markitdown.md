---
title: "微软又开源神器了，任何文件秒变 Markdown，已狂揽 17 万 Star！"
date: 2026-08-15
repo: microsoft/markitdown
url: https://github.com/microsoft/markitdown
language: Python
license: MIT
stars: 173804
tags: [ai, rag, llm, markdown, microsoft]
---

# 微软又开源神器了，任何文件秒变 Markdown，已狂揽 17 万 Star！

最近在做 AI 应用的朋友，估计都踩过同一个坑。

一份 PDF、一个 Excel，甩给大模型，结果表格全乱、排版稀碎，token 全喂给了垃圾字符。😮‍💨

而我也注意到，这个「喂料」环节，正在悄悄变成 RAG 和 Agent 落地时最脏最累的活。

今天要分享的这个 **MarkItDown**，就是微软官方下场，专门来治这个病的。

它做的事只有一件，但做到了极致——

**任何文件，都给你转成干净、大模型能直接读的 Markdown。**

PDF、Word、Excel、PPT、图片、甚至音频，通通通吃。一行命令搞定：

```bash
markitdown 财报.pdf > 财报.md
```

## 我个人比较欣赏的几点

- **全格式一个 API 搞定**，不用东拼西凑一堆库；
- **简单到发指**，一条命令的事；
- **MIT 协议，微软背书**，商用也放心。

## 一个小遗憾

⚠️ 图片和音频的解析要挂 LLM key，做不到纯本地离线，介意的朋友注意下。

## 小结

恰恰是这种「不花哨、只解决真问题」的工具，最容易被低估。它目前已经悄悄冲到 **17.3 万 Star**，还在涨。做 AI 应用的，建议直接收藏，下次做 RAG 你一定会回来找它。

🔗 **项目地址：** https://github.com/microsoft/markitdown

---

*说到底，好项目从来不缺价值，缺的只是被看见的那一下。如果你自己的开源项目也卡在「没人 star」的冷启动，不妨试试 [GithubStarMate](https://www.githubstarmate.com)，让对的人先看见你。今天的分享就到这里，我们下期再见，respect！*
