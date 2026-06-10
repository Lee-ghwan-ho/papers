# Literature Watch Report — Run #8

> 날짜: 2026-06-10  
> 모델: claude-sonnet-4-6  
> 신규 논문: **7편** (Published Journal 4편 + Preprint 3편)

---

## 요약

이번 Run에서는 두 가지 관점에서 중요한 신규 논문이 발견되었다.

1. **Novelty 충돌 위험 논문 2편** (IEEE TMI 2026, Neurocomputing 2025):
   - TSIAA: "instance-level"이라는 용어로 "image 내 서로 다른 구조에 다른 augmentation 규칙"을 적용 → 내 핵심 주장과 방향 유사. 즉시 full text 독해로 "instance"의 정확한 의미 파악 필수.
   - MRFFD/DAGBA: "distance from pixels to foreground and edges" 기반으로 brightness aug 강도를 위치별로 다르게 적용 → 공간-conditioned augmentation 아이디어.

2. **방법론 참고 논문 5편** (DG-TTA, GLP-SEG, WaveSDG, TopoLoRA-SAM, Mamba-Sea).

핵심 분석: TSIAA의 "instance-level" 정의가 "cross-class" (서로 다른 semantic class 간)인지 "intra-class" (동일 class 내 서로 다른 구조 간)인지에 따라 내 Continuous-ONA와의 충돌 강도가 결정된다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI Vol 45, pp 764–776 (2026) ⚠️ 최우선 독해

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol 45, pp 764–776, 2026  
**Published Online**: September 2, 2025 (early access)  
**IEEE Xplore**: Document ID 11146907  
**Status**: Published Journal Article  
**arXiv**: 미발견 (IEEE TMI direct submission으로 추정)

#### 방법 요약

- **Instance-level Image Augmenter (IIAG)**: 여러 Instance-level Augmentation Modules(IAMs)로 구성
  - IAM: learnable constrained Bézier transformation function 기반
  - "Compared to image-level adversarial augmentation, instance-level adversarial augmentation **breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity"
- **Teacher-Student adversarial framework**:
  - Teacher: out-of-source and plausible data를 탐색 (novel distribution exploration)
  - Student: original + augmented feature의 consistent + generalized representation 학습
  - 두 모듈이 adversarial하게 번갈아 업데이트
- **Explicitly addresses "over-augmentation problem"**: image-level aug의 획일성이 문제라는 진단

#### 실험
- 4가지 single domain generalization tasks에서 SOTA over previous methods
- Datasets: prostate MRI (6-center), fundus retinal vessel, 기타 (확인 필요)

#### 내 방법과의 관계

**공통점**:
- "uniform augmentation rules are harmful" — 핵심 motivation 동일
- Learnable Bézier transformation family (ADA와도 공유)
- Single domain generalization 설정

**핵심 차이 (확인 필요)**:
- TSIAA의 "instance-level": 현재까지의 정보로는 "within an image의 different structures"가 서로 다른 semantic class/object instance 간 다른 aug를 의미할 가능성이 높음
  - 예: 의료영상의 prostate + background + boundary가 각각 다른 aug를 받는 구조
- 나의 "Continuous-ONA": 동일 vessel foreground class 안에서 local radius/observability에 따라 **연속적(continuous)** aug 강도를 조절
  - TSIAA가 vessel 내 thin/thick 구분을 하지 않는다면 → 핵심 gap 유지
  - TSIAA가 intra-class structure-specific conditioning을 한다면 → 직접 충돌

**Novelty 위협도**: **High (확인 전)** → Full text 독해 후 "Medium" 또는 "High" 최종 판단

**활용 방향 (만약 cross-class instance-level로 확인 시)**:
- TSIAA를 baseline으로 삼아 "TSIAA는 cross-class level에서 aug를 다변화하나, vessel 내부의 intra-class structural fragility를 무시한다"고 명시
- 내 방법이 TSIAA를 vessel-type DG task에서 보완한다는 주장

---

### MRFFD — Neurocomputing 2025

**논문**: Multi-receptive Field Feature Disentanglement with Distance-Aware Gaussian Brightness Augmentation for Single-source Domain Generalization in Medical Image Segmentation  
**Venue**: Neurocomputing, 2025  
**DOI**: 10.1016/j.neucom.2025.130120  
**Status**: Published Journal Article  
**arXiv**: 미발견

