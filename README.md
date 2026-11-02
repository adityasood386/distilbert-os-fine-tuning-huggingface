# DistilBERT fine-tuned on Operating Systems text

Continued pretraining (masked language modeling) of `distilbert-base-uncased`
on operating-systems text, so it predicts OS terms better.

**Model:** https://huggingface.co/your-hf-username/distilbert-os-fill-mask

## Results
| | Perplexity (lower is better) |
|---|---|
| Before fine-tuning | X |
| After fine-tuning | Y |

Example: `The [MASK] schedules processes on the cpu.`
- Before: (paste your Step 5 output)
- After: (paste your Step 7 output)

## Files
- `train.ipynb`: full training notebook (run in Colab on a T4 GPU)
- `os.txt`: training text

## How to run
Open the notebook in Colab, upload `os.txt`, run all cells.

## Notes
Small learning project with a small corpus, so gains are modest.
