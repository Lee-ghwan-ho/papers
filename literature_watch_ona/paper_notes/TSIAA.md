# TSIAA — Paper Note

> Run #8 신규 발견 (2026-06-28)  
> Novelty 위협도: HIGH  
> 즉시 full text 독해 필요

---

## 서지 정보

**제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Status**: Published Journal Article

---

## 핵심 방법

### Instance-level Image Augmenter (IIAG)

IIAG는 여러 개의 Instance-level Augmentation Modules (IAMs)로 구성된다.

**IAM의 역할**:
- 각 IAM은 이미지 내 하나의 segment region에 독립적으로 적용
- Learnable constrained Bézier transformation function을 사용해 각 region의 contrast를 비선형적으로 변환
- Bézier curve: `B(t) = (1-t)²P₀ + 2t(1-t)P₁ + t²P₂` (t = intensity ratio, P₁/P₂ = learnable control points)
- Control points는 제한된 범위 내에서 학습 → over-augmentation 방지

**Adversarial training**:
- Student model이 어렵다고 판단하는 방향으로 IAM의 control points를 optimizing
- "Out-of-source data distribution" 탐색: 학습 데이터 분포 밖의 어려운 sample 생성

### Teacher-Student (TS) 학습

- Augmented image → Student encoder → student feature
- Original image → Teacher encoder (EMA update) → teacher feature
- Consistency constraint: student와 teacher feature가 같은 domain-invariant representation을 학습하도록 유도

---

## 내 방법(Continuous-ONA)과의 비교

### 표면적 유사점

1. **공통 목표**: SSDG에서 appearance augmentation을 intra-image 단위로 차별화
2. **공통 기술**: Bézier curve 기반 nonlinear intensity transformation
3. **공통 주장**: "image-level uniform augmentation is insufficient" (TSIAA), "uniform augmentation budget is harmful to fragile structures" (ONA)

### 근본적 차이점

| 차이점 | TSIAA | Continuous-ONA |
|--------|-------|----------------|
| **augmentation 방향** | Adversarial = harder (model이 어렵다고 판단하는 방향) | Protective = conservative (얇은 혈관은 약하게) |
| **conditioning 변수** | 없음 (adversarially optimized per region) | Local vessel radius / observability score (GT 기반) |
| **thin vessel 처리** | 별도 처리 없음; adversarial이면 thin도 강하게 변형 가능 | 핵심 보호 대상: radius 작을수록 aug 강도 감소 |
| **conditioning 방식** | Spatial region partition (binary / patch-level) | Continuous scalar field per point (radius-based) |
| **문제의식** | diversity 부족 (intra-image) | label-image inconsistency (thin vessel fragility) |
| **학습 필요성** | 필요 (IAM parameters learnable, adversarial) | 불필요 (analytical radius → strength mapping) |
| **연산 복잡도** | adversarial loop 추가 필요 | radius 계산 + deterministic mapping |

### 구분 논거 (related work에서 활용)

> "TSIAA (Ref) breaks uniformity adversarially — all structures are pushed toward harder appearances. Our Continuous-ONA breaks uniformity in the opposite direction: resolved (thick) vessels receive stronger perturbations to suppress shortcut learning, while fragile (thin) vessels receive conservative perturbations to avoid label-image inconsistency. This protective asymmetry is the fundamental principle our work contributes."

---

## 내 연구에의 영향

1. **Related Work**: TSIAA를 "adversarial intra-image augmentation" 계열로 언급하고, 우리 방법이 "structure-observability conditioned augmentation"으로 구분되는 새로운 범주임을 주장
2. **Baseline**: TSIAA를 POC 비교군에 추가 여부 검토 (vs. ADA, ConStyX, SRCSM 등)
3. **동기 강화**: "adversarial 접근은 thin vessel을 더 강하게 변형할 수 있어 label-image inconsistency 위험이 있다"는 내 주장을 TSIAA와 대비해 강화 가능

---

## 독해 체크리스트 (미완료)

- [ ] IAM의 segment region은 어떻게 정의? (semantic label 기반? spatial grid? superpixel?)
- [ ] 각 IAM은 몇 개의 region을 담당?
- [ ] adversarial loop의 gradient 계산 방식
- [ ] Prostate / Cardiac / Brain tumor 등 어떤 4개 task에서 실험?
- [ ] thin vessel 성능 분석 있는지 확인
- [ ] 내 방법의 "thin vessel 보호" 부재를 TSIAA로 비판하는 리뷰어 시나리오 대비
