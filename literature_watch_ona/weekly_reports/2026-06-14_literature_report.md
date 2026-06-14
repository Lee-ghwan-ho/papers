# Literature Watch Report — Run #8
**날짜**: 2026-06-14  
**모델**: claude-sonnet-4-6  
**신규 논문**: 9편 (Published Journal 6편 + Accepted Conference 1편 + Official Proceedings 1편 + Preprint 2편)  
**누적 수록**: 105편

---

## 이번 실행 요약

Run #8에서는 최근 Neurocomputing, TPAMI, Image and Vision Computing에 게재된 논문들과 MICCAI 2025 신규 blood vessel 논문, ACM Multimedia 2024 주파수 증강 논문, 그리고 최신 March 2026 arXiv 논문을 발굴했다.

가장 중요한 발견은 **MRFD-DAGBA (Neurocomputing 2025)**: "Distance-Aware Gaussian Brightness Augmentation"이라는 명칭이 내 observability-conditioned augmentation 개념과 개념적 근접성을 보이므로 즉시 정독 후 차별점 확인이 필요하다. 단, DAGBA의 "distance"가 vessel centerline 기반인지 image-level spatial 기반인지에 따라 충돌 여부가 결정된다.

두 번째 중요 발견은 **SDCL (IEEE TPAMI 2025)**: causal inference 기반으로 style을 confounding factor로 명시적으로 모델링. TPAMI 최고 tier 논문으로 내 "shortcut suppression" 주장의 이론적 배경 강화에 활용 가능하다.

**"radius-conditioned augmentation"** 또는 **"observability-conditioned augmentation"** 을 명시적으로 다룬 논문은 Run #8에서도 발견되지 않았다. Continuous-ONA의 핵심 gap은 유지된다.

---

## 신규 논문 목록

### 최우선 확인 (P0) — Novelty 충돌 가능성

#### MRFD_DAGBA
**Multi-Receptive Field Feature Disentanglement with Distance-Aware Gaussian Brightness Augmentation for Single-Source Domain Generalization in Medical Image Segmentation**  
- **Venue**: Neurocomputing, 2025 | **DOI**: 10.1016/j.neucom.2025.130120  
- **Status**: Published Journal Article  
- **Category**: B (방법론 유사)

**핵심 방법:**
- **MRFFD**: 다양한 크기의 convolutional kernel로 fine-grained detail과 global context를 동시에 캡처하면서 style feature와 structure feature를 channel-level에서 분리
- **DAGBA (Distance-Aware Gaussian Brightness Augmentation)**: 공간적 거리 정보를 활용해 의료영상의 복잡한 밝기 변화 패턴을 시뮬레이션하는 데이터 증강

**⚠️ Novelty 주의:**  
"Distance-Aware"라는 개념이 내 Continuous-ONA의 vessel radius/observability 기반 augmentation 조절과 개념적으로 근접할 수 있다. 그러나 현재 확보된 정보에 따르면:
- DAGBA의 "distance"는 이미지 공간 내 위치 기반 (아마도 tissue boundary 또는 image center에서의 거리) brightness variation 패턴 생성으로 추정됨
- 내 방법의 "radius"는 개별 혈관 centerline까지의 거리 (vessel-level)

**예상 차이점:**
- DAGBA = image-level spatial brightness variation simulation (이미지 전체에 공간적 밝기 패턴 적용)
- Continuous-ONA = intra-image vessel-level radius 기반 nonlinear appearance strength 연속 조절 (같은 이미지 내 얇은 혈관은 약하게, 굵은 혈관은 강하게)
- DAGBA에는 thin/thick vessel의 structural observability 차별화 개념이 없을 것으로 추정

**즉시 수행 필요**: full text 확보 후 DAGBA의 정확한 "distance" 정의 확인.

---

#### PCSDG
**Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation**  
- **Venue**: Biomedical Signal Processing and Control, 2025 | **DOI**: 10.1016/j.bspc.2024.106801  
- **Status**: Published Journal Article  
- **Category**: B (방법론 유사)  
- **GitHub**: HopkinsKwong/PCSDG

