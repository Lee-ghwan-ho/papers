# IDEA COMPARISON TABLE — Continuous-ONA vs 기존 방법

> 이 표는 내 방법(Continuous-ONA)의 novelty가 어디에 있는지,
> 그리고 어떤 기존 방법과 충돌 또는 차별화되는지를 추적한다.

---

## 핵심 Novelty Claim

> **Tubular structures should not receive a uniform augmentation budget.**  
> Resolved structures can tolerate stronger label-preserving appearance interventions,  
> whereas fragile structures require conservative perturbations  
> to preserve weak but essential structural evidence.

---

## 비교 차원 (Comparison Axes)

| 차원 | 설명 |
|------|------|
| **Aug. Target** | 증강 대상이 전체 이미지인가, 클래스 단위인가, 구조 단위인가 |
| **Aug. Condition** | 증강 강도가 고정인가, 이진 조건인가, 연속 조건인가 |
| **Structure Signal** | 구조 특성 (thickness, radius, vesselness)을 증강 설계에 직접 쓰는가 |
| **Label Safety** | 얇은 구조의 label-image consistency를 명시적으로 보호하는가 |
| **Domain Info** | target domain 정보를 사용하는가 |
| **Loss Change** | loss를 수정하는가, 순수 augmentation인가 |

---

## 방법별 비교

### A. SSDG Augmentation 계열 직접 경쟁

