# Literature Report — 2026-05-27

**연구 주제**: Continuous-ONA: Observability-Conditioned Nonlinear Augmentation for TOF-MRA Cerebrovascular Segmentation  
**검색 범위**: Category A–D 전체, 2024–2026 우선, 필요시 2021–2023 foundational 포함  
**신규 발견 논문 수**: 44편 (기존 인덱스 없음 — 최초 실행)

---

## Executive Summary

이번 검색에서 확인한 핵심 결론:

1. **내 방법의 핵심 gap은 실재한다.** 혈관 두께/관찰 가능성에 따라 연속적으로 augmentation 강도를 조절하는 방법은 현재까지 없다.
2. **가장 위험한 충돌 논문은 FIESTA와 SRCSM이다.** FIESTA는 uncertainty-guided aug strength 조절을, SRCSM은 label-wise 다른 augmentation을 적용한다. 차별화 논리를 명확히 준비해야 한다.
3. **혈관 DG 논문이 2025–2026에 급증하고 있다.** AngioDG, WaveRNet, DGSSA, ISAC, Multi-Domain Brain Vessel 등. 이 시장이 경쟁적으로 성장 중이므로 제출 시기가 중요하다.
4. **ICCV 2025에 tubular structure 논문이 집중됐다.** TopoTTA와 HarmonySeg 모두 ICCV 2025에서 발표. 이 venue가 내 논문 투고 후보이다.
5. **내 observability 계산에 VesselMorph (Hessian bipolar tensor field)와 Hessian VF (MedIA 2024)의 방법론을 인용 근거로 사용할 수 있다.**

---

## Category A — 직접 경쟁 논문 최신 현황

### 최우선 독해 필요 논문

---

#### [A-1] SRCSM — Semantic-aware Random Convolution and Source Matching
- **arXiv**: 2512.01510 (2025년 12월)
- **Status**: Preprint Only
- **핵심 아이디어**: 학습 시 annotation label을 기준으로 이미지 영역마다 다른 Random Convolution을 적용 (semantic-aware RC). 테스트 시 target domain의 intensity를 source domain에 가깝게 source matching.
- **내 방법과의 관계**: 가장 표면적으로 유사한 방법. SRCSM은 서로 다른 semantic class(장기 vs 배경)에 다른 RC를 적용하지만, **단일 foreground class 내에서 구조 두께에 따라 연속적으로 강도를 조절하지는 않는다.** 차별화 포인트: (1) intra-class conditioning, (2) continuous vs discrete, (3) 구조 관찰 가능성 보호라는 명시적 목적.
- **위험도**: ★★★ — 논문 리뷰어가 "SRCSM의 단순 확장 아닌가?"라고 물을 수 있음.

---

#### [A-2] FIESTA — Fourier-Based Semantic Augmentation with Uncertainty Guidance
- **arXiv**: 2406.14308 (2024년 6월)
- **Status**: Preprint Only
- **핵심 아이디어**: Fourier 공간에서 semantic-aware amplitude+phase 조절. Epistemic uncertainty를 사용하여 aug 강도를 동적으로 조절 — 불확실한 영역에 더 강한 aug.
- **내 방법과의 관계**: aug strength를 구조별로 다르게 준다는 개념을 공유. 그러나 **FIESTA의 uncertainty는 prediction entropy 기반(model-centric, 학습 중 동적 변화)이고, 내 observability는 annotation geometry 기반(data-centric, 사전 계산)이다.** FIESTA는 Fourier 공간 aug이고, 내 방법은 intensity/contrast 공간의 nonlinear transformation.
- **위험도**: ★★★ — 개념 유사성 높음. 차별화 논리 필요.

---

#### [A-3] ConStyX — Content Style Augmentation for Generalizable Medical Image Segmentation
- **Venue**: MICCAI 2025 (Accepted Conference Paper)
- **arXiv**: 2506.10675
- **핵심 아이디어**: Content (해부 구조 변형)와 Style (global appearance) 증강을 함께 적용. Over-augmented feature는 원래 prediction과 consistency를 강제하여 억제.
- **내 방법과의 관계**: Over-augmented feature 억제 개념이 내 thin vessel 보호와 개념적으로 유사. 그러나 ConStyX는 post-hoc feature suppression이고, 내 방법은 proactive aug budget allocation. 또한 ConStyX는 구조별 조건을 사용하지 않음.
- **위험도**: ★★ — 개념적 유사성은 있으나 기술적 접근이 다름.

