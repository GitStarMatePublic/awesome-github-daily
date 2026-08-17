---
title: "14MB 塞进一整个 Agent 大脑！Needle 2 让手表、机器人也能离线调用工具，半年狂揽 6200+ Star"
date: 2026-08-16
repo: cactus-compute/needle
url: https://github.com/cactus-compute/needle
language: Python
license: MIT
stars: 6269
tags: [ai, llm, on-device-ai, agents, python]
---

# 14MB 塞进一整个 Agent 大脑！Needle 2 让手表、机器人也能离线调用工具

![Needle](https://raw.githubusercontent.com/cactus-compute/needle/main/assets/banner.png)

你要是想给智能手表、智能音箱、或者一个电池供电的小机器人做个「能调用工具的 AI Agent」，很快就会撞上同一堵墙：几乎所有靠谱的 tool-calling 模型，默认你有 GPU、有稳定的网络。Cactus Compute 交出的答案是 **Needle 2**——一个 4500 万参数的基础模型，量化压缩到**一个 14MB 的二进制文件**，工具调用、设备控制、结构化信息提取全部在设备本地完成。这个仓库 2026 年 2 月才创建，现在已经冲到 **6200+ Star**、400 多个 Fork——对一个扎在模型底层基建里的项目来说，这个涨速相当猛。

## 它到底在解决什么问题

云端的 tool-calling agent 到处都是：把 LLM 指向一个天气 API 或者一个开灯函数，它就能自己想清楚怎么调用。但把这套模式搬到没有稳定网络的手机、可穿戴设备、机器人上，选项一下子就没剩多少了。走云端 API，意味着延迟、数据隐私风险，一旦设备离线，功能直接失效。市面上也有一些「小模型」，但真正能做到*可靠*结构化工具调用的那批，往往还是几百 MB 起步，吃的内存也是嵌入式硬件根本给不出的。

Needle 2 就是专门为了填上这个坑而生的：一整套 tool-calling 会话跑起来大概只占 **28MB 内存**，推理时完全不发网络请求。

## 怎么上手

跟普通 Python 包一样装：

```bash
pip install cactus-needle
```

给任意函数加个装饰器，剩下的交给模型：

```python
import needle

@needle.tool
def get_weather(city: str):
    "Get the current weather for a city."
    return {"city": city, "temp_c": 27, "sky": "clear"}

agent = needle.Needle(tools=[get_weather])
print(agent.run("what's it like in Lagos right now?")["results"])
# [{'city': 'Lagos', 'temp_c': 27, 'sky': 'clear'}]
```

结构化信息提取也支持，直接吐出一个 Pydantic 对象：

```python
from pydantic import BaseModel

class Invoice(BaseModel):
    vendor: str
    total: float
    due_date: str

invoice = needle.extract("Invoice from Acme Corp, $1,200.00, due 2026-09-01", Invoice)
print(invoice.vendor, invoice.total)   # -> Acme Corp 1200.0
```

它还自带一条命令就能起的浏览器 Playground（`needle playground`），接线之前先在里面把模型摸清楚。

## 14MB 是怎么做到的

![Size-quality frontier: mobile-class and below](https://raw.githubusercontent.com/cactus-compute/needle/main/assets/frontier.png)

这不是拿个聊天模型蒸馏一下、再硬塞个工具调用功能进去，而是从头就是为这个场景设计的。几个关键设计点在扛住体积的同时保住了可靠性：模型用 Cactus 自研的量化方案压到 **2-bit 权重**；工具调用被一套从你的 schema 编译出来的**字节级语法（grammar）**约束住，输出永远是合法 JSON，而不是「大概率合法」；内置的**检索头（retrieval head）**会从一个很大的工具目录里，每一轮只挑出最相关的 5 个候选，而不是把整份清单硬塞进上下文；再加上一个 **256 token 的滑动窗口**、把工具固定成 KV sink，对话不管跑多久，内存占用都是平的。除此之外，每一次回复都带一个经过校准的置信度分数，你可以设一个阈值，高于它就自动执行，低于它就升级给更大的模型处理。按官方公布的 benchmark，Needle 2 能和体积大它 5～70 倍的模型（比如 FunctionGemma 270M、LFM2.5 230M）打得有来有回——背后这套「Simple Attention Network」架构的设计细节，写在他们的 [arXiv 论文](https://arxiv.org/abs/2607.18363) 里。

## 我个人比较欣赏的几点

- **是真的小**：14MB、约 28MB 内存，这已经不是「稍微小一点」，而是跟动辄 500MB 的模型完全不是一个量级；
- **语法约束输出**：工具调用在解码层就被 schema 强制约束，而不是靠 prompt 祈祷模型「应该」输出合法 JSON——这才是给小模型上可靠性的正确姿势；
- **自带置信度门控**：每条回复都有校准过的分数，「自动执行 vs 升级处理」有了一个有依据的阈值，而不是拍脑袋；
- **方便针对自己的工具微调**：LoRA 微调加上一条数据合成 CLI，意味着你不会被锁死在 base 模型自带的那点工具词汇表里。

## 一个小遗憾

⚠️ 4500 万参数、2-bit 精度，这决定了它是个「专才」而不是「通才」——它是为工具调用和信息提取而生的，不是为开放式推理或聊天而生的。如果你的场景需要广泛的世界知识，或者超出「挑对函数、填对参数」这个范围的多步推理，你背后还是得配一个更大的模型；Needle 2 扮演的是离线、快速的第一道防线，不是全面替代品。

## 小结

如果你想把一个真正的 Agent——而不只是个聊天小组件——塞进手机、可穿戴设备或机器人里，Needle 2 是少数几个专门为这套约束条件设计出来的模型之一。它替代不了你云端那个处理开放式任务的 LLM，但要说「离线状态下、不到 30MB 内存里，调对函数、填对参数」，目前很难找到比它更合适的选择。

🔗 **项目地址：** https://github.com/cactus-compute/needle

---

*说到底，好项目从来不缺价值，缺的只是被看见的那一下。如果你自己的开源项目也卡在「没人 star」的冷启动，不妨试试 [GithubStarMate](https://www.githubstarmate.com)，让对的人先看见你。今天的分享就到这里，我们下期再见，respect！*
