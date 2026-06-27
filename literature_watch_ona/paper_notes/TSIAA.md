# Paper Note: TSIAA

## 기본 정보

| 항목 | 내용 |
|------|------|
| KEY | TSIAA |
| 제목 | Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation |
| 저자 | Zhengshan Wang et al. |
| Venue | IEEE Transactions on Medical Imaging |
| Year | 2026 |
| Volume / Pages | vol 45, pp 764–776 |
| DOI/IEEEXplore | https://ieeexplore.ieee.org/document/11146907/ |
| Status | Published Journal Article |
| Category | A (직접 경쟁) |
| Novelty 위험도 | ⚠️ 높음 — 즉시 독해 필요 |

---

## 핵심 방법

### 구조

- **IIAG (Instance-level Image Augmenter)**: 복수의 IAM(Instance-level Augmentation Module)으로 구성  
- 각 **IAM**: learnable constrained Bézier transformation function 기반  
- **Teacher-Student (TS) learning**: adversarial 구조  
  - Teacher: 새로운 image augmentation 탐색 (out-of-source data distribution)  
  - Student: 원본 + 증강 feature의 consistent + generalized representation 학습  

### 학습 전략

- Alternating training: augmentation 탐색 ↔ representation generalization  
- Over-augmentation 방지: augmenter의 학습 강도를 제약

### 실험

- Prostate MRI: NCI-ISBI13 (source) → 5 other sites (target)
- Fundus segmentation: REFUGE training set (source) → 3 target sites
- Four challenging SDG tasks에서 SOTA

---

## 내 방법 (Continuous-ONA)과의 비교

### 공통점 (위험 요소)

TSIAA 논문의 핵심 서술:
> "Compared to image-level adversarial augmentation, instance-level adversarial augmentation **breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity."

이 문장은 내 핵심 주장과 단어 수준에서 거의 동일:
> "Tubular structures should not receive a uniform augmentation budget."

### 차이점 (구분 논거)

| 비교 축 | TSIAA | Continuous-ONA (내 방법) |
|---------|-------|--------------------------|
| "instance-level" 정의 | semantic instance 단위 (각 전경 region/patch)가 다른 Bézier 파라미터를 adversarially 학습 | vessel foreground 단일 class 내 local radius(물리량)에 따른 연속 조절 |
| Aug. Condition | Adversarially learned (discrete, per-instance) | Annotation-derived continuous (local radius → budget) |
| Structure Signal | ❌ 구조의 물리적 속성 미사용 | ✅ local vessel radius / vesselness |
| Label Safety | ❌ 얇은 구조 보호 개념 없음 (diversity 극대화가 목적) | ✅ thin vessel label-image consistency 보호 |
| 동기 | 이미지 단위 균일 aug의 다양성 한계 극복 | 얇은 혈관의 fragility로 인한 label-image inconsistency 방지 |
| Target domain | ❌ 불필요 (SSDG) | ❌ 불필요 (SSDG) |
| 전체 이미지 vs. 구조 내부 | 이미지 내 여러 semantic instance 간 비균일 | 동일 class 내 구조 물리 속성에 따른 비균일 |

### 가장 중요한 구분

TSIAA의 "instance-level"은 서로 다른 **semantic region 간** augmentation 다양성을 높이는 것이다  
(예: 전경 object마다 다른 Bézier 파라미터).

내 ONA의 conditioning은 **단일 semantic class(vessel) 내부**에서  
local radius라는 **연속 물리량**에 따라 augmentation budget을 smooth하게 달리 부여하는 것이다.

TSIAA는 "어떤 semantic region이냐"에 따라 Bézier 파라미터를 달리하고,  
나는 "같은 semantic class 내에서 얼마나 관찰 가능하냐"에 따라 augmentation 강도를 달리한다.

---

## 내 논문에서 활용 방법

### Related Work에서의 위치

> "TSIAA [citation] proposes instance-level Bézier augmentation that applies different transformation parameters to different semantic instances within an image, claiming to 'break the uniformity of augmentation rules across different structures.' While sharing the motivation of non-uniform augmentation, TSIAA differs fundamentally: it assigns augmentation parameters at the **semantic instance level** (discrete, adversarially learned), whereas our method conditions augmentation strength on the **continuous local radius** of vessels within a single foreground class. Critically, TSIAA does not consider the physical observability of thin structures and does not protect against label-image inconsistency in fragile tubular regions."

### 핵심 구분 한 줄 요약

- **TSIAA**: different augmentations for different semantic objects in an image (discrete, cross-instance diversity)
- **ONA**: different augmentation budgets for thin vs. thick regions of the same vessel class (continuous, intra-class observability conditioning)

---

## 추가 독해 필요 항목

- [ ] "instance-level"의 정확한 구현: pixel-level? patch-level? semantic region-level?
- [ ] IAM 수 (몇 개의 augmentation module이 instance마다 적용되는가?)
- [ ] Bézier 파라미터 conditioning: content feature 기반? or random adversarial?
- [ ] Over-augmentation 방지 메커니즘: 내 thin vessel 보호와 유사한 제약 있는지 확인
- [ ] Prostate + fundus 실험에서 thin structure (small gland, thin vessel) 성능 수치 확인
