# SEARCH LOG

---

## 2026-05-30 — 정기 탐색 (Run #4)

### 실행 환경
- 날짜: 2026-05-30
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024
- 신규 발견: **13편** (Accepted Conference 5편 + Published Journal 7편 + Preprint 1편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation NeurIPS 2025 CVPR 2025 adaptive | 기존 논문 재확인, NeurIPS 2025 직접 미발견 |
| A | BucketAugment reinforcement learning augmentation policy domain generalization CT segmentation 2024 | **BUCKETAUG (IEEE OJEMB 2024)** 확인: Q-learning 기반 augmentation policy |
| A | deep learning domain randomization image feature space multiorgan CT MRI 2025 Radiology AI | **DOMRAND_IMG_FEAT (Radiology AI 2025)** 확인: image+feature space 동시 randomization |
| A | NeurIPS 2025 poster retinal vessel domain generalization graph prior | **GRAPHSEG (NeurIPS 2025)** 확인: Deformable Graph Priors for retinal vessel DG |
| A | FASAM fully automated SAM SSDG medical image segmentation 2507.17281 | **FASAM (IEEE SMC 2025)** 확인: Auto-prompted SAM for SSDG |
| B | spatially adaptive augmentation budget structure-conditioned per-pixel appearance 2025 | **ASAUG (arXiv 2505.23438)** 발견: entropy-based adaptive aug weight |
| B | domain generalization rejecting extreme augmentations reward function WACV 2024 | **REAUG (WACV 2024)** 확인: augmentation 강도 reward function으로 거부 |
| B | dual stream feature augmentation domain generalization hard fictitious ACM MM 2024 | **DFA (ACM MM 2024)** 확인: uncertainty-guided hard cross-domain feature generation |
| B | causal style bias deconfounding domain generalization IEEE TIP 2025 | **SDCL (IEEE TIP 2025)** 확인: backdoor causal learning + style-guided expert module |
| B | Stylizing ViT anatomy-preserving style transfer DG medical ISBI 2026 | **STYLIZING_VIT (ISBI 2026)** 확인: weight-shared ViT attention for DG |
| B | SCSD semantic consistency style diversity AAAI 2025 domain generalized segmentation | **SCSD (AAAI 2025)** 확인: Semantic Query Booster + Text-Driven Style Transform |
| C | OVS-Net optimized vessel segmentation small vessel enhancement IEEE TIP 2025 | **OVSNET (IEEE TIP 2025)** 확인: dual-branch macro/micro vessel extraction, 17-dataset benchmark |
| C | topology-aware circle of willis MRA CTA segmentation Computers in Biology and Medicine 2026 | **COW_TOPOLOGY (CBM 2026)** 확인: nnUNet + Skeleton Recall + topology post-processing, TopCoW 2024 winner |
| C | vessel diameter conditioned augmentation radius adaptive observability 2025 | 직접 명시 논문 없음 — 내 방법의 핵심 gap 재확인 |
| C | partial volume effect vessel augmentation simulation thin MRI 2025 | 해당 논문 없음 |
| D | NeurIPS 2025 domain generalization augmentation causal structure robust proceedings | GRAPHSEG (NeurIPS 2025) 발견 외 추가 신규 없음 |
| D | CVPR 2025 domain generalization medical image augmentation structure style | SCSD 재확인, 직접 CVPR 2025 의료영상 DG 논문 없음 |
| D | domain-randomized deep learning neuroimage analysis IEEE Signal Processing Magazine 2025 | **DOMRAND_NEURO (IEEE SPM 2025)** 확인: 합성영상 기반 intensity randomization |
| Follow-up | DomainDrop follow-up domain-sensitive feature suppression 2024 2025 | DomainDrop 직접 후속 없음 — 원 논문 ICCV 2023 에서 DFA, SDCL 등이 유사 방향 |
| Follow-up | VectorField Transformer retinal vessel MIDL 2022 follow-up | OpenReview에서 MIDL 2022 확인 — 직접 후속 논문 없음, HESSIAN_VF가 가장 가까운 발전 |
| Follow-up | SLAug citation 2025 2026 novelty-related | GRAPHSEG, REAUG가 augmentation budget 방향에서 가장 주목할 후속 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 충돌 위험)

