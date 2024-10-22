---
aliases:
  - lecture-5-cs194-294-196
title: Lecture 5 - Compound AI Systems and the DSPy Framework
link:
  - "[[Large Language Model Agents|CS194-294-196]]"
  - "[[Large Language Model|LLM]]"
draft: false
tags:
  - CS194-294-196
  - note
---
# Lecturer

Omar Khattab:
+ CS Ph.D. candidate at [Stanford NLP](https://nlp.stanford.edu/) and a Research Scientist at Databricks.
+ Before coming to Stanford, I got my B.S. in CS in May 2019 from Carnegie Mellon University Qatar.
+ Assistant Professor at MIT EECS in Fall 2025.

---
# Compound AI System

## Definition

Compound AI Systems which is modular programs that use LLMs as specialized components.
+ Example: RAG or Multi-Hop RAG or Compositional Report Generation.

### Text corpus 

It is a large and structured set of texts that are used for ML, NLP,... It is often a collection of written or spoken material that can be used to study language patterns, build language models, or train AI systems like chatbots. Text corpora can be `general` (covering a wide range of topics) or specialized (focused on a specific subject area or domain).

## Benefit

Compound AI System benefit:
1. Quality: more reliable composition of better-scoped LM capabilities.
2. Control: can iteratively improve the system & ground it via tools.
3. Transparency: can debug trajectories & offer user-facing attribution.
4. Efficiency: can use smaller LMs, offloading knowledge & control flow.
5. Inference time scaling: can systematically search for better outputs.

## Programming

Instead or prompting, we can use programming.

LMs are highly sensitive to how they're instructed to solve the tasks, so each prompt often combines the 5 roles:
1. The core input to output behavior, a `Signature`
2. The computation specializing an inference-time strategy to the `Signature`. `Predictor` *(Let's think step by step)*
3. The computation formatting the `Signature`'s inputs and parses its typed outputs, An `Adapter`
4. The computations defining objectives and constraints on behavior, `Metrics` and `Assertions` *(dos and don'ts)*
5. The strings that instruct (or weights that adapt) the LM for desired behavior, an `Optimizer`

---
# DSPy

DSPy stand for Declaratively Self-improving Python, that's just mean Python but smarter.

## MultiHop

| Program                            | Optimized | GPT 3.5 | Llama2-13b |
| ---------------------------------- | :-------: | :-----: | :--------: |
| dspy.Predict("question -> answer") |    NO     |  34.3   |    27.5    |
| dspy.RAG(with CoT)                 |    NO     |  36.4   |    34.5    |
| dspy.RAG(with CoT)                 |    YES    |  42.3   |    38.3    |
| MultiHop                           |    NO     |  36.9   |    34.7    |
| MultiHop                           |    YES    |  54.7   |    50.0    |

Example of implementation of MultiHop
```python
class MultiHop(dspy.Module):
	def __init__(self):
		# Signature: What to do.
		self.generate_query = dspy.ChainOfThought("context, question -> query")
		self.generate_answer = dspy.ChainOfThought("context, question -> answer")
	
	def forward(self, question):
		context = []
		for hop in range(2):
			query = self.generate_query(context, question).query
			context += dspy.Retrieve(k=3)(query).passages
		answer = self.generate_answer(context, question)
		
		return answer
```

## Adapter

Adapters are used to translate modules into basic prompts
```python
dspy.Adapter(self.generate_query)

# Given the fields "context" and "question", respond with the field "query"
# Follow these following format
# Context: <context>
# Question: <question>
# Reasoning: Let's think step by step to <...>
# Query: <query>
```

## Optimizer

Optimizers can tune these prompts.
```python
optimizer = MIPROv2()
optimized_program = optimizer.compile(program) # -> better prompts

# Carefully read the provided "context" and "question". Your task is to formulate a concise and relevant "query" that could be used to retrieve information from a search engine to anwser the question most effectively. The "query" should encapsulate ...

# Follow these following format
# Context: <context>
# Question: <question>
# Reasoning: Let's think step by step to <...>
# Query: <query>

# Here are some examples: <...>
```

# Other terminology

+ Glorified Classifier is a term used somewhat critically to describe a machine learning model. The term implies that despite the complex architecture of the model, its fundamental task is just classifying inputs into different categories, something that can sometimes be done with much simpler models.

`STORM`: generates Wikipedia articles from a topic

`MIPRO`: Multi-prompt Instruction Proposal Optimizer. MIRPO uses a Bayesian Surrogate Model for Credit Assignment.

`OPRO`: Optimization through Prompting