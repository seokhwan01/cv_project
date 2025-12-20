# Attention 비교 실험: RNN Seq2Seq NMT (Multi30k EN-DE)

RNN 기반 Seq2Seq 신경망 기계번역에서 attention 메커니즘 종류에 따른 번역 성능 및 오류 양상(반복 생성, `<unk>`, 긴 문장 의미 소실)을 비교합니다.  
동일한 데이터셋과 동일한 모델 구조를 유지한 채 attention 모듈만 교체하여 실험했습니다.

## Models
- Bahdanau (Additive) Attention (Baseline)
- Dot Attention (Luong Global-Dot)
- Scaled Dot-Product Attention
- Luong Local-m Attention

## Dataset
- Multi30k (English–German)
- Split: Train 29k / Val 1k / Test 1k
- Vocab: EN 5,921 / DE 7,859
- Rare word 처리: 1회 등장 단어 → `<unk>`

## Environment
- Python: [버전]
- PyTorch: [버전]
- GPU: NVIDIA A100 40GB (optional)

## Project Structure (예시)
