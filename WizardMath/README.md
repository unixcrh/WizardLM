# WizardMath : Empowering Mathematical Reasoning for Large Language Models via Reinforced Evol-Instruct

[![Code License](https://img.shields.io/badge/Code%20License-Apache_2.0-green.svg)](CODE_LICENSE)
[![Data License](https://img.shields.io/badge/Data%20License-CC%20By%20NC%204.0-red.svg)](DATA_LICENSE)
[![Model Weight License](https://img.shields.io/badge/Model%20Weights%20License-LLaMA2-yellow)](WizardMath/LICENSE)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/release/python-390/)

To develop our WizardMath model, we begin by adapting the Evol-Instruct method specifically for math tasks, like GAM8k and MATH. This involves tailoring the prompt to the domain of math-related instructions. Subsequently, we fine-tune the LLaMA 2, utilizing the newly created instruction-following math training set.

## News
- 🔥 Our **WizardMath-70B-V1.0** model achieves slightly outperforms some closed-source LLMs on the GSM8K, including ChatGPT, Claude Instant, PaLM 2 540B.
- 🔥 Our **WizardMath-70B-V1.0** model achieves the **81.58 pass@1** on the [GSM8k Benchmarks](https://github.com/openai/grade-school-math), which is **24.8** points higher than the SOTA open-source LLMs.
- 🔥 Our **WizardMath-70B-V1.0** model achieves the **22.72 pass@1** on the [MATH Benchmarks](https://github.com/hendrycks/math), which is **9.2** points higher than the SOTA open-source LLMs.
- &#x1F4E3; Please refer to our Twitter account https://twitter.com/WizardLM_AI and HuggingFace Repo https://huggingface.co/WizardLM . We will use them to announce any new release at the 1st time. 


## Comparing WizardMath with the LLM models.

🔥 The following figure shows that our **WizardMath attains the fifth position in this GSM8k benchmark**, surpassing Claude Instant (81.6 vs. 80.9), ChatGPT (81.6 vs. 80.8) and PaLM 2 540B (81.6 vs. 80.7). Notably, our model exhibits a substantially smaller size compared to these models.

<p align="center" width="100%">
<a ><img src="imags/wizardmath_gsm8k.png" alt="WizardMath" style="width: 86%; min-width: 300px; display: block; margin: auto;"></a>
</p>

❗❗❗**Note: This performance is 100% reproducible! If you cannot reproduce it, please follow the steps in [Evaluation](#evaluation).**

❗❗❗**Note: The scores of ChatGPT reported by [Model Selection](https://arxiv.org/pdf/2305.14333v1.pdf) are 80.8%.**

The following table clearly demonstrates that our **WizardMath** exhibits a substantial performance advantage over all the open-source models on the GSM8k and MATH benchmarks. ❗ **If you are confused with the different scores of our 7B, 13B and 70B models (54.9, 63.9 and 81.6), please check the Notes.**


| Model               | GSM8k Pass@1 | MATH Pass@1 |
|---------------------|--------------|-------------|
| MPT-7B              | 6.8          | 3.0         |
| Falcon-7B           | 6.8          | 2.3         |
| LLaMA-1-7B          | 11.0         | 2.9         |
| LLaMA-2-7B          | 14.6         | 2.5         |
| MPT-30B             | 15.2         | 3.1         |
| LLaMA-1-13B         | 17.8         | 3.9         |
| GPT-Neo-2.7B        | 19.5         | --          |
| Falcon-40B          | 19.6         | 2.5         |
| Baichuan-chat-13B   | 23.9         | --          |
| Vicuna-v1.3-13B     | 27.6         | --          |
| LLaMA-2-13B         | 28.7         | 3.9         |
| InternLM-7B         | 31.2         | --          |
| ChatGLM-2-6B        | 32.4         | --          |
| GPT-J-6B            | 34.9         | --          |
| LLaMA-1-33B         | 35.6         | 3.9         |
| LLaMA-2-34B         | 42.2         | 6.24        |
| RFT-7B              | 50.3         | --          |
| LLaMA-1-65B         | 50.9         | 10.6        |
| Qwen-7B             | 51.6         | --          |
| WizardMath-7B-v1.0  | **54.9**     | **10.7**    |
| LLaMA-2-70B         | 56.8         | 13.5        |
| WizardMath-13B-v1.0 | **63.9**     | **14.0**    |
| WizardMath-70B-v1.0 | **81.6**     | **22.7**    |


❗ **Note: The above table conducts a comprehensive comparison of our **WizardMath** with other models on the GSM8k and MATH benchmarks. In this study, to ensure equitable and cohesive evaluations, we report the socres of all models within the settings of greedy decoding and CoT.**

## Contents

1. [Online Demo](#online-demo)

2. [Fine-tuning](#fine-tuning)

3. [Inference](#inference)

4. [Evaluation](#evaluation)

5. [Citation](#citation)

6. [Disclaimer](#disclaimer)

## Online Demo

We will provide our latest models for you to try for as long as possible. If you find a link is not working, maybe there are a lot of people visiting at the same time, so please be patient !

[Demo Link](xxxxxx)

## Fine-tuning

We fine-tune WizardMath using the modified code `WizardMath/train/train_wizardmath.py` from [Llama-X](https://github.com/AetherCortex/Llama-X).
We fine-tune WizardMath-13B with the following hyperparameters:

| Hyperparameter | LLaMA 2 13B |
|----------------|-------------|
| Batch size     | 128         |
| Learning rate  | 2e-5        |
| Epochs         | 3           |
| Max length     | 2048        |
| LR scheduler   | cosine      |

To reproduce our fine-tuning of WizardMath, please follow the following steps:
1. According to the instructions of [Llama-X](https://github.com/AetherCortex/Llama-X), install the environment, download the training code, and deploy. (Note: `deepspeed==0.10.0` and `transformers==4.31.0`)
2. Replace the `train.py` with the `train_wizardmath.py` in our repo (`WizardMath/train/train_wizardmath.py`)
3. Login Huggingface:
```bash
huggingface-cli login
```
4. Execute the following training command:
```bash
deepspeed train_wizardmath.py \
    --model_name_or_path "/your/path/to/llama-2-13b" \
    --data_path  "/your/path/to/math_instruction_data.json"\
    --output_dir  "/your/path/to/save_ckpt"\
    --num_train_epochs 3 \
    --model_max_length 1536 \
    --per_device_train_batch_size 16 \
    --per_device_eval_batch_size 1 \
    --gradient_accumulation_steps 1 \
    --evaluation_strategy "no" \
    --save_strategy "steps" \
    --save_steps 50 \
    --save_total_limit 50 \
    --learning_rate 2e-5 \
    --warmup_steps 10 \
    --logging_steps 2 \
    --lr_scheduler_type "cosine" \
    --report_to "tensorboard" \
    --gradient_checkpointing True \
    --deepspeed config/deepspeed_config.json \
    --fp16 True \
```

## Inference

We provide the decoding script for WizardMath, which reads a input file and generates corresponding responses for each sample, and finally calculate the score.

###  Install Inference environment :
Note: We used vllm for inference which can speed up inference and save time. Please refer to the official github [vllm](https://github.com/vllm-project/vllm/tree/main) for questions about vllm installation.
```bash
conda create -n wizardmath python=3.8 -y
conda activate wizardmath
pip install vllm
pip install jsonlines
pip install Fraction
pip install openai
```

## Evaluation

The inference prompt for our WizardMath is:
```
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
{instruction}

### Response: Let's think step by step.
```


### GSM8k benchmarks

1. The format of `gsm8k_test.jsonl` should be:
```
{"question": "Janet\u2019s ducks lay 16 eggs per day. She eats three for breakfast every morning and bakes muffins for her friends every day with four. She sells the remainder at the farmers' market daily for $2 per fresh duck egg. How much in dollars does she make every day at the farmers' market?", "answer": "Janet sells 16 - 3 - 4 = <<16-3-4=9>>9 duck eggs a day.\nShe makes 9 * 2 = $<<9*2=18>>18 every day at the farmer\u2019s market.\n#### 18"}
{"question": "A robe takes 2 bolts of blue fiber and half that much white fiber.  How many bolts in total does it take?", "answer": "It takes 2/2=<<2/2=1>>1 bolt of white fiber\nSo the total amount of fabric is 2+1=<<2+1=3>>3 bolts of fabric\n#### 3"}
{"question": "Josh decides to try flipping a house.  He buys a house for $80,000 and then puts in $50,000 in repairs.  This increased the value of the house by 150%.  How much profit did he make?", "answer": "The cost of the house and repairs came out to 80,000+50,000=$<<80000+50000=130000>>130,000\nHe increased the value of the house by 80,000*1.5=<<80000*1.5=120000>>120,000\nSo the new value of the house is 120,000+80,000=$<<120000+80000=200000>>200,000\nSo he made a profit of 200,000-130,000=$<<200000-130000=70000>>70,000\n#### 70000"}
{"question": "James decides to run 3 sprints 3 times a week.  He runs 60 meters each sprint.  How many total meters does he run a week?", "answer": "He sprints 3*3=<<3*3=9>>9 times\nSo he runs 9*60=<<9*60=540>>540 meters\n#### 540"}
.....
```

2. Run the following script to generate the answer.
```bash
python gsm8k_test_wizardmath.py --data_files WizardMath/data/gsm8k_test.jsonl --model "/your/path/to/load_ckpt" --batch_size 60 --tensor_parallel_size 1
```
You can specify `tensor_parallel_size` , which indicates the number of gpus. You are able to slice the datasets using the `start` and `end`.


### MATH benchmarks

1. The format of `MATH_test.jsonl` should be:
```
{"idx": "hendrycks_math_1", "instruction": "Find the matrix $\\mathbf{M}$ such that\n\\[\\mathbf{M} \\begin{pmatrix} 1 & -2 \\\\ 1 & 4 \\end{pmatrix} = \\begin{pmatrix} 6 & 0 \\\\ 0 & 6 \\end{pmatrix}.\\]", "output": "The inverse of $\\begin{pmatrix} 1 & -2 \\\\ 1 & 4 \\end{pmatrix}$ is\n\\[\\frac{1}{(1)(4) - (-2)(1)} \\begin{pmatrix} 4 & 2 \\\\ -1 & 1 \\end{pmatrix} = \\frac{1}{6} \\begin{pmatrix} 4 & 2 \\\\ -1 & 1 \\end{pmatrix}.\\]So, multiplying by this inverse on the right, we get\n\\[\\mathbf{M} = \\begin{pmatrix} 6 & 0 \\\\ 0 & 6 \\end{pmatrix} \\cdot \\frac{1}{6} \\begin{pmatrix} 4 & 2 \\\\ -1 & 1 \\end{pmatrix} = \\boxed{\\begin{pmatrix} 4 & 2 \\\\ -1 & 1 \\end{pmatrix}}.\\]", "input": "", "type": "Precalculus"}
{"idx": "hendrycks_math_2", "instruction": "Compute $\\arccos (-1).$  Express your answer in radians.", "output": "Since $\\cos \\pi = -1,$ $\\arccos (-1) = \\boxed{\\pi}.$", "input": "", "type": "Precalculus"}
.....
```

2. Run the following script to generate the answer.
```bash
python MATH_test_wizardmath.py --data_files WizardMath/data/MATH_test.jsonl --model "/your/path/to/load_ckpt" --batch_size 50 --tensor_parallel_size 1
```
You can specify `tensor_parallel_size` , which indicates the number of gpus. You are able to slice the datasets using the `start` and `end`.


## Disclaimer

WizardMath model follows the same license as LLaMA 2. The content produced by any version of WizardMath is influenced by uncontrollable variables such as randomness, and therefore, the accuracy of the output cannot be guaranteed by this project. This project does not accept any legal liability for the content of the model output, nor does it assume responsibility for any losses incurred due to the use of associated resources and output results.