---

#### [A-4] StyCona — Style Content Decomposition-based Data Augmentation
- **arXiv**: 2502.20619 (2025년 2월)
- **Status**: Preprint Only
- **핵심 아이디어**: 이미지를 style code와 content map으로 분해. Style은 global, content는 local anatomical 구조 변형. 단순 plug-and-play.
- **내 방법과의 관계**: Local content 변형이 구조별로 다를 수 있다는 점에서 유사 방향. 그러나 StyCona의 content aug는 전체 해부 구조를 이동/크기 변환하는 것이고, 내 방법은 intensity appearance 변형 강도를 구조 두께별로 다르게 적용.
- **위험도**: ★★

---

#### [A-5] Learning Semantic Directions (SEMDIR)
- **arXiv**: 2507.23326 (2025년 7월)
- **Status**: Preprint Only
- **핵심 아이디어**: Feature space에서 learnable semantic direction selector로 domain-variant feature를 modulate. Covariance-based intensity sampler. 해부 구조 일관성 보존.
- **내 방법과의 관계**: **Feature space aug** (내 방법은 image space aug). 목적은 유사하지만 구현 공간이 다름. 내 방법에 비해 annotation geometry를 직접 이용하지 않음.
- **위험도**: ★★

---

### 중요 기준 논문 최신 현황 확인

#### [A-6] RASS (MICCAI 2024)
- Random Amplitude Spectrum Synthesis. 3D fetal brain + 2D fundus 실험. MICCAI 2024 accepted.
- **내 비교 baseline으로 포함 고려.**

#### [A-7] MoreStyle (MICCAI 2024)
- Fourier 저주파 제약 완화로 더 다양한 style을 생성. Plug-and-play. MICCAI 2024 accepted.
- **내 비교 baseline으로 포함 고려.**

#### [A-8] AngioDG (arXiv:2511.17724, 2025)
- Coronary vessel segmentation SSDG. Channel-informed feature reweighting으로 domain-invariant feature amplify.
- **혈관 SSDG 직접 경쟁. 단, X-ray angiography 특화라 TOF-MRA와 domain 다름.**

#### [A-9] SI2CRL (MedIA 2025)
- Spectrum intervention + causal representation learning. SDG에서 prostate/abdominal/cardiac 실험.
- **Causal 관점 SSDG 최신 journal 논문. 내 방법의 causal 해석을 보강할 수 있음.**

---

## Category B — 방법론 유사/경쟁

#### [B-1] DGSSA — Domain Generalization with Structural and Stylistic Augmentation (Neural Networks 2025)
- **arXiv**: 2501.03466
- **핵심 아이디어**: Space colonization algorithm으로 혈관 유사 구조를 생성(structural aug) + PixMix로 photometric aug(stylistic aug). 4개 retinal dataset에서 실험.
- **내 방법과의 관계**: 혈관 구조를 aug에 활용한다는 점에서 방향이 유사. 그러나 DGSSA의 structural aug는 **완전히 새로운 혈관 구조를 생성**하는 것이고, 내 방법은 **기존 혈관 구조의 appearance를 thickness 조건에 따라 변형**. 접근 방향 자체가 다름.
- **위험도**: ★★

#### [B-2] BIRF-SDG (ICIC 2025)
- Band importance scoring으로 중요한 frequency band를 보호하면서 random freq filtering으로 SSDG.
- **Frequency band importance ↔ vessel thickness importance의 개념 유사. 적용 대상이 다름.**

---

## Category C — 혈관 / Tubular Structure 특화

### 가장 중요한 신규 논문

