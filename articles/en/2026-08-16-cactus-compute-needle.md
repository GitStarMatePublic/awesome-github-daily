---
title: "Needle 2: a 14MB tool-calling model built to live on your phone, not the cloud — 6,200+ stars in under 6 months"
date: 2026-08-16
repo: cactus-compute/needle
url: https://github.com/cactus-compute/needle
language: Python
license: MIT
stars: 6269
tags: [ai, llm, on-device-ai, agents, python]
---

# Needle 2: a 14MB tool-calling model built to live on your phone, not the cloud

![Needle](https://raw.githubusercontent.com/cactus-compute/needle/main/assets/banner.png)

Try to build an "AI agent" for a smartwatch, a smart-home hub, or a robot with a coin-cell budget, and you hit the same wall fast: every serious tool-calling model assumes a GPU and a network connection. Cactus Compute's answer is **Needle 2** — a 45-million-parameter foundation model, quantized down to a single **14MB binary**, that does tool calling, device control and structured extraction entirely on-device. Created in February 2026, the repo has already passed **6,200 stars** and 400+ forks — fast growth for something this deep in the model-infra weeds.

## The problem it's actually solving

Tool-calling agents are everywhere in the cloud — point an LLM at a weather API or a smart-light function and it figures out the call. But try to run that same pattern on a phone, a wearable, or a robot with no reliable connectivity, and your options collapse. Cloud APIs mean latency, a data-privacy leak, and a dead feature the moment the device goes offline. Small on-device models exist, but the "small" ones capable of *reliable* structured tool calling still tend to be hundreds of megabytes and hungry for RAM you don't have on embedded hardware.

Needle 2 is built specifically to close that gap: a full tool-calling session running in roughly **28MB of RAM**, with no network call at inference time at all.

## Getting started

Install it as a normal Python package:

```bash
pip install cactus-needle
```

Then decorate any function as a tool and let the model drive it:

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

It also does structured extraction straight into a Pydantic model:

```python
from pydantic import BaseModel

class Invoice(BaseModel):
    vendor: str
    total: float
    due_date: str

invoice = needle.extract("Invoice from Acme Corp, $1,200.00, due 2026-09-01", Invoice)
print(invoice.vendor, invoice.total)   # -> Acme Corp 1200.0
```

There's also a one-command browser playground (`needle playground`) to poke at the model before wiring it into anything.

## How it gets to 14MB

![Size-quality frontier: mobile-class and below](https://raw.githubusercontent.com/cactus-compute/needle/main/assets/frontier.png)

This isn't a distilled chat model with the tool-calling bolted on — it's purpose-built for the job. A few design choices do the heavy lifting: the model is compressed to **2-bit weights** via Cactus's own quantization scheme, tool calls are constrained by a **byte-level grammar** compiled from your schemas (so output is always valid JSON, not "usually valid"), a built-in **retrieval head** narrows a large tool catalogue down to the top five candidates per turn instead of stuffing the whole list into context, and a **256-token sliding window** with tools pinned as KV sinks keeps memory flat no matter how long the conversation runs. On top of that, every response ships a calibrated confidence score, so you can act automatically above a threshold and escalate to a bigger model below it. On the team's published benchmarks, Needle 2 trades wins with models 5–70x larger, like FunctionGemma 270M and LFM2.5 230M — the underlying "Simple Attention Network" architecture is documented in their [arXiv paper](https://arxiv.org/abs/2607.18363).

## What I like

- **Genuinely tiny** — 14MB and ~28MB RAM is a different category of "small," not just a marketing claim next to a 500MB model.
- **Grammar-constrained output** — tool calls being schema-validated at the decoding level, not hoped for via prompting, is the right way to build reliability into a small model.
- **Confidence gating built in** — a calibrated score per response gives you a principled escalate/act threshold instead of guessing.
- **Easy to fine-tune for your own tools** — LoRA fine-tuning plus a data-synthesis CLI means you're not stuck with the base model's tool vocabulary.

## One gripe

⚠️ At 45M parameters and 2-bit precision, this is a specialist, not a generalist — it's built for tool calling and extraction, not open-ended reasoning or conversation. If your use case needs broad world knowledge or multi-step reasoning beyond picking and filling a function call, you'll still want a bigger model behind it; Needle 2 is the fast, offline front line, not a full replacement.

## Verdict

If you're trying to put an actual agent — not just a chat widget — onto a phone, wearable, or robot, Needle 2 is one of the few models purpose-built for exactly that constraint set. It won't replace your cloud LLM for open-ended tasks, but for "call the right function with the right arguments, offline, in under 30MB of RAM," it's hard to beat.

🔗 **Repo:** https://github.com/cactus-compute/needle

---

*Shipping something you want developers to actually notice? The first stars are the hardest part of any launch — that's the whole reason we built [GithubStarMate](https://www.githubstarmate.com), a community where developers help each other get discovered.*
