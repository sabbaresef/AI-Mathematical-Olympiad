# AI Mathematical Olympiad - Progress Prize 1

## Challenge description
The goal of this competition is to create algorithms and models that can solve tricky math problems written in LaTeX format. Your participation will help to advance AI models’ mathematical reasoning skills and drive frontier knowledge.

Problem Set: 110 novel math problems (intermediate high-school level), covering arithmetic, algebra, and geometry.

Benchmark: Current Gemma 7B performance is 3/50 on both public and private test sets.

No Train–Test Leakage: All problems were created by an international team to ensure a transparent, fair evaluation without data contamination.

Submit predictions using the provided Python evaluation API, which serves test instances one by one in random order. 

## Proposed solution 
This repository implements various generation strategies using the Deepseek‑math model (8‑bit quantized) via the Transformers library. The model is downloaded from Hugging Face.
Model behavior is controlled via generation parameters such as temperature, top_p, and max_new_tokens. 

The proposed solution obtained a score of 7/8 solved problems and can be further improved.

### Approaches: 
1. **Single-Shot Generation**: In this approach, the model receives the problem as a prompt and is expected to produce the correct answer in a single forward pass. It is fast and straightforward but may struggle with complex problems.

2. **Few-Shot Generation**: The model is prompted with a few examples of solved problems before being asked to solve a new one. This helps guide the model’s reasoning and formatting, often leading to better results when examples are carefully chosen.

3. **Tree-Based Generation**: This innovative strategy builds a tree of possible solution paths to explore multiple hypotheses:

- Step 1: Initialization
A root node is created by prompting the model to generate an initial response. The output tokens and average transition probability are stored.

- Step 2: Recursive Expansion
New child nodes are generated from the current node using various temperature settings. Each child’s continuation is evaluated based on average probability and termination condition (e.g., presence of EOS token).

- Step 3: Pruning and Skipping
Continuations below a probability threshold or randomly skipped paths (to prevent over-expansion) are not pursued. The tree expands only up to a specified time budget.

- Step 4: Path Selection
Once the tree is fully expanded or the time limit is hit, one of the following strategies is used to select the final answer:

	- Best Path: Chooses the path with the highest average probability per step.

	- Most Common: Selects the most frequent valid answer found across all leaf nodes.

	- Best Valid: Picks the highest-probability valid answer.

- Step 5: Iterative Refinement
If no valid answer is found, the tree-building process is repeated with adjusted thresholds and time constraints for up to a fixed number of retries.

This approach offers a flexible, robust exploration of solution paths, trading off between quality and diversity.

## Setup
This code is developed for Kaggle environment so you need to create an environment and to install all packages.