**핵심 방법:**
- **Pixel-level Contrastive SDG**: annotation saliency map으로 content/style representation 분리
- **SABA (Structure-Aware Brightness Augmentation)**: annotation-guided saliency로 optic disc/cup 구조 영역과 background에 다른 brightness transform 적용

**내 방법과의 관계:**
- 공통점: annotation을 활용한 region-specific brightness augmentation
- 핵심 차이: PCSDG의 SABA = 두 class (disc vs. background) 간 binary region 분리, 나 = 단일 foreground class(혈관) 내 vessel radius에 따른 연속적 augmentation strength 조절
- PCSDG는 intra-class 구조 이질성 (얇은 혈관 vs 굵은 혈관) 개념 없음
- Task: 안저 영상의 optic disc/cup, 나 = TOF-MRA 뇌혈관

**활용**: 내 related work에서 "region-specific brightness aug 선행 연구"로 인용, 차별점 명시 가능.

---

### 높은 우선순위 (P1)

#### SDCL
**Causal Inference via Style Bias Deconfounding for Domain Generalization**  
- **Venue**: IEEE TPAMI, 2025 | **arXiv**: 2503.16852  
- **Status**: Published Journal Article  
- **Category**: D (Top-tier Vision 아이디어 전이)

**핵심 방법:**
- 구조인과모델(SCM)에서 style을 confounding factor로 명시적 모델링
- Backdoor adjustment로 style confounding의 영향 제거
- **Style-Guided Expert Module (SGEM)**: domain label 없이 style clustering으로 expert 할당, "stratification" 과정 구현
- **Backdoor Causal Learning Module (BDCL)**: causal representation 학습

**내 방법 적용 가능성:**
- SDCL이 "style shortcut을 causally 제거"하는 방법 → 내 방법이 "strong augmentation으로 shortcut을 줄인다"는 주장의 이론적 뒷받침
- 단, SDCL은 자연영상 DG이고 my method는 medical image SSDG
- 내 논문 related work에서 "causal DG 관점"을 소개할 때 인용 가능

---

#### UNIFREQSDG
**Universal Frequency Domain Perturbation for Single-Source Domain Generalization**  
- **Venue**: ACM Multimedia 2024 | **DOI**: 10.1145/3664647.3681536  
- **Status**: Official Proceedings Paper  
- **Category**: A (직접 경쟁)

**핵심 방법:**
- **LSP (Learnable Spectral Perturbation)**: learnable LF radius + Gaussian 강도로 adaptive frequency 범위 확장
- **CPR (Content-Preserving Recombination)**: perturbation 전/후 feature를 decouple + recombine
- **ADI (Adaptive Domain Intervention)**: 3개 loss function으로 perturbation 효과 제어
- 결과: Fundus Dice +7.47%, Prostate +4.99% over SOTA SSDG

**내 방법과의 관계:**
- Frequency domain global perturbation (image 전체에 동일한 frequency band 조절)
- 나 = spatial domain에서 structure-specific nonlinear intensity 조절 (intra-image vessel별 다른 강도)
- 겹치지 않음. baseline competition pool에 포함해야 할 SSDG 경쟁 방법.

---

#### VESSELSDF
**VesselSDF: Distance Field Priors for Vascular Network Reconstruction**  
- **Venue**: MICCAI 2025, Paper 2121 | **arXiv**: 2506.16556  
- **Status**: Accepted Conference Paper  
- **Category**: C (구조·혈관 특화)  
- **저자**: Esposito, Rebain, Onken, Li, Mac Aodha (Edinburgh, UBC)

**핵심 방법:**
- Vessel segmentation을 SDF regression 문제로 재정의: 각 voxel에서 가장 가까운 vessel surface까지의 부호 거리 예측
- Adaptive Gaussian regularizer: vessel surface에서 먼 영역은 smooth, 가까운 영역은 precise
- SDF 표현이 vessel의 tubular geometry와 branching pattern을 자연스럽게 인코딩

