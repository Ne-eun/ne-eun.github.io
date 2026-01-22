---
layout: default
title: Benchmark
parent: Others
nav_order: 2
has_children: true
has_toc: true
---

# Benchmark

## 주요 평가 지표

### Precision (정밀도)
- **정의**: 모델이 True로 예측한 것 중에 실제로 True인 것의 비율
- **의미**: 모델이 예측한 게 정확한지

### Recall (재현율)
- **정의**: 실제로 True인 것 중에 모델이 True로 답한 것의 비율
- **의미**: 모델이 놓친 정답이 없는지

### Precision과 Recall의 관계
Precision과 Recall은 서로 반대의 관계(Trade-off)를 가짐:
- 모델이 True를 남발할수록 → Recall ↑, Precision ↓
- 모델이 True를 신중하게 예측할수록 → Precision ↑, Recall ↓

---

## F-Score (F-Measure)

### $F_{\beta}$ Score
두 지표의 가중 조화평균:

$$
F_{\beta} = (1 + \beta^2) \cdot \frac{\text{Precision} \cdot \text{Recall}}{(\beta^2 \cdot \text{Precision}) + \text{Recall}}
$$

**β 값에 따른 특성**:
- `β = 1` (F1 Score): 둘의 균형을 중시
- `β > 1` (예: F2): 재현율(Recall)을 중시
- `β < 1` (예: F0.5): 정밀도(Precision)를 중시

### F1 Score
Precision과 Recall의 조화평균 (동일한 가중치):

$$
F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}
$$

### F2 Score (재현율 강조)

$$
F_2 = 5 \cdot \frac{\text{Precision} \cdot \text{Recall}}{(4 \cdot \text{Precision}) + \text{Recall}}
$$

### F0.5 Score (정밀도 강조)

$$
F_{0.5} = 1.25 \cdot \frac{\text{Precision} \cdot \text{Recall}}{(0.25 \cdot \text{Precision}) + \text{Recall}}
$$

---

## RAGAS - RAG 평가 방식

### Context Recall
- **정의**: 질문에 필요한 핵심정보를 얼마나 잘 가져왔는가?

### Context Precision
- **정의**: 가져온 정보가 얼마나 도움되는 정보인가?
- **예시**: Retriever가 DB의 모든 정보를 다 가져오면 Recall = 100%, Precision = 매우 낮음

### Answer Relevancy
- **정의**: 답변이 질문과 얼마나 관련성이 높은가?
- **측정 방법**:
  - 사용자의 질문과 답변을 벡터로 변환하여 코사인 유사도 측정
  - 답변을 기준으로 질문을 생성하고 원래 질문과 비교

### Faithfulness
- **정의**: 답변이 검색된 컨텍스트에 얼마나 충실한가?
- **측정 방법**:
  1. 답변을 원자적 주장(atomic claims)으로 분해
  2. 쪼개진 주장이 컨텍스트에 근거하는지 체크
  
$$
\text{Faithfulness} = \frac{\text{의미있는 주장의 수}}{\text{전체 주장 수}}
$$
