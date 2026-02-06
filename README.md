# Fine-Tuning FLAN-T5 for Text Coherence Improvement

## Project Overview
This project fine-tunes Google's FLAN-T5-base model on the Grammarly CoEdIT dataset to improve text coherence. The model learns to add connective words, restructure sentences, and improve logical flow between sentences.

## Task
Given disconnected or incoherent text, the model produces a more cohesive version by:
- Adding transition words (e.g., "however", "although", "because")
- Merging fragmented sentences
- Improving logical consistency

**Example:**
- Input: `She studied hard. She failed the exam.`
- Output: `Although she studied hard, she failed the exam.`

## Model
- **Base Model:** [google/flan-t5-base](https://huggingface.co/google/flan-t5-base) (248M parameters)
- **Architecture:** Encoder-Decoder (Seq2Seq)
- **Fine-tuning:** Full parameter training on coherence correction tasks

## Dataset
- **Source:** [Grammarly CoEdIT](https://huggingface.co/datasets/grammarly/coedit)
- **Filtered for:** Grammar, fluency, clarity, coherence, and correction tasks
- **Split:** Train (9,494) / Validation (1,187) / Test (1,187)

## Hyperparameter Optimization
Three configurations were tested:

| Config | Learning Rate | Epochs | Batch Size | Test Loss |
|--------|:------------:|:------:|:----------:|:---------:|
| Config 1: Baseline | 3e-4 | 3 | 4 | 0.0784 |
| Config 2: Lower LR | 1e-4 | 3 | 4 | 0.2292 |
| Config 3: Fewer Epochs | 3e-4 | 2 | 4 | **0.0476** |

**Best configuration:** Config 3 (lr=3e-4, 2 epochs)

## Results
- **Baseline (original FLAN-T5) Test Loss:** 11.0116
- **Fine-tuned (Config 3) Test Loss:** 0.0476
- **Improvement:** 99.6%

## Error Analysis
Analyzed 30 test examples (26.7% exact match rate). Error categories identified:
- **Category A - Different but Valid:** Model uses different connective words than reference (most common)
- **Category B - Minor Word Choice:** Slightly different words but meaning preserved
- **Category C - Incomplete Corrections:** Missing word insertions in some cases
- **Category D - Over-conservative:** Returns input unchanged for complex restructuring tasks

## Project Structure
```
├── Fine_Tuning_LLM.ipynb    # Main notebook with all code
└── README.md                 # This file
```

## How to Run
1. Open `Fine_Tuning_LLM.ipynb` in Google Colab
2. Set runtime to **GPU (T4)**
3. Run all cells sequentially
4. Use the inference pipeline:
```python
result = improve_text("Your text here.")
result = improve_text("Your text.", task="Make the text more cohesive")
```

## Tools and Libraries
- Python 3.12
- PyTorch (CUDA)
- Hugging Face Transformers
- Hugging Face Datasets
- Google Colab (T4 GPU)

## Author
CSYE 7230 - Software Engineering, Northeastern University
