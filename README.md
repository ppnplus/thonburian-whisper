<p align="center">
  <img src="assets/thonburian-whisper-logo.png" width="400"/>
</p>

[🤖 Model](https://huggingface.co/biodatlab/whisper-th-medium-combined) | [📔 Jupyter Notebook](https://github.com/biodatlab/thonburian-whisper/blob/main/thonburian_whisper_notebook.ipynb) | [🤗 Huggingface Space Demo](https://huggingface.co/spaces/biodatlab/whisper-thai-demo) | [📃 Medium Blog (Thai)](https://medium.com/@Loolootech/thonburian-whisper-asr-27c067c534cb)

**Thonburian Whisper** is an Automatic Speech Recognition (ASR) model for Thai, fine-tuned using [Whisper](https://openai.com/blog/whisper/) model
originally from OpenAI. The model is released as a part of Huggingface's [Whisper fine-tuning event](https://github.com/huggingface/community-events/tree/main/whisper-fine-tuning-event)  (December 2022). We fine-tuned Whisper models for Thai using [Commonvoice](https://commonvoice.mozilla.org/th) 13, [Gowajee corpus](https://github.com/ekapolc/gowajee_corpus), [Thai Elderly Speech](https://github.com/VISAI-DATAWOW/Thai-Elderly-Speech-dataset/releases/tag/v1.0.0), [Thai Dialect](https://github.com/SLSCU/thai-dialect-corpus) datasets. Our models demonstrate robustness under environmental noise and fine-tuned abilities to domain-specific audio such as financial and medical domains. We release models and distilled models on Huggingface model hubs (see below).

## Usage

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/biodatlab/thonburian-whisper/blob/main/thonburian_whisper_notebook.ipynb)

Use the model with [Huggingface's transformers](https://github.com/huggingface/transformers) as follows:

```py
import torch
from transformers import pipeline

MODEL_NAME = "biodatlab/whisper-th-medium-combined"  # see alternative model names below
lang = "th"

device = 0 if torch.cuda.is_available() else "cpu"

pipe = pipeline(
    task="automatic-speech-recognition",
    model=MODEL_NAME,
    chunk_length_s=30,
    device=device,
)

# Perform ASR with the created pipe.
pipe("audio.mp3", generate_kwargs={"language":"<|th|>", "task":"transcribe"}, batch_size=16)["text"]
```

## Requirements

Use `pip` to install the requirements as follows:

```sh
!pip install git+https://github.com/huggingface/transformers
!pip install librosa
!sudo apt install ffmpeg
```

## Model checkpoint and performance

We measure word error rate (WER) of the model with [deepcut tokenizer](https://github.com/rkcosmos/deepcut) after
normalizing special tokens (▁ to _ and — to -) and simple text-postprocessing (เเ to แ and  ํา to  ำ).

| Model                    | WER (Commonvoice 13) | Model URL |
|------------------------------|--------------------------|---------------|
| Thonburian Whisper (small)   | 13.14     | [Link](https://huggingface.co/biodatlab/whisper-th-small-combined) |
| Thonburian Whisper (medium)  | 7.42      | [Link](https://huggingface.co/biodatlab/whisper-th-medium-combined) |
| Thonburian Whisper (large-v2)| 7.69      | [Link](https://huggingface.co/biodatlab/whisper-th-large-combined) |
| Thonburian Whisper (large-v3)| 6.59      | [Link](https://huggingface.co/biodatlab/whisper-th-large-v3-combined) |


Thonburian Whisper is fine-tuned with a combined dataset of Thai speech including common voice, google fleurs, and curated datasets.
The common voice test splitting is based on original splitting from [`datasets`](https://huggingface.co/docs/datasets/index).

**Inference time**

We have performed benchmark average inference speed on 1 minute audio with different model sizes (small, medium, and large)
on NVIDIA A100 with 32 fp, batch size of 32. The medium model presents a balanced trade-off between WER and computational costs.

Certainly! Here's the modified table with the model URL separated into a new column:

| Model                            | Memory usage (Mb) | Inference time (sec / 1 min) | Number of Parameters | Model URL |
|----------------------------------|-------------------|------------------------------|----------------------|-----------|
| Thonburian Whisper (small)           | 7,194       | 4.83                | 242M       | [Link](https://huggingface.co/biodatlab/whisper-th-small-combined) |
| Thonburian Whisper (medium)          | 10,878      | 7.11                | 764M       | [Link](https://huggingface.co/biodatlab/whisper-th-medium-combined) |
| Thonburian Whisper (large)           | 18,246      | 9.61                | 1540M      | [Link](https://huggingface.co/biodatlab/whisper-th-large-combined) |
| Distilled Thonburian Whisper (small) | 4,944       | TBA                 | 166M       | [Link](https://huggingface.co/biodatlab/distill-whisper-th-small) |
| Distilled Thonburian Whisper (medium)| 7,084       | TBA                 | 428M       | [Link](https://huggingface.co/biodatlab/distill-whisper-th-medium) |

This new table structure separates the model URL into its own column at the end, making it clearer and easier to read. The links are preserved and will still function as clickable URLs in the markdown format.

## Long-form Inference

Thonburian Whisper can be used for long-form audio transcription by combining VAD, Thai-word tokenizer, and chunking for word-level alignment.
We found that this is more robust and produce less insertion error rate (IER) comparing to using Whisper with timestamp. See `README.md` in [longform_transcription](https://github.com/biodatlab/thonburian-whisper/tree/main/longform_transcription) folder for detail usage.


## Developers

- [Biomedical and Data Lab, Mahidol University](https://biodatlab.github.io/)
- [WordSense](https://www.facebook.com/WordsenseAI) by [Looloo technology](https://loolootech.com/)

<p align="center">
  <img width="50px" src="assets/wordsense-looloo.png" />
</p>

## Citation

If you use the model, you can cite it with the following bibtex.

```
@misc {thonburian_whisper_med,
    author       = { Zaw Htet Aung, Thanachot Thavornmongkol, Atirut Boribalburephan, Vittavas Tangsriworakan, Knot Pipatsrisawat, Titipat Achakulvisut },
    title        = { Thonburian Whisper: A fine-tuned Whisper model for Thai automatic speech recognition },
    year         = 2022,
    url          = { https://huggingface.co/biodatlab/whisper-th-medium-combined },
    doi          = { 10.57967/hf/0226 },
    publisher    = { Hugging Face }
}
```
