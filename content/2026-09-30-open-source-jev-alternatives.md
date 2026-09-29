Title: Open-Source Jev Alternatives: Every Self-Hosted Decision Model Worth Knowing
Date: 2026-09-30
Category: GenAI Engineering
Tags: Jev, TypeSafe, System One Models, Decision Models, Open Source, Lean Inference, Self-Hosting
Slug: open-source-jev-alternatives
Status: Published
Cover: image/2026-09-30-open-source-jev-alternatives/1790644839913-opt.jpg

![1790644839913](image/2026-09-30-open-source-jev-alternatives/1790644839913.png)

On September 15, 2026, TypeSafe AI released Jev, a "System One" model that makes decisions and never writes text. You send it application state and a typed question, and it returns a choice, a score, or a true/false answer with a probability attached. It costs $0.042 per million input tokens, output is free, and it became the fastest-adopted model on Vercel's AI Gateway in its first week.

But Jev is closed. TypeSafe has not published weights, a paper, training data, or a local version. For teams that need data to stay on their own hardware, such as banks, clinics, and education platforms, that is a blocker. Within days, the open-source community responded with a wave of alternatives. This article maps them all.

## How the Open Alternatives Work

Every open Jev alternative uses one of two approaches, and knowing which one a project uses tells you most of what you need to know about it.

**Wrappers**
Train nothing new. They take an open-weight model you already host, declare the allowed options, and read the probability of each option straight off the logits in a single forward pass. No answer text is ever sampled. They are fast to adopt but inherit the base model's calibration.

**Trained models**
Ship their own weights, fine-tuned or trained from scratch specifically to answer choice, score, and yes/no questions. They take more effort to build, but they can be optimised for honest probabilities.

Many projects also copy Jev's `POST /v1/systemone` request format, so you can switch between hosted Jev and a local model by changing only the base URL.

## Trained Models

**Laya**
Released by Convai Innovations on September 18, 2026, under Apache 2.0. At 421M parameters, it makes decisions in about 33 ms and supports more than 100 languages. It runs well on CPU, including OpenVINO INT8. It is weak at zero-shot but strong after fine-tuning. Its limit is option count: accuracy drops sharply past about 20 options.

**Kev**
A 9B model under Apache 2.0 that reports 0.852 accuracy against Jev's 0.857 on its own evaluation. Kev runs as a local System One server whose wire API matches Jev's, so existing Jev client code works unchanged. The first server launch downloads the weights automatically.

**Von**
A small model of roughly 0.4B parameters that has led the independent community benchmark among open entrants. It runs locally and exposes a Jev-style interface.

**Bespoke Nimble**
A Qwen-based fine-tune trained specifically for Jev-style decisions. It offers open weights, a sensible middle ground in size, and a published training approach.

**tev1**
Another Qwen fine-tune in the trained-model camp. It is useful as a comparison point when benchmarking options of a similar size.

**NanoJev**
A 0.6B model aimed at real-time control loops, where every millisecond matters. It is better suited to learning and experimentation than to general-purpose routing.

**JevK5**
An open-weight decision model that returns typed decisions with probabilities in one forward pass. It is a newer entrant, so check its documentation and license before adopting it.

**OpenJev (AlexWortega)**
Despite the shared name, this version takes the trained route: it retrains Qwen3.5-4B specifically for judgment rather than reading logits off a frozen model. MIT-licensed.

## Wrappers and Layers

**open-alternative-jev**
An Apache 2.0 Python package (import name `so1`) that extracts typed, calibrated decisions from any open-weight LLM with a ChatML template, using Hugging Face or vLLM. It has the best reported calibration in the field, with an ECE of 0.020. It is an in-process library, not an HTTP server, so it has no drop-in endpoint.

**SemIf**
Formerly called OpenJev. It makes no claim to reproduce Jev's model. It reproduces the pattern: freeze an open-weight model you already host, declare your options, and read their probabilities off the logits. It is the easiest path if you already run a model.

**OpenJev (zhihz and others)**
Several independent projects use the OpenJev name. Most read option logits from a frozen Qwen3.5-4B and state clearly that they reproduce Jev's interface, not its undisclosed training. One reports 0.845 modal agreement with Jev on a 102-row subset, measured by its own author.

**OpenJev (DiffusionGemma)**
A heavier variant running DiffusionGemma 26B-A4B. It stands out as the only alternative that answers questions about images.

**openjev-sglang**
Serves Qwen3.6-35B-A3B on SGLang for B200-class hardware. It is built for high-throughput production rather than laptops.

**mini-jev**
The Mac option. It gives you a working HTTP endpoint quickly on Apple Silicon and is well suited to prototyping.

**AnyJev**
A generic wrapper that turns an existing open model into a Jev-style decision endpoint.

**Rizzo Flow**
Runs locally and exposes a Jev-style interface, making it another option for teams that want the API shape without the hosted service.

**jevlike**
Ships a trainer, not a model. You bring labelled options and train an option-attention head on a laptop. It is ideal for learning how decision heads work.

## The Honest Benchmark Picture

Treat accuracy claims carefully. Most projects compare themselves against Jev numbers taken from other people's runs, with different prompts and sample sizes. On an independent 49-task classifier benchmark, Jev scored 0.966 macro accuracy against 0.704 for the best open entrant. The open ecosystem copied Jev's interface in about a day and has spent the time since trying to match its accuracy.

Calibration is the bigger issue. Reading logits off a frozen chat model gives you a ranking, but not necessarily honest probabilities. Jev's real selling point is that a 0.8 confidence means the answer is right about 80% of the time. Few open projects have proven the same.

## How to Choose

**Coming from hosted Jev?**
Use Kev. The API matches, so your code does not change.

**Already running an open model?**
Use SemIf or open-alternative-jev and read the logits off what you have.

**Need calibrated confidence?**
Start with open-alternative-jev, given its 0.020 ECE.

**Small label set, CPU budget, multiple languages?**
Fine-tune Laya. For routing with under 20 labels, it is the cheapest option to run at almost any volume.

**Need image inputs?**
The DiffusionGemma-based OpenJev is currently the only choice.

**Want to learn the internals?**
Train a head with jevlike or experiment with NanoJev.

## Final Word

None of these projects is Jev. They all prove the same idea, though: a decision does not need generated text, and a single forward pass is dramatically faster and cheaper than token-by-token generation. Whichever you pick, measure calibration on your own data before trusting any confidence threshold. Temperature scaling or isotonic regression on a few hundred labelled examples is often all it takes to turn a fast ranker into a system you can let act on its own.
