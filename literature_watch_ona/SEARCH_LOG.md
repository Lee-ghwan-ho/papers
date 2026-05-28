# SEARCH LOG

---

## 2026-05-28 — 정기 탐색 (Run #2)

### 실행 환경
- 날짜: 2026-05-28
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024
- 신규 발견: 13편 (Accepted Conference 5편 + Workshop 1편 + Journal 1편 + Preprint 6편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation MICCAI 2025 2026 augmentation | **ADA (MICCAI 2025)** — Learnable Bezier Remap, Channel Shift Control, Gradient-guided Feature Weaken |
| A | vessel segmentation domain generalization cerebrovascular TOF-MRA 2025 2026 | SPOCKMIP(2024), Multi-center brain vessel challenge 재확인 |
| A | ADA adaptive augmentation MICCAI 2025 Bezier remap content feature gradient weaken | ADA (MICCAI 2025) 상세 확인: 7개 데이터셋, per-sample adaptive aug |
| A | CVPR 2025 accepted medical segmentation domain generalization augmentation | 직접적 신규 CVPR 2025 미발견 (UniDDG 2501.04741 주목) |
| A | generative feature style augmentation domain generalization medical segmentation 2025 Pattern Recognition | **GFSA (Pattern Recognition 2025)** 확인 |
| A | CRISP rank-guided iterative squeezing robust medical image segmentation domain shift 2026 | **CRISP (arXiv 2604.05409)** 확인 |
| A | boundless across domains adaptive feature cross-attention DG medical segmentation 2025 | BOUNDLESS (arXiv 2411.14883) 확인 |
| B | class-wise region-conditioned structure-aware augmentation medical segmentation DG 2025 | DG-TTA 재확인, SRCSM/SEMDIR 재확인 |
| B | CF-Seg counterfactuals segmentation medical image 2025 2026 | **CF_SEG (MICCAI 2025)** 확인 |
| B | frequency-based federated domain generalization polyp segmentation 2024 2025 | **FDGP (ICASSP 2025)** 확인 |
| B | frequency prior guided matching semi-supervised polyp segmentation DG 2025 | **FPGM (arXiv 2508.06517)** 확인 |
| C | thin tubular structure segmentation topology preservation DG CVPR ICCV ECCV 2025 | **GLCP (MICCAI 2025)**, SPATIAL_TOPO (ICCV 2025W), FMS² 발견 |
| C | GLCP global-to-local connectivity preservation tubular structure 2025 | GLCP (MICCAI 2025) 상세 확인: IMS + DAR 모듈 |
| C | topology preserving segmentation spatial-aware persistent feature matching 2024 2025 | **SPATIAL_TOPO (ICCV 2025 Workshop)** 확인: arXiv 2412.02076 |
| C | FMS2 flow matching segmentation synthesis thin structures 2026 | **FMS² (arXiv 2603.13659)** 확인 |
| C | SPOCKMIP segmentation vessels MRA maximum intensity projection loss 2024 | **SPOCKMIP (arXiv 2407.08655)** 확인: TOF-MRA 특화, MIP loss |
| C | MICCAI 2025 vessel segmentation domain generalization multi-center brain MRA | **MBFCV (MICCAI 2025)** 발견: Multi-branch for vessel thickness |
| C | multi-branch framework cross-domain vessel segmentation few-shot MICCAI 2025 thickness | MBFCV (MICCAI 2025) 상세 확인: MBFE로 두께 다른 혈관 구분 |
| C | perivascular space segmentation MRI challenge MICCAI 2024 domain generalization small vessel | MICCAI 2024 EPVS Challenge 확인, **DRIPS (medRxiv 2025)** 발견 |
| D | CVPR ICCV NeurIPS ICLR 2025 DG segmentation augmentation robust distribution shift | CRISP 재확인, 기존 목록 재확인 |
| D | NeurIPS 2025 augmentation budget adaptive structure-aware perturbation DG | 직접 신규 발견 없음 |
| D | ICLR 2025 DG invariant representation shape texture bias segmentation | 기존 방향 재확인 |
| Follow-up | SLAug TPAMI follow-up citing 2025 | ADA, SRCSM, ConStyX 등이 직접 인용하는 follow-up 확인 |
| Follow-up | DomainDrop domain-sensitive feature suppression 2024 2025 follow-up | 직접 후속 논문 없음 (원 논문 ICCV 2023) |
| Follow-up | morphology-conditioned augmentation vessel thickness radius DG 2025 | 직접 명시 논문 없음 — 내 방법의 gap 재확인 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 충돌 위험)

