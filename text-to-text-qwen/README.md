# Text-to-Text Qwen Inference

[![Model](https://img.shields.io/badge/model-Qwen2.5--1.5B--Instruct-6f42c1)](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct)
[![Framework](https://img.shields.io/badge/framework-Hugging%20Face%20Transformers-ffcc4d)](https://huggingface.co/docs/transformers)
[![Runtime](https://img.shields.io/badge/runtime-Google%20Colab-F9AB00)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/license-not%20specified-lightgrey)](#license)

Reusable text-to-text AI inference with Qwen2.5 and Hugging Face Transformers. The repository provides a Google Colab-oriented notebook that loads `Qwen/Qwen2.5-1.5B-Instruct` and exposes a reusable `chat()` function for text generation.

> **Portfolio note:** This project demonstrates practical inference integration. The model is a pretrained Qwen instruct model loaded through Hugging Face Transformers; it was not trained by the repository author.

## Overview

The notebook keeps the inference path compact and reusable: authenticate with Hugging Face through a Colab Secret, select CUDA when it is available, load the tokenizer and causal language model, apply the model's chat template, generate a response, and decode only the newly generated text.

### Key features

- Reusable `chat()` helper for repeated prompts.
- Chat-style formatting through `tokenizer.apply_chat_template(...)`.
- CUDA/CPU device selection based on PyTorch availability.
- Automatic model placement through Transformers' `device_map="auto"`.
- Sampling controls exposed through `max_new_tokens` and `temperature`.
- Secret handling through Google Colab Secrets rather than hard-coded credentials.

## Model

| Item | Value |
| --- | --- |
| Model | [`Qwen/Qwen2.5-1.5B-Instruct`](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct) |
| Model type | Pretrained instruction-tuned causal language model |
| Access path | Hugging Face Transformers model and tokenizer loaders |
| Prompt format | System and user messages converted with the tokenizer chat template |
| Repository role | Inference integration and reusable notebook workflow; no training code |

The notebook uses a helpful-assistant system prompt, accepts a user prompt, generates up to 200 new tokens by default, and samples with a temperature of `0.7` by default. These are the notebook's current defaults, not benchmark claims.

## Technology stack

- Python in Jupyter Notebook / Google Colab
- [PyTorch](https://pytorch.org/) for tensor operations and device detection
- [Hugging Face Transformers](https://huggingface.co/docs/transformers) for tokenizer and model loading/generation
- [Accelerate](https://huggingface.co/docs/accelerate) for model placement support
- [Hugging Face Hub](https://huggingface.co/docs/huggingface_hub) for authentication
- `bitsandbytes` as installed by the notebook's dependency cell

## Workflow

```text
User Prompt → Chat Template → Qwen2.5 Inference → Generated Text
```

1. Install the notebook dependencies.
2. Read `HF_TOKEN` from Google Colab Secrets.
3. Authenticate with the Hugging Face Hub.
4. Detect CUDA when available and configure the model load.
5. Load the tokenizer and `AutoModelForCausalLM`.
6. Call `chat(prompt)` to format, generate, and decode a response.

## Repository structure

```text
text-to-text-qwen/
├── .gitignore
├── README.md
├── requirements.txt
└── notebook/
    └── Text_to_Text_reuseabel.ipynb
```

The notebook remains in its original location and is the project's runnable entry point.

## Requirements

- Python 3
- A Jupyter environment; Google Colab is the documented environment.
- Internet access to download the model and tokenizer from Hugging Face.
- A GPU is recommended for a smoother notebook experience, but the notebook selects CPU when CUDA is unavailable.
- A Hugging Face account and token available through the documented Secret setup.

Install the project dependencies in a compatible Python environment with:

```bash
pip install -r requirements.txt
```

The notebook itself also contains a quiet upgrade/install cell. `torch` may already be supplied by Google Colab; it is listed in `requirements.txt` because the notebook imports and uses it directly.

## Google Colab setup

1. Open [`notebook/Text_to_Text_reuseabel.ipynb`](notebook/Text_to_Text_reuseabel.ipynb) in Google Colab.
2. Select a runtime suitable for the model. The notebook metadata is configured for a GPU-oriented Colab session.
3. In Colab, open the **Secrets** panel (key icon) and add a secret named exactly:

   ```text
   HF_TOKEN
   ```

4. Grant notebook access to that secret when prompted.
5. Run the cells from top to bottom. The first model load downloads model files, so network access and sufficient runtime storage are required.

## Hugging Face authentication and security

The notebook intentionally keeps authentication in the Colab Secret store:

```python
HF_TOKEN = userdata.get("HF_TOKEN")
```

Do **not** replace this with a literal token or commit a token to the repository. Never place credentials in notebook output, README examples, `.env` files, or configuration files. If a token has ever been exposed, revoke it in Hugging Face and create a replacement.

## How to use the notebook

After the setup and model-loading cells have completed, call the reusable function with a natural-language prompt:

```python
response = chat("Explain photosynthesis in simple words.")
print(response)
```

The current `chat()` function:

1. Creates a system message and a user message.
2. Applies the tokenizer's chat template with a generation prompt.
3. Tokenizes the formatted text.
4. Moves input tensors to the loaded model's device.
5. Calls `model.generate(...)` under `torch.no_grad()`.
6. Removes the original prompt tokens from the generated sequence.
7. Decodes and returns the generated response as stripped text.

The function currently accepts `prompt`, `system_prompt`, `max_new_tokens`, and `temperature`. It uses sampling and the tokenizer end-of-sequence token for padding, matching the notebook implementation.

## Example prompts

These prompts reflect the general text-generation behavior demonstrated by the notebook:

```python
chat("What is artificial intelligence? Explain it in simple words.")
chat("Summarize the main idea of renewable energy in five bullet points.")
chat("Explain the difference between machine learning and deep learning.")
chat("Write a short, imaginative description of a city floating above the clouds.")
```

Outputs are generated by the model at runtime and can vary. The examples describe prompt types, not guaranteed answers, quality, factual accuracy, or benchmark results.

## Example input/output

**Input**

```text
Explain the difference between machine learning and deep learning.
```

**Output**

The notebook returns a generated text response from Qwen2.5. No fixed output is claimed here because generation is runtime-dependent and may vary with environment, model state, and sampling.

## Limitations

- This is an inference notebook, not a training or fine-tuning pipeline.
- Generated text may be incorrect, incomplete, biased, or inconsistent; verify important information.
- Response quality and speed depend on available hardware, runtime memory, prompt length, and generation settings.
- The workflow depends on downloading the model from Hugging Face and on a valid `HF_TOKEN` Secret.
- The repository does not provide an API server, web interface, evaluation suite, persistent chat history, or production deployment configuration.
- No performance benchmarks or quality scores are included.

## Future improvements

Potential next steps include adding a small evaluation set, separating inference code from presentation, supporting configurable generation settings, adding conversation history explicitly, and providing an optional local/non-Colab entry point. These are not part of the current implementation.

## Project status

**Portfolio-ready notebook demonstration.** The current scope is a focused, reusable text-to-text inference workflow built around the existing Colab notebook.

## Credits

- Model: [Qwen/Qwen2.5-1.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct)
- Inference tooling: [Hugging Face Transformers](https://huggingface.co/docs/transformers), [Hugging Face Hub](https://huggingface.co/docs/huggingface_hub), [PyTorch](https://pytorch.org/), and [Accelerate](https://huggingface.co/docs/accelerate)
- Notebook environment: [Google Colab](https://colab.research.google.com/)

## License

No license file is currently present in the repository, so the project does not currently declare an open-source license. Add a license before presenting the code for reuse under specific legal terms.
