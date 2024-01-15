# MARIO

This is the official repository for the paper [MARIO: MAth Reasoning with code Interpreter Output -- A Reproducible Pipeline](www.arxiv.org). We release our codes.


MARIO REACT Corpus coming soon.


|     	| Base Model: Llemma                                           	| Outcome Value Model                                                    	| 
|-----	|---------------------------------------------------------------	|---------------------------------------------------------------------------	|
| 7B  	|[🤗](https://huggingface.co/MARIO/MARIO-7B)[🤖](https://www.modelscope.cn/models/damo/MARIO-7B)MARIO-7B| [🤗](https://huggingface.co/MARIO/MARIO-OVM-7B)[🤖](https://www.modelscope.cn/models/damo/MARIO-OVM-7B)MARIO-OVM-7B|
| 34B 	|[🤗](https://huggingface.co/MARIO/MARIO-34B)[🤖](https://www.modelscope.cn/models/damo/MARIO-34B)MARIO-34B||


## Performance
We demonstrate the results of our MARIO-7B and MARIO-34B as follows:

| **Model**             	| **Decoding** 	| **GSM**  	| **MATH** 	| **OCWCourse** | **Gaokao-2023-ME** | 
|---------------------------|---------------|-----------|-----------|-----------|-----------|
| MARIO-7B             	| **Hybrid**   	| 53.6  	| 31.5 	    | 44.5   	|44.5   	|
| ToRA-Coder-7B  	    | **Hybrid**   	| 72.6  	| 44.6  	| 4.8  	| 23.9	|
| MAmmoTH-Coder-7B  	    | **Hybrid**   	| 59.4  	| 33.4  	| 11.0  	| 15.3	|
| MathCoder-7B  	    | **Hybrid**   	| 59.4  	| 33.4  	| -  	|-   	|
| MetaMath-7B-Mistral       | **CoT**   	| 77.7  	| 28.2 	    | -      |-   	|
| OpenChat-3.5-7B           | **CoT**   	| 77.3 	    | 28.6 	    | -      |-   	|
| ChatGLM-3-6B              | **CoT**       | 72.3      | 25.7      | -  | - |

| **Model**             	| **Decoding** 	| **GSM**  	| **MATH** 	| **OCWCourse** | **Gaokao-2023-ME** | 
|---------------------------|---------------|-----------|-----------|-----------|-----------|
| MARIO-34B             	| **Hybrid**   	| 53.6  	| 31.5 	    | 44.5   	|44.5   	|
| ToRA-Coder-34B  	    | **Hybrid**   	| 80.7  	| 50.8  	| 5.5  	|31.7	|
| MAmmoTH-Coder-34B  	    | **Hybrid**   	| 59.4  	| 33.4  	| 14.0  |25.2 	|
| MathCoder-34B  	    | **Hybrid**   	| 59.4  	| 33.4  	| -	| -	|
| MetaMath-34B-Mistral       | **CoT**   	| 77.7  	| 28.2 	    | - |-	|
| DeepSeek-Coder-34B        | **PoT**   	| 58.2   	| 35.3 	    | -     |-	|
| QWen-72B                  | **CoT**       | 78.9      | 35.2      | -         |-   	|

## **Installation**

Clone this repository and install the required packages:

```bash
git clone https://github.com/MARIO-Math-Reasoning/MARIO.git
cd MARIO
pip install -r requirements.txt
pip install -e ./math_evaluation
```

## **Training and Inference**

Coming Soon

### **Quick Start**
To play with our model, run:

```python
from transformers import pipeline
pipeline = pipeline("text-generation", "TIGER-Lab/MAmmoTH-Coder-7B")

alpaca_template = "Below is an instruction that describes a task. Write a response that appropriately completes the request.\n### Instruction:\n{query}\n\n### Response:"

query = "Janet's ducks lay 16 eggs per day. She eats three for breakfast every morning and bakes muffins for her friends every day with four. She sells the remainder at the farmers' market daily for $2 per fresh duck egg. How much in dollars does she make every day at the farmers' market?"

### By default, MAmmoTH will output the Chain-of-thought (CoT) rationale
rationale_prefix = ""

### You can let MAmmoTH output Program-of-thought (PoT) rationale by simply adding
rationale_prefix = " Let's write a program."

input = alpaca_template.format(query = query + rationale_prefix)

output = pipeline(input)[0]['generated_text']
print(output)
```

### **Large-scale Evaluation with vllm**

To replicate the experimental results in our paper, run:

```bash
### For open-eneded questions, the dataset should be one of 
### ['gsm8k', 'svamp', 'math', 'numglue', 'deepmind', 'simuleq'] 
### We first try PoT and if the generated program is not executable, we shift to CoT

dataset='math'

python run_open.py \
  --model "TIGER-Lab/MAmmoTH-7B-Mistral" \
  --shots 0 \
  --stem_flan_type "pot_prompt" \
  --batch_size 8 \
  --dataset $dataset \
  --model_max_length 1500 \
  --cot_backup \
  --print \
  --use_vllm
```

## **Citation**

Please cite our paper if you use our data, model or code. Please also kindly cite the original dataset papers. 

```
@article{
}
```