#### 방법 요약

- **MRFFD (Multi-Receptive Field Feature Disentanglement)**:
  - Dual-branch: 다양한 kernel size의 convolutional feature 추출 (multi-scale)
  - Channel-level feature disentanglement: style vs. structure 분리
  - Cross-domain style 변화에 robust한 domain-invariant feature 학습
- **DAGBA (Distance-Aware Gaussian Brightness Augmentation)**:
  - **"Dynamically adjusts brightness by incorporating the distance from pixels to the foreground and image edges"**
  - 픽셀이 foreground boundary에서 얼마나 떨어져 있는지에 따라 brightness aug 강도를 다르게 적용
  - 의료영상의 "uneven brightness distribution" 문제를 동기로 함
- 실험: Prostate T2-MRI (multi-center) + Fundus retinal segmentation

#### 내 방법과의 관계

**공통점**:
- Single-source DG에서 augmentation 강도를 공간적 위치에 따라 다르게 적용
- Foreground annotation을 활용한 structure-conditioned aug
- "uneven distribution" 문제를 augmentation 설계에 반영

**핵심 차이**:
- **DAGBA = foreground boundary proximity** (경계에서 가까운 픽셀 vs 먼 픽셀)
  - 이는 "어느 위치가 경계인가" 기반 → edge-aware brightness aug
  - 경계 근처 픽셀과 내부 픽셀이 다른 brightness를 받는 구조
- **나의 ONA = vessel local radius (tubular observability)**
  - "얇은 혈관 vs 굵은 혈관"이 다른 nonlinear aug budget을 받는 구조
  - 혈관 두께(radius)가 label-image consistency의 취약성을 결정
  - DAGBA는 vessel 두께를 인식하지 못함: 얇은 혈관의 픽셀이 boundary 근처라는 것과 그 혈관이 "fragile observability"를 갖는다는 것은 다른 개념

**Novelty 위협도**: **Medium** — DAGBA와 ONA는 둘 다 "위치-conditioned augmentation"이지만 conditioning 기준(boundary distance vs. vessel radius)과 동기(edge artifact vs. structural fragility)가 다름.

**활용 방향**:
- MRFFD/DAGBA를 Related Work에서 "nearest neighbor" 중 하나로 언급 가능
- DAGBA와의 차이를 명확히 기술: "DAGBA adjusts brightness based on proximity to annotated boundaries, whereas our ONA conditions augmentation strength on the local vessel radius—a measure of structural observability—allowing us to protect fragile thin vessels from label-image inconsistency."

---

### DG-TTA — Sensors 2025

**논문**: DG-TTA: Out-of-Domain Medical Image Segmentation Through Augmentation, Descriptor-Driven Domain Generalization, and Test-Time Adaptation  
**Venue**: Sensors, Vol 25, Issue 17, Art 5603, September 2025  
**arXiv**: 2312.06275  
**DOI**: 10.3390/s25175603  
**Status**: Published Journal Article  

#### 방법 요약
- **GIN (Global Intensity Nonlinear)** augmentation으로 input-space domain randomization
- **SSC (Self-Supervised Contrastive) descriptor**: domain-invariant feature representation
- **DeTTA**: test-time에서 동일한 aug-descriptor 조합으로 consistency-based adaptation
- 5 datasets (abdominal, spine, cardiac CT/MRI), 3D segmentation

#### 내 방법과의 관계
- GIN은 내 uniform nonlinear aug baseline의 핵심 구성 요소
- DG-TTA는 GIN을 test-time adaptation과 결합한 논문
- 내 방법과는 paradigm 다름 (training-time SSDG vs GIN+TTA)
- baseline 구성에서 "GIN-only" vs "GIN+TTA" vs "ONA"를 비교할 때 참고 가능

---

### GLP-SEG — IEEE TMI 2025

**논문**: Enhancing Domain Generalization in Medical Image Segmentation With Global and Local Prompts  
**Venue**: IEEE Transactions on Medical Imaging (IEEE Xplore Document ID: 11119233)  
**Status**: Published Journal Article

#### 방법 요약
- Global+Local Prompts (GLP): domain-shared + domain-specific knowledge 분리
- Individualized domain adapter: target sample과 source domains 간 관계 탐색
- ViT 기반 PVM(Pre-trained Vision Models) 활용