**ADA (MICCAI 2025)** — arXiv 미확인, MICCAI 2025 Proceedings (Paper 0315)
- Learnable Bezier Remap: content feature에 따라 Bezier 파라미터를 **per-sample** 동적으로 조절
- Channel Shift Control: 채널별 shift/scale 동적 조절
- Gradient-guided Feature Weaken: high-impact feature 억제
- 7개 데이터셋 실험
- **내 방법과 겹치는 부분**: "aug 강도를 image content에 따라 적응적으로 조절"이라는 아이디어
- **핵심 차이**: ADA는 per-sample (전체 이미지 단위) 적응, 나는 intra-image structural observability 기반 (같은 이미지 내 혈관마다 다른 강도). ADA에는 얇은 혈관 보호 개념이 없음.

**MBFCV (MICCAI 2025)** — Paper 0782
- Multi-Branch Feature Extractor(MBFE): 두께가 다른 혈관 구조를 구분하는 multi-branch
- High-Frequency Auxiliary Modality: 혈관 고주파 구조에 집중
- Few-shot paradigm으로 cross-domain
- **내 방법과 겹치는 부분**: 혈관 두께를 명시적으로 모델링
- **핵심 차이**: 내 방법은 augmentation budget 조절, MBFCV는 feature representation 계층 분리. MBFCV는 test-time support 필요.

### 미탐색 / 추가 탐색 필요 구역

- [ ] UniDDG (2501.04741) — "One image as one domain" 아이디어 상세 확인
- [ ] Anatomy-Guided Texture Aug for Cervical Tumor (MICCAI 2024, 10.1007/978-3-031-73360-4_8)
- [ ] ICCV 2025 정식 accepted paper 중 structure/vessel 관련 추가 탐색
- [ ] Causality-inspired Latent Feature Aug (arXiv 2406.05980) 확인 필요
- [ ] NeurIPS 2025 proceedings 추가 탐색 (schedule 확인)

---

## 2026-05-27 — 초기 전체 탐색 (Run #1)

