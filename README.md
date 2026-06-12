# Attention Comparison: RNN Seq2Seq NMT (Multi30k EN-DE)

RNN 기반 Seq2Seq 기계번역 모델에서 attention 방식에 따라 번역 성능과 오류 양상이 어떻게 달라지는지 비교한 프로젝트입니다.  
동일한 데이터셋과 기본 구조를 유지한 상태에서 attention 모듈만 바꿔 실험하는 형태로 구성되어 있습니다.

## Overview

- Task: English-German neural machine translation
- Architecture: RNN encoder-decoder with Seq2Seq
- Goal: attention 종류별 성능 비교 및 번역 오류 분석
- Main notebook: `cv_project.ipynb`

## Attention Variants

- Bahdanau (Additive) Attention
- Luong Global Dot Attention
- Scaled Dot-Product Attention
- Luong Local-m Attention

## Dataset

- Dataset: Multi30k (EN-DE)
- Split: Train 29k / Validation 1k / Test 1k
- Vocabulary size: EN 5,921 / DE 7,859
- Special tokens: `<pad>`, `<sos>`, `<eos>`, `<unk>`
- Rare words are mapped to `<unk>` when they appear only once

## Experiment Focus

- attention 방식별 번역 품질 비교
- 반복 생성, `<unk>` 발생, 문장 누락 같은 오류 패턴 확인
- 동일 조건에서 모듈 차이만 반영한 비교 실험

## Training Setup

- Encoder: Bi-GRU
- Decoder: GRU
- Embedding size: 256
- Hidden size: 512
- Optimizer: Adam (`lr = 0.0005`)
- Batch size: 64
- Epochs: 20
- Loss: CrossEntropyLoss (`label smoothing = 0.1`)
- Teacher forcing ratio: `1.0 -> 0.1`
- Dropout: Encoder `0.5`, Decoder `0.3`, Attention `0.1`
- Training device: NVIDIA A100 40GB

## Results

최종보고서 기준 BLEU 비교 결과는 다음과 같습니다.

| Model | BLEU |
| --- | ---: |
| Scaled Dot-Product Attention | 29.30 |
| Bahdanau (Baseline) | 29.17 |
| Luong Local-m Attention | 25.78 |
| Luong Global Dot Attention | 24.07 |

핵심 관찰 결과:

- Scaled Dot-Product Attention이 가장 높은 BLEU를 기록했습니다.
- Bahdanau와 Scaled Dot은 상대적으로 안정적인 성능을 보였습니다.
- Local-m은 대각선 정렬을 비교적 잘 유지했지만, 한 번 정렬이 틀어지면 오류가 크게 커지는 경향이 있었습니다.
- Dot 계열은 반복 생성과 후반부 정렬 붕괴가 더 자주 관찰되었습니다.
- 모든 모델에서 긴 문장 후반부로 갈수록 반복, 의미 손실, `<unk>` 발생이 증가했습니다.

## Qualitative Analysis

- `boston terrier`, `karate` 같은 희귀 표현은 여러 모델에서 `<unk>`로 치환되었습니다.
- Bahdanau와 Dot/Scaled Dot은 반복 생성 문제가 공통적으로 나타났습니다.
- Local-m은 반복은 다소 적었지만 명사 오번역 같은 다른 유형의 오류가 나타났습니다.
- attention heatmap 관찰 결과, Local-m은 대각선 정렬을 유지하는 경향이 강했고, 다른 모델은 문장 후반부에서 수직 붕괴 경향이 나타났습니다.

## Conclusion

- attention 메커니즘만 바꾸었을 때 성능 차이는 아주 크지 않았지만, 오류 양상은 분명하게 달라졌습니다.
- 번역 품질 개선 자체보다 attention 종류에 따라 정렬 특성과 반복 오류 패턴이 어떻게 달라지는지가 더 뚜렷하게 관찰되었습니다.
- RNN 기반 Seq2Seq 모델은 문장이 길어질수록 반복과 의미 손실이 증가하는 공통 한계를 보였습니다.

## Project Progression

- 1차 중간발표에서는 Transformer와 self-attention 구조를 중심으로 attention 메커니즘 자체를 먼저 정리했습니다.
- 2차 중간발표에서는 Bahdanau attention baseline을 직접 구현하고 Multi30k 기준 학습 결과와 heatmap 해석 틀을 정리했습니다.
- 최종발표 및 최종보고서에서는 Bahdanau, Dot, Local-m, Scaled Dot 네 가지 attention을 동일 조건에서 비교해 BLEU와 정성 오류 양상을 함께 분석했습니다.

이 프로젝트는 단순 성능 비교보다, attention 형태가 alignment와 반복 오류에 어떤 차이를 만드는지 추적하는 데 초점을 둡니다.


