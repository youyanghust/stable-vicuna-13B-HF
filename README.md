---
license: other
---
# StableVicuna-13B

This is an HF format unquantised model of [CarterAI's StableVicuna 13B](https://huggingface.co/CarperAI/stable-vicuna-13b-delta).

It is the result of merging the deltas from the above repository with the original Llama 13B weights.

## Repositories available

* [4bit GPTQ models for GPU inference](https://huggingface.co/TheBloke/stable-vicuna-13B-GPTQ).
* [4bit and 5bit GGML models for CPU inference](https://huggingface.co/TheBloke/stable-vicuna-13B-GGML).
* [Unquantised 16bit model in HF format](https://huggingface.co/TheBloke/stable-vicuna-13B-HF).

# Original StableVicuna-13B model card

## Model Description

StableVicuna-13B is a [Vicuna-13B v0](https://huggingface.co/lmsys/vicuna-13b-delta-v0) model fine-tuned using reinforcement learning from human feedback (RLHF) via Proximal Policy Optimization (PPO) on various conversational and instructional datasets.

## Model Details

* **Trained by**: [Duy Phung](https://github.com/PhungVanDuy) of [CarperAI](https://carper.ai)
* **Model type:**  **StableVicuna-13B** is an auto-regressive language model based on the LLaMA transformer architecture.
* **Language(s)**: English
* **Library**: [trlX](https://github.com/CarperAI/trlx)
* **License for delta weights**: [CC-BY-NC-SA-4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
  * *Note*: License for the base LLaMA model's weights is Meta's [non-commercial bespoke license](https://github.com/facebookresearch/llama/blob/main/MODEL_CARD.md).
* **Contact**: For questions and comments about the model, visit the [CarperAI](https://discord.com/invite/KgfkCVYHdu) and [StableFoundation](https://discord.gg/stablediffusion) Discord servers.

| Hyperparameter            | Value |
|---------------------------|-------|
| \\(n_\text{parameters}\\) | 13B   |
| \\(d_\text{model}\\)      | 5120  |
| \\(n_\text{layers}\\)     | 40    |
| \\(n_\text{heads}\\)      | 40    |

## Training

### Training Dataset

StableVicuna-13B is fine-tuned on a mix of three datasets. [OpenAssistant Conversations Dataset (OASST1)](https://huggingface.co/datasets/OpenAssistant/oasst1), a human-generated, human-annotated assistant-style conversation corpus consisting of 161,443 messages distributed across 66,497 conversation trees, in 35 different languages;
[GPT4All Prompt Generations](https://huggingface.co/datasets/nomic-ai/gpt4all_prompt_generations), a dataset of 400k prompts and responses generated by GPT-4; and [Alpaca](https://huggingface.co/datasets/tatsu-lab/alpaca),  a dataset of 52,000 instructions and demonstrations generated by OpenAI's text-davinci-003 engine.

The reward model used during RLHF was also trained on [OpenAssistant Conversations Dataset (OASST1)](https://huggingface.co/datasets/OpenAssistant/oasst1) along with two other datasets: [Anthropic HH-RLHF](https://huggingface.co/datasets/Anthropic/hh-rlhf), a dataset of preferences about AI assistant helpfulness and harmlessness; and [Stanford Human Preferences Dataset](https://huggingface.co/datasets/stanfordnlp/SHP) a dataset of 385K collective human preferences over responses to questions/instructions in 18 different subject areas, from cooking to legal advice.

### Training Procedure

`CarperAI/stable-vicuna-13b-delta` was trained using PPO as implemented in [`trlX`](https://github.com/CarperAI/trlx/blob/main/trlx/trainer/accelerate_ppo_trainer.py) with the following configuration:

|  Hyperparameter   |  Value  |
|-------------------|---------|
| num_rollouts      | 128     |
| chunk_size        | 16      |
| ppo_epochs        | 4       |
| init_kl_coef      | 0.1     |
| target            | 6       |
| horizon           | 10000   |
| gamma             | 1       |
| lam               | 0.95    |
| cliprange         | 0.2     |
| cliprange_value   | 0.2     |
| vf_coef           | 1.0     |
| scale_reward      | None    |
| cliprange_reward  | 10      |
| generation_kwargs |         |
| max_length        | 512     |
| min_length        | 48      |
| top_k             | 0.0     |
| top_p             | 1.0     |
| do_sample         | True    |
| temperature       | 1.0     |

## Use and Limitations

### Intended Use

This model is intended to be used for text generation with a focus on conversational tasks. Users may further fine-tune the model on their own data to improve the model's performance on their specific tasks in accordance with the non-commercial [license](https://creativecommons.org/licenses/by-nc/4.0/).

### Limitations and bias

The base LLaMA model is trained on various data, some of which may contain offensive, harmful, and biased content that can lead to toxic behavior. See Section 5.1 of the LLaMA [paper](https://arxiv.org/abs/2302.13971). We have not performed any studies to determine how fine-tuning on the aforementioned datasets affect the model's behavior and toxicity. Do not treat chat responses from this model as a substitute for human judgment or as a source of truth. Please use responsibly.

## Acknowledgements

This work would not have been possible without the support of [Stability AI](https://stability.ai/).

## Citations

```bibtex
@article{touvron2023llama,
  title={LLaMA: Open and Efficient Foundation Language Models},
  author={Touvron, Hugo and Lavril, Thibaut and Izacard, Gautier and Martinet, Xavier and Lachaux, Marie-Anne and Lacroix, Timoth{\'e}e and Rozi{\`e}re, Baptiste and Goyal, Naman and Hambro, Eric and Azhar, Faisal and Rodriguez, Aurelien and Joulin, Armand and Grave, Edouard and Lample, Guillaume},
  journal={arXiv preprint arXiv:2302.13971},
  year={2023}
}
```

```bibtex
@misc{vicuna2023,
    title = {Vicuna: An Open-Source Chatbot Impressing GPT-4 with 90%* ChatGPT Quality},
    url = {https://vicuna.lmsys.org},
    author = {Chiang, Wei-Lin and Li, Zhuohan and Lin, Zi and Sheng, Ying and Wu, Zhanghao and Zhang, Hao and Zheng, Lianmin and Zhuang, Siyuan and Zhuang, Yonghao and Gonzalez, Joseph E. and Stoica, Ion and Xing, Eric P.},
    month = {March},
    year = {2023}
}
```

```bibtex
@misc{gpt4all,
  author = {Yuvanesh Anand and Zach Nussbaum and Brandon Duderstadt and Benjamin Schmidt and Andriy Mulyar},
  title = {GPT4All: Training an Assistant-style Chatbot with Large Scale Data Distillation from GPT-3.5-Turbo},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/nomic-ai/gpt4all}},
}
```

```bibtex
@misc{alpaca,
  author = {Rohan Taori and Ishaan Gulrajani and Tianyi Zhang and Yann Dubois and Xuechen Li and Carlos Guestrin and Percy Liang and Tatsunori B. Hashimoto },
  title = {Stanford Alpaca: An Instruction-following LLaMA model},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/tatsu-lab/stanford_alpaca}},
}
```

```bibtex
@software{leandro_von_werra_2023_7790115,
  author       = {Leandro von Werra and
                  Alex Havrilla and
                  Max reciprocated and
                  Jonathan Tow and
                  Aman cat-state and
                  Duy V. Phung and
                  Louis Castricato and
                  Shahbuland Matiana and
                  Alan and
                  Ayush Thakur and
                  Alexey Bukhtiyarov and
                  aaronrmm and
                  Fabrizio Milo and
                  Daniel and
                  Daniel King and
                  Dong Shin and
                  Ethan Kim and
                  Justin Wei and
                  Manuel Romero and
                  Nicky Pochinkov and
                  Omar Sanseviero and
                  Reshinth Adithyan and
                  Sherman Siu and
                  Thomas Simonini and
                  Vladimir Blagojevic and
                  Xu Song and
                  Zack Witten and
                  alexandremuzio and
                  crumb},
  title        = {{CarperAI/trlx: v0.6.0: LLaMa (Alpaca), Benchmark 
                   Util, T5 ILQL, Tests}},
  month        = mar,
  year         = 2023,
  publisher    = {Zenodo},
  version      = {v0.6.0},
  doi          = {10.5281/zenodo.7790115},
  url          = {https://doi.org/10.5281/zenodo.7790115}
}
```
