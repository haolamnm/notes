---
aliases:
  - lecture-4-cs194-294-196
title: Lecture 4 - Enterprise Trends for Generative AI
link:
  - "[[Large Language Model Agents|CS194-294-196]]"
  - "[[Large Language Model|LLM]]"
draft: false
tags:
  - CS194-294-196
  - note
---
# Lecturer

Burak Gokturk:
+ Graduated  from Bogazici University, Turkey, with a Bachelor of Science in Electrical Engineering and Computer Science (1999).
+ Member of the Stanford Vision Laboratory.
+ Working at Google.

---
# Enterprise Trends

## Trend 1: AI is moving so fast

The enterprise used to collect more data to train their LLM, which take them months. Now, many enterprises can take one of the base models and can use in their application.

Anyone, or any company can develop AI.

## Trend 2: Technical trends

Now we have separate model for different task, we want to have a single model that can generalize across millions of tasks.

We want go from `dense models` to `efficient sparse models`.

## Trend 3: Choice of platform

We can expect to see another model every few weeks, every few months.

Now it is really the choice of `platform` rather than the `model`.

A platform for managing models in production. (Vertex AI?)

## Trend 4: Cost of API calls are approaching 0

There are lots of improvement in chip space, GPU, TPU.

## Trend 5: Search

Realization: LLM and Search need to come together.

Because LLM is
- Still can not answer the new / recent information.
- Still have a small chance of generate `hallucinations`.
- Still can not provide citation.

## Trend 6: Enterprise Assistant

For example: `Glean`, `ChatGPT enterprise` (with enhanced security, privacy and speed)

Which making the employees more productive.

Vertex AI - Model Garden has around 130+ curated models.

---
# Key component for Success

Key components of `Agent Builder`
1. `Fine Tuning`: Customize based on specific data and use case
2. `Distillation`: Create a smaller model for cost / latency purposes 
3. `Grounding`: Combine with search to make it factual / practical
4. Extensions
5. `Function calling`:  Function calling to be able to make LLMs on areas where they perform poorly (call for help) ***(first of all, need to know that the model cant answer the question then call a proper function to help)***

Simple, cost efficient to Complex, more expensive
1. Prompt design
2. SFT, Distillation
3. RLHF
4. Full fine-tuning

## Fine tuning

|         | Fine Tuning               | Prompt Design                    | Prompt Tuning                | Distillation                               |
| ------- | ------------------------- | -------------------------------- | ---------------------------- | ------------------------------------------ |
| Input   | Text                      | Prompt, text                     | Soft prompt, text            | None                                       |
| How?    | Regular model fine-tuning | No training, just design prompts | Train only 1-100 soft tokens | Distill 64B teacher to a 1B ULM or xxxM T5 |
| Data    | 10k - 100k                | 1- 10 examples                   | 100 - 1k                     | 100 - 1k labelled<br>1000x if unlabelled   |
| Cost    | Expensive                 | None                             | Average                      | Expensive                                  |
| Time    | Months                    | None                             | Minutes                      | Days                                       |
| Quality | Very High                 | High                             | Very High                    | Very High                                  |

Fine-tuning basic steps:
- Get a pretrained model checkpoint.
- Have a new dataset / task.
- Do a supervised learning on new dataset and update the weights of the new model.

Prompt-tuning basic step:
- Freeze the model backbone.
- Prepend a soft prompt (e.g. learnable) to the input.
- Only optimize the prompt to adapt to downstream tasks.

### Parameter-Efficient Fine Tuning

### Low-Rank Adaption

## Distillation

Reduce the serving cost and latency while maintaining a high performance.

`Softmax activation function`

## Grounding

Right context
- Private documents: Data that the LLM was not trained on.
- Fresh content: Up to date content from the web.
- Authoritative content: Content from authoritative sources.
- RAG

Better models
- RLHF
- Redesign Reward model to highly punish ungrounded responses.
- Create realistic high-quality training data.

User experience

Grounding on Vertex AI

## Function calling

Limitations
- Outdated information
- Consistent output (difficult to get LLMs to produce consistently structured output)
-  Have no inherent way of interacting with the world. (reference an external video, website or API)

LLM is great in getting information, but it's not great in taking actions. We need to call for a help.
1. Define your function.
2. Wrap functions in a tool.
3. Call Gemini with a tools argument.