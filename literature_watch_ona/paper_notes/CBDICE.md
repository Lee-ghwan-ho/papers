# cbDice — Centerline Boundary Dice Loss for Vascular Segmentation

**KEY**: CBDICE  
**Category**: C (혈관 특화)  
**Status**: Accepted Conference Paper  
**Venue**: MICCAI 2024  
**GitHub**: https://github.com/PengchengShi1220/cbDice  
**Authors**: Shi et al.

---

## 핵심 요약

clDice loss의 한계를 해결하는 vascular segmentation용 손실 함수.

### clDice의 문제점
- Topology preservation은 잘 되지만 geometric detail (경계, 직경)을 놓침
- Translation/deformation에 취약
- Dice + clDice를 함께 쓰면 **large vessel에 편향** (diameter imbalance)

### cbDice의 해결책
- Centerline (topology 보존) + Boundary (geometric detail 보존)를 결합
- 직경 불균형 해소 → 크기에 무관하게 균형 있는 vascular segmentation

### 평가
2D/3D, binary/multi-class 포함 3개 vascular dataset. MICCAI 2023 TopCoW Challenge dataset에서 특히 우수.

---

## 내 연구와의 관계

### 동기 강화 (★★★★)

**cbDice의 핵심 발견**: clDice + Dice 조합이 large vessel에 편향되어 thin vessel이 상대적으로 불이익을 받는다.

이를 내 augmentation 동기에 연결:
> "The bias toward large vessels is not limited to loss functions: uniform augmentation strategies 
> similarly fail to account for the differential vulnerability of thin vessels. Just as cbDice 
> addresses diameter imbalance in the loss function, our method addresses it in the 
> augmentation budget."

### 구체적 인용 전략

**Introduction**: "Thin vessels are underrepresented not only in standard loss functions [cbDice] but also in uniform augmentation pipelines, where strong appearance perturbation can destroy weak structural evidence."

**Motivation 섹션**: cbDice가 loss level에서 해결하는 문제와 내가 aug level에서 해결하는 문제를 병렬로 제시.

---

## 평가 Metric으로의 활용

내 방법 평가 시 cbDice를 얇은 혈관 분할 성능 측정에 사용 고려.
특히 thin vessel과 thick vessel을 분리하여 metric 계산 시 cbDice 분석 방식 참고.
