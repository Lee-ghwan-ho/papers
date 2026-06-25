# TSIAA — Novelty Concern Note

> **Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation**  
> Zhengshan Wang, Long Chen, Xuelin Xie, Yang Zhang, Yunpeng Cai, Weiping Ding  
> IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
> DOI: 10.1109/TMI.2025.3605162 | IEEEXplore: 11146907  
> Online: September 2, 2025  
> Category: A (직접경쟁) | Relevance: **High** ⚠️

---

## 핵심 방법

- **IIAG (Instance-level Image Augmenter)**: learnable constrained Bézier transformation function으로 구성된 여러 IAM (Instance-level Augmentation Module)의 집합
- **Teacher-Student 구조**: Student는 augmented 이미지로 domain-invariant feature 학습, Teacher는 EMA로 안정적 guidance 제공
- **Adversarial loop**: augmenter와 segmenter가 adversarial 방식으로 번갈아 업데이트
  - Augmenter: student segmenter를 최대한 어렵게 만드는 방향으로 Bézier parameter 탐색
  - Segmenter: augmented 이미지에서도 일반화된 representation 학습

## 내 방법(Continuous-ONA)과의 비교

### 공통점
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 핵심 주장 | "breaks the uniformity of augmentation rules across different structures within an image" | "tubular structures should not receive a uniform augmentation budget" |
| 비균일 증강 | 같은 이미지 내 다른 구조에 다른 aug 적용 | 같은 class 내 다른 위치에 다른 aug 강도 적용 |
| Bézier transform | IAM이 Bézier function 사용 | Nonlinear monotonic mapping (Bézier 또는 spline) 사용 가능 |

### 핵심 차이 (내 방법의 차별점)

1. **작용 단위**:
   - TSIAA: **semantic instance 단위** (서로 다른 class 또는 anatomical object 사이)
     - 예: 심장 instance vs. 폐 instance vs. 신장 instance 각각에 다른 aug 규칙
   - ONA: **동일 class 내 spatial 단위** (혈관이라는 단일 foreground class 안에서 local radius/observability에 따라 연속 조절)
     - 예: 같은 이미지 내 1px 혈관 vs. 5px 혈관에 다른 aug 강도

2. **조절 기준**:
   - TSIAA: adversarial feedback (student loss) → 학습된 per-instance Bézier parameter
   - ONA: GT annotation에서 직접 계산된 local vessel radius / distance transform → 명시적 observability score

3. **타깃 문제**:
   - TSIAA: general multi-organ segmentation DG (prostate, fundus, skin 등)
   - ONA: TOF-MRA single-source vessel segmentation DG (단일 tubular foreground class)

4. **보호 개념**:
   - TSIAA: thin vessel 보호 개념 없음. adversarial이므로 thin vessel에도 최대 강도 aug 부여 가능
   - ONA: thin/fragile vessel은 label-image inconsistency 방지를 위해 명시적으로 **보수적 aug** 적용

5. **Intra-class 이질성**:
   - TSIAA: 혈관이라는 class 내부의 두께 차이를 augmentation budget에 반영하는 mechanism 없음
   - ONA: 이것이 핵심 contribution

## 내 논문에서의 활용 전략

### 차별점 서술 예시 (Related Work 또는 Introduction)

> "TSIAA [Wang et al., 2026] demonstrates that applying uniform augmentation rules across semantically distinct anatomical structures degrades domain generalization. However, their instance-level approach operates at the inter-class boundary — differentiating between different organ instances. In contrast, our work identifies a finer-grained problem: within a single foreground class of tubular structures, spatial heterogeneity in observability demands differentiated augmentation treatment. TSIAA's adversarial augmenter would still apply maximal transformation to fragile thin vessels, potentially creating label-image inconsistencies that are absent in thicker vessels."

### 내 motivation 지지 근거로 활용

- TSIAA가 "non-uniform augmentation"이 DG에 효과적임을 IEEE TMI 2026에서 증명 → 내 intra-class non-uniform augmentation의 필요성을 더 강하게 정당화
- "같은 class 내부에서도 non-uniform이 필요하다"는 주장 = TSIAA의 확장

## 실험 설정

- 데이터셋: fundus (5 domains), prostate (6 centers), MRI segmentation tasks
- Baseline 비교: SLAug, GIN, RandConv 등
- Metric: Dice score under leave-one-domain-out protocol

## 추가 확인 필요 사항

- [ ] IIAG의 "instance" 정의: 이미지 내 어떤 단위를 instance로 보는지 (object mask 단위? spatial region 단위?)
- [ ] Bézier parameter 학습 방식: per-image adaptive vs. per-instance adaptive 구분
- [ ] Thin vessel 관련 ablation 또는 분석이 있는지 확인
- [ ] 내 POC 실험 데이터셋과 overlap 확인 (fundus 제외, 혈관 분야 비교 실험 없음으로 예상)
