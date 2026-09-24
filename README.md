# Text-to-Text Qwen

Reusable text-to-text AI inference with Qwen2.5 and Hugging Face Transformers.

This repository provides a lightweight, reusable notebook workflow for running the Qwen2.5 instruction-tuned model in a Google Colab environment. The main project files live in the `text-to-text-qwen/` directory.

## Overview

This project demonstrates how to:

- authenticate with Hugging Face Hub
- load the `Qwen/Qwen2.5-1.5B-Instruct` tokenizer and model
- apply a chat-style prompt template
- generate responses with a reusable `chat()` helper
- run inference efficiently on GPU when available, with CPU fallback

## Project structure

```text
text-to-text-qwen/
├── .gitignore
├── README.md
├── requirements.txt
└── notebook/
    └── Text_to_Text_reuseabel.ipynb
```

## Model

- Model: [Qwen/Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct)
- Type: instruction-tuned causal language model
- Framework: Hugging Face Transformers
- Runtime: Python + Jupyter / Google Colab

## Features

- reusable prompt helper for repeated inference calls
- chat template formatting using `tokenizer.apply_chat_template(...)`
- automatic device selection with CUDA/CPU fallback
- model loading using `device_map="auto"`
- configurable generation options like `max_new_tokens` and `temperature`

## Requirements

- Python 3
- Jupyter Notebook or Google Colab
- internet access for model download
- Hugging Face account and a valid `HF_TOKEN`
- GPU recommended, but CPU is supported if no CUDA device is available

## Setup

1. Open the notebook in the `text-to-text-qwen/notebook/` folder.
2. Install the dependencies:

```bash
pip install -r text-to-text-qwen/requirements.txt
```

3. Add a Hugging Face token named `HF_TOKEN` in Google Colab Secrets.
4. Run the notebook cells in order.

## Example usage

```python
response = chat("Explain photosynthesis in simple words.")
print(response)
```

## Security note

Do not hardcode your Hugging Face token into the notebook or commit credentials to the repository. Use Colab secrets or a secure environment variable.

## Notes

This repository is focused on inference and notebook-based experimentation rather than training or deployment. It is suitable for portfolio and demonstration purposes.

## License

No license file is currently included in this repository, so no explicit open-source license is declared yet.
