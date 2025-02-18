(supported-models)=

# Supported Models

vLLM supports generative and pooling models across various tasks.
If a model supports more than one task, you can set the task via the {code}`--task` argument.

For each task, we list the model architectures that have been implemented in vLLM.
Alongside each architecture, we include some popular models that use it.

## Loading a Model

### HuggingFace Hub

By default, vLLM loads models from [HuggingFace (HF) Hub](https://huggingface.co/models).

To determine whether a given model is supported, you can check the {code}`config.json` file inside the HF repository.
If the {code}`"architectures"` field contains a model architecture listed below, then it should be supported in theory.

````{tip}
The easiest way to check if your model is really supported at runtime is to run the program below:

```python
from vllm import LLM

# For generative models (task=generate) only
llm = LLM(model=..., task="generate")  # Name or path of your model
output = llm.generate("Hello, my name is")
print(output)

# For pooling models (task={embed,classify,reward,score}) only
llm = LLM(model=..., task="embed")  # Name or path of your model
output = llm.encode("Hello, my name is")
print(output)
```

If vLLM successfully returns text (for generative models) or hidden states (for pooling models), it indicates that your model is supported.
````

Otherwise, please refer to [Adding a New Model](#adding-a-new-model) and [Enabling Multimodal Inputs](#enabling-multimodal-inputs) for instructions on how to implement your model in vLLM.
Alternatively, you can [open an issue on GitHub](https://github.com/vllm-project/vllm/issues/new/choose) to request vLLM support.

### ModelScope

To use models from [ModelScope](https://www.modelscope.cn) instead of HuggingFace Hub, set an environment variable:

```shell
$ export VLLM_USE_MODELSCOPE=True
```

And use with {code}`trust_remote_code=True`.

```python
from vllm import LLM

llm = LLM(model=..., revision=..., task=..., trust_remote_code=True)

# For generative models (task=generate) only
output = llm.generate("Hello, my name is")
print(output)

# For pooling models (task={embed,classify,reward,score}) only
output = llm.encode("Hello, my name is")
print(output)
```

## List of Text-only Language Models

### Generative Models

See [this page](#generative-models) for more information on how to use generative models.

#### Text Generation (`--task generate`)

```{eval-rst}
.. list-table::
  :widths: 25 25 50 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`AquilaForCausalLM`
    - Aquila, Aquila2
    - :code:`BAAI/Aquila-7B`, :code:`BAAI/AquilaChat-7B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`ArcticForCausalLM`
    - Arctic
    - :code:`Snowflake/snowflake-arctic-base`, :code:`Snowflake/snowflake-arctic-instruct`, etc.
    -
    - ✅︎
  * - :code:`BaiChuanForCausalLM`
    - Baichuan2, Baichuan
    - :code:`baichuan-inc/Baichuan2-13B-Chat`, :code:`baichuan-inc/Baichuan-7B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`BloomForCausalLM`
    - BLOOM, BLOOMZ, BLOOMChat
    - :code:`bigscience/bloom`, :code:`bigscience/bloomz`, etc.
    -
    - ✅︎
  * - :code:`BartForConditionalGeneration`
    - BART
    - :code:`facebook/bart-base`, :code:`facebook/bart-large-cnn`, etc.
    -
    -
  * - :code:`ChatGLMModel`
    - ChatGLM
    - :code:`THUDM/chatglm2-6b`, :code:`THUDM/chatglm3-6b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`CohereForCausalLM`,:code:`Cohere2ForCausalLM`
    - Command-R
    - :code:`CohereForAI/c4ai-command-r-v01`, :code:`CohereForAI/c4ai-command-r7b-12-2024`, etc.
    - ✅︎
    - ✅︎
  * - :code:`DbrxForCausalLM`
    - DBRX
    - :code:`databricks/dbrx-base`, :code:`databricks/dbrx-instruct`, etc.
    -
    - ✅︎
  * - :code:`DeciLMForCausalLM`
    - DeciLM
    - :code:`Deci/DeciLM-7B`, :code:`Deci/DeciLM-7B-instruct`, etc.
    -
    - ✅︎
  * - :code:`DeepseekForCausalLM`
    - DeepSeek
    - :code:`deepseek-ai/deepseek-llm-67b-base`, :code:`deepseek-ai/deepseek-llm-7b-chat` etc.
    -
    - ✅︎
  * - :code:`DeepseekV2ForCausalLM`
    - DeepSeek-V2
    - :code:`deepseek-ai/DeepSeek-V2`, :code:`deepseek-ai/DeepSeek-V2-Chat` etc.
    -
    - ✅︎
  * - :code:`DeepseekV3ForCausalLM`
    - DeepSeek-V3
    - :code:`deepseek-ai/DeepSeek-V3-Base`, :code:`deepseek-ai/DeepSeek-V3` etc.
    -
    - ✅︎
  * - :code:`ExaoneForCausalLM`
    - EXAONE-3
    - :code:`LGAI-EXAONE/EXAONE-3.0-7.8B-Instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`FalconForCausalLM`
    - Falcon
    - :code:`tiiuae/falcon-7b`, :code:`tiiuae/falcon-40b`, :code:`tiiuae/falcon-rw-7b`, etc.
    -
    - ✅︎
  * - :code:`FalconMambaForCausalLM`
    - FalconMamba
    - :code:`tiiuae/falcon-mamba-7b`, :code:`tiiuae/falcon-mamba-7b-instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GemmaForCausalLM`
    - Gemma
    - :code:`google/gemma-2b`, :code:`google/gemma-7b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Gemma2ForCausalLM`
    - Gemma2
    - :code:`google/gemma-2-9b`, :code:`google/gemma-2-27b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GlmForCausalLM`
    - GLM-4
    - :code:`THUDM/glm-4-9b-chat-hf`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GPT2LMHeadModel`
    - GPT-2
    - :code:`gpt2`, :code:`gpt2-xl`, etc.
    -
    - ✅︎
  * - :code:`GPTBigCodeForCausalLM`
    - StarCoder, SantaCoder, WizardCoder
    - :code:`bigcode/starcoder`, :code:`bigcode/gpt_bigcode-santacoder`, :code:`WizardLM/WizardCoder-15B-V1.0`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GPTJForCausalLM`
    - GPT-J
    - :code:`EleutherAI/gpt-j-6b`, :code:`nomic-ai/gpt4all-j`, etc.
    -
    - ✅︎
  * - :code:`GPTNeoXForCausalLM`
    - GPT-NeoX, Pythia, OpenAssistant, Dolly V2, StableLM
    - :code:`EleutherAI/gpt-neox-20b`, :code:`EleutherAI/pythia-12b`, :code:`OpenAssistant/oasst-sft-4-pythia-12b-epoch-3.5`, :code:`databricks/dolly-v2-12b`, :code:`stabilityai/stablelm-tuned-alpha-7b`, etc.
    -
    - ✅︎
  * - :code:`GraniteForCausalLM`
    - Granite 3.0, Granite 3.1, PowerLM
    - :code:`ibm-granite/granite-3.0-2b-base`, :code:`ibm-granite/granite-3.1-8b-instruct`, :code:`ibm/PowerLM-3b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GraniteMoeForCausalLM`
    - Granite 3.0 MoE, PowerMoE
    - :code:`ibm-granite/granite-3.0-1b-a400m-base`, :code:`ibm-granite/granite-3.0-3b-a800m-instruct`, :code:`ibm/PowerMoE-3b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`GritLM`
    - GritLM
    - :code:`parasail-ai/GritLM-7B-vllm`.
    - ✅︎
    - ✅︎
  * - :code:`InternLMForCausalLM`
    - InternLM
    - :code:`internlm/internlm-7b`, :code:`internlm/internlm-chat-7b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`InternLM2ForCausalLM`
    - InternLM2
    - :code:`internlm/internlm2-7b`, :code:`internlm/internlm2-chat-7b`, etc.
    - ✅︎
    - ✅︎
  * - :code:`JAISLMHeadModel`
    - Jais
    - :code:`inceptionai/jais-13b`, :code:`inceptionai/jais-13b-chat`, :code:`inceptionai/jais-30b-v3`, :code:`inceptionai/jais-30b-chat-v3`, etc.
    -
    - ✅︎
  * - :code:`JambaForCausalLM`
    - Jamba
    - :code:`ai21labs/AI21-Jamba-1.5-Large`, :code:`ai21labs/AI21-Jamba-1.5-Mini`, :code:`ai21labs/Jamba-v0.1`, etc.
    - ✅︎
    - ✅︎
  * - :code:`LlamaForCausalLM`
    - Llama 3.1, Llama 3, Llama 2, LLaMA, Yi
    - :code:`meta-llama/Meta-Llama-3.1-405B-Instruct`, :code:`meta-llama/Meta-Llama-3.1-70B`, :code:`meta-llama/Meta-Llama-3-70B-Instruct`, :code:`meta-llama/Llama-2-70b-hf`, :code:`01-ai/Yi-34B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`MambaForCausalLM`
    - Mamba
    - :code:`state-spaces/mamba-130m-hf`, :code:`state-spaces/mamba-790m-hf`, :code:`state-spaces/mamba-2.8b-hf`, etc.
    -
    - ✅︎
  * - :code:`MiniCPMForCausalLM`
    - MiniCPM
    - :code:`openbmb/MiniCPM-2B-sft-bf16`, :code:`openbmb/MiniCPM-2B-dpo-bf16`, :code:`openbmb/MiniCPM-S-1B-sft`, etc.
    - ✅︎
    - ✅︎
  * - :code:`MiniCPM3ForCausalLM`
    - MiniCPM3
    - :code:`openbmb/MiniCPM3-4B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`MistralForCausalLM`
    - Mistral, Mistral-Instruct
    - :code:`mistralai/Mistral-7B-v0.1`, :code:`mistralai/Mistral-7B-Instruct-v0.1`, etc.
    - ✅︎
    - ✅︎
  * - :code:`MixtralForCausalLM`
    - Mixtral-8x7B, Mixtral-8x7B-Instruct
    - :code:`mistralai/Mixtral-8x7B-v0.1`, :code:`mistralai/Mixtral-8x7B-Instruct-v0.1`, :code:`mistral-community/Mixtral-8x22B-v0.1`, etc.
    - ✅︎
    - ✅︎
  * - :code:`MPTForCausalLM`
    - MPT, MPT-Instruct, MPT-Chat, MPT-StoryWriter
    - :code:`mosaicml/mpt-7b`, :code:`mosaicml/mpt-7b-storywriter`, :code:`mosaicml/mpt-30b`, etc.
    -
    - ✅︎
  * - :code:`NemotronForCausalLM`
    - Nemotron-3, Nemotron-4, Minitron
    - :code:`nvidia/Minitron-8B-Base`, :code:`mgoin/Nemotron-4-340B-Base-hf-FP8`, etc.
    - ✅︎
    - ✅︎
  * - :code:`OLMoForCausalLM`
    - OLMo
    - :code:`allenai/OLMo-1B-hf`, :code:`allenai/OLMo-7B-hf`, etc.
    -
    - ✅︎
  * - :code:`OLMo2ForCausalLM`
    - OLMo2
    - :code:`allenai/OLMo2-7B-1124`, etc.
    -
    - ✅︎
  * - :code:`OLMoEForCausalLM`
    - OLMoE
    - :code:`allenai/OLMoE-1B-7B-0924`, :code:`allenai/OLMoE-1B-7B-0924-Instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`OPTForCausalLM`
    - OPT, OPT-IML
    - :code:`facebook/opt-66b`, :code:`facebook/opt-iml-max-30b`, etc.
    -
    - ✅︎
  * - :code:`OrionForCausalLM`
    - Orion
    - :code:`OrionStarAI/Orion-14B-Base`, :code:`OrionStarAI/Orion-14B-Chat`, etc.
    -
    - ✅︎
  * - :code:`PhiForCausalLM`
    - Phi
    - :code:`microsoft/phi-1_5`, :code:`microsoft/phi-2`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Phi3ForCausalLM`
    - Phi-3
    - :code:`microsoft/Phi-3-mini-4k-instruct`, :code:`microsoft/Phi-3-mini-128k-instruct`, :code:`microsoft/Phi-3-medium-128k-instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Phi3SmallForCausalLM`
    - Phi-3-Small
    - :code:`microsoft/Phi-3-small-8k-instruct`, :code:`microsoft/Phi-3-small-128k-instruct`, etc.
    -
    - ✅︎
  * - :code:`PhiMoEForCausalLM`
    - Phi-3.5-MoE
    - :code:`microsoft/Phi-3.5-MoE-instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`PersimmonForCausalLM`
    - Persimmon
    - :code:`adept/persimmon-8b-base`, :code:`adept/persimmon-8b-chat`, etc.
    -
    - ✅︎
  * - :code:`QWenLMHeadModel`
    - Qwen
    - :code:`Qwen/Qwen-7B`, :code:`Qwen/Qwen-7B-Chat`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Qwen2ForCausalLM`
    - Qwen2
    - :code:`Qwen/QwQ-32B-Preview`, :code:`Qwen/Qwen2-7B-Instruct`, :code:`Qwen/Qwen2-7B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Qwen2MoeForCausalLM`
    - Qwen2MoE
    - :code:`Qwen/Qwen1.5-MoE-A2.7B`, :code:`Qwen/Qwen1.5-MoE-A2.7B-Chat`, etc.
    -
    - ✅︎
  * - :code:`StableLmForCausalLM`
    - StableLM
    - :code:`stabilityai/stablelm-3b-4e1t`, :code:`stabilityai/stablelm-base-alpha-7b-v2`, etc.
    -
    - ✅︎
  * - :code:`Starcoder2ForCausalLM`
    - Starcoder2
    - :code:`bigcode/starcoder2-3b`, :code:`bigcode/starcoder2-7b`, :code:`bigcode/starcoder2-15b`, etc.
    -
    - ✅︎
  * - :code:`SolarForCausalLM`
    - Solar Pro
    - :code:`upstage/solar-pro-preview-instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`TeleChat2ForCausalLM`
    - TeleChat2
    - :code:`TeleAI/TeleChat2-3B`, :code:`TeleAI/TeleChat2-7B`, :code:`TeleAI/TeleChat2-35B`, etc.
    - ✅︎
    - ✅︎
  * - :code:`XverseForCausalLM`
    - XVERSE
    - :code:`xverse/XVERSE-7B-Chat`, :code:`xverse/XVERSE-13B-Chat`, :code:`xverse/XVERSE-65B-Chat`, etc.
    - ✅︎
    - ✅︎
```

```{note}
Currently, the ROCm version of vLLM supports Mistral and Mixtral only for context lengths up to 4096.
```

### Pooling Models

See [this page](pooling-models) for more information on how to use pooling models.

```{important}
Since some model architectures support both generative and pooling tasks,
you should explicitly specify the task type to ensure that the model is used in pooling mode instead of generative mode.
```

#### Text Embedding (`--task embed`)

```{eval-rst}
.. list-table::
  :widths: 25 25 50 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`BertModel`
    - BERT-based
    - :code:`BAAI/bge-base-en-v1.5`, etc.
    -
    -
  * - :code:`Gemma2Model`
    - Gemma2-based
    - :code:`BAAI/bge-multilingual-gemma2`, etc.
    -
    - ✅︎
  * - :code:`GritLM`
    - GritLM
    - :code:`parasail-ai/GritLM-7B-vllm`.
    - ✅︎
    - ✅︎
  * - :code:`LlamaModel`, :code:`LlamaForCausalLM`, :code:`MistralModel`, etc.
    - Llama-based
    - :code:`intfloat/e5-mistral-7b-instruct`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Qwen2Model`, :code:`Qwen2ForCausalLM`
    - Qwen2-based
    - :code:`ssmits/Qwen2-7B-Instruct-embed-base` (see note), :code:`Alibaba-NLP/gte-Qwen2-7B-instruct` (see note), etc.
    - ✅︎
    - ✅︎
  * - :code:`RobertaModel`, :code:`RobertaForMaskedLM`
    - RoBERTa-based
    - :code:`sentence-transformers/all-roberta-large-v1`, :code:`sentence-transformers/all-roberta-large-v1`, etc.
    -
    -
  * - :code:`XLMRobertaModel`
    - XLM-RoBERTa-based
    - :code:`intfloat/multilingual-e5-large`, etc.
    -
    -
```

```{note}
{code}`ssmits/Qwen2-7B-Instruct-embed-base` has an improperly defined Sentence Transformers config.
You should manually set mean pooling by passing {code}`--override-pooler-config '{"pooling_type": "MEAN"}'`.
```

```{note}
Unlike base Qwen2, {code}`Alibaba-NLP/gte-Qwen2-7B-instruct` uses bi-directional attention.
You can set {code}`--hf-overrides '{"is_causal": false}'` to change the attention mask accordingly.

On the other hand, its 1.5B variant ({code}`Alibaba-NLP/gte-Qwen2-1.5B-instruct`) uses causal attention
despite being described otherwise on its model card.
```

If your model is not in the above list, we will try to automatically convert the model using
:func:`vllm.model_executor.models.adapters.as_embedding_model`. By default, the embeddings
of the whole prompt are extracted from the normalized hidden state corresponding to the last token.

#### Reward Modeling (`--task reward`)

```{eval-rst}
.. list-table::
  :widths: 25 25 50 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`LlamaForCausalLM`
    - Llama-based
    - :code:`peiyi9979/math-shepherd-mistral-7b-prm`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Qwen2ForRewardModel`
    - Qwen2-based
    - :code:`Qwen/Qwen2.5-Math-RM-72B`, etc.
    - ✅︎
    - ✅︎
```

If your model is not in the above list, we will try to automatically convert the model using
:func:`vllm.model_executor.models.adapters.as_reward_model`. By default, we return the hidden states of each token directly.

```{important}
For process-supervised reward models such as {code}`peiyi9979/math-shepherd-mistral-7b-prm`, the pooling config should be set explicitly,
e.g.: {code}`--override-pooler-config '{"pooling_type": "STEP", "step_tag_id": 123, "returned_token_ids": [456, 789]}'`.
```

#### Classification (`--task classify`)

```{eval-rst}
.. list-table::
  :widths: 25 25 50 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`JambaForSequenceClassification`
    - Jamba
    - :code:`ai21labs/Jamba-tiny-reward-dev`, etc.
    - ✅︎
    - ✅︎
  * - :code:`Qwen2ForSequenceClassification`
    - Qwen2-based
    - :code:`jason9693/Qwen2.5-1.5B-apeach`, etc.
    - ✅︎
    - ✅︎
```

If your model is not in the above list, we will try to automatically convert the model using
:func:`vllm.model_executor.models.adapters.as_classification_model`. By default, the class probabilities are extracted from the softmaxed hidden state corresponding to the last token.

#### Sentence Pair Scoring (`--task score`)

```{eval-rst}
.. list-table::
  :widths: 25 25 50 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`BertForSequenceClassification`
    - BERT-based
    - :code:`cross-encoder/ms-marco-MiniLM-L-6-v2`, etc.
    -
    -
  * - :code:`RobertaForSequenceClassification`
    - RoBERTa-based
    - :code:`cross-encoder/quora-roberta-base`, etc.
    -
    -
  * - :code:`XLMRobertaForSequenceClassification`
    - XLM-RoBERTa-based
    - :code:`BAAI/bge-reranker-v2-m3`, etc.
    -
    -
```

(supported-mm-models)=

## List of Multimodal Language Models

The following modalities are supported depending on the model:

- **T**ext
- **I**mage
- **V**ideo
- **A**udio

Any combination of modalities joined by {code}`+` are supported.

- e.g.: {code}`T + I` means that the model supports text-only, image-only, and text-with-image inputs.

On the other hand, modalities separated by {code}`/` are mutually exclusive.

- e.g.: {code}`T / I` means that the model supports text-only and image-only inputs, but not text-with-image inputs.

See [this page](#multimodal-inputs) on how to pass multi-modal inputs to the model.

### Generative Models

See [this page](#generative-models) for more information on how to use generative models.

#### Text Generation (`--task generate`)

```{eval-rst}
.. list-table::
  :widths: 25 25 15 20 5 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Inputs
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
    - V1
  * - :code:`AriaForConditionalGeneration`
    - Aria
    - T + I
    - :code:`rhymes-ai/Aria`
    -
    - ✅︎
    -
  * - :code:`Blip2ForConditionalGeneration`
    - BLIP-2
    - T + I\ :sup:`E`
    - :code:`Salesforce/blip2-opt-2.7b`, :code:`Salesforce/blip2-opt-6.7b`, etc.
    -
    - ✅︎
    -
  * - :code:`ChameleonForConditionalGeneration`
    - Chameleon
    - T + I
    - :code:`facebook/chameleon-7b` etc.
    -
    - ✅︎
    -
  * - :code:`FuyuForCausalLM`
    - Fuyu
    - T + I
    - :code:`adept/fuyu-8b` etc.
    -
    - ✅︎
    -
  * - :code:`ChatGLMModel`
    - GLM-4V
    - T + I
    - :code:`THUDM/glm-4v-9b` etc.
    - ✅︎
    - ✅︎
    -
  * - :code:`H2OVLChatModel`
    - H2OVL
    - T + I\ :sup:`E+`
    - :code:`h2oai/h2ovl-mississippi-800m`, :code:`h2oai/h2ovl-mississippi-2b`, etc.
    -
    - ✅︎
    -
  * - :code:`Idefics3ForConditionalGeneration`
    - Idefics3
    - T + I
    - :code:`HuggingFaceM4/Idefics3-8B-Llama3` etc.
    - ✅︎
    -
    -
  * - :code:`InternVLChatModel`
    - InternVL 2.5, Mono-InternVL, InternVL 2.0
    - T + I\ :sup:`E+`
    - :code:`OpenGVLab/InternVL2_5-4B`, :code:`OpenGVLab/Mono-InternVL-2B`, :code:`OpenGVLab/InternVL2-4B`, etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`LlavaForConditionalGeneration`
    - LLaVA-1.5
    - T + I\ :sup:`E+`
    - :code:`llava-hf/llava-1.5-7b-hf`, :code:`TIGER-Lab/Mantis-8B-siglip-llama3` (see note), etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`LlavaNextForConditionalGeneration`
    - LLaVA-NeXT
    - T + I\ :sup:`E+`
    - :code:`llava-hf/llava-v1.6-mistral-7b-hf`, :code:`llava-hf/llava-v1.6-vicuna-7b-hf`, etc.
    -
    - ✅︎
    -
  * - :code:`LlavaNextVideoForConditionalGeneration`
    - LLaVA-NeXT-Video
    - T + V
    - :code:`llava-hf/LLaVA-NeXT-Video-7B-hf`, etc.
    -
    - ✅︎
    -
  * - :code:`LlavaOnevisionForConditionalGeneration`
    - LLaVA-Onevision
    - T + I\ :sup:`+` + V\ :sup:`+`
    - :code:`llava-hf/llava-onevision-qwen2-7b-ov-hf`, :code:`llava-hf/llava-onevision-qwen2-0.5b-ov-hf`, etc.
    -
    - ✅︎
    -
  * - :code:`MiniCPMV`
    - MiniCPM-V
    - T + I\ :sup:`E+`
    - :code:`openbmb/MiniCPM-V-2` (see note), :code:`openbmb/MiniCPM-Llama3-V-2_5`, :code:`openbmb/MiniCPM-V-2_6`, etc.
    - ✅︎
    - ✅︎
    -
  * - :code:`MllamaForConditionalGeneration`
    - Llama 3.2
    - T + I\ :sup:`+`
    - :code:`meta-llama/Llama-3.2-90B-Vision-Instruct`, :code:`meta-llama/Llama-3.2-11B-Vision`, etc.
    -
    -
    -
  * - :code:`MolmoForCausalLM`
    - Molmo
    - T + I
    - :code:`allenai/Molmo-7B-D-0924`, :code:`allenai/Molmo-72B-0924`, etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`NVLM_D_Model`
    - NVLM-D 1.0
    - T + I\ :sup:`E+`
    - :code:`nvidia/NVLM-D-72B`, etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`PaliGemmaForConditionalGeneration`
    - PaliGemma, PaliGemma 2
    - T + I\ :sup:`E`
    - :code:`google/paligemma-3b-pt-224`, :code:`google/paligemma-3b-mix-224`, :code:`google/paligemma2-3b-ft-docci-448`, etc.
    -
    - ✅︎
    -
  * - :code:`Phi3VForCausalLM`
    - Phi-3-Vision, Phi-3.5-Vision
    - T + I\ :sup:`E+`
    - :code:`microsoft/Phi-3-vision-128k-instruct`, :code:`microsoft/Phi-3.5-vision-instruct` etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`PixtralForConditionalGeneration`
    - Pixtral
    - T + I\ :sup:`+`
    - :code:`mistralai/Pixtral-12B-2409`, :code:`mistral-community/pixtral-12b` etc.
    -
    - ✅︎
    - ✅︎
  * - :code:`QWenLMHeadModel`
    - Qwen-VL
    - T + I\ :sup:`E+`
    - :code:`Qwen/Qwen-VL`, :code:`Qwen/Qwen-VL-Chat`, etc.
    - ✅︎
    - ✅︎
    -
  * - :code:`Qwen2AudioForConditionalGeneration`
    - Qwen2-Audio
    - T + A\ :sup:`+`
    - :code:`Qwen/Qwen2-Audio-7B-Instruct`
    -
    - ✅︎
    -
  * - :code:`Qwen2VLForConditionalGeneration`
    - Qwen2-VL
    - T + I\ :sup:`E+` + V\ :sup:`E+`
    - :code:`Qwen/QVQ-72B-Preview`, :code:`Qwen/Qwen2-VL-7B-Instruct`, :code:`Qwen/Qwen2-VL-72B-Instruct`, etc.
    - ✅︎
    - ✅︎
    -
  * - :code:`UltravoxModel`
    - Ultravox
    - T + A\ :sup:`E+`
    - :code:`fixie-ai/ultravox-v0_3`
    -
    - ✅︎
    -
```

```{eval-rst}
:sup:`E` Pre-computed embeddings can be inputted for this modality.

:sup:`+` Multiple items can be inputted per text prompt for this modality.
```

````{important}
To enable multiple multi-modal items per text prompt, you have to set {code}`limit_mm_per_prompt` (offline inference)
or {code}`--limit-mm-per-prompt` (online inference). For example, to enable passing up to 4 images per text prompt:

```python
llm = LLM(
    model="Qwen/Qwen2-VL-7B-Instruct",
    limit_mm_per_prompt={"image": 4},
)
```

```bash
vllm serve Qwen/Qwen2-VL-7B-Instruct --limit-mm-per-prompt image=4
```
````

```{note}
vLLM currently only supports adding LoRA to the language backbone of multimodal models.
```

```{note}
To use {code}`TIGER-Lab/Mantis-8B-siglip-llama3`, you have pass {code}`--hf_overrides '{"architectures": ["MantisForConditionalGeneration"]}'` when running vLLM.
```

```{note}
The official {code}`openbmb/MiniCPM-V-2` doesn't work yet, so we need to use a fork ({code}`HwwwH/MiniCPM-V-2`) for now.
For more details, please see: <gh-pr:4087#issuecomment-2250397630>
```

### Pooling Models

See [this page](pooling-models) for more information on how to use pooling models.

```{important}
Since some model architectures support both generative and pooling tasks,
you should explicitly specify the task type to ensure that the model is used in pooling mode instead of generative mode.
```

#### Text Embedding (`--task embed`)

Any text generation model can be converted into an embedding model by passing {code}`--task embed`.

```{note}
To get the best results, you should use pooling models that are specifically trained as such.
```

The following table lists those that are tested in vLLM.

```{eval-rst}
.. list-table::
  :widths: 25 25 15 25 5 5
  :header-rows: 1

  * - Architecture
    - Models
    - Inputs
    - Example HF Models
    - :ref:`LoRA <lora-adapter>`
    - :ref:`PP <distributed-serving>`
  * - :code:`LlavaNextForConditionalGeneration`
    - LLaVA-NeXT-based
    - T / I
    - :code:`royokong/e5-v`
    -
    - ✅︎
  * - :code:`Phi3VForCausalLM`
    - Phi-3-Vision-based
    - T + I
    - :code:`TIGER-Lab/VLM2Vec-Full`
    - 🚧
    - ✅︎
  * - :code:`Qwen2VLForConditionalGeneration`
    - Qwen2-VL-based
    - T + I
    - :code:`MrLight/dse-qwen2-2b-mrl-v1`
    -
    - ✅︎
```

______________________________________________________________________

# Model Support Policy

At vLLM, we are committed to facilitating the integration and support of third-party models within our ecosystem. Our approach is designed to balance the need for robustness and the practical limitations of supporting a wide range of models. Here’s how we manage third-party model support:

1. **Community-Driven Support**: We encourage community contributions for adding new models. When a user requests support for a new model, we welcome pull requests (PRs) from the community. These contributions are evaluated primarily on the sensibility of the output they generate, rather than strict consistency with existing implementations such as those in transformers. **Call for contribution:** PRs coming directly from model vendors are greatly appreciated!
2. **Best-Effort Consistency**: While we aim to maintain a level of consistency between the models implemented in vLLM and other frameworks like transformers, complete alignment is not always feasible. Factors like acceleration techniques and the use of low-precision computations can introduce discrepancies. Our commitment is to ensure that the implemented models are functional and produce sensible results.

```{tip}
When comparing the output of {code}`model.generate` from HuggingFace Transformers with the output of {code}`llm.generate` from vLLM, note that the former reads the model's generation config file (i.e., [generation_config.json](https://github.com/huggingface/transformers/blob/19dabe96362803fb0a9ae7073d03533966598b17/src/transformers/generation/utils.py#L1945)) and applies the default parameters for generation, while the latter only uses the parameters passed to the function. Ensure all sampling parameters are identical when comparing outputs.
```

3. **Issue Resolution and Model Updates**: Users are encouraged to report any bugs or issues they encounter with third-party models. Proposed fixes should be submitted via PRs, with a clear explanation of the problem and the rationale behind the proposed solution. If a fix for one model impacts another, we rely on the community to highlight and address these cross-model dependencies. Note: for bugfix PRs, it is good etiquette to inform the original author to seek their feedback.
4. **Monitoring and Updates**: Users interested in specific models should monitor the commit history for those models (e.g., by tracking changes in the main/vllm/model_executor/models directory). This proactive approach helps users stay informed about updates and changes that may affect the models they use.
5. **Selective Focus**: Our resources are primarily directed towards models with significant user interest and impact. Models that are less frequently used may receive less attention, and we rely on the community to play a more active role in their upkeep and improvement.

Through this approach, vLLM fosters a collaborative environment where both the core development team and the broader community contribute to the robustness and diversity of the third-party models supported in our ecosystem.

Note that, as an inference engine, vLLM does not introduce new models. Therefore, all models supported by vLLM are third-party models in this regard.

We have the following levels of testing for models:

1. **Strict Consistency**: We compare the output of the model with the output of the model in the HuggingFace Transformers library under greedy decoding. This is the most stringent test. Please refer to [models tests](https://github.com/vllm-project/vllm/blob/main/tests/models) for the models that have passed this test.
2. **Output Sensibility**: We check if the output of the model is sensible and coherent, by measuring the perplexity of the output and checking for any obvious errors. This is a less stringent test.
3. **Runtime Functionality**: We check if the model can be loaded and run without errors. This is the least stringent test. Please refer to [functionality tests](gh-dir:tests) and [examples](gh-dir:main/examples) for the models that have passed this test.
4. **Community Feedback**: We rely on the community to provide feedback on the models. If a model is broken or not working as expected, we encourage users to raise issues to report it or open pull requests to fix it. The rest of the models fall under this category.