#### [C-1] TopoTTA — Topology-Enhanced Test-Time Adaptation for Tubular Structure Segmentation (ICCV 2025)
- **arXiv**: 2508.00442
- **핵심 아이디어**: Tubular structure segmentation의 cross-domain topological discrepancy 문제를 TTA로 해결. TopoMDCs (Topological Meta Difference Convolutions)로 topological representation 강화. TopoHG (Topology Hard sample Generation)으로 pseudo-break region 생성 및 hard sample alignment.
- **내 연구와의 접점**: 내 방법(training-time aug 기반 DG)에 이어 test-time에서 topological shift를 보정하는 방법으로 후속 적용 가능. 또한 TopoTTA에서 사용하는 "topological hard sample" 개념이 내 thin vessel (fragile structure) 개념과 연결됨.
- **중요도**: ICCV 2025. 높은 venue. 직접 경쟁은 아니나 관련 ecosystem.

#### [C-2] HarmonySeg — Tubular Structure Segmentation (ICCV 2025)
- **arXiv**: 2504.07827
- **핵심 아이디어**: Deep-to-Shallow Decoder + vesselness map auxiliary input + Growth-Suppression Balanced Loss. 2D/3D 모두 지원. 작은 tubular structure recall 개선.
- **내 연구와의 접점**: Vesselness map을 보조 정보로 사용하는 아이디어는 내 observability 계산과 유사한 방향. Growth-suppression balanced loss는 내 연구의 loss 보강 옵션.
- **중요도**: ★★

#### [C-3] Skeleton Recall Loss (ECCV 2024)
- **arXiv**: 2404.03010
- **핵심 아이디어**: CPU-efficient skeleton recall loss. Standard Dice/CE가 volumetric overlap에 집중하여 thin structure connectivity를 희생하는 문제 해결.
- **내 연구와의 접점**: **Thin vessel이 연결성 측면에서 특별한 보호가 필요하다는 것을 loss 관점에서 보여줌 → 내 aug 관점 동기와 완벽히 대응.** "thin vessel은 표준 loss로도 보호받지 못함 → augmentation level에서도 보호가 필요"라는 논리 전개에 인용 가능.
- **중요도**: ★★★ (내 동기 논거로 사용)

#### [C-4] cbDice — Centerline Boundary Dice Loss (MICCAI 2024)
- **핵심 아이디어**: clDice가 large vessel에 편향되는 문제 해결. Centerline + Boundary를 결합하여 크기별 균형 segmentation.
- **내 연구와의 접점**: **Large vessel에 편향된 기존 방법의 문제 → 이는 aug 설계에서도 마찬가지. 균일한 aug budget은 thick vessel에 편향.** 내 동기를 cbDice의 발견과 연결 가능.

#### [C-5] Multi-Domain Brain Vessel Segmentation (MELBA 2025)
- **arXiv**: 2510.00665v2
- **핵심 아이디어**: Image-to-image translation + feature disentanglement로 MRA, CTA, MRV 간 brain vessel segmentation. UCL, EURECOM 공동 연구.
- **내 연구와의 접점**: TOF-MRA 포함 brain vessel DG 직접 경쟁. 단, multi-domain (우리는 single-source).

#### [C-6] Domain Generalization for Retinal Vessel via Hessian-based Vector Field (MedIA 2024)
- **핵심 아이디어**: Hessian secondary eigenvector field를 domain-invariant feature로 사용. Vision Transformer와 결합.
- **내 연구와의 접점**: **내 observability 계산에서 Hessian 기반 vesselness / local radius 계산의 기술적 기반.** 이 논문의 방법론을 observability score 계산의 technical foundation으로 인용 가능.

---

## Category D — Top-tier Vision 아이디어 전이

#### [D-1] Generalize or Detect? (NeurIPS 2024)
- **핵심 아이디어**: OOD detection과 DG를 동시에 다루는 생성 기반 augmentation. Semantic shift ↔ domain shift 구분.
- **내 연구로의 전이**: "Semantic shift (thin vessel → invisible)"와 "domain shift (style change)"를 동시에 다루는 나의 방법과 개념 연결 가능.

#### [D-2] Skeleton Recall Loss (ECCV 2024)
→ Category C에서 이미 설명.

