---
aliases:
  - lecture-6-cs194-294-196
title: Lecture 6 - Agents for Software development
link:
  - "[[Large Language Model Agents|CS194-294-196]]"
  - "[[Large Language Model|LLM]]"
draft: false
tags:
  - CS194-294-196
  - note
---
# Lecturer

Graham Neubig profile:
+ Professor at Carnegie Mellon University (CMU).
+ Chief Scientist at All Hands AI (building open-source coding agents).
+ Maintainer of OpenHands. [OpenHands GitHub](https://github.com/All-Hands-AI/OpenHands).
+ Software developer.

# Software development

## Time

| Content           | Percentage |
| ----------------- | :--------: |
| Coding            |     15     |
| Bugfixing         |     14     |
| Testing           |     8      |
| Document/ Reviews |     10     |
| Communication     |     36     |
| Other             |     17     |

An old study from Microsoft in 2019. Maybe the time for `coding` become lesser nowadays.

## Support levels

1. No automation.
2. Driver Assistance/ Code completion.
3. Partial Automation.
4. Conditional Automation.
5. High Automation.
6. Full Automation.

## Environment

Type of environments:
+ Actual Environments:
	+ Source Repositories: GitHub, GitLab
	+ Task Management Software: Jira, Linear, GitHub Issue
	+ Office Software: Google Docs, Microsoft Office
	+ Communication Tools: Gmail, Slack
	+ Others: Server, Cloud Infrastructure
+ Testing Environments:
	+ Mostly focused on coding!
	+ Browsing the web (details in Lecture 7)

---
# Coding Agents

## Copilots vs Agents

`Copilots` support autocomplete code

### Copilot Prompting Strategy

+ Extract prompt given current doc and cursor position.
+ Identify relative path and language.
+ Find most recent accessed 20 files of the same language.
+ Include: text before, text after, similar files, imported files, metadata about language and path.
+ TL; DR: lots of prompt to get useful context!

`Agents` like `OpenHands`, we tell the agent which file to fix, fit a large amount of command; then it creates some tests, installs the dependencies for the project, runs the tests according to the direction we gave it. After that it commits the branch, adds unit tests, pushes using the credentials that we provided it. Finally it can create a link for us - human to review the code before using it.

`Autonomous Issue Resolution`, we can add `fix-me` tag for GitHub issue. If the `Agent` decided that it did a good job on fixing that bug, it will make a pull request. Then us - human can go in and accept it or not.

## Challenges

+ Defining the Environment.
+ Designing an Observations/ Actions.
+ Code Generation (atomic actions). 
+ File Localization (exploration). *How can we identify which parts of the code base that we would want to be edited (This has parallels to reinforcement learning, because in RL, we have the idea of exploring our environment)*
+ Planning and Error Recovery.
+ Safety.

## Must do

+ Understand repository structure.
+ Read in existing code.
+ Modify or produce code.
+ Run code and debug.

## Example: CodeAct

(Wang et el. 2024)

The one OpenHands use for their coding model and coding platform.

Traditional Agents: The idea is every time step, we call a tool, and then get a result back, based on the make the next action. (*Do a lots of API calls*)

CodeAct: Write actual program, for example write a `for loop`. This model can execute bash commands, Jupyter commands. This result in faster and higher resolution than direct tool use.

## Example: SWE-Agent

(Yang + Jimenez et al. 2024)

Another important thing to do is modify code.

Idea: "Define specialized tools that make it possible to efficiently explore repositories and edit code".

This work by `editing` and `observation` actions.

This allows we to do is even if we have files that are thousands of lines in our code repository, we can still parse them in a reasonable size for the LM context window without expanding our entire context length.

## Example: OpenHands

(Wang et al. 2024)

This is kind of a combination of `CodeAct` and `SWE-Agent`.

+ Define "Event Stream" in order to keep track of all Action - Observation.

---
# Research papers

## Simple Coding

(Chen et al. 2021, Austin et al. 2021)

Testing the ability of the model to go from a specification to code. Example: HumanEval, MBPP

Use only very standard Python libraries. Includes docstring, some example inputs/ outputs, and tests.

## Broader Domains: CoNaLa/ ODEX

(Yen et al. 2018, Wang et al. 2022)

+ `CoNaLa`: Broader data scraped from StackOverflow. (based on the titles context in the code snippet)
+ `ODEX`: Adds execution-based evaluation. (instead of the overlap between generated code)

Cover a wide variety of Python libraries: `random`, `pandas`, `numpy`, `re`, `sklearn`, `matplotlib`, `sys`, `os`, `json`.

## Data Science Notebooks: ARCADE

(Yin et al. 2022)

This is a great paper by people at Google. So basically, we have a notebook and people going through the notebook and then they can generate the next cells based on the context we have already, which is suitable for long practical implementation.

+ Data Science notebooks (e.g. Jupyter) allow for incremental implementation.
+ Allows evaluation of conde in context.

## Dataset: SWEBench

(Jimenez et al. 2023)

This is a very popular data set nowadays. The way it works is it scraps issues from GitHub and the code bases. Then output a pull request and unit tests to evaluate that pull request. (very similar to the OpenHands example)

This requires long-context understanding (which is a whole code base) and precise implementation.

There is some problems:
+ one of those is it kind of limited only high quality repos, where on PRs, where they introduce new tests. They are heavily biased towards non-documentation or non-refactoring or other related stuff.
+ another problem is lots of data has leaked into LLMs, because the LLMs are trained on data from GitHub.

## Dataset: Design2Code

(Si et al. 2024)

There is a direction that people are working on recently is multi-modal coding models.

This is a data set by people at Stanford.

The way it works is it does code generation from a bunch of website designs.

*There is also a multi-modal version of SWEBench*.

## Metric: Pass@k

(Chen et al. 2021)

Consider how we measure success, some of the most common metrics are something called Pass@k.

Basic idea: "If we generate $K$ examples, will at least one of them pass unit tests".

Generating only $K$ will result in high variance, so we generate $N > K$ with $C$ correct answers, and then we can use statistics to figure out what it would be at $K$.

The only disadvantage to this process is that it requires unit tests.

## Metric: Lexical/ Semantic Overlap

So there are also some other methods based on lexical or semantic overlap.

Basically, we look at how well the generated code overlaps with the standard code created by programmers.

The are a lots of method to do this:
1. `BLEU`: consider $n$-gram overlap with human code. (*has been used for a long time in machine translation*)
2. `CodeBLEU`: also consider syntax and semantic flow. (Ren et al. 2020)
3. `CodeBERTScore`: This is an embedding based method, BERTScore with CodeBERT trained on lots of code (Zhou et al. 2023)

## Metric: Visual Similarity of Website

+ Design2Code evaluate by two metrics:
	1. High-level visual similarity: Similarity between visual embeddings of the generated sites.
	2. Low-level element similarity: Recall of each individual element.

---
# Problem

## Dataset Leakage

+ Leakage of datasets is a big problem.

## File Localization

+ Finding the correct files given user intent. 

### Solution 1: Offload to the User

+ Experienced users familiar with prompting and the project can specify which files to use.

### Solution 2: Prompt the Agent with Search Tools

+ e.g. SWE-Agent provides a tool for searching repositories. 

Type of Searches:
1. No Search: Agent using manual search with `ls` and `cd`, or uses `grep`.
2. Iterative Search: Actions to show next/ previous search result are repeated many times until results are exhausted.
3. Summarized Search: (1) Show all the results in single output. (2) Tell Agent to retry if too many results.

### Solution 3: A-priori Map the Repo

+ Create a map of the repo and prompt agent with it.
+ Aider repo-map creates a `tree-structured map` of the repo.
+ Agentless (Xia et al. 2024) does a hierarchical search for every GitHub issue.

## Solution 4: Retrieval augmented Code Generation

+ Retrieve similar code, and fill it in with a retrieval-augmented LM (Hayati et al. 2018)
+ Particularly, in code there is also documentation, which can be retrieved (Zhou et al. 2022)

## Planning and Error Recovery

### Hard-coded Task Completion Process

e.g. Agentless (Xia et al. 2024) has a hard-coded progress of:
+ File Localization
+ Function Localization
+ Patch Generation
+ Patch Application (the best one that pass the tests)

But for example, what if we want the Agent to browse the web/ read the document before go into solving the task, we simply can't achieve it because this is a hard-coded method.

### LLM-Generated Plans

+ LLM-generated planning step, then one or more executors, this had a semi hard-coded structure between all of the subagents. Here is an example of those subagent roles:
	1. Manager
	2. Reproducer
	3. Fault Localizer
	4. Editor
	5. Verifier
+ e.g. CodeR (Chen et al. 2024)

### Planning and Revisiting

So if the agent that is executing the plan fails at doing any of the steps in the plan, it can kick it back to the `planner` and generate a new plan.

### Fixing Based on Error Messages

Example: ChatGPT 4.0 sometimes when it fails at fixing the issue, then it will attempt to fix the issue the same way, and get stuck in a forever loop. 

So fixing error based on the error messages isn't always return a good result.

## Safety

### Coding Models can Cause Harm

+ By accident:
	+ The coding model accidentally assumes that we want to pushed our code to the main branch.
	+ When the coding model is told to "make the tests pass", after struggling, it will just delete the tests.
+ Intentionally:
	+ Coding agents can be used for hacking! (Yang et al. 2023)

### Safety Mitigation 1: Sandboxing

+ We can improve the safety by limiting the `execution environment`. (*Docker Sandbox*)

### Safety Mitigation 2: Credentialing

+ The principle of least privilege. Example: GitHub access tokens.

### Safety Mitigation 3: Post-hoc Auditing

+ e.g. OpenHands security analyzer.
+ Using LMs, analysis or both. 

---
# Code-based LLMs

## Basic Method: Code-generating LM

+ Feed instructions and/ or input code to an LM
+ Virtually all serious LMs are trained on code nowadays, but some are specialized.

## Code Dataset Example: The Stack 2

Adding code improve the reasoning ability of models in general.

This is a code pre-training dataset (with license considerations), which has been used in training open models.

Basically, this dataset contains lots of code base in a wide range of language.

## Method: Code Infilling

(Fried et al. 2022)

+ In code generation, we often want to fill in code.

The way this works is that looking in the original documents, we take a coherent section, mask out that section, then move that mask-0 to the end of the document and finally leave a mask-0 where the code is taken.

This is one of ways people use to train model to be good at infilling data.

## Method: Long-context Extension

(Lu et al. 2024)

+ Models usually train on short-context for efficiency reasons.
+ In LMs, it is standard to use `RoPE`, a method for encoding positional information.
+ It does not generalize well beyond the training data, but code is long!

---
# Conclusion

+ Copilots 