# SEARCH LOG

---

## 2026-07-06 — 정기 탐색 (Run #8)

### 실행 환경
- 날짜: 2026-07-06
- 모델: claude-sonnet-5 (4개 병렬 서브에이전트: Lane A+D, Lane B, Lane C, Lane 5 follow-up)
- 연도 우선: 2026 (Run #7 이후, 2026-06-03 ~ 2026-07-06 구간 집중), 보조: 2025 foundational
- 신규 발견: **10편** (Published Journal 2편 + Accepted Conference 1편 + Preprint Only 7편)

### 수행한 검색 쿼리 (병렬 에이전트 4개, 총 ~189회 검색/도구 호출)

| Lane | 담당 | 주요 발견 |
|------|------|-----------|
| A+D | SSDG 직접경쟁 + Top-tier Vision (CVPR/ICCV/ECCV/NeurIPS/ICLR/AAAI/ICML) | TSIAA (IEEE TMI), MARVEL, GPDG, Low-Freq Shortcuts, MorVess 재확인 |
| B | 방법론 유사 (class/region/structure-conditioned aug, nonlinear intensity, Bezier, spline 등) | TSIAA (Bezier per-instance adversarial aug) 발견 — 이번 Run 최우선 논문 |
| C | 혈관/tubular 구조 특화 (thin vessel, topology, centerline, vesselness, Hessian) | MorVess, vesselFM-CT, CSWinUNETR (MICCAI 2026) 발견 |
| Follow-up (Lane 5) | SLAug/RASS/Causality_SDG/MoreStyle/VesselMorph/VectorFieldTransformer/clDice/DomainDrop 후속 탐색 | TopBrain Challenge (medRxiv), WaveSDG, TubeMLLM 발견. SLAug/RASS/Causality_SDG/DomainDrop 직접 후속 논문 없음 확인 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 충돌 위험) — ★★★ 즉시 정독 필요

**TSIAA — Teacher-Student Instance-Level Adversarial Augmentation (IEEE TMI 2026)** — DOI: 10.1109/TMI.2025.3605162, Vol 45, pp 764-776
- **Run #8 최우선 발견.** 기존 known-list에 없던 **Published Journal** 논문.
- Learnable constrained Bézier 변환을 사용하는 Instance-level Augmentation Module(IAM)을 통해 **이미지 내 서로 다른 구조(instance)마다 다른 augmentation을 적용** — teacher-student adversarial loop으로 학습.
- Single-source DG, target domain 정보 미사용 — 내 세팅과 정확히 동일.
- **내 방법과의 핵심 차이**: TSIAA의 instance별 차등 augmentation은 **adversarially learned** (무엇을 얼마나 강하게 augment할지 학습을 통해 탐색), 나의 Continuous-ONA는 **명시적으로 측정된 local vessel radius/observability에 continuous하게 조건화**. TSIAA는 vessel radius/thickness 개념을 전혀 사용하지 않고, "instance" 단위도 혈관이 아닌 일반적인 해부학적 구조/영역 단위로 추정됨(원문 직접 확인 필요).
- **Novelty 방어 논거**: "구조 단위로 augmentation을 다르게 적용해야 한다"는 motivation을 공유하는 가장 가까운 선행 연구. 그러나 (1) conditioning 신호가 명시적 형태학적 측정값(radius)이 아닌 학습된 adversarial policy라는 점, (2) 혈관 특유의 "관찰 가능성(observability)" 개념이 없다는 점에서 명확히 구분됨. Related Work에서 반드시 상세 비교 필요.
- ⚠️ WebFetch로 원문 직접 확인 실패(네트워크 정책상 arxiv.org 차단 + IEEE Xplore 접근 제한) — 서지정보는 검색 스니펫 기반 재구성. **원문 PDF 직접 확인 최우선 과제.**

#### 방법론 신규 논문

**WaveSDG (arXiv 2603.28463, ~2026-03)** — Preprint Only
- Wavelet sub-band decomposition으로 anatomical structure vs. domain-specific appearance 분리, SSDG for fundus (vessel 포함) segmentation.
- SLAug/RASS의 Bezier/frequency perturbation과는 다른 구조 분리 전략. Radius 조건화 없음.

