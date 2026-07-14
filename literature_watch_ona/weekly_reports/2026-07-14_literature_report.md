# Literature Watch Report — Run #8

> 날짜: 2026-07-14
> 모델: claude-sonnet-5 (2개 병렬 subagent: Lane A/B, Lane C/D+Follow-up)
> 신규 논문: **11편** (Published Journal 3편 + Accepted Conference 3편 + Workshop 2편 + Preprint 3편)

---

## 요약

직전 실행(Run #7, 2026-06-03)로부터 약 6주 공백이 있었다. 이 기간 동안 MICCAI 2026
accepted list는 아직 공개되지 않았고, CVPR/ICLR/ICML 2026도 일부 워크숍/조기 accepted
정보만 확인 가능했다. 그럼에도 SSDG, 혈관 특화, top-tier vision 세 범주에서 총 11편의
신규 논문을 발견했다.

가장 중요한 발견은 **두 subagent 모두 Continuous-ONA의 핵심 메커니즘(연속적,
source-annotation 기반 vessel radius/observability score가 단일 nonlinear appearance
augmentation family의 강도를 조절)을 구현한 논문을 전혀 발견하지 못했다**는 점이다.
6주간의 공백에도 불구하고 novelty gap이 그대로 유지되고 있다.

1. **Vessel SSDG 직접 경쟁 (Category A/C)**: WaveSDG, DCCD (일반 SSDG), DOMAINFLOW
   (coronary vessel, connectivity mask 예측 — target representation 레버)
2. **TOF-MRA/혈관 특화 벤치마크 (Category C)**: SMILE-UHURA (7T TOF-MRA 소혈관
   벤치마크), TopoLoRA-SAM, COMMA (대형 3D vessel 데이터셋+백본)
3. **"비균일 augmentation 강도" 원칙을 공유하는 Top-tier Vision 논문 (Category D)**:
   GAUDA (WACV 2025, uncertainty-guided diffusion aug), MAPJ (CVPR 2026 workshop,
   magnitude-aware phase jittering), PMDG (pseudo multi-source), GHEAPC (CVPR 2025,
   point cloud hard-example augmentation)

---

## Category A — 신규 직접경쟁 논문

### WaveSDG — arXiv 2603.28463 (2026)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation
**Status**: Preprint Only

Wavelet sub-band 분해 기반 WISER(Wavelet-based Invariant Structure Extraction and
Refinement) 모듈로 feature space에서 anatomical structure와 domain-specific appearance를
분리. Fundus(vessel-adjacent) SSDG 대상.

**내 방법과의 관계**: Feature-level frequency decomposition이며 pixel-space nonlinear
appearance augmentation이 아님. Per-vessel continuous modulation 개념 없음. Novelty 충돌 없음.

---

### DCCD — Neurocomputing (2026)

**논문**: Dual-contrastive channel disentanglement for single-source domain generalization in medical image segmentation
**Status**: Published Journal Article (ScienceDirect S0925231226018564)

Style-shift + spatial/structural-shift를 분리하는 dual-contrastive channel
disentanglement, Residual Structure Gain 모듈 포함. 일반 organ SSDG (vessel-specific 아님).

**내 방법과의 관계**: Representation-disentanglement 접근, augmentation-strength
modulation과 무관. Novelty 충돌 없음.

---

## Category B — 신규 방법론 논문

### UniFreqSDG — ACM Multimedia 2024

**논문**: Universal Frequency Domain Perturbation for Single-Source Domain Generalization
**Status**: Official Proceedings Paper (DOI 10.1145/3664647.3681536)

Learnable spectral perturbation 모듈 + Content Preservation Reconstruction + Active
Domain-variance Inducement Loss. 전체 이미지에 단일 학습된 frequency perturbation policy
적용 (fundus, prostate 실험).

**내 방법과의 관계**: 단일 global policy이며 per-vessel/per-structure conditioning 없음.
Novelty 충돌 없음. SSDG augmentation-policy 계열 baseline 후보로 참고 가치.

---

## Category C — 신규 혈관·구조 특화 논문

### DOMAINFLOW — Springer LNCS (~2025) ⚠️ 주목

**논문**: Single-Source Domain Generalization for Coronary Vessels Segmentation in X-Ray Angiography via Connectivity Mask Prediction
**Status**: Accepted Conference Paper (정확한 venue명 검증 필요)

모델이 binary mask 대신 **connectivity mask**를 예측하도록 학습하여, appearance
변화에 덜 민감한 topology 표현을 학습. Coronary vessel SSDG (X-ray angiography).

**내 방법과의 관계**: 같은 문제(vessel SSDG)를 다루지만 완전히 다른 레버(target
representation vs. augmentation strength) — orthogonal. Related work에서 "non-
augmentation 경로로 vessel topology robustness를 얻는 대안"으로 인용 가치. 상세 노트:
`paper_notes/DOMAINFLOW.md`.

**Novelty 위협도**: Low — mechanism이 완전히 다름 (label/target 재정의 vs. augmentation 강도).

---

### SMILE-UHURA Challenge — ISBI 2023 Workshop / arXiv 2411.09593

**논문**: SMILE-UHURA Challenge — Small Vessel Segmentation at Mesoscopic Scale from Ultra-High-Resolution 7T MRA
**Status**: Workshop Paper (results report, arXiv 2411.09593)

7T ultra-high-resolution TOF-MRA에서 소혈관 분할을 다루는 벤치마크 챌린지. 16개 제출
방법 + 2개 baseline 비교. Partial-volume/소혈관 가시성 문제를 직접 다룸.

**내 방법과의 관계**: 방법론적 경쟁은 아니지만(챌린지 리포트), 내 정확한 imaging
modality(TOF-MRA)에서 "얇고 희미한 혈관은 관찰이 본질적으로 어렵다"는 motivation을
뒷받침하는 강력한 인용 근거.

**Novelty 위협도**: None (벤치마크 논문).

---

### TopoLoRA-SAM — arXiv 2601.02273 (2026)

**논문**: TopoLoRA-SAM: Topology-Aware Parameter-Efficient Adaptation of Foundation Segmenters for Thin-Structure and Cross-Domain Binary Semantic Segmentation
**Status**: Preprint Only

Frozen SAM ViT encoder에 LoRA + spatial adapter + differentiable clDice topology
supervision을 결합, thin structure(retinal vessel) cross-domain segmentation
(noisy modality 포함) 대상. ~5% trainable parameters.

**내 방법과의 관계**: Architecture/loss-side 해법이며 augmentation 전략이 아님 — 내
POC(augmentation-only 비교) 범위 밖. Competing thin-structure cross-domain 솔루션으로
related work 인용 가치.

**Novelty 위협도**: Low — 완전히 다른 solution axis (foundation model PEFT + topology loss).

---

### COMMA — IEEE TIP (2026), arXiv 2503.02332

**논문**: COMMA: Coordinate-aware Modulated Mamba Network for 3D Dispersed Vessel Segmentation
**Status**: Published Journal Article

Mamba 기반 global/local branch + coordinate-aware modulation block. 570-case, 2
modality, 5 vascular tissue type의 최대 규모 공개 3D vessel 데이터셋과 함께 릴리즈.
DG/augmentation 논문 아님 — architecture + dataset 기여.

**내 방법과의 관계**: 방법론적 충돌 없음. 추가 벤치마크/백본 후보로만 참고 가치.

---

## Category D — 신규 Top-tier Vision 논문

### GAUDA — WACV 2025

**논문**: GAUDA: Generative Adaptive Uncertainty-Guided Diffusion-Based Augmentation for Surgical Segmentation
**Status**: Accepted Conference Paper

Diffusion 기반 augmentation을 segmentation uncertainty map으로 adaptively 조절 (surgical
scene segmentation).

**내 방법과의 관계**: "augmentation을 균일하지 않게 적용"한다는 원칙을 공유하지만
조절 신호가 학습된 uncertainty(indirect)이며, 내 방법은 source annotation에서 직접
도출한 anatomical radius(direct, geometry-grounded)를 사용. 다른 신호원을 사용하는
비의료 도메인 사례로서 "non-uniform augmentation budget" 원칙의 일반성을 뒷받침.

**Novelty 위협도**: Low-Medium — 원칙은 유사하나 신호원과 augmentation family, domain 모두 다름.

---

### Magnitude-Aware Phase Jittering (MAPJ) — CVPR 2026 Workshop (DG-EBF)

**논문**: Magnitude-Aware Phase Jittering for Domain-Generalized Semantic Segmentation
**Status**: Workshop Paper

자연영상 street-scene DG. Frequency magnitude로 phase jitter 강도를 local하게 조절
(균일하지 않은 frequency-domain augmentation).

**내 방법과의 관계**: "local signal이 augmentation 강도를 결정한다"는 구조적으로
유사한 원칙을 비의료/비혈관 도메인에서 독립적으로 뒷받침 — 내 핵심 주장의 일반성에
대한 방증으로 인용 가능. Mechanism(frequency magnitude vs. vessel radius), augmentation
family(phase jitter vs. nonlinear intensity transform) 모두 다름.

**Novelty 위협도**: Low.

---

### PMDG — arXiv 2505.23173 (2025)

**논문**: Pseudo Multi-Source Domain Generalization: Bridging the Gap Between Single and Multi-Source Domain Generalization
**Status**: Preprint Only

단일 source를 여러 pseudo-domain으로 분해(style transfer + augmentation) 후 기존
multi-source DG 기법을 적용.

**내 방법과의 관계**: "단일 source를 구조화된 하위 집단으로 분해한다"는 프레이밍이
내 vessel radius stratification 논리와 개념적으로 유사하지만, 분해 축이 pseudo-domain
(전체 이미지 단위)이지 vessel structure(intra-image, 국소) 단위가 아님.

**Novelty 위협도**: Low-Medium.

---

### GHEAPC — CVPR 2025 (point cloud)

**논문**: Generative Hard Example Augmentation for Semantic Point Cloud Segmentation
**Status**: Accepted Conference Paper

생성 모델이 hard example을 만들고, downstream segmentation error가 어떤 생성 예제를
학습에 재투입할지 결정하는 error-driven augmentation loop. Point cloud 도메인.

**내 방법과의 관계**: "downstream 성능 신호로 augmentation 강도/선택을 동적으로
조절"하는 대안적 신호원(error-driven vs. 내 방법의 고정된 anatomy-derived signal) 참고.
도메인(point cloud)과 augmentation family 모두 다름.

**Novelty 위협도**: Low.

---

## Novelty Gap 재확인 (Run #8)

이번 Run에서도 다음 키워드를 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget" (in the augmentation-strength sense; AG-TAL은 loss 쪽에서만 존재)
- "continuous intra-class structure-specific augmentation strength"
- "thin vessel appearance protection during augmentation" (as a continuous schedule)

6주의 공백 기간에도 불구하고 핵심 gap은 유지된다. 가장 근접한 strawman들(SRCSM의
discrete class-wise modulation, FIESTA/GAUDA의 uncertainty-guided modulation, RandDG의
uniform GIN augmentation)은 모두 이전 Run에서 이미 확인된 논문들이다.

---

## 다음 Run 우선 탐색 항목

- [ ] MICCAI 2026 accepted paper list 공개 시 즉시 재탐색
- [ ] DOMAINFLOW 정확한 venue/DOI 확인 (Springer LNCS chapter — MICCAI workshop 추정)
- [ ] Multi-Domain Brain Vessel Segmentation (MELBA) 논문 전문 확인 — WebFetch가
      arXiv/PMC/ResearchGate에서 403을 반환해 스니펫 기반 요약만 확보된 상태
- [ ] CVPR/ICCV/ECCV 2026 정식 accepted list 공개 후 재탐색
- [ ] ICLR 2026 OpenReview 직접 검색 재시도 (이번 실행에서 낮은 성공률)
- [ ] IJCAI/ICML 2026 accepted list 공개 시 탐색
