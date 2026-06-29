# TSIAA — Teacher-Student Instance-Level Adversarial Augmentation

> **KEY**: TSIAA  
> **Category**: A (직접 경쟁 — SSDG)  
> **Relevance**: High  
> **Status**: Published Journal Article  
> **Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
> **DOI**: ieeexplore.ieee.org/document/11146907  
> **발견**: Run #8 (2026-06-29)

---

## 논문 요약

Teacher-Student Instance-level Adversarial Augmentation (TSIAA)는 single domain generalized 의료영상 분절을 위한 adversarial augmentation 프레임워크다.

### 핵심 문제의식

기존 adversarial 기반 SSDG 방법들은:
- 상대적으로 단순한 augmenter 구조 사용 (image-level augmentation만)
- 전체 이미지 수준의 단일 강도로 augmentation
- 생성된 augmented image의 diversity가 제한됨

### 방법론

**Instance-Level Image Augmenter (IIAG)**:
- 여러 **Instance-level Augmentation Modules (IAMs)**로 구성
- 각 IAM은 **learnable constrained Bézier transformation function** 기반
- per-instance (개별 이미지마다) 동적으로 augmentation 파라미터 결정
- Bézier curve의 control point를 content feature에 따라 학습

**Teacher-Student 구조**:
- **Teacher**: adversarial augmentation을 탐색 — segmentation model을 최대한 혼란시키는 방향으로 IAM 파라미터 업데이트
- **Student**: augmented image에서 올바른 분절을 학습
- 두 모델 간 knowledge distillation로 over-augmentation 방지

**Over-augmentation 방지 메커니즘**:
- adversarial training이 무한정 강도를 높이지 않도록 constraint 적용
- label-preserving augmentation 범위 내로 제한

### 실험 결과

- Prostate T2-MRI (6-center SSDG)
- Fundus optic disc/cup segmentation
- 심장 MRI
- 기존 SOTA (SLAug, ADA 등) 대비 개선

---

## 내 방법과의 관계

### 공통점

1. **Bézier transformation** 사용: TSIAA IAM = learnable Bézier, 내 ONA도 nonlinear appearance transformation family의 하나로 monotonic Bézier curve 사용
2. **"over-augmentation 방지"** 명시: TSIAA도 label-preserving range를 강조 → 내 "thin vessel 보호"와 같은 방향의 문제의식
3. **SSDG 직접 경쟁 설정**

### 핵심 차이

| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| **조절 단위** | 이미지 전체 (per-image instance) | 이미지 내 혈관 픽셀별 (intra-image spatial) |
| **조절 근거** | adversarial signal (student's error) | vessel radius/observability (structural property) |
| **공간 분해** | 없음 (이미지 전체에 동일 augmentation) | 있음 (thin/thick vessel에 다른 강도 map) |
| **thin vessel 보호** | 직접 없음 (image-level protection) | 명시적 (얇은 혈관은 weak aug 보호 zone) |
| **학습 방식** | adversarial (teacher-student, RL-style) | structural property-guided (deterministic) |
| **설정** | SSDG (target domain 없이 학습) | SSDG (동일) |
| **augmentation** | learnable Bézier (content-adaptive per image) | nonlinear monotonic map (radius-conditioned per pixel) |

### 핵심 구분 논거

TSIAA는 **"어떤 이미지에 얼마나 강한 augmentation을 줄지"** 를 adversarial하게 탐색한다.  
내 Continuous-ONA는 **"같은 이미지 내 thin/thick 혈관별로 다른 augmentation strength를 어떻게 할당할지"** 를 vessel observability에 따라 결정한다.

TSIAA는 전체 이미지가 하나의 단위이므로, 동일 이미지 내 얇은 혈관과 굵은 혈관이 같은 augmentation 강도를 받는다. 내 방법은 이 intra-image structural heterogeneity를 직접 다룬다는 점에서 근본적으로 다른 granularity에서 작동한다.

### 내 논문에서의 활용

**Related Work에서 구분 명시**:
> "Prior adversarial augmentation methods such as TSIAA [cite] determine augmentation intensity at the per-image level, leaving intra-image structural heterogeneity unaddressed. Specifically, within the same image, thin and thick vessel segments receive identical augmentation budgets despite their fundamentally different observability properties."

**내 novelty 강화에 활용**:
- TSIAA의 "over-augmentation prevention"과 내 "thin vessel protection"의 차이를 모티베이션 강화에 사용
- 이미지 단위 적응(TSIAA) → pixel-level 구조 단위 적응(ONA)으로의 발전 방향 제시 가능

---

## 관련 논문 연결

- **ADA (MICCAI 2025)**: 동일한 learnable Bézier remap 아이디어, per-sample adaptive aug, TSIAA보다 먼저 나온 유사 방법
- **AADG (IEEE TMI 2022)**: augmentation policy를 adversarial RL로 탐색한 foundational paper
- **DCON (Pattern Recognition 2025)**: dual-view augmentation으로 다른 방향에서 aug diversity 추구

---

## Novelty 위협 평가

**위협도: High → Medium**

TSIAA와 내 방법은 표면적으로 "learnable adaptive Bézier aug for SSDG"라는 방향을 공유한다. 그러나:
- 내 방법의 핵심은 **intra-image spatial observability conditioning** (같은 이미지 내 혈관별 차별화)
- TSIAA는 이를 전혀 다루지 않음
- augmentation granularity (image-level vs. pixel/vessel-level)의 차이는 방법론적으로 명확

심사위원이 "이미 유사한 adaptive Bézier aug 방법이 IEEE TMI에 나왔는데 무엇이 새로운가?"라고 물을 수 있음 → 대답: TSIAA = per-image, 나 = per-vessel-pixel; TSIAA = adversarial exploration of aug strength, 나 = deterministic structural observability as conditioning signal; TSIAA has no concept of "protecting fragile structures within an image"

---

## 읽기 우선순위

**P0** — 논문 제출 전 반드시 읽어야 함.  
Related Work 절에서 명확히 비교해야 하는 핵심 논문.