#### 혈관·tubular 특화 신규 논문

**MorVess (arXiv 2606.24214, 2026-06-23)** — Preprint Only
- Pulmonary vessel: Vessel Thickness Map(VTM, medial-axis 기반 continuous thickness)을 mask/distance map과 함께 **auxiliary supervision target**으로 예측 — augmentation이 아닌 loss/supervision 측면에서 "continuous vessel thickness" 개념을 사용한 최신 사례.
- **내 연구 지지 근거로 활용 가능**: continuous vessel thickness가 "유용한 신호"라는 독립적 증거. 단, augmentation에는 전혀 적용되지 않음 — gap 유지.

**TopBrain Segmentation Challenge (medRxiv, DOI 10.64898/2026.05.28.26354312, 2026-05-28)**
- TopCoW를 whole-brain 48-class 혈관으로 확장한 새 challenge/benchmark. CTA+MRA 90 volumes, per-vessel caliber(반지름) ground truth 포함.
- Menze lab 계열 (TopCoW 연장선). "smaller/complex vessels remain the true bottleneck" — 내 motivation을 독립적으로 재확인.
- **내 TOF-MRA 연구와 직접 관련된 신규 평가 리소스** — 향후 벤치마크 활용 가능성 검토 필요.

**vesselFM-CT (arXiv 2606.09400)** — CVPR 2025 vesselFM의 CT 확장판. TubeLoss로 대동맥~미세혈관까지 radius 이질성 처리. Loss 설계 관점, augmentation 아님.

**CSWinUNETR (arXiv 2606.19824, MICCAI 2026 accepted 표기)** — Cross-shaped stripe attention 기반 thin structure 분할 아키텍처. Augmentation/DG 실험 없음.

**TubeMLLM (arXiv 2603.09217)** — Vessel-like anatomy를 위한 topology-aware multimodal foundation model. Augmentation 무관.

**MARVEL (arXiv 2605.25363)** — Murray's Law 기반 radius-aware topology loss, multi-source/multi-modality. Radius를 loss에 사용(AG-TAL과 유사 축), augmentation 아님.

#### Top-tier Vision (참고용, 낮은 관련성)

**GPDG (Frontiers of Computer Science, 2026-06-15)** — Domain을 latent environment의 샘플로 재정의하는 이론적 DG 프레임워크. 의료영상/증강과 무관하나 "단일 invariant mapping의 한계"라는 이론적 motivation으로 인용 가능.

**Low-Frequency Shortcuts in Texture-Driven Visual Learning (arXiv 2606.03493)** — texture-driven domain에서 low-frequency shortcut 의존성 분석. 내 "thick vessel의 intensity shortcut 억제" motivation을 지지하는 방계 근거로 활용 가능.

### Novelty Gap 재확인

- **"continuous radius/observability-conditioned augmentation strength"** 키워드: Run #8에서도 명시적으로 이를 구현한 논문 없음 확인.
- 가장 근접한 논문은 TSIAA (adversarial instance-level, radius 미사용)와 MorVess/MARVEL/vesselFM-CT (radius를 loss/supervision에 사용, augmentation 미사용) — **두 축 모두 내 방법과 정확히 겹치지 않음**.
- **핵심 gap 유지**: augmentation strength를 vessel radius/observability의 continuous function으로 조건화하는 논문은 여전히 부재.

### 미탐색 / 추가 탐색 필요 구역

- [ ] TSIAA 원문 PDF 직접 확인 (IEEE Xplore 접근 방법 확보 필요) — instance 정의가 혈관 구조 단위인지, augmentation 강도 결정 메커니즘 상세
- [ ] TSIAA와 ADA(MICCAI 2025)의 관계 확인 — 동일 연구 그룹 여부, ADA의 후속판 가능성
- [ ] TopBrain 데이터셋 실제 접근 가능성 및 caliber ground truth 형식 확인
- [ ] MorVess VTM 계산 방식(medial-axis propagation) 상세 — 내 observability score 계산과 비교
- [ ] MARVEL Murray's Law exponent와 AG-TAL radius-aware Dice의 관계 비교
- [ ] MICCAI 2026 정식 accepted list 공개 시 (통상 7~8월) 재탐색 필요
- [ ] arXiv 직접 접근 네트워크 정책 확인 (이번 Run에서 일부 서브에이전트가 403 겪음) — 원문 확인 대체 경로 마련

