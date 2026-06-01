# SEARCH LOG

---

## 2026-05-31 — 정기 탐색 (Run #4)

### 실행 환경
- 날짜: 2026-05-31
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024
- 신규 발견: **8편** (Accepted Conference 5편 + Preprint 3편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation augmentation MICCAI 2025 2026 vessel brain | 기존 논문 재확인 (ADA, RASS 등) |
| A | domain generalization cerebrovascular TOF-MRA vessel segmentation 2025 2026 MICCAI TMI | VesselVerse (MICCAI 2025 dataset 논문) 발견, 기존 확인 |
| A | class-conditioned structure-aware augmentation medical image segmentation domain generalization 2025 2026 | DGSSA, SRCSM 등 기존 논문 재확인 |
| A | NeurIPS 2025 domain generalization augmentation segmentation structure preserving | **GRAPHSEG (NeurIPS 2025)** 발견: Deformable Graph Priors for retinal vessel DG |
| A | ICLR 2025 domain generalization segmentation data augmentation structure invariance | 기존 논문 재확인 (XDomainMix 등) |
| B | CVPR ICCV 2025 augmentation budget adaptive structure aware perturbation domain generalization | PASTA 등 기존 재확인, ICCV 2025 전체 탐색 시도 |
| B | morphology-aware augmentation vessel radius thickness conditioned domain generalization 2025 2026 | 직접 명시 논문 여전히 없음 → 내 gap 재확인 |
| C | tubular structure segmentation thin vessel domain generalization 2025 2026 CVPR ICCV ECCV NeurIPS | **VessShape (arXiv 2510.27646)** 발견: few-shot vessel shape prior, DS-Mamba 발견 |
| C | VessShape few-shot blood vessel segmentation shape priors synthetic images 2025 | **VessShape (arXiv 2510.27646)** 확인: tubular geometry + diverse textures으로 shape bias 유도 |
| C | MICCAI 2025 vessel brain domain generalization single source (open access portal) | SAM-OSLN (MICCAI 2025), **L2CP** (MICCAI 2025 Paper 3277) 발견 |
| C | "test-time training" vessel segmentation domain generalization copy-paste local contrast MICCAI 2025 | **L2CP (MICCAI 2025)** 상세 확인: morphological closing으로 thin vessel 제거, local contrast copy-paste |
| D | CVPR 2025 open access vessel vascular segmentation domain generalization | **vesselFM (CVPR 2025)** 확인: Foundation model for 3D blood vessel DG (arXiv 2411.17386) |
| D | CVPR 2025 test-time domain generalization medical image segmentation morphological prior | **TTDG-MGM (CVPR 2025)** 확인: Universe learning + multi-graph matching (arXiv 2503.13012) |
| D | NeurIPS 2025 medical image vessel domain generalization augmentation openreview accepted | **GRAPHSEG (NeurIPS 2025)** 확인: openreview zVkbsGlKn9, poster 115045 |
| D | LangDAug Langevin data augmentation multi-source DG medical image ICML 2025 | **LangDAug (ICML 2025)** 확인: arXiv 2505.19659, EBM + Langevin dynamics |
| D | CVPR 2025 domain generalization segmentation robust distribution shift new | DROPGEN (arXiv 2604.02564) 발견 |
| A | DROPGEN invariance biomedical domain generalization biomedical arXiv 2604.02564 | **DROPGEN (arXiv 2604.02564)** 확인: foundation model repr. + source intensities, 3D biomedical seg |
| A | semantic data augmentation invariant risk minimization medical image domain generalization arXiv 2502.05593 | **SDAIRM (arXiv 2502.05593)** 확인: domain-oriented direction selector for IRM |
| Follow-up | SLAug RASS ConStyX follow-up citation 2025 2026 MICCAI ICCV NeurIPS | 기존 논문들의 follow-up 탐색 (특별한 신규 없음) |
| Follow-up | vessel diameter conditioned augmentation radius adaptive augmentation domain generalization 2025 2026 | **직접 명시 논문 여전히 없음** → Continuous-ONA의 핵심 gap 재확인 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 관련)

**L2CP (MICCAI 2025)** — Paper 3277
- "thin vessel 구조를 morphological closing으로 제거한다"는 아이디어를 DG에서 명시적으로 사용
- **내 방법과의 공통 전제**: thin vessel은 appearance/domain 측면에서 다르게 취급되어야 한다
- **핵심 차이**: L2CP = test-time adaptation (target 필요), 나 = training-time SSDG (target 불필요)
- Thin vessel의 특수성을 DG에서 독립적으로 인식한 논문 → 내 동기를 지지하는 선행 근거로 활용 가능

#### 최고 티어 신규 논문 (High-impact venue)