#### [D-3] HarmonySeg (ICCV 2025)
→ Category C에서 이미 설명.

#### [D-4] TopoTTA (ICCV 2025)
→ Category C에서 이미 설명.

---

## 내 연구 Novelty 확인 — 이번 검색 결과 기준

### 여전히 비어있는 Gap (내 논문의 고유 기여)

1. **단일 foreground class 내 intra-class continuous aug conditioning**: 검색된 44편 중 어느 논문도 이를 명시적으로 수행하지 않는다. SRCSM이 가장 유사하나 inter-class (FG vs BG) 수준에 머문다.

2. **Label-image inconsistency for thin vessels as augmentation safety motivation**: clDice, cbDice, Skeleton Recall이 thin vessel의 loss-level 문제를 다루지만, augmentation-level에서 이를 보호해야 한다고 명시한 논문은 없다.

3. **Source annotation geometry → aug budget계산**: 내 방법처럼 annotation의 local radius / observability를 사전 계산하여 augmentation 강도 배분에 사용하는 방법은 없다. VesselMorph, Hessian VF는 유사한 기하 계산을 사용하지만 목적이 다르다.

---

## 새로 설계에 반영할 아이디어

1. **cbDice의 diameter imbalance 발견**: clDice가 large vessel에 편향된다는 cbDice의 발견 → 내 thin vessel 보호의 동기를 강화하는 인용.

2. **Skeleton Recall의 connectivity 관점**: Thin tubular connectivity가 volumetric loss로는 보호받지 못함 → Aug 수준에서도 동일한 문제가 발생할 수 있음을 논리적으로 연결.

3. **FIESTA의 uncertainty guidance**: FIESTA가 prediction entropy로 aug 강도를 조절하는 것처럼, 나는 annotation geometry로 aug 강도를 조절. 두 방법의 관계를 명시하면 더 설득력 있는 framing 가능.

4. **HarmonySeg의 vesselness map**: Vesselness map을 보조 입력으로 사용하는 HarmonySeg의 접근처럼, 내 방법에서도 vesselness/local radius map을 미리 계산하여 augmentation strength map으로 직접 사용하는 구현이 자연스럽다.

---

## 추가 탐색 필요 항목 (다음 실행에서)

- [ ] VectorField Transformer for vessel (Hu et al., MIDL 2022) 최신 후속
- [ ] DomainDrop (domain-sensitive feature drop) 최신 후속
- [ ] MICCAI 2025 전체 accepted paper 리스트에서 vessel/tubular DG 추가 스캔
- [ ] CVPR 2025 에서 "augmentation" + "generalization" 관련 paper 직접 확인
- [ ] Partial volume effect in MRA vessel imaging — 명시 논문 탐색
- [ ] Circle of Willis (CoW) segmentation 특화 DG 논문

---

## 기준 논문별 후속 연구 현황 (요청된 항목)

| 기준 논문 | 주요 후속 / 관련 최신 논문 |
|-----------|--------------------------|
| SLAug | SRCSM (2025, semantic-aware RC), ConStyX (MICCAI 2025), SEMDIR (2025) |
| RASS | BIRF-SDG (ICIC 2025, band importance 확장), WaveRNet (2026, wavelet 확장) |
| Causality_SDG | SI2CRL (MedIA 2025), MCDRL (2025), INVCAUSAL (2024) |
| MoreStyle | RaffeSDG (2024, 유사 freq 방향), FIESTA (2024, uncertainty 결합) |
| VesselMorph | DGSSA (2025, structural aug 확장), Hessian VF (MedIA 2024, 동일 Hessian 기반) |
| Hessian VF (Vector Field Transformer) | WaveRNet (wavelet으로 확장), AngioDG (channel reweighting 방향) |
| clDice | cbDice (MICCAI 2024), clCE (MICCAI 2024), Skeleton Recall (ECCV 2024), HarmonySeg (ICCV 2025), TopoTTA (ICCV 2025) |
| DomainDrop 계열 | 직접 후속 미확인 — 다음 탐색 예정 |