---

## 2026-06-03 — 정기 탐색 (Run #7)

### 실행 환경
- 날짜: 2026-06-03
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024 (foundational 한정)
- 신규 발견: **5편** (Published Journal 3편 + Preprint 1편 + Accepted Conference 1편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation augmentation CVPR MICCAI 2026 arXiv new | 기존 목록 재확인 |
| A | vessel segmentation domain generalization cerebrovascular TOF-MRA brain 2026 arXiv new method | 기존 목록 재확인 |
| B | structure-conditioned augmentation intra-class observability vessel radius domain generalization 2025 2026 | DGSSA 재확인, 기존 gap 재확인 |
| C | tubular structure segmentation thin vessel topology domain generalization CVPR ICCV 2026 new | 기존 목록 재확인 |
| D | CVPR 2026 domain generalization segmentation augmentation distribution shift robust | DET2PROB, DROPGEN 재확인 |
| A | arXiv 2606 domain generalization medical image segmentation augmentation vessel 2026 | 기존 목록 재확인 |
| A | MICCAI 2026 domain generalization vessel segmentation cerebrovascular accepted paper | MICCAI 2026 미공개 확인 |
| D | CVPR 2026 accepted papers domain generalization segmentation robust | FLEX-Seg arXiv 2511.22948 AAAI 2026 발견 |
| B | adaptive augmentation strength structure-aware label-preserving transformation medical segmentation 2025 2026 | 기존 목록 재확인 |
| B | "augmentation budget" OR "augmentation strength" intra-class structure-conditioned medical segmentation DG | **DCON (Pattern Recognition 2025)** 발견 — dual-view aug + bilevel contrastive SSDG |
| A | DCON "dual-augmentation constraint" SSDG medical image segmentation Pattern Recognition 2025 | **DCON** 상세 확인: Ruofan Wang et al., Prostate +2.52% over SLAug |
| D | FLEX-Seg arXiv 2511.22948 AAAI 2025/2026 domain generalized segmentation noise robust | **FLEX-SEG (AAAI 2026)** 확인: ojs.aaai.org article 37492 = AAAI 2026 |
| A | "anatomically-robust" "feature-unbiased" domain generalization medical segmentation ScienceDirect 2025 2026 | **ARFU (Expert Systems w/ Applications 2025)** 발견: SRG+APG, Sep 2025 |
| C | "topology-aware multiclass segmentation" "Circle of Willis" MRA CTA 2026 arXiv ScienceDirect | **COW_TOPO (Comput. Biol. Med. Vol 204, 2026)** 발견: TopCoW 2024 1위 |
| C | AG-TAL arXiv 2604.27357 "Anatomically-Guided Topology-Aware Loss" Circle of Willis multi-center | **AG-TAL** 확인: **radius-aware Dice loss** = GT radius를 loss weighting에 직접 활용 |
| A | SLAug ConStyX SRCSM follow-up citation 2026 new domain generalization medical segmentation | 기존 목록 재확인 |
| D | NeurIPS 2025 accepted papers medical image segmentation domain generalization augmentation | GRAPHSEG 재확인, 기존 목록 재확인 |
| D | ICLR 2026 medical image segmentation domain generalization augmentation accepted papers | ICLR 2026 의료영상 DG 직접 히트 없음 (5,355편 중 탐색 어려움) |
| A | DCON Pattern Recognition 2025 SSDG prostate cardiac fundus experiments | DCON 실험 상세 확인: 3 datasets, code available |
| A | ARFU "shape regularization" "anatomical prior" DG medical segmentation Expert Systems 2025 | ARFU 상세 확인: CT-MRI abdominal + cardiac, no arXiv preprint found |
| C | "topology-aware multiclass" "Circle of Willis" "Computers in Biology" 2026 method domain shift | COW_TOPO 상세 확인 + AG-TAL (2604.27357) 발견 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 관련)

