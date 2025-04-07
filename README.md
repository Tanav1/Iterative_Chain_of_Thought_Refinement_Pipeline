# Iterative Chain-of-Thought Refinement: Self-Correcting Generative Reasoning Through Verifier-Guided Feedback Loop**  
Author: Tanav Thanjavuru (UC Berkeley)

## Overview

Large Language Models (LLMs) frequently produce logically flawed reasoning even when the final answer is correct. This project introduces **Iterative Chain-of-Thought Refinement**, a verifier-guided framework that improves the logical validity, coherence, and readability of reasoning chains generated by LLMs.

The method employs a fine-tuned **Gemma-2B** verifier to provide feedback on each reasoning step generated by a frozen **LLaMA-3B** model. This feedback is used to iteratively refine the chain of thought until either the reasoning is fully valid or a maximum number of iterations is reached. No modification of the base generator is required.

## Motivation

While Chain-of-Thought (CoT) prompting improves final-answer accuracy, it does not guarantee logically sound intermediate reasoning steps. This limitation reduces the trustworthiness of LLMs in applications where transparency and justification are critical, such as education, scientific research, and decision-making.

The proposed framework addresses this issue by introducing a verifier into the reasoning loop. This enables step-level feedback and structured refinement, leading to more interpretable and logically valid outputs.

## Methodology

### Architecture

- **Generator:** LLaMA-3B (Meta)
- **Verifier:** Gemma-2B (Google)
- **Fine-tuning:** Verifier fine-tuned using LoRA with FP16 precision on the REVEAL dataset
- **Iteration Limit:** Maximum of 3 refinement loops per sample

### Iterative Process

1. The generator produces a reasoning chain for a given prompt.
2. The verifier evaluates each reasoning step as correct or incorrect.
3. If flaws are detected:
   - The verifier provides targeted natural language feedback.
   - The generator regenerates only the incorrect steps.
4. This process repeats until the chain is valid or the iteration limit is reached.

### Datasets

- **GSM8K**: Benchmark of grade-school math problems.
- **REVEAL**: Human-annotated reasoning steps across logic, science, commonsense, and factual tasks.

### Verifier Fine-Tuning

- Fine-tuned using balanced subset of REVEAL (312 examples)
- Few-shot prompting templates adapted for logical and attribution reasoning
- Trained using Hugging Face Transformers with causal language modeling objective
- Gradient checkpointing enabled for memory efficiency

## Evaluation Metrics

- **Reasoning Validity**: Percentage of chains with all steps verified as correct
- **Refinement Efficiency**: Average number of iterations and time per task
- **Readability**: Flesch-Kincaid Grade Level (FKGL)
- **Coherence**: Mean pairwise cosine similarity between TF-IDF vectors of steps
- **Conciseness**: Step and word count before and after refinement
- **Perplexity**: GPT-2-based measurement of fluency and diversity

## Results Summary

| Metric                          | GSM8K       | REVEAL     |
|--------------------------------|-------------|------------|
| Total Samples                  | 92          | 96         |
| Valid Before Refinement        | 71.7%       | 58.3%      |
| Valid After Refinement         | 93.5%       | 89.6%      |
| Average Refinement Iterations  | 1.37        | 1.65       |
| Average Refinement Time (sec)  | 36.24       | 48.07      |
| Readability Improvement        | +1.07       | +1.22      |
| Coherence Improvement          | +0.033      | +0.094     |
| Perplexity Change              | -2.27       | -4.60      |

The results demonstrate that iterative verifier-guided refinement significantly improves reasoning quality across both mathematical and multi-domain tasks, with minimal computational overhead.

## Technologies

- Python, PyTorch
- Hugging Face Transformers
- Google Colab Pro (A100 GPU)
- Weights & Biases (W&B)
- scikit-learn, NLTK
- LoRA (Low-Rank Adaptation)

## Future Work

- Scaling the framework to larger models and more domains
- Integrating retrieval-augmented generation
- Extending to multimodal and dialogue settings
- Conducting human evaluations for qualitative assessment
- Improving verifier robustness and coverage

## Contact


**Tanav Thanjavuru**  
MIDS Program, UC Berkeley  
Email: tanav@berkeley.edu  
LinkedIn: [linkedin.com/in/tanavt](https://www.linkedin.com/in/tanavt)
