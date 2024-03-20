[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/hm5nZYSf)
# NLP Dialogue Summarization | 일상 대화 요약
## 3조

| ![박패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![이패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![최패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![김패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![오패캠](https://avatars.githubusercontent.com/u/156163982?v=4) |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: |
|            [김태한](https://github.com/UpstageAILab)             |            [김소현](https://github.com/UpstageAILab)             |            [권혁찬](https://github.com/UpstageAILab)             |            [문정의](https://github.com/UpstageAILab)             |            [이현진](https://github.com/UpstageAILab)             |
|                            팀장, 리서치, 모델링                             |                            발표, 리서치, 모델링                             |                            리서치, 모델링                             |                            리서치, 모델링                             |                            리서치, 모델링                             |

## 0. Overview
### Environment
- AMD Ryzen Threadripper 3960X 24-Core Processor
- NVIDIA GeForce RTX 3090
- CUDA Version 12.2

### Requirements
- pandas==2.1.4
- numpy==1.23.5
- wandb==0.16.1
- tqdm==4.66.1
- pytorch_lightning==2.1.2
- transformers[torch]==4.35.2
- rouge==1.0.1
- jupyter==1.0.0
- jupyterlab==4.0.9

## 1. Competiton Info

### Overview

- 이번 대회에서 일상 대화를 바탕으로 요약문을 생성하는 모델을 구축함이 목적입니다. 제공되는 데이터셋은 오직 "대화문과 요약문"입니다. 회의, 일상 대화 등 다양한 주제를 가진 대화문과, 이에 대한 요약문을 포함하고 있습니다.
- 참가자들은 이러한 비정형 텍스트 데이터를 고려하여 모델을 훈련하고, 요약문의 생성 성능을 높이기 위한 최적의 방법을 찾아야 합니다.

### Timeline

- March 08, 2024 - Start Date
- March 06, 2024 - Mentoring1 (Before Competition)
- March 15, 2024 - Mentoring2
- March 20, 2024 - Mentoring3
- March 20, 2024 - Final submission deadline

## 2. Components

### Directory

- _Insert your directory structure_

e.g.
```
├── code
│   ├── jupyter_notebooks
│   │   └── model_train.ipynb
│   └── train.py
├── docs
│   ├── pdf
│   │   └── (Template) [패스트캠퍼스] Upstage AI Lab 1기_그룹 스터디 .pptx
│   └── paper
└── input
    └── data
        ├── eval
        └── train
```
```
├── code
│   ├── baseline_code_05_linux.ipynb
│   ├── baseline_code_05_linux_model2.ipynb
│   └── baseline_code_05_linux_model3.ipynb
├── docs
│   └── [패스트캠퍼스] Upstage AI Lab 1기_그룹 스터디_3조.pptx.pdf
└── data
    ├── test
    ├── train
    ├── train.csv
    ├── meta.csv
    └── sample_submission.csv
```

## 3. Data descrption

### Dataset overview

![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/assets/data.png)


모든 데이터는 .csv 형식으로 제공되고 있으며, 각각의 데이터 건수는 다음과 같습니다.
dev는 validation 데이터이며, test는 public, hidden-test는 private test 데이터입니다.

- train : 12457
- dev : 499
- test : 250
- hidden-test : 249

### EDA

- _Describe your EDA process and step-by-step conclusion_

### Data Processing

- _Describe data processing process (e.g. Data Labeling, Data Cleaning..)_

## 4. Modeling

### Model descrition

- _Write model information and why your select this model_

### Modeling Process

- _Write model train and test process with capture_

## 5. Result

### Leader Board

- _Insert Leader Board Capture_
- _Write rank and score_

### Presentation

- _Insert your presentaion file(pdf) link_

## etc

### Meeting Log

- [1~2주차 (3/8 ~ 3/15)](https://quickest-asterisk-75d.notion.site/1-2-03-08-03-15-516f22361648486c825ca5e5848651b4)
- [3주차 (3/18 ~ 3/21)](https://quickest-asterisk-75d.notion.site/2-3-18-21-f7d2df8ba77b438fa6ace836c7eb7184)

### Reference

- _Insert related reference_
