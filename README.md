# DistilBERT Fine-Tuned on Operating Systems Text


[![Hugging Face Model](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-yellow)](https://huggingface.co/aditya500/distilbert-os-fill-mask)

Domain adaptation of `distilbert-base-uncased` using masked language modeling (MLM) on operating systems text, so the model predicts OS vocabulary (kernel, semaphore, thrashing, inode, and so on) better than the generic base model.

## Overview

DistilBERT was pretrained on general English text. Specialized vocabulary from a subject like operating systems appears rarely in that data, so the base model often guesses generic words. This project continues the model's original training task, filling in hidden words, on a corpus of OS text. No labels are needed, only raw text.

| | |
|---|---|
| **Base model** | `distilbert-base-uncased` (about 66M parameters) |
| **Task** | Masked language modeling (fill-mask) |
| **Training data** | Operating systems text: processes, scheduling, synchronization, deadlocks, memory management, paging, file systems, I/O |
| **Hardware** | Free Google Colab T4 GPU |
| **Model on the Hub** | [aditya500/distilbert-os-fill-mask](https://huggingface.co/YOUR-HF-USERNAME/distilbert-os-fill-mask) |

## Results

Perplexity measured on a held-out 10% of the text (lower is better):

## Quick start

### Use the trained model

```python
from transformers import pipeline

fill = pipeline("fill-mask", model="aditya500/distilbert-os-fill-mask")
print(fill("The [MASK] schedules processes on the cpu."))
```

### Reproduce the training

1. Click the **Open in Colab** badge above.
2. Set **Runtime → Change runtime type → T4 GPU**.
3. Run all cells.

## How it works

1. **Data:** `text` holds one paragraph per line. It is split 90% train and 10% test.
2. **Tokenization:** text is tokenized and cut into chunks of up to 128 tokens.
3. **Masking:** a data collator hides 15% of the tokens at random in every batch, and the model learns to predict them. Because the masks change each epoch, the same text gives varied training signal.
4. **Training:** the Hugging Face `Trainer` updates all model weights with a small learning rate so the model keeps its general English knowledge while adapting to the new domain.
5. **Evaluation:** perplexity (`exp` of the eval loss) is measured before and after training on the same held-out set.

### Training settings

| Setting | Value |
|---|---|
| Epochs | 10 |
| Batch size | 8 |
| Learning rate | 5e-5 |
| Max sequence length | 128 |
| MLM probability | 0.15 |
| Precision | fp16 |

_Update these to match the values you actually used._


## Limitations

- The training corpus is small, so improvements are modest and perplexity is noisy on such a small test split.
- This is a **fill-in-the-blank** model. It does not generate text or answer questions.
- It can overfit if trained for many epochs on a small corpus.
- The corpus covers core OS topics only and does not represent the whole subject.

## Possible improvements

- Train on a larger corpus, such as more Wikipedia articles or lecture notes.
- Compare full fine-tuning with LoRA.
- Fine-tune a causal model such as GPT-2 for OS text generation.
- Add a downstream task, such as classifying exam questions by OS topic.

## Acknowledgements

- Base model: [DistilBERT](https://huggingface.co/distilbert-base-uncased) by Hugging Face.
- Built with [Transformers](https://github.com/huggingface/transformers) and [Datasets](https://github.com/huggingface/datasets).
- If you add Wikipedia text to the corpus, credit Wikipedia (CC BY-SA 4.0).

## Author

**Aditya**: B.Tech CSE student, GNDU Regional Campus, Jalandhar
[GitHub](https://github.com/adityasood386) · [LinkedIn](https://linkedin.com/in/aditya-sood-b12b41267)

## License

Code is released under the MIT License. The base model is licensed under Apache 2.0.