**vesselFM (CVPR 2025)** — arXiv 2411.17386
- 3D blood vessel DG를 위한 foundation model
- TOF-MRA 포함 4가지 modality에서 zero-shot 일반화
- Domain randomization + flow matching 생성 모델 사용
- 내 방법과는 paradigm이 다름 (large-scale foundation vs. lightweight SSDG)

**TTDG-MGM (CVPR 2025)** — arXiv 2503.13012
- Morphological prior를 multi-graph matching에 통합한 test-time DG for medical seg
- Retinal fundus + polyp benchmark에서 SOTA

**LangDAug (ICML 2025)** — arXiv 2505.19659
- EBM + Langevin dynamics로 source domain 간 intermediate 샘플 생성
- Multi-source DG (내 SSDG 설정과 다름)
- Rademacher complexity 상한 이론 분석 포함

**GRAPHSEG (NeurIPS 2025)** — OpenReview zVkbsGlKn9
- 변형 가능한 retinal atlas graph prior + variational Bayesian framework
- Structure-preserved와 structure-degraded 분해로 domain-invariant representation
- CHASE/DRIVE/HRF에서 domain shift 조건 SOTA

### 미탐색 / 추가 탐색 필요 구역

- [ ] vesselFM 전문 독해: domain randomization scheme 세부 사항 및 TOF-MRA 실험 결과
- [ ] LangDAug full text: Langevin dynamics aug 과정 상세 및 SSDG 적용 가능성 확인
- [ ] GRAPHSEG full text (OpenReview PDF): deformable graph prior 구현 상세
- [ ] TTDG-MGM GitHub 코드 확인: morphological prior 사용 방식
- [ ] L2CP: morphological closing scale 파라미터 → 혈관 두께 선택 기준 확인
- [ ] "vessel diameter conditioned augmentation" 키워드: 여전히 명시적 논문 없음 → 내 ONA gap 유지
- [ ] NeurIPS 2025 추가 탐색: medical DG 관련 논문 더 있을 수 있음

---

## 2026-05-29 — 정기 탐색 (Run #3)

### 실행 환경
- 날짜: 2026-05-29
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2023–2024
- 신규 발견: **15편** (Accepted Conference 7편 + Published Journal 2편 + Workshop 1편 + Preprint 5편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation augmentation MICCAI 2025 2026 | FreeSDG (MICCAI 2023) foundational 재확인 |
| A | vessel segmentation domain generalization cerebrovascular TOF-MRA 2025 2026 | 기존 목록 재확인 |
| A | UniDDG "one image as one domain" single domain generalization medical segmentation arXiv 2025 | **UniDDG (2501.04741)** 확인: OIOD hypothesis + Expansion Mask Attention + Style Aug |
| A | CVPR 2025 domain generalization segmentation augmentation distribution shift | 직접 신규 CVPR 2025 의료영상 논문 없음 |
| A | nonlinear intensity augmentation structure-aware morphology-aware medical image DG 2025 2026 | DualNorm (CVPR 2022) 재확인, **RandDG (Medical Physics 2025)** 발견 |
| A | class-invariant test-time augmentation domain generalization 2509.14420 | CI-TTA (arXiv) 확인 (자연영상) |
| A | DualNorm domain generalization medical segmentation Bezier normalization 2024 2025 | DualNorm CVPR 2022 재확인 (이미 알고 있는 논문) |
| A | NeurIPS 2025 domain generalization segmentation augmentation robust | NeurIPS 2025 proceedings 미공개 확인 |
| A | ICLR 2025 domain generalization segmentation invariant representation | XDomainMix 발견, 기존 목록 재확인 |
| A | frequency-aware domain randomization single-source DG medical image Medical Physics 2025 | **RandDG (Medical Physics 2025)** 상세 확인: GIN + ULoFT + consistency loss |
| A | FreeSDG frequency-mixed SSDG medical segmentation MICCAI 2023 | **FreeSDG (MICCAI 2023)** 공식 확인: 미인덱싱 상태였음 |
| B | anatomy-guided texture augmentation SSDG MICCAI 2024 cervical | **AGTA (MICCAI 2024 Workshop CMMCA)** 발견: DOI 10.1007/978-3-031-73360-4_8 |
| B | ICRN invariant content representation generalizable medical image segmentation IEEE TMI 2024 | **ICRN (IEEE TMI 2024)** 확인: gamma correction LSA + foreground/background 분리 증강 |
| B | cross-domain feature augmentation XDomainMix IJCAI 2024 | **XDomainMix (IJCAI 2024)** 확인: class/domain specific component decomposition |
| B | structure-aware stylized image synthesis robust medical image 2412.04296 | **STRUCSTYLE (arXiv 2412.04296)** 확인 |
| C | tubular structure segmentation domain generalization thin vessel topology 2025 2026 | DynSnake upsampling, SDF-TopoNet 발견 |
| C | dynamic snake upsampling boundary skeleton weighted loss tubular 2505.08525 | **DSUSNAKE (arXiv 2505.08525)** 확인 |
| C | SDF-TopoNet topology-aware tubular segmentation SDF pre-training 2503.14523 | **SDF-TopoNet (arXiv 2503.14523)** 확인 |
| C | brain vessel Circle of Willis segmentation DG cross-center 2024 2025 | TopCoW 2024 challenge 재확인, TopBrain 2025 발견 |
| C | MICCAI 2025 vessel brain artery topology morphology multi-center | V-DiSNet (one-shot active learning for vessel MICCAI 2025) 발견 |
| C | partial volume effect vessel segmentation thin structure MRI augmentation 2024 2025 | 해당 논문 없음 (내 방법의 탐색 gap 재확인) |
| D | CVPR 2025 domain generalization segmentation counterfactual augmentation adaptive | CVPR 2025 의료영상 DG 직접 신규 없음 |
| D | ICCV 2025 domain generalization medical segmentation augmentation structure | **ADAL (ICCV 2025, 2507.04302)** 확인: Lyapunov Exponent-Guided Optimization |
| D | exploiting domain properties language-driven DG segmentation ICCV 2025 2512.03508 | **DPMFormer (ICCV 2025)** 확인 |
| D | bi-level optimization single domain generalization 2604.06349 | **BiSDG (arXiv 2604.06349)** 확인 |
| D | adversarial data augmentation SDG Lyapunov 2507.04302 | ADAL ICCV 2025 확인 |
| D | NeurIPS 2025 proceedings DG augmentation | NeurIPS 2025 proceedings 아직 미공개 |
| D | domain generalization semantic segmentation survey CVPRW 2025 2510.03540 | Survey (CVPRW 2025) 확인 |
| Follow-up | MICCAI 2025 open access DG augmentation style appearance | D-CAM (MICCAI 2025) 발견: weakly-supervised DG |
| Follow-up | MICCAI 2025 label scarcity domain shift wavelet frequency exchange | **WFEX (MICCAI 2025)** 발견: Parametric Spline + Wavelet Frequency Exchange |
| Follow-up | causality latent feature augmentation SDG arXiv 2406.05980 | **CLFA (arXiv 2406.05980)** 확인 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 충돌 위험)