**내 방법 관련성:**
- SDF는 vessel radius의 implicit 인코딩: SDF value = signed distance to vessel surface ≈ local vessel radius
- VesselSDF의 SDF regression 방식이 내 observability 측정의 대안적 공식화로 참고 가능
- Domain generalization이 아닌 reconstruction task → 직접 경쟁 없음
- 내 local vessel radius 계산에 SDF 기반 방법 참고 가능

---

### 중간 우선순위 (P2)

#### WAVESDG
**Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation**  
- **Venue**: arXiv 2603.28463, March 2026 | **Status**: Preprint Only  
- **Category**: A (직접 경쟁)

- WISER(Wavelet-based Invariant Structure Extraction and Refinement) module
- LL sub-band → global structural context (anatomy), LH/HL → directional edges, HH → noise/artifacts
- Encoder feature를 wavelet 도메인에서 처리해 domain-invariant anatomy feature 추출
- WaveRNet(2601.05942)과 방향 유사하지만 sub-band 역할 명시적 분리가 차별점

#### RETSTYNORM
**Enhancing Cross-Domain Generalization in Retinal Image Segmentation via Style Randomization and Style Normalization**  
- **Venue**: Image and Vision Computing, Aug 2025 | **DOI**: 10.1016/j.imavis.2025.xxx  
- **Status**: Published Journal Article  
- **Category**: A (직접 경쟁)

- LAB 색공간에서 style randomization (scaling transformation) + channel-wise style normalization
- Retinal vessel/OD/cup/hard exudate 4개 task에서 검증
- SSDG 맥락, fundus 도메인. 내 방법과 직접 충돌 없음 (global color aug vs. structure-specific intensity)

---

### 낮은 우선순위 (P3)

#### AD_DGCL
**Multi-Organ Medical Image Segmentation via Adaptive Disentangled Domain Generalization Collaborative Learning**  
- **Venue**: Neurocomputing, Oct 2025  
- Semi-supervised 3D multi-organ DG. adaptive region-specific loss (pixel frequency 기반 소기관 가중치)
- 내 방법과 다른 task (semi-supervised, organ segmentation)

#### CQI
**Color-Quality Invariance for Robust Medical Image Segmentation**  
- **Venue**: arXiv 2502.07200, Feb 2025 | **Status**: Preprint Only  
- DCIN (Dynamic Color Image Normalization) + CQG (Color-Quality Generalization) loss
- Color + quality variation을 함께 다루는 SSDG. 내 방법과 직접 충돌 없음.

---

## 내 방법 Novelty 상태 업데이트

| 요소 | 상태 | 근거 |
|------|------|------|
| intra-class vessel radius → augmentation budget (continuous) | ✅ **gap 유지** | Run #8에서도 직접 명시 논문 없음 |
| distance/observability를 augmentation에 활용하는 개념 | ⚠️ **MRFD-DAGBA 확인 필요** | "Distance-Aware" 메커니즘 상세 정의 미확인 |
| annotation-based region-specific brightness aug | ⚠️ **PCSDG 인지** | class-level binary vs. intra-class continuous — 차별점 명확 |
| thin vessel protection during augmentation (SSDG context) | ✅ **gap 유지** | L2CP(TTA), MBFCV(few-shot) 등 다른 setting |
| TOF-MRA cerebrovascular 특화 SSDG augmentation | ✅ **gap 유지** | COSTA 데이터셋 기반 방법은 없음 |

---

## 다음 실행 중점 탐색 구역

- [ ] MRFD-DAGBA full text 확보 (Neurocomputing paywall) → DAGBA "distance" 정의 확인
- [ ] PCSDG GitHub 코드 확인: SABA의 saliency map 생성 방식
- [ ] ICLR 2026 medical image DG 논문 openreview 직접 탐색
- [ ] AAAI 2026 proceedings 중 추가 SSDG 논문 탐색
- [ ] NeurIPS 2025 proceedings (GRAPHSEG 외) — 의료영상 DG 추가 논문
- [ ] SLAug venue discrepancy 확인: AAAI 2023 conference vs. possible TPAMI journal extension