| 방법 | Aug. Target | Aug. Condition | Structure Signal | Label Safety | Domain Info | Loss Change | 차별화 포인트 |
|------|-------------|----------------|------------------|--------------|-------------|-------------|---------------|
| **Continuous-ONA (제안)** | Intra-class (vessel) | **Continuous** (radius/observability) | ✅ local radius | ✅ thin 보호 | ❌ | ❌ | 유일하게 intra-class 연속 조건부 증강 |
| SLAug | Global + Class-level | Binary (FG/BG) | ❌ | ❌ | ❌ | ❌ | Class 단위는 같지만 class 내 구조 차이 무시 |
| SRCSM | Semantic-aware RC | Binary label mask | ❌ | ❌ | ✅ (test-time) | ❌ | Test-time source matching 포함 |
| RASS | Global (freq) | None | ❌ | ❌ | ❌ | ❌ | 순수 frequency 증강 |
| MoreStyle | Global (Fourier) | None | ❌ | ❌ | ❌ | ❌ | Low-freq 제약 완화 |
| RaffeSDG | Global (freq) | None | ❌ | ❌ | ❌ | ❌ | Structural saliency 보조 학습 |
| FIESTA | Semantic (freq) | Uncertainty-guided | ❌ | Partially | ❌ | ❌ | Uncertainty로 증강 강도 조절 (but pixel-level, not structure-level) |
| ConStyX | Content + Style | Global | ❌ | ❌ | ❌ | ❌ | Over-augmented feature mitigate |
| StyCona | Style + Content (local) | Region-based | ❌ | ❌ | ❌ | ❌ | 해부학적 구조 변형 포함 |
| SEMDIR | Feature-space | Semantic direction | ❌ | ❌ | ❌ | ❌ | Feature space aug (image space ❌) |
| Causality_SDG | Global + Causal | None | ❌ | ❌ | ❌ | ❌ | Causal factorization |
| SI2CRL | Spectrum (causal) | None | ❌ | ❌ | ❌ | ❌ | Causal representation |
| DGSSA | Structural + Stylistic | Binary (vessel/bg) | ⚠️ topology-inspired | ❌ | ❌ | ❌ | 구조 다양성은 새 vessel 생성, 내 방법과 방향 다름 |
| **ADA** *(Run#2)* | Global (per-sample) | **Content-adaptive** (Bezier param dynamic) | ❌ structure signal 없음 | ❌ | ❌ | ❌ | Per-sample adaptive aug이지만 intra-class structural property 사용 안 함. Thin vessel 보호 개념 없음. |
| GFSA *(Run#2)* | Feature-space style | None (VAE sampling) | ❌ | ❌ | ❌ | ❌ | Feature style diversity 증가. Image-space aug 아님. |
| **ICRN** *(Run#3)* | **Region-specific** (FG/BG gamma) | **Binary** (foreground vs. background) | ❌ (annotation region만 사용) | Partially (foreground style 조절) | ❌ | ❌ | FG/BG 분리 gamma aug. 내 방법은 이를 single FG class 내로 세분화 + continuous radius 사용. |
| **RandDG** *(Run#3)* | Global (all pixels) | None (global GIN + ULoFT) | ❌ | ❌ | ❌ | ❌ | GIN + freq feature-space aug 조합. 모든 픽셀에 동일 강도. 내 방법과 공학적으로 유사하나 thickness conditioning 없음. |
| **AGTA** *(Run#3)* | Class-level (tumor/normal) | **Binary** (anatomy class 기반) | ⚠️ anatomy map 사용 | ⚠️ tumor texture 보존 | ❌ | ❌ | Anatomy-guided texture aug. class-level binary. 내 방법은 single vessel class 내 continuous radius conditioning. |

---

### B. 혈관 특화 DG 계열

| 방법 | 핵심 아이디어 | 내 방법과의 관계 |
|------|---------------|-----------------|
| VesselMorph | Hessian-based shape-aware representation | 구조 표현을 aug conditioning에 쓰는 아이디어 공유. 단, VesselMorph는 feature representation 변환이고, 내 방법은 image-space aug conditioning. |
| Hessian VF (MedIA 2024) | Hessian eigenvector field → domain-invariant feature | 내 radius/observability 계산과 동일한 Hessian 기반 수치를 사용. 단, 목적이 다름: 저들은 feature backbone, 나는 aug budget 계산. |
| AngioDG | Channel-informed feature reweighting | Feature 공간 reweighting. 내 image-space aug conditioning과 방향 다름. |
| BIRF-SDG | Band importance scoring for freq filter | Band scoring ↔ 내 observability scoring 개념 유사. 단, 적용 대상이 freq band vs vessel thickness. |
| WaveRNet | Wavelet multi-source DG | Multi-source 설정. 내 방법은 single-source. |
| ISAC | Mask completion for vascular cross-domain | Mask completion 기반 cross-domain. |
| MULTIDOMAIN_BRAIN | Feature disentanglement | Multi-domain 설정. |
| MBFCV *(Run#2)* | Multi-Branch Feature Extractor로 두께별 혈관 구분. Few-shot cross-domain. | Feature representation 분리. 내 방법(aug budget)과 방향 다름. Test-time support 필요. |
| CRISP *(Run#2)* | Rank-based domain-shift robustness (model-agnostic, training-free). | 내 train-time aug과 방향 다름. Post-hoc inference 방식. |

---

### C. Structural/Topology Loss 계열 (Loss는 내가 바꾸지 않지만 관련성 추적)

| 방법 | 핵심 기여 | 내 연구와의 접점 |
|------|-----------|----------------|
| clDice | Skeleton-based topology loss | 내 observability 정의 시 centerline/skeleton 추출과 관련 |
| cbDice | Centerline + Boundary 결합 | 혈관 크기별 균형 (clDice의 large vessel 편향 해결) → thin vessel 중요성 근거 |
| clCE | Cross-entropy 기반 centerline loss | Topology consistency without accuracy 손해 |
| Skeleton Recall | CPU-efficient skeleton connectivity loss | Thin tubular 구조 connectivity 기반 지표 |
| HarmonySeg | Growth-suppression balanced loss | 2D/3D 모두 작동하는 topology-aware loss |
| Deep Closing | Topology autoencoder + erosion | Disconnected region 탐지 → 내 thin vessel 탐지 관련 |
| Topology-Aware Uncertainty | DMT 기반 구조 단위 uncertainty | Structural unit에서 uncertainty 추정 → thin vessel uncertainty 관련 |
| TopoTTA | TTA for tubular topology | Cross-domain topological shift 탐지 |

---

---

## Run #3 (2026-05-29) 신규 위험 논문 업데이트

| 논문 | 위험 이유 | 대응 방향 |
|------|-----------|-----------|
| **AGTA** (MICCAI 2024 Workshop) | "anatomy-guided texture aug"라는 개념이 내 "observability-conditioned aug"와 논리 방향 동일. "특정 구조는 texture를 보호해야 한다"는 주장 공유. | 핵심 차이 강조: AGTA = class-level binary (tumor vs. 주변), 나 = single vessel class 내 continuous radius. AGTA는 tumor texture cue 보존이 목적, 나는 thin vessel label-image inconsistency 방지가 목적. Workshop paper이므로 영향력 낮음. |
| **ICRN** (IEEE TMI 2024) | Gamma correction 기반 Local Style Augmentation이 foreground/background를 분리 증강. "annotation-region-specific aug"라는 개념 선행. | 핵심 차이: ICRN = FG/BG binary gamma aug, 나 = vessel foreground 내에서 local radius에 따라 continuous nonlinear aug 강도 조절. ICRN은 FG 내 thick/thin 구분 없음. |
| **RandDG** (Medical Physics 2025) | GIN input-space + freq feature-space augmentation 조합. 내 nonlinear aug baseline과 공학적으로 가장 유사. | 핵심 차이: RandDG는 모든 픽셀에 동일한 GIN 강도 적용. 내 방법은 GIN 강도를 local vessel radius에 따라 nonuniform하게 적용. RandDG에는 thin vessel 보호 개념 없음. |

## Run #2 (2026-05-28) 신규 위험 논문 업데이트

| 논문 | 위험 이유 | 대응 방향 |
|------|-----------|-----------|
| **ADA** (MICCAI 2025) | Learnable Bezier Remap으로 per-sample adaptive Bezier augmentation. "content에 따라 aug 강도를 달리 한다"는 아이디어 공유. 7개 데이터셋 SOTA. | 핵심 차이 강조: ADA는 전체 이미지 단위 per-sample adaptive aug (image-level content feature → global Bezier param). 내 방법은 같은 이미지 내에서 혈관마다 다른 강도 (local structural observability → local aug budget). ADA는 thin vessel 보호 목적이 전혀 없음. |
| **MBFCV** (MICCAI 2025) | Multi-Branch Feature Extractor가 두께별 혈관을 구분. 혈관 두께를 명시적으로 모델링한다는 점에서 개념 유사. | 내 방법은 augmentation 시점에서 두께 조건 적용, MBFCV는 inference 시 feature 분리. 또한 MBFCV는 few-shot (support 필요), 나는 single-source DG (test-time 정보 없음). |

---

## 가장 위험한 경쟁 논문 (Novelty 충돌 가능성)

| 논문 | 위험 이유 | 대응 방향 |
|------|-----------|-----------|
| **SLAug** | Class-level location-scale aug의 원조. 내 방법이 "SLAug를 thin/thick class로 나눈 것"처럼 보일 위험 | 핵심 차이 강조: SLAug는 FG/BG binary class, 내 방법은 single class 내 continuous structural property. SLAug는 class간 다양성, 나는 class 내 구조 보호. |
| **FIESTA** | Uncertainty-guided augmentation. Pixel-level epistemic uncertainty로 aug 강도 조절 | FIESTA는 prediction uncertainty 기반(model-centric), 나는 annotation-derived structural observability 기반(data-centric). FIESTA는 thin vessel 보호 목적이 명시적이지 않음. |
| **StyCona** | Local content augmentation 포함 | StyCona의 content augmentation은 전체 해부 구조 변형(이동/크기 변환), 내 방법은 intensity/appearance 변환의 강도 조절. |
| **SRCSM** | Semantic-aware RC (label별 다른 증강) | SRCSM은 서로 다른 semantic class에 다른 RC를 적용. 나는 같은 class 내에서 구조 두께에 따라 연속적으로 강도 조절. |
| **ConStyX** | Over-augmented feature 억제 | ConStyX는 over-augmented image의 feature를 줄이는 post-hoc 방식. 나는 처음부터 구조별로 aug budget을 다르게 배분하는 proactive 방식. |

---

## 내 방법이 채워주는 Gap

1. **Intra-class continuous conditioning**: 기존 방법들은 모두 전체 이미지 또는 semantic class(FG/BG) 단위로 aug를 설계한다. **단일 foreground class 내에서 구조의 관찰 가능성에 따라 연속적으로 aug budget을 다르게 배분**하는 방법은 없다.

2. **Label-image consistency for fragile structures**: 얇고 희미한 혈관에 강한 appearance 변형을 가하면 label은 있지만 영상에서는 안 보이는 상황이 발생한다는 문제를 **명시적으로 정의하고 aug 설계에 반영**한 논문이 없다.

3. **Source annotation 기반 observability**: target domain 정보 없이, **source annotation에서 직접 계산 가능한 local radius 또는 vesselness score**를 aug conditioning에 사용. Target distribution을 가정하지 않으므로 진정한 SSDG.

4. **Nonlinear appearance transformation과의 통합**: Bézier/spline 기반 nonlinear intensity mapping 자체는 기존(SLAug, Causality_SDG 등)에서 사용되었으나, 이를 **구조별로 차별화된 강도로 적용**한 것은 없다.