**DCON (Pattern Recognition 2025)**
- SSDG 직접경쟁, dual-view augmentation (image + feature), bilevel contrastive learning
- SLAug보다 +2.52% on Prostate (6 centers)
- **내 방법과의 차이**: DCON = class-level/image-level diversity, 나 = intra-class radius별 augmentation budget. DCON에는 thin vessel 보호 개념 없음.

**AG-TAL (arXiv 2604.27357, April 2026)** — ⚠️ High priority read
- **radius-aware Dice loss**: GT vessel radius를 localized weighting으로 Dice loss에 통합
- 내 ONA도 vessel radius를 augmentation budget에 사용 → 동일 "radius" 개념의 다른 활용
- **완전히 다른 mechanism** (loss vs. augmentation) + **다른 task setting** (closed-set vs. SSDG)
- 내 동기 지지 근거로 활용 가능: "radius-aware training이 이미 loss 측면에서 효과적임을 AG-TAL이 증명"

#### 방법론 신규 논문

**ARFU (Expert Systems w/ Applications 2025)**
- Shape Regularization-Guided Augmentation + Anatomical Prior-Guided Augmentation
- low-frequency 구조를 appearance transform의 regularizer로 활용
- 내 방법과 유사한 "구조 보존 증강" 방향 — 하지만 organ-level, 나는 intra-vessel level

#### 혈관 특화 신규 논문

**COW_TOPO (Computers in Biology and Medicine Vol 204, March 2026)**
- Topology refinement post-processing for CoW MRA+CTA multiclass segmentation
- TopCoW 2024 hidden test 1위, out-of-domain MRA Dice 0.81
- 내 TOF-MRA 연구와 해부학적으로 인접한 데이터셋/문제

#### Top-tier Vision

**FLEX-SEG (AAAI 2026)**
- Diffusion-generated image misalignment을 robust learning의 기회로 전환
- 자연영상 city-scene DG (의료영상 아님)
- Uncertainty Boundary Emphasis가 내 observability 개념과 방향 유사하나 mechanism 완전히 다름

### Novelty Gap 재확인

- **"vessel observability conditioned augmentation"** 키워드: Run #7에서도 직접 명시 논문 없음
- **"radius-conditioned augmentation budget"** 키워드: AG-TAL이 radius를 loss에 활용하지만 augmentation에는 없음
- **내 핵심 gap 유지**: intra-class vessel radius → augmentation budget (continuous) mapping

### 미탐색 / 추가 탐색 필요 구역

- [ ] AG-TAL 전문 독해: radius 계산 방식 상세 (skeleton distance transform 공식)
- [ ] DCON 전문 독해: GLSA "controllability" 파라미터 정의 + ablation 결과
- [ ] ICLR 2026 DG/augmentation 관련 논문 direct 탐색 (openreview.net에서 직접 검색)
- [ ] "Topology-Aware Exploration of Circle of Willis" (arXiv 2410.15614) Cat C 추가 여부 검토
- [ ] Expert Systems w/ Applications / Pattern Recognition에서 추가 SSDG 논문 탐색
- [ ] IJCAI 2026 accepted list 공개 시 DG 관련 논문 탐색

---

## 2026-06-02 — 정기 탐색 (Run #6)

