Title: Jev vs Drex vs Laya: The Rise of Decision Models in LLM Trends
Date: 2026-09-29
Category: AI Trends
Tags: Jev, Drex, Laya, Decision Models, System 1 AI, LLM Inference, Agentic AI
Slug: jev-vs-drex-vs-laya-decision-models
Status: Published
Cover: image/2026-09-29-jev-vs-drex-vs-laya-decision-models/1790643296002-opt.jpg

![1790643296002](image/2026-09-29-jev-vs-drex-vs-laya-decision-models/1790643296002.png)


September 2026 brought a new kind of model into AI conversations. It does not chat, write essays, or generate code. It makes decisions. Jev, Laya, and Drex are the three names leading this new category, called decision models or "System 1" models, and they are changing how developers think about building agentic pipelines.

This article explains what decision models are, how the three contenders compare, and which one fits which kind of team.

## What Is a Decision Model?

Most of the work an AI model does inside real software is not creative. It is repetitive judgment: route this support ticket, flag this email as spam, rate this incident's severity, decide whether an agent should take action A or action B. For years, developers have handed these small decisions to large generative LLMs. That is expensive and slow.

A generative LLM answers one token at a time. To tell your code that a ticket belongs to billing, it streams a sentence such as "The correct category is: billing", and your code then parses the label back out. That adds latency, costs tokens, and creates parsing failures.

A decision model works differently. It reads the input once, scores a fixed set of options you define, and returns a typed answer with a probability for each option. No text generation happens. The answer always fits the schema you gave it.

**System 1**
The name comes from Daniel Kahneman's dual-process theory. System 1 is fast, pattern-based judgment. System 2 is slow, deliberate reasoning.

**Non-autoregressive**
The model does not generate output token by token. It produces its decision in a single pass over the input.

**Calibrated confidence**
The model returns how sure it is. Teams can set thresholds, so that confident decisions run automatically and uncertain ones go to a human or a larger LLM.

## The Three Contenders

**Jev (TypeSafe AI)**
The closed, hosted original that started the category. It launched on September 15, 2026, from a lab founded by a former OpenAI researcher.

**Laya (Convai Innovations)**
The open-source challenger. It was released under Apache 2.0 three days after Jev, with weights on Hugging Face.

**Drex (Nace AI)**
The newest entrant. It claims the top spot on the public Decision Index and is aimed squarely at Jev.

## Jev: The Category Creator

Jev defined what a decision model API looks like. You send it a state, which can be text, an email, or JSON, along with typed questions: a multiple choice, a numeric score, or a true/false check. It returns the choice, a probability for every option, and a confidence score.

Adoption was fast. In its first week, Jev became the fastest-adopted model on Vercel's AI Gateway. Demand was strong enough that TypeSafe paused new signups within a week of launch. Developers used it to build web agents, memory systems, and game-playing bots.

**Pricing**
About $0.042 per million input tokens, with output free. An intake workflow of 40,000 decisions a month can cost around a dollar or two.

**Latency**
Roughly 236 to 276 milliseconds at the median for a single request.

**Strength**
Broad zero-shot accuracy with no setup, good calibration out of the box, and strong handling of large label sets such as 77-way intent classification.

**Weakness**
The weights are closed. Every decision leaves your infrastructure, you cannot pin a version forever, and you cannot fine-tune it yourself.

## Laya: The Open-Source Speed Play

Laya arrived as the open answer to Jev. It is a 421-million-parameter model family built on a ModernBERT encoder. Rather than generating text, the encoder reads the input and scores markers that stand for each answer choice. It supports more than 100 languages and runs comfortably on a T4-class GPU.

**Pricing**
Free. It is Apache 2.0 licensed and self-hosted, so there is no per-token bill.

**Latency**
About 33 milliseconds at the median on the routed multilingual checkpoint. That is roughly 7.8x faster than Jev's reported numbers, although the comparison was not a controlled head-to-head.

**Strength**
Very low latency, full data control, and a clean base for fine-tuning on your own labels. On specific tasks such as email spam filtering, the tuned models report excellent accuracy.

**Weakness**
Zero-shot performance is poor. The base model barely beats a majority-class baseline on its own benchmark, and choice questions degrade once there are more than about 20 options. The Laya team says openly that it should be treated as a fast base to specialize, not a ready-made decision engine.

## Drex: The Benchmark Challenger

Drex is Nace AI's direct shot at Jev. It uses a different architecture from the other two, a small diffusion model trained with reinforcement learning, and it is priced slightly below Jev.

**Pricing**
About $0.04 per million input tokens, marginally cheaper than Jev.

**Benchmarks**
Drex 1.1 reports 52.82 on Decision Index 0.2, ahead of Jev 1.13.0 at 51.67. With under 10 billion parameters, it is the smallest model in the index's top ten. In a board-game tournament Nace ran, Drex beat Jev in five of eight games.

**Deployment**
It fits on a single accelerator, so it can run in a private cloud, at the edge, or on-premises. Nace also offers to tune Drex on a customer's labelled outcomes and ship the weights.

**Weakness**
It is new and mostly self-reported. Open weights and a full technical report were promised at launch but had not yet appeared, and official index scores were initially marked as pending.

## Head-to-Head Summary

**Ease of use**
Jev leads. It works well with no training and no infrastructure.

**Speed**
Laya leads by a wide margin on raw latency, especially when self-hosted next to existing GPU infrastructure.

**Benchmark accuracy**
Drex claims the lead, with Jev close behind. Laya trails badly without fine-tuning.

**Cost**
Laya is free to run. Jev and Drex are both extremely cheap compared with frontier LLMs.

**Data privacy**
Laya and Drex can stay inside your network. Jev cannot.

## The Reality Check

Vendor speed claims are best-case numbers. Independent testing of real pipelines puts decision models at roughly 7x to 25x faster than a frontier LLM. That is still impressive, but it falls well short of the most dramatic marketing figures.

None of these models replaces an LLM. All three fail when asked to write, summarize, or reason through multiple steps. The right architecture uses both kinds of model: a decision model handles the fast routing, scoring, and gating, and the LLM handles the smaller share of tasks that actually require generation.

The open ecosystem is also moving quickly beyond Laya. Kev-9B, an Apache 2.0 model, reportedly comes within half a percentage point of Jev's accuracy, which may make it a closer open substitute for teams that need Jev-level zero-shot quality.

## Which One Should You Choose?

**Choose Jev**
If you want to ship this week, need broad zero-shot coverage, or work with large option sets.

**Choose Laya**
If you have labelled data, care about latency and cost above everything else, and want full control over your weights.

**Choose Drex**
If you want benchmark-leading accuracy with a private deployment option, and you can wait for independent verification and the promised open weights.

## Final Thoughts

Decision models mark a real change in AI system design. Not every problem needs a model that can write poetry. Many problems need a model that can make a fast, calibrated choice and step aside. Jev proved the demand, Laya proved the idea can be open, and Drex proved the benchmark race has already begun. For teams building agentic systems, the question is no longer whether to add a decision layer, but which one to trust with it.
