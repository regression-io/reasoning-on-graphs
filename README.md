# Reasoning on Graphs (RoG)
Official Implementation of "[Reasoning on Graphs: Faithful and Interpretable Large Language Model Reasoning](https://arxiv.org/abs/2310.01061)".

<img src="resources/rog.png" width = "600" />

Reasoning on graphs (RoG) synergizes LLMs with KGs to enable faithful and interpretable reasoning. We present a planning-retrieval-reasoning framework, where RoG first generates relation paths grounded by KGs as faithful plans. These plans are then used to retrieve valid reasoning paths from the KGs for LLMs to conduct faithful reasoning and generate interpretable results.

## Requirements
```
pip install -r requirements.txt
```

## Pre-trained weights
You can find the pre-trained weights [here](https://huggingface.co/rmanluo/RoG).

## Datasets
[RoG-WebQSP](https://huggingface.co/datasets/rmanluo/RoG-webqsp)   
[RoG-CWQ](https://huggingface.co/datasets/rmanluo/RoG-cwq)
## Inference

### Step1: Planning (Generate relation paths)
Run: `./scripts/planning.sh`
```bash
python src/align_kg/gen_rule_path.py \
--model_name RoG \
--model_path rmanluo/RoG \
-d {RoG-webqsp,RoG-cwq} \
--split test \
--n_beam 3
```
Generated rules will be saved at: `results/gen_rule_path/{dataset}/{model_name}/{split}`
### Step2: Reasoning (Generate answers with RoG)
Run: `./scripts/rog-reasoning.sh`
```bash
python src/qa_prediction/predict_answer.py \
--model_name RoG \
--model_path rmanluo/RoG \
-d {RoG-webqsp,RoG-cwq} \
--prompt_path prompts/llama2_predict.txt \
--add_rul \
--rule_path {rule_path} \
```
Answers will be saved at: `results/KGQA/{dataset}/{model_name}/{split}`

### Plug-and-play Reasoning (Generate answers with different LLMs)
> Note: you need to set your openai key at `.env` to use ChatGPT.

Run: `./scripts/plug-and-play.sh`
```bash
python src/qa_prediction/predict_answer.py \
                --model_name {gpt-3.5-turbo,alpaca,llama2-chat-hf,flan-t5} \
                -d {RoG-webqsp,RoG-cwq} \
                --prompt_path {prompt_path} \
                --add_rule \
                --rule_path {rule_path}
```

## Training

Training code will be available soon.

## Results
<img src="resources/results.png" width = "600" />
<img src="resources/lack_of_knowledge.png" width = "600" />
<img src="resources/hallucination.png" width = "600" />