**AGTA (MICCAI 2024 Workshop CMMCA)** — DOI: 10.1007/978-3-031-73360-4_8
- "texture를 무차별 파괴하면 tumor boundary에 중요한 cue를 잃는다"고 주장
- Anatomy-guided texture augmentation으로 class-level 보호
- **내 방법과의 공통점**: "uniformly augmenting appearance is harmful to certain structures"
- **핵심 차이**: AGTA = class-level binary (tumor vs. 주변), 나 = single class 내 continuous radius. Workshop 논문.

**ICRN (IEEE TMI 2024)** — PMC: 11612095
- Gamma correction 기반 Local Style Augmentation: foreground / background 각각 별도 증강
- **내 방법과의 공통점**: annotation-region-specific augmentation 개념
- **핵심 차이**: ICRN = foreground/background binary, 나 = vessel foreground 내 thickness-continuous

#### 주요 신규 발견

**RandDG (Medical Physics 2025)** — DOI: 10.1002/mp.70118
- GIN input-space aug + ULoFT feature-space perturbation 조합
- 내 nonlinear aug baseline과 공학적으로 가장 유사한 논문 → baseline 비교 필수

**FreeSDG (MICCAI 2023)** — arXiv 2307.09005
- Frequency-mixed SSDG, MICCAI 2023 accepted. 기존에 알고 있었으나 미인덱싱 상태였음.

**ADAL (ICCV 2025)** — arXiv 2507.04302
- Lyapunov Exponent-Guided SDG. "edge of chaos" 학습. 자연영상이지만 SDG aug 이론에 기여.

### 미탐색 / 추가 탐색 필요 구역

- [ ] NeurIPS 2025 proceedings 정식 공개 후 재탐색 (예상: 2026-01 이후)
- [ ] ICRN full text 상세 확인: LSA gamma 적용 범위 및 foreground 정의
- [ ] AGTA full text 접근: anatomy segmentation pipeline 상세
- [ ] TopBrain 2025 challenge data: whole brain vessel annotation 활용 가능성
- [ ] CVPR 2025 main track 중 augmentation budget / structural DG 논문 추가 확인
- [ ] "vessel diameter conditioned augmentation" 또는 "radius adaptive augmentation" 직접 키워드 재탐색

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