#### 내 방법과의 관계
- Prompt-based DG (augmentation-based와 완전히 다른 paradigm)
- Related work에 prompt/adapter 기반 DG의 대표 사례로 언급 가능

---

## Category C — 신규 구조·혈관 특화 (Preprint)

### TopoLoRA-SAM — arXiv 2601.02273

**논문**: TopoLoRA-SAM: Topology-Aware Parameter-Efficient Adaptation of Foundation Segmenters for Thin-Structure and Cross-Domain Binary Semantic Segmentation  
**arXiv**: 2601.02273, January 5, 2026  
**Status**: Preprint Only

#### 방법 요약
- SAM (ViT encoder) + LoRA (Low-Rank Adaptation): frozen encoder + lightweight adaptation
- Spatial convolutional adapter: local spatial features 보강
- Optional topology-aware supervision via differentiable clDice
- Only 5.2% params (~4.9M) trained
- 5 benchmarks: DRIVE, STARE, CHASE_DB1 (retinal vessel), Kvasir-SEG (polyp), SAR
- Best retina-average Dice + best overall Dice across datasets
- Best clDice on DRIVE/CHASE_DB1 → thin vessel connectivity 향상

#### 내 방법과의 관계
- Thin structure cross-domain 문제를 다룬 최신 foundation model adaptation 논문
- 내 방법과는 paradigm 다름 (foundation model PEFT vs. training-time SSDG aug)
- TopoLoRA-SAM의 cross-domain 성능이 내 방법의 비교 context로 사용 가능

---

## Category D — 신규 Top-tier / 방법론 참고 (Preprint)

### Mamba-Sea — arXiv 2504.17515

**논문**: Mamba-Sea: A Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation  
**arXiv**: 2504.17515, April 24, 2025  
**Status**: Preprint Only

#### 방법 요약
- Global augmentation: 전체 이미지에 대한 site-level variation simulation
- Sequence-wise augmentation: token sub-sequences 내 style statistics를 modeling + resampling
  - Mamba SSM의 토큰 시퀀스를 부분적으로 augment → local style diversity
- 의료영상 DG에서 Mamba를 최초로 활용

#### 내 방법과의 관계
- Mamba sequence 내 augmentation = 내 spatial-conditioned aug와 방향 다름
- 새로운 architectural paradigm (Mamba) 추적 가치
- Sequence-level token-wise aug ≠ structure-radius conditioned aug

---

### WaveSDG — arXiv 2603.28463

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463, March 2026  
**Status**: Preprint Only

#### 방법 요약
- WaveSDG: WISER (Wavelet-based Invariant Structure Extraction and Refinement) module
- Low-freq sub-band: global anatomy anchor
- High-freq sub-band: directional edge enhancement + noise suppression
- Fundus optic cup/disc SSDG, 1 source + 5 unseen target datasets
- 기존 7가지 SOTA 방법 능가

#### 내 방법과의 관계
- Wavelet-based SSDG approach, fundus 도메인 (혈관 아님)
- 내 방법과 paradigm 다름 (frequency decoupling vs. spatial structure-conditioned aug)
- 새로운 wavelet-SSDG 트렌드 추적

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class vessel radius adaptive augmentation"
- "thin vessel appearance protection during training-time augmentation"

**TSIAA가 "intra-class structural heterogeneity를 condition으로 한 연속 augmentation budget 조절"을 한다고 full text에서 확인되지 않는 이상**, 내 Continuous-ONA의 핵심 gap은 유지된다.

MRFFD/DAGBA는 boundary-distance 기반으로 augmentation을 공간-conditioned 하지만, vessel radius/observability 기반이 아니라는 점에서 내 방법과 기술적으로 구분된다.

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA full text 독해**: instance-level의 정확한 정의 — cross-class vs. intra-class. 실험 dataset 및 성능 수치 확인.
- [ ] **MRFFD full text 독해**: DAGBA 수식 상세 — boundary distance 계산 방식 (L1? Euclidean? DT?) 및 적용 범위
- [ ] CVPR 2026 accepted papers 목록 공개 시 DG/segmentation 논문 탐색
- [ ] ICLR 2026 accepted list 직접 탐색 (openreview.net)
- [ ] Mamba-based DG 후속 논문 탐색
- [ ] SEMDIR (2507.23326) venue 확인: MICCAI 2025/2026 공식 수록 여부
- [ ] TSIAA 인용 논문 탐색 (IEEE Xplore citation search)