### 실행 환경
- 날짜: 2026-06-02
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024 (foundational 한정)
- 신규 발견: **5편** (Accepted Conference 2편 + Published Journal 1편 + Preprint 2편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation augmentation 2026 MICCAI CVPR arXiv | 기존 목록 재확인 |
| A | vessel segmentation domain generalization 2026 cerebrovascular TOF-MRA brain arXiv IEEE TMI | 기존 확인, MultiDomain Brain 재확인 |
| C | tubular structure segmentation thin vessel topology domain generalization 2025 2026 CVPR ICCV NeurIPS | HarmonySeg 등 기존 재확인 |
| A | CVPR 2025 domain generalization segmentation structure-preserving augmentation adaptive | 기존 목록 재확인 |
| B | nonlinear appearance augmentation structure-conditioned observability medical image segmentation 2025 2026 | SPAD (arXiv 2603.07889) 발견 (내 주제와 다름) |
| A | MICCAI 2025 domain generalization cerebrovascular vessel segmentation augmentation new method | ISAC, MBFCV 재확인 |
| A | MICCAI 2025 open access domain generalization style augmentation frequency single source new | **MixStyleFlow (MICCAI 2025 Paper 3460)** 발견 — normalizing flows for domain style generation |
| B | arXiv 2505.10223 Data-Agnostic Augmentations MixUp Fourier OOD MRI segmentation MIDL 2025 | **DAGMRI (MIDL 2025)** 확인: MixUp + AFA in nnU-Net, accepted full paper at MIDL 2025 |
| B | AADG automatic augmentation domain generalization retinal image segmentation TMI 2022 | **AADG (IEEE TMI 2022)** 확인: adversarial RL로 augmentation policy search, Sinkhorn 기반 domain diversity 극대화. 미인덱스 foundational paper |
| A | class-invariant test-time augmentation domain generalization segmentation 2025 arXiv | CI-TTA (arXiv 2509.14420) 발견 — 자연영상, 낮은 관련성, 미수록 유지 |
| A | ICCV 2025 domain generalization segmentation augmentation robust distribution shift | ADAL 재확인, 기존 목록 재확인 |
| D | NeurIPS 2025 medical image segmentation domain generalization augmentation | 기존 목록 재확인 |
| B | BucketAugment reinforced domain generalisation CT segmentation Q-learning augmentation policy | BucketAugment (IEEE OJEMB 2024) 발견 — RL+Q-learning으로 CT augmentation policy 탐색. 내 방법과 다른 paradigm (policy search vs. structure-conditioned) |
| A | arXiv June 2026 domain generalization medical image segmentation vessel new | **FA-SAM (IEEE SMC 2025, arXiv 2507.17281)** 발견 — 낮은 tier (SMC), 낮은 관련성 |
| A | WACV 2025 invariant causal mechanisms single-source cross-modality medical image segmentation | INVCAUSAL(2411.05223) = WACV 2025 공식 확인 (기존 인덱스 상태 "Preprint Only" → 실제 WACV 2025 Accepted) |
| C | arXiv 2605 2026 vessel segmentation domain generalization synthetic annotations | **VesselSim (arXiv 2605.26277)** 발견 — 3D vessel 16,500 synthetic volumes + domain randomized intensity, Concordia U |
| A | arXiv 2605.09925 Frequency Adapter SAM generalized medical image segmentation 2026 | **FreqAdapSAM (arXiv 2605.09925)** 발견 — SAM + frequency adapter for DG, May 2026 preprint |
| Follow-up | CDDSA contrastive domain disentanglement style augmentation MedIA 2023 | CDDSA (MedIA Oct 2023) 존재 확인 — 2023 논문, 유사 논문(CONSTYX, ICRN) 이미 indexed → 수록 보류 |
| Follow-up | MixStyleFlow MICCAI 2025 normalizing flows domain generalization | MixStyleFlow 세부 확인: 저자 홍콩대, prostate MRI + fundus, arXiv preprint 미확인 |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (Novelty 충돌 주의)

**AADG (IEEE TMI 2022)** — arXiv: 2207.13249
- 내 novelty claim과 가장 가까운 foundational 선행 연구
- Adversarial training + deep RL로 augmentation diversity를 자동 탐색
- Sinkhorn distance로 multiple augmented domain 간 diversity를 proxy task로 최대화
- **내 방법과의 차이**: AADG = cross-image augmentation policy search (어떤 operation을 얼마나 강하게 적용할지 전체 이미지 단위로 탐색). 나 = **intra-image structure-specific** augmentation strength 조절 (같은 이미지 내 thin/thick vessel이 서로 다른 강도). AADG는 혈관 내부 구조 이질성(thin vs. thick)을 완전히 무시.
- foundational paper로 수록 필수

**MixStyleFlow (MICCAI 2025, Paper 3460)** — papers.miccai.org
- Normalizing flows를 사용해 도메인 스타일 분포를 명시적으로 모델링 후 MixStyle과 결합
- Feature channel dimension을 따라 원본 통계와 모델링된 스타일을 mix
- 내 방법과는 paradigm이 다름: MixStyleFlow = feature-level style randomization (전체 feature map 단위), 나 = pixel-level structure-conditioned appearance (intra-image vessel-specific). 직접 충돌은 없지만 같은 DG 문제의 경쟁 방법.

#### 방법론 유사 논문

**DAGMRI (MIDL 2025)** — arXiv: 2505.10223
- nnU-Net에 MixUp + Auxiliary Fourier Augmentation(AFA)을 통합해 OOD generalization 향상
- "feature separability + compactness 향상"이 일반화를 개선한다는 관점
- 내 방법과 겹치지 않음 (MixUp/Fourier aug = 전체 이미지 uniform, 나 = structure-specific)

#### 최신 혈관 특화 논문

**VesselSim (arXiv 2605.26277)** — May 2026
- 실제 annotation 없이 3D 혈관 분할 학습: 기하학적 혈관 시뮬레이션 + domain-randomized intensity synthesis
- vesselFM과 경쟁하는 합성 데이터 기반 접근
- 내 방법과 paradigm 다름 (합성 데이터 학습 vs. real source 기반 SSDG aug)

### 상태 업데이트 (기존 논문)

- **INVCAUSAL** (arXiv 2411.05223): 기존 인덱스에 "Preprint Only"로 기록됨. 실제 WACV 2025에 accepted (pp. 3592-3602). 내용 변경 금지 원칙에 따라 메인 인덱스 수정 안 하되, 이 로그에 기록.

### 미탐색 / 추가 탐색 필요 구역

- [ ] AADG 전문 독해: augmentation operation search space 구성 및 Sinkhorn proxy 상세
- [ ] MixStyleFlow 전문 독해: normalizing flow 구성 + prostate/fundus 실험 상세
- [ ] VesselSim code/data: 합성 혈관 domain randomization scheme (TOF-MRA 적용 가능성)
- [ ] "vessel observability conditioned augmentation" 키워드 여전히 없음 → Continuous-ONA gap 유지
- [ ] BucketAugment 독해 가치 평가: Q-learning aug policy search vs. 내 연속적 구조 기반 조절
- [ ] CI-TTA (arXiv 2509.14420): 자연영상 test-time augmentation, 수록 여부 재검토

---

## 2026-06-01 — 정기 탐색 (Run #5)

### 실행 환경
- 날짜: 2026-06-01
- 모델: claude-sonnet-4-6
- 연도 우선: 2025–2026, 보조: 2024
- 신규 발견: **6편** (Accepted Conference 1편 + Published Journal 2편 + Workshop 2편 + Preprint 1편)

### 수행한 검색 쿼리

| Lane | 쿼리 | 주요 발견 |
|------|------|-----------|
| A | single source domain generalization medical image segmentation augmentation 2026 arXiv MICCAI | 기존 목록 재확인, WACV 2026 SSDG paper 발견 (classification, 낮은 관련성) |
| A | NeurIPS 2025 domain generalization segmentation augmentation robust distribution shift proceedings | XDiff3D (NeurIPS 2025, 3D seg) 발견; 의료영상 직접 DG 신규 없음 |
| A | CVPR 2025 domain generalization semantic segmentation augmentation structure-aware | **TTDG-MGM (CVPR 2025, arXiv 2503.13012)** 발견 (Run #4에서 이미 인덱싱됨) |
| A | CVPR 2025 medical image segmentation domain generalization single source open access thecvf | TTDG-MGM 재확인; **MoSE (CVPR 2025W, arXiv 2504.09601)** 발견 |
| A | TOF-MRA cerebrovascular segmentation domain generalization multi-center 2025 2026 | **COSTA (IEEE TMI 2024)** 발견: 8-center TOF-MRA + CESAR style self-consistency |
| A | COSTA IEEE TMI 2024 cerebrovascular TOF-MRA CESAR style self-consistency network | COSTA 공식 확인: DOI 10.1109/tmi.2024.3424976, Vol 43(12), pp 4442-4456 |
| A | "domain game" arXiv 2406.02125 SSDG MICCAI CMMCA 2024 | **Domain Game (MICCAI 2024 Workshop CMMCA)** 확인: geometric sensitivity로 anatomical/domain feature 분리 |
| B | class-specific augmentation strength intra-class structure-conditioned perturbation segmentation 2025 2026 | 기존 목록 재확인 |
| B | arXiv 2025 2026 nonlinear appearance augmentation observability-conditioned morphology-aware vessel DG | 직접 명시 논문 없음 — Continuous-ONA gap 재확인 |
| B | domain generalization medical segmentation 2026 arXiv June frequency augmentation nonlinear | **FL-AugDG (arXiv 2602.20773)** 발견: GIN + frequency aug evaluation in federated setting |
| C | optimized vessel segmentation small vessel enhancement arXiv 2411.15251 | **OVS-Net (IEEE TIP 2025)** 확인: Vol 34, pp 7168-7179; dual-branch small vessel + morphological correction |
| C | MICCAI 2025 vessel brain Circle of Willis cerebrovascular segmentation new 2025 | **VesselVerse (MICCAI 2025)** 발견: 950-image brain vessel annotation dataset |
| C | partial volume effect vessel MRI segmentation thin vessel augmentation DG | 직접 다룬 신규 논문 없음 (내 방법의 gap 재확인) |
| D | NeurIPS 2025 proceedings DG segmentation augmentation 3D | XDiff3D ("No Object Is an Island") NeurIPS 2025 확인 — 3D semantic seg 일반화, 의료영상 아님 |
| D | ICLR 2025 augmentation policy domain generalization robustness segmentation | 직접 신규 없음 |
| Follow-up | DomainDrop ICCV 2023 follow-up 2024 2025 cite | 직접 follow-up 논문 없음 (확인) |

### 핵심 신규 발견 요약

#### 최우선 주의 논문 (즉시 읽기)

**COSTA (IEEE TMI 2024)** — DOI: 10.1109/tmi.2024.3424976
- 8개 imaging center TOF-MRA 데이터셋 (COSTA) + CESAR network
- Style Self-Consistency Loss: 서로 다른 center style을 표준 style로 정렬
- Coarse-to-fine architecture + automatic feature selection (long-range + local context)
- **내 연구와 직접 연관**: TOF-MRA multi-center cerebrovascular segmentation에서 style heterogeneity를 직접 다룬 논문. 내 방법의 baseline 비교 필수. COSTA 데이터셋 활용 가능성 탐색 필요.

#### 주요 신규 발견

**OVS-Net (IEEE TIP 2025)** — arXiv: 2411.15251, IEEE TIP Vol 34 pp 7168-7179
- SAM backbone 기반 macro/micro 이중 분기: macro vessel 추출 + micro (small) vessel enhancement
- "segmentation algorithms optimized for overlap scores overlook small/fragile structures" 진술 → 내 동기 지지
- **내 방법과의 관계**: inference-time 추가 모듈 방식 vs 내 training-time augmentation — 겹치지 않음.

**Domain Game (MICCAI 2024 Workshop, CMMCA)** — arXiv: 2406.02125
- Geometric transformation sensitivity로 anatomical feature vs domain-specific feature 분리
- prostate segmentation +11.8%, brain tumor +10.5% 개선

**FL-AugDG (arXiv 2602.20773)** — Feb 2026 preprint
- Federated setting에서 GIN, spatial aug, frequency aug, normalization을 체계적으로 비교
- GIN이 모든 설정에서 일관되게 최상 성능 → 내 uniform nonlinear aug baseline 구성 근거

### 미탐색 / 추가 탐색 필요 구역

- [ ] COSTA 데이터셋 실제 접근 가능성 (GitHub iMED-Lab/COSTA 확인)
- [ ] OVS-Net morphological correction module 상세: 내 thin vessel 보호 mechanism과 연결 가능성
- [ ] "radius-conditioned augmentation" 또는 "thickness-conditioned appearance transform" 직접 키워드 재탐색 → 여전히 없음 (내 핵심 novelty gap 유지)

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