**REAUG (WACV 2024)** — arXiv:2310.06670, WACV 2024 pp.2215-2225
- "augmentation이 너무 강하면 학습에 해롭다" → reward function으로 extreme augmentation 거부
- **내 방법과의 공통점**: augmentation budget 개념, 강한 aug가 오히려 해가 된다는 인식
- **핵심 차이**: REAUG = 이미지 전체 단위에서 augmentation 거부/수용 (binary decision at image-level), 나 = 동일 이미지 내 혈관 구조의 관찰 가능성에 따라 연속적으로 강도 조절 (intra-image continuous). REAUG는 thin vessel 개념 없음.

**GRAPHSEG (NeurIPS 2025)** — OpenReview ID: zVkbsGlKn9
- 구조 보존(structure-preserved) vs 구조 열화(structure-degraded) 컴포넌트 분리 + deformable graph prior
- **내 방법과의 공통점**: "구조 가시성(observability)"에 따른 분리 표현 개념
- **핵심 차이**: GRAPHSEG는 feature decomposition + graph prior 기반 representation learning, 나는 augmentation budget 조절. GRAPHSEG는 test-time에 graph prior가 필요. NeurIPS 2025 논문으로 중요도 높음.

**OVSNET (IEEE TIP 2025)** — arXiv:2411.15251, DOI: 10.1109/TIP.2025.3607583
- Micro vessel enhancement module이 명시적으로 small vessel을 별도 처리
- **내 방법과의 공통점**: "thin/small vessel을 굵은 vessel과 다르게 처리해야 한다"는 동기 공유
- **핵심 차이**: OVSNET는 segmentation 모델의 feature extraction branch를 분리, 나는 augmentation 단계에서 strength를 조절. OVSNET는 DG 논문이 아님.

#### 주요 신규 발견

**SCSD (AAAI 2025)** — arXiv:2412.12050
- Semantic Query Booster + Text-Driven Style Transform: 자연영상 DG segmentation (GTAV→Cityscapes)
- 상위 tier 논문으로 semantic consistency + style diversity를 동시에 다루는 방법

**DFA (ACM MM 2024)** — arXiv:2409.04699, DOI: 10.1145/3664647.3680652
- uncertainty-guided hard cross-domain feature 생성 + 비인과적 특징 adversarial masking
- feature-space augmentation의 두 축(diversity + causality)을 동시에 다룸

**SDCL (IEEE TIP 2025)** — arXiv:2503.16852
- Style-guided expert module(SGEM) + backdoor causal learning: 스타일을 confounder로 처리
- 의료+자연영상 멀티태스크 실험

**BUCKETAUG (IEEE OJEMB 2024)** — DOI: 10.1109/OJEMB.2024.3397623
- Q-learning으로 augmentation stack 최적 policy 탐색. medical DG.

**DOMRAND_IMG_FEAT (Radiology AI 2025)** — DOI: 10.1148/ryai.240586
- image space + feature space 동시 domain randomization으로 nnUNet 기반 multiorgan DG

**COW_TOPOLOGY (CBM 2026)** — PubMed:41637822
- TopCoW 2024 challenge 우승 솔루션: nnUNet + Skeleton Recall + topology post-processing
- 내 TOF-MRA 실험 데이터셋과 가장 직접 관련된 방법 중 하나

**OVSNET (IEEE TIP 2025)** — arXiv:2411.15251
- Dual-branch (macro/micro) vessel 처리 + 17 데이터셋 벤치마크

### 미탐색 / 추가 탐색 필요 구역

- [ ] CVPR 2025 main track augmentation 논문 — 직접 검색에서 의료영상 DG 신규 없음
- [ ] NeurIPS 2025 proceedings에 GRAPHSEG 외 vessel/DG 추가 논문 가능성 확인
- [ ] GRAPHSEG arXiv ID 확인 (OpenReview만 확인, arXiv preprint 별도 확인 필요)
- [ ] REAUG와 내 방법의 차이 논문에서 명시적으로 articulate하는 방안 검토
- [ ] "intra-image structure-conditioned augmentation" 또는 "within-class augmentation budget" 키워드 재탐색
- [ ] TopBrain 2025 challenge dataset 활용 가능성 탐색

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
