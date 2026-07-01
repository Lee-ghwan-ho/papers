# Literature Watch Report — Run #8

> 날짜: 2026-07-01
> 모델: claude-sonnet-5
> 실행 방식: 4개 병렬 search agent (Category A/B/C/D)
> 신규 논문: **7편** (Published Journal Article 2편 + Accepted Conference Paper 1편 + Preprint Only 4편)

---

## 요약

이전 Run(#7, 2026-06-03) 이후 약 4주 만의 탐색으로, CVPR 2026(6월 개최) 및 MICCAI 2026 accept 통보(~6/12) 시점을 겹쳐 탐색했다. 가장 중요한 발견은 **TSIAA(IEEE TMI 2026)** — 두 개의 독립 search agent가 각각 별도 경로로 발견했으며, "이미지 내 구조마다 non-uniform하게 augmentation 강도를 적용한다"는 주장이 내 핵심 novelty claim과 표현 수준에서 가장 근접한 논문이다. 즉시 전문 확인이 필요하다.

그 외에는 monotonic intensity augmentation의 저널 버전(ADVERIN), MICCAI 2026 최초 accept 확인 사례(CSWinUNETR), vessel radius를 skeleton tracking에 명시적으로 사용하는 최신 기법(TopoVST) 등을 확인했다.

---

## ⚠️ 최우선 주의 논문

### TSIAA — IEEE TMI 2026

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation
**Venue**: IEEE Transactions on Medical Imaging, 2026
**Status**: Published Journal Article
**발견 경위**: Category A agent와 Category B agent가 서로 독립적으로 동일 논문을 발견 — 우연이 아닌 실제 novelty 근접성을 시사

#### 방법 요약
- Instance-level Image Augmenter (IIAG): 여러 Instance-level Augmentation Module(IAM)이 learnable constrained Bézier transformation을 사용
- 핵심 주장: 기존 방법(SLAug 등)의 "이미지 전체에 uniform하게 augmentation을 적용한다"는 가정을 깨고, **같은 이미지 내 서로 다른 구조에 non-uniform하게** augmentation 적용
- Teacher-student adversarial scheme으로 out-of-source augmentation 탐색과 consistent representation 학습을 번갈아 수행

#### 내 방법과의 관계
- **공통점**: "uniform augmentation은 틀렸다"는 상위 motivation이 사실상 동일한 문장으로 표현됨
- **핵심 차이(잠정, 전문 미확인)**:
  - TSIAA = adversarial min-max로 non-uniformity를 **학습** (model-driven, 블랙박스). 이 경우 오히려 이미 취약한 얇은 구조를 adversarial하게 더 강하게 공격하는 방향으로 수렴할 위험이 있다 — 내가 방지하려는 정확한 실패 모드
  - Continuous-ONA = source annotation에서 유도한 **local vessel radius**로 강도를 explicit하게 결정 (data-driven, interpretable, "약한 구조 보호"라는 방향성이 명시적)
- **Novelty 위협도**: **Medium-High** — 상위 프레이밍의 겹침 때문에 관련 연구 서술을 반드시 "non-uniform augmentation" 수준이 아니라 "data-driven interpretable radius conditioning vs. adversarial model-driven conditioning"으로 한 단계 구체화해야 한다

자세한 분석은 `paper_notes/TSIAA.md` 참고.

---

## Category A — 신규 직접경쟁 논문

### ADVERIN — Medical Image Analysis 2025

**논문**: AdverIN: Monotonic Adversarial Intensity Attack for Domain Generalization in Medical Image Segmentation (원 아이디어: arXiv 2304.02720, 2023)
**Status**: Published Journal Article

- Adversarial하게 학습된 monotonic intensity mapping + spatial mask로 다양한 intensity style 생성
- 2D 망막 fundus, 3D 전립선 MRI 실험
- **내 방법과의 관계**: monotonic nonlinear intensity transform family를 공유하지만, 강도를 결정하는 신호가 adversarial loss(model-driven)인지 vessel radius(data-driven)인지가 핵심 차이. Baseline 비교 후보로 유력.

### WAVE_SDG — arXiv 2026 (Preprint Only)

**논문**: WaveSDG: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation (arXiv 2603.28463)

- Wavelet sub-band 분해로 구조/스타일을 분리하는 WISER 모듈
- Augmentation이 아닌 architectural feature decoupling 방식 — 내 방법과 mechanism이 다름 (참고용)

---

## Category B — 신규 방법론 논문

### PIXCF_CL — arXiv 2026 (Preprint Only)

**논문**: Pixel-level Counterfactual Contrastive Learning for Medical Image Segmentation (arXiv 2603.17110, Ben Glocker Lab)

- Causal generative model(DSCM/HVAE) 기반 pixel-level counterfactual contrastive pretraining
- Scanner/protocol confounder invariance를 목표로 함 — shortcut suppression이라는 목표는 유사하나 mechanism(causal pretraining vs. structure-conditioned augmentation)이 근본적으로 다름

---

## Category C — 신규 혈관·구조 특화 논문

### CSWinUNETR — MICCAI 2026 (Accepted Conference Paper)

**논문**: CSWinUNETR: Segmentation of Thin Anatomical Structures in Medical Images (arXiv 2606.19824)

- Cross-shaped stripe self-attention + detail-enhanced multi-scale attention으로 얇고 굴곡진 구조(retinal vessel, cerebral vasculature 포함) 분할
- MICCAI 2026 첫 accept 확인 사례 중 하나 (공식 accepted list는 아직 비공개, arXiv self-report로 확인)
- DG 논문은 아니지만 "얇은 구조의 낮은 contrast로 인한 fragmentation" 문제의식이 내 동기와 직결. Architecture-only이므로 내 augmentation 방법과 직교(orthogonal) — 향후 backbone 후보로 고려 가능

### TopoVST — arXiv 2026 (Preprint Only)

**논문**: TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking (arXiv 2603.14909)

- GNN이 매 tracking step마다 방향과 **local vessel radius를 함께 예측**하며 wave-propagation 방식으로 skeleton 추출
- 내 ONA의 observability score 계산(local radius 추정) 방법론과 직접 비교 가능한 최신 기법. DG 논문은 아님

### TubeMLLM — arXiv 2026, MICCAI 2026 submission (Preprint Only)

**논문**: TubeMLLM: A Foundation Model for Topology Knowledge Exploration in Vessel-like Anatomy (arXiv 2603.09217)

- Vessel-like anatomy 특화 topology-aware foundation model. Topology-critical 영역에 adaptive loss weighting + zero-shot cross-modality OOD 일반화
- Radius를 augmentation에 쓰지 않으나 "구조 중요도에 따른 차등 처리"라는 상위 motivation 공유

---

## Category D — 조사 결과 (채택 보류)

CVPR 2026(6월 개최, accepted 공개) 및 ICML 2026(accepted 공개) 시점을 고려해 집중 탐색했으나, 직접적으로 검증 가능한 신규 논문은 확보하지 못했다. PEPR, Shape-Bias-MaxPool-Dilation, Magnitude-Aware-Phase-Jittering(DG-EBF workshop) 후보를 발견했으나 venue/acceptance 상태와 전문 내용을 확인하지 못해(WebFetch 403 오류) 이번 Run에서는 인덱스에 포함하지 않았다. 다음 Run에서 재확인 필요.

---

## Novelty Gap 재확인

- **"vessel radius/observability를 augmentation strength에 continuous하게 conditioning"**하는 논문은 Run #8에서도 발견되지 않음 — 핵심 gap 유지
- **단, TSIAA의 등장으로 상위 프레이밍("이미지 내 non-uniform augmentation")은 더 이상 unique하지 않다.** 논문 작성 시 novelty 서술을 "non-uniform" 수준이 아니라 **"source-annotation-derived, interpretable, direction-aware(fragile 구조 보호) radius conditioning vs. adversarial model-driven non-uniformity"**로 한 단계 구체화해야 한다.
- MICCAI 2026 공식 accepted list는 아직 비공개 (notification만 이루어짐) — 공개 시 전체 재탐색 필요

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA 전문 독해 최우선** — instance-level 정의, radius/anatomical prior 사용 여부, teacher-student의 target 정보 사용 여부 확인
- [ ] ADVERIN 전문 독해 — monotonic mapping 수식, baseline 비교 상세
- [ ] MICCAI 2026 공식 accepted list 공개 여부 재확인
- [ ] CVPR 2026 / ICML 2026 accepted 목록에서 의료영상 DG 세부 트랙 직접 재탐색
- [ ] PEPR / Shape-Bias-MaxPool-Dilation / Magnitude-Aware-Phase-Jittering venue 및 전문 재확인 후 채택 여부 결정
