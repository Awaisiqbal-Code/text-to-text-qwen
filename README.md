# Text-to-Text Qwen Inference

[![Model](https://img.shields.io/badge/model-Qwen2.5--1.5B--Instruct-6f42c1)](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct)
[![Framework](https://img.shields.io/badge/framework-Hugging%20Face%20Transformers-ffcc4d)](https://huggingface.co/docs/transformers)
[![Runtime](https://img.shields.io/badge/runtime-Google%20Colab-F9AB00)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/license-not%20specified-lightgrey)](#license)

A reusable text-to-text inference workflow built with Qwen2.5 and Hugging Face Transformers. This project provides a compact Google Colab notebook that loads the `Qwen/Qwen2.5-1.5B-Instruct` model and exposes a simple `chat()` function for prompting the model with natural-language input.

> Portfolio note: This repository demonstrates practical inference integration using a pretrained instruction-tuned model. It is designed as a reusable notebook workflow rather than a training or fine-tuning project.

## Overview

This project keeps the inference flow lightweight and reusable. The notebook handles Hugging Face authentication using a Colab secret, selects the best available device, loads the tokenizer and model, and then generates responses through a single helper function.

## Key features

- Reusable `chat()` helper for repeated prompts
- Chat-style formatting using `tokenizer.apply_chat_template(...)`
- Automatic CUDA/CPU device selection
- Model loading through `device_map="auto"`
- Configurable generation settings via `max_new_tokens` and `temperature`
- Secure secret-based authentication with Google Colab

## Model

| Item | Value |
| --- | --- |
| Model | [`Qwen/Qwen2.5-1.5B-Instruct`](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) |
| Type | Pretrained instruction-tuned causal language model |
| Access path | Hugging Face `AutoTokenizer` and `AutoModelForCausalLM` |
| Prompt format | System + user messages via chat template |
| Project role | Inference + notebook workflow; no training pipeline |

The notebook uses a helpful assistant system prompt, accepts a single user prompt, generates up to 200 new tokens by default, and samples with a temperature of `0.7`.

## Tech stack

- Python
- Jupyter Notebook / Google Colab
- [PyTorch](https://pytorch.org/)
- [Hugging Face Transformers](https://huggingface.co/docs/transformers)
- [Accelerate](https://huggingface.co/docs/accelerate)
- [Hugging Face Hub](https://huggingface.co/docs/huggingface_hub)
- `bitsandbytes` (as installed in the notebook)

## Workflow

```text
User Prompt → Chat Template → Qwen2.5 Inference → Generated Text
```

1. Install the project dependencies.
2. Read `HF_TOKEN` from Google Colab Secrets.
3. Authenticate with Hugging Face Hub.
4. Detect CUDA and configure device loading.
5. Load the tokenizer and language model.
6. Call `chat(prompt)` to generate a response.

## Repository structure

```text
text-to-text-qwen/
├── .gitignore
├── README.md
├── requirements.txt
└── notebook/
    └── Text_to_Text_reuseabel.ipynb
```

## Requirements

- Python 3
- Jupyter environment or Google Colab
- Internet connection to download the model and tokenizer
- GPU recommended, but CPU fallback works if CUDA is unavailable
- Hugging Face account with a valid token stored in Colab Secrets

Install dependencies:

```bash
pip install -r requirements.txt
```

## Google Colab setup

1. Open [`notebook/Text_to_Text_reuseabel.ipynb`](notebook/Text_to_Text_reuseabel.ipynb) in Google Colab.
2. Choose a runtime with GPU support if available.
3. In the Colab **Secrets** panel, create a secret named exactly:

   ```text
   HF_TOKEN
   ```

4. Allow the notebook to access the secret.
5. Run the cells from top to bottom.

The first model load downloads the model files, so internet access and enough disk space are required.

## Security note

Authentication is kept in Colab Secrets:

```python
HF_TOKEN = userdata.get("HF_TOKEN")
```

Do not hardcode tokens into the notebook or commit them to the repository. Avoid storing credentials in README examples, `.env` files, or config files.

## How to use it

After the setup and model-loading cells finish, call the helper with a prompt:

```python
response = chat("Explain photosynthesis in simple words.")
print(response)
```

The `chat()` function does the following:

1. Builds a system and user message
2. Applies the tokenizer chat template
3. Tokenizes the prompt
4. Moves tensors to the model device
5. Calls `model.generate(...)` under `torch.no_grad()`
6. Removes the original input tokens from the output
7. Decodes and returns the generated response

## Example prompts

```python
chat("What is artificial intelligence? Explain it in simple words.")
chat("Summarize the main idea of renewable energy in five bullet points.")
chat("Explain the difference between machine learning and deep learning.")
chat("Write a short imaginative description of a city floating above the clouds.")
```

Outputs are generated at runtime and can vary by environment, hardware, and sampling settings.

## Limitations

- This is an inference notebook, not a training pipeline
- Generated responses can be incorrect, incomplete, or biased
- Quality and speed depend on hardware, memory, and prompt length
- The workflow depends on downloading the model from Hugging Face
- No web UI, API server, persistent chat memory, or deployment config is included
- No benchmark or evaluation suite is provided

## Future improvements

Possible next improvements include:

- adding a small evaluation set
- separating inference logic from notebook presentation
- supporting more configurable generation parameters
- adding explicit conversation history support
- improving code modularity for reuse in scripts or apps

## Project status

This project is a focused, reusable inference demo built around a Colab notebook and designed to showcase practical LLM integration in a portfolio-friendly format.

## Credits

- Model: [Qwen/Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct)
- Inference tooling: [Hugging Face Transformers](https://huggingface.co/docs/transformers), [Hugging Face Hub](https://huggingface.co/docs/huggingface_hub), [PyTorch](https://pytorch.org/), and [Accelerate](https://huggingface.co/docs/accelerate)
- Notebook environment: [Google Colab](https://colab.research.google.com/)

## License

No license file is currently present in the repository, so the project does not currently declare an open-source license. If you plan to share or reuse this code commercially or publicly, add a license before distribution.
