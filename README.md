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

### Evaluation Metric
![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/blob/main/assets/rouge01.png)

## 2. Components

### Directory

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

![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/blob/main/assets/data.png)


모든 데이터는 .csv 형식으로 제공되고 있으며, 각각의 데이터 건수는 다음과 같습니다.  
dev는 validation 데이터이며, test는 public, hidden-test는 private test 데이터입니다.

- train : 12457
- dev : 499
- test : 250
- hidden-test : 249

### EDA

- _Describe your EDA process and step-by-step conclusion_

### Data Processing

- EasyDataAugmentation (EDA)
  - RandomDeletion (RD)
  - RandomInsertion (RI)
  - SynonymReplacement (SR)
  - RandomSwap (RS)
- AEasierDataAugmentation (AEDA)

```
from koeda import AEasierDataAugmentation
from koeda import EasyDataAugmentation

eda = EasyDataAugmentation(
              morpheme_analyzer = "Okt",
              alpha_sr = 0.1,
              alpha_ri = 0.1,
              alpha_rs = 0.1,
              prob_rd = 0.1
            )

repetition = 1

aeda = AEasierDataAugmentation(
        morpheme_analyzer="Okt", punctuations=[".", ",", "!", "?", ";", ":"]
    )

print("원문:", ex_data)
# First, apply EDA
result = eda(ex_data, repetition=repetition)
print("EDA:", result)
# Second, apply AEDA
result = aeda(ex_data, p=0.3, repetition=repetition)
print("AEDA:", result)
```
![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/blob/main/assets/eda_aeda.png)

- BT (Back Translation)
  
한글의 경우 EasyDataAugmentation 보다는 BT가 성능 향상이 좋음

```
from googletrans import Translator


def augment_text_data_with_BT(text,repetition):
    """입력된 문장에 대해서 BT를 통해 데이터 증강"""
    # Translator 객체 생성
    translator = Translator()
    result = []

    dialogue_list = text.split('\n')

    try :

        for dialogue in dialogue_list:
            str_temp = dialogue.split(':')

            # 번역 실행 (한국어 > 영어 > 한국어)
            for i in range(repetition):
                #print(str_temp[1])
                translated = translator.translate(str_temp[1], src='ko', dest='en')
                re_translated = translator.translate(translated.text, src='en', dest='ko')
                result.append(str_temp[0] + ':' + re_translated.text)

    except Exception as e:    # 모든 예외의 에러 메시지를 출력할 때는 Exception을 사용
        print('예외가 발생했습니다.', e)                

    return "\n".join(result)
```

## 4. Modeling

### Model description
- KoBART  

![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/blob/main/assets/bart_model.png)

Text Summarization에는 Machine Reading Comprehension과  
Text Generation 모두가 필요한 Encoder-Decoder 모델인 KoBART를 사용했습니다.  
  
huggingface의 digit82/kobart-summarization.

### Modeling Process

- _Write model train and test process with capture_
- 다양한 모델과 하이퍼파라미터 튜닝을 진행하였으나, 성능향상이 미미함
  ![image](https://github.com/UpstageAILab/upstage-nlp-summarization-nlp3/blob/main/assets/wanda01.png)

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

- [koEDA](https://github.com/toriving/KoEDA)
- [Huggingface Korean Summarization Models](https://huggingface.co/models?pipeline_tag=summarization&language=ko&sort=trending)