### 실행 환경
- 날짜: 2026-05-27
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2023–2024

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation 2025 2026 MICCAI TMI | ConStyX (MICCAI 2025), Experience with SDG in Real World (2026), SEMDIR (2025) |
| A | vessel segmentation domain generalization cerebrovascular TOF-MRA 2024 2025 | Multi-Domain Brain Vessel (MELBA 2025), Cerebrovascular Topology Adversarial (ACM MM 2023) |
| B | class-wise structure-aware morphology-aware augmentation medical image segmentation DG 2024 2025 | SRCSM (2025), SEMDIR (2025), HALLUDG (2025) |
| B | nonlinear intensity augmentation Bezier spline monotonic transformation DG segmentation 2024 2025 | SRCSM, DG-TTA, Causality_SDG 재확인 |
| A | SLAug follow-up citation vessel segmentation DG 2024 2025 | AngioDG (2025), WaveRNet (2026) |
| D | NeurIPS ICLR 2025 counterfactual augmentation structure preserving DG | 직접 의료영상 적용 논문 위주로 발견 |
| C | clDice topology vessel segmentation skeleton connectivity loss 2024 2025 MICCAI | cbDice (MICCAI 2024), clCE (MICCAI 2024) |
| A | VesselMorph RASS MoreStyle DG medical segmentation follow-up 2024 2025 | RASS (MICCAI 2024), MoreStyle (MICCAI 2024), DGSSA (2025) |
| C | multi-domain brain vessel segmentation feature disentanglement 2025 arxiv | MULTIDOMAIN_BRAIN (MELBA 2025) 상세 확인 |
| A | AngioDG coronary vessel segmentation SSDG 2025 | AngioDG (2511.17724) 확인 |
| C | DGSSA retinal vessel structural stylistic augmentation DG 2025 | DGSSA (2501.03466) 확인 |
| C | WaveRNet wavelet frequency DG retinal vessel 2025 2026 | WaveRNet (2601.05942) 확인 |
| C | skeleton recall loss thin tubular connectivity segmentation 2024 MICCAI TMI | SKELRECALL (ECCV 2024) 확인 |
| D | ICLR CVPR ECCV 2025 shape bias robust segmentation frequency DG invariance | SHAPEBIAS (2503.12453), SHAPEBIAS_CNN (2509.11355) |
| A | RaffeSDG random frequency filtering SSDG medical segmentation 2024 | RAFFESDG 확인, BIRF-SDG 발견 |
| C | TopoTTA topology enhanced TTA tubular structure 2025 | TOPOTTA (ICCV 2025) 확인 |
| C | HarmonySeg tubular structure deep-shallow feature 2025 | HARMONYSEG (ICCV 2025) 확인 |
| C | fractal feature maps topological self-similarity tubular 2024 2025 | FRACTAL_FFM (ECCV 2024) 확인 |
| B | SRCSM semantic-aware random convolution source matching DG medical segmentation | SRCSM (2512.01510) 상세 확인 |
| B | learning semantic directions feature augmentation DG medical segmentation 2025 | SEMDIR (2507.23326) 확인 |
| A | BIRF-SDG band importance random frequency filter SSDG retinal vessel 2025 | BIRF-SDG (ICIC 2025) 확인 |
| C | dynamic snake convolution topological geometric constraints tubular ICCV 2023 follow-up | DYNSNAKE 확인, HarmonySeg가 직접 후속 |
| B | ConStyX content style augmentation generalizable medical image segmentation 2025 | CONSTYX (MICCAI 2025) 확인 |
| A | probabilistic DG medical image segmentation contrastive 2024 2025 | DET2PROB (2412.05572) 확인 |
| A | SAM SSDG medical segmentation 2024 2025 | SAM_SDG, DAPSAM 확인 |
| C | deep closing topological connectivity medical tubular 2024 MICCAI | DEEPCLOSING (IEEE TMI 2024) 확인 |
| A | style content decomposition augmentation domain generalizable medical segmentation 2025 | STYCONA (2502.20619) 확인 |
| D | CVPR ICCV ECCV 2025 augmentation policy uncertainty hard example DG segmentation | FIESTA 관련 재확인, GENORDETECT 확인 |
| D | generalize detect robust semantic segmentation multiple distribution NeurIPS 2024 | GENORDETECT (NeurIPS 2024) 확인 |
| C | local vessel radius observability vesselness Hessian scale space augmentation DG | VesselMorph, Hessian VF 관련 확인 |
| A | FIESTA Fourier semantic augmentation uncertainty guidance medical segmentation DG 2024 | FIESTA (2406.14308) 확인 |
| A | experience SSDG real world medical imaging deployment 2025 2026 | SDG_REALWORLD (2601.16359) 확인 |
| C | geometric topological deep transfer learning vessel 3D medical npj 2025 | FLOWAXIS (npj Digital Medicine 2026) 확인 |
| D | CVPR ICCV ECCV 2025 causal invariance representation learning segmentation | MCDRL (2508.05008), SI2CRL (MedIA 2025) 확인 |
| C | cbDice centerline boundary dice vascular MICCAI 2024 | CBDICE (MICCAI 2024) 확인 |
| A | Hallucinated domain generalization network 2025 | HALLUDG (Neural Networks 2025) 확인 |
| A | spectrum intervention invariant causal representation SSDG medical segmentation 2025 | SI2CRL (MedIA 2025) 확인 |
| C | RASS random amplitude spectrum synthesis SSDG MICCAI 2024 | RASS (MICCAI 2024) 상세 확인 |
| C | topology aware uncertainty image segmentation NeurIPS vessel connectivity | TOPUNCERT (NeurIPS 2023) 확인 |
| C | domain generalization retinal vessel Hessian vector field 2024 2025 | HESSIAN_VF (MedIA 2024) 확인 |

### 미탐색 / 추가 탐색 필요 구역

- [ ] VectorField Transformer for vessel (MIDL 2022, Hu et al.) 후속 연구
- [ ] DomainDrop 계열 (domain-sensitive feature suppression) 최신 후속
- [ ] CVPR 2025 / ICCV 2025 augmentation 관련 추가 직접 확인 필요
- [ ] nnUNet과 결합된 DG 방법들 (tabular/clinical context)
- [ ] Partial volume effect in vessel imaging — 명시적 다룬 논문 탐색 부족
- [ ] CoW (Circle of Willis) 특화 segmentation DG

### 발견하지 못한 논문 (검색 시도했으나 미확인)

- RASS의 후속 / 직접 비교 논문 (아직 시간 짧아서 없을 수 있음)
- 혈관 두께 조건부 augmentation을 명시적으로 다룬 논문: **현재 없음** (내 방법의 핵심 gap 확인)
