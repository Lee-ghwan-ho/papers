# Literature Watch Report — Run #8

> 날짜: 2026-06-24  
> 모델: claude-sonnet-4-6  
> 신규 논문: **11편** (Published Journal 3편 + Accepted Conference 5편 + Preprint 3편)

---

## 요약

이번 Run #8에서 발견된 가장 중요한 논문은 **TSIAA (IEEE TMI 2026)**이다.

TSIAA는 "같은 이미지 내 다른 structure에 다른 augmentation 규칙을 적용"해야 한다는 내 방법과 방향이 유사한 주장을 IEEE TMI 2026에 게재했다. 단, TSIAA는 **inter-structure adversarial diversity** (서로 다른 semantic 구조 간)이고, 내 Continuous-ONA는 **intra-class vessel-radius observability conditioning** (동일 vessel class 내 위치별 강도 조절)으로 granularity와 원리가 명확히 다르다.

이 외에 SCNP (CVPR 2026)가 topology-accuracy 개선을 위한 새로운 loss로 내 thin vessel topology 논거와 결합 가능하며, FVAC (IJCAI 2025)는 vessel morphology inconsistency를 uncertainty로 해결하는 접근이 방향적으로 유사하다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 확인 필요

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, 2026  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Code**: https://github.com/Wangzts0228/TSIAA  
**Status**: Published Journal Article

#### 방법 요약
- **IIAG (Instance-level Image Augmenter)**: 여러 IAM (Instance-level Augmentation Module)로 구성
- 각 IAM은 **learnable constrained Bézier transformation** 기반
- 서로 다른 semantic structure/instance에 각각 독립적인 Bézier 파라미터 적용
- **Teacher-Student adversarial 학습**: IIAG가 segmentation model이 어려워하는 방향으로 aug 생성

#### 핵심 주장 (내 방법과 가장 유사한 표현)
> "Compared to image-level adversarial augmentation, instance-level adversarial augmentation **breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity."

#### 내 방법과의 관계
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 증강 다양성 단위 | inter-structure (서로 다른 객체 간) | intra-class (같은 vessel 내 위치별) |
| 강도 결정 원리 | adversarial (모델 반응 기반) | radius/observability (구조 물리적 속성 기반) |
| 적용 대상 | 일반 의료영상 4개 task | TOF-MRA cerebrovascular SSDG |
| thin vessel 보호 | 개념 없음 | 핵심 motivation |

**Novelty 위협도**: **Medium** — "uniform augmentation을 탈피"라는 방향 유사, 세부 메커니즘은 완전히 다름.  
→ paper_notes/TSIAA.md에 상세 차별화 논거 작성 완료.

---

### WaveSDG — arXiv 2603.28463 (April 2026) [Preprint]

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: https://arxiv.org/abs/2603.28463  
**Status**: Preprint Only

#### 방법 요약
- **WaveSDG**: wavelet sub-band decomposition으로 도메인 불변 구조 표현 학습
- **WISER module** (Wavelet-based Invariant Structure Extraction and Refinement):
  - LL sub-band: global structural context (anatomy)
  - LH/HL sub-bands: horizontal/vertical edge responses
  - HH sub-band: sensor noise / acquisition artifacts
- Fundus image (optic disc, retinal vessel) SSDG

#### 내 방법과의 관계
- wavelet decomposition으로 "구조 vs. 외관 분리"를 표현하는 방향이 내 ONA의 content-style 분리와 개념적으로 유사
- 차이: WaveSDG = frequency-domain feature decomposition, 나 = input-space observability-conditioned augmentation
- 내 방법의 baseline 비교군 후보

---

### GrInAdapt — MICCAI 2025 [Accepted Conference]

**논문**: GrInAdapt: Source-Free Multi-Target Domain Adaptation for Retinal Vessel Segmentation  
**MICCAI 2025**: papers.miccai.org/miccai-2025/0389-Paper0903.html  
**arXiv**: https://arxiv.org/abs/2503.05991  
**Status**: Accepted Conference Paper

#### 방법 요약
- 3단계: grounding → integrating → adapting
- Multi-view OCTA 이미지로 label consensus 개선
- Source-free multi-target domain adaptation (DG가 아닌 DA)
- OCTA fundus retinal vessel에 특화

#### 내 방법과의 관계
- **Domain Adaptation (target 필요)** 설정이라 직접 경쟁 아님
- 그러나 retinal vessel cross-domain 방법으로 background 참고 가능
- **Relevance**: Low

---

### FASAM — arXiv 2507.17281 (July 2025) [Preprint]

**논문**: FA-SAM: Fully Automated SAM for Single-source Domain Generalization in Medical Image Segmentation  
**arXiv**: https://arxiv.org/abs/2507.17281  
**Status**: Preprint Only

#### 방법 요약
- **AGM branch** (Auto-prompted Generation Model): SAM prompt 자동 생성
- **SUFM** (Shallow Feature Uncertainty Modeling): 불확실성 기반 prompt 품질 향상
- **IPEF** (Image-Prompt Embedding Fusion): image feature와 prompt embedding 통합
- 문제: SAM이 domain-specific expert prompt에 의존 → 자동화 어려움

#### 내 방법과의 관계
- SAM 계열 SSDG 방법론의 최신 동향 참고
- 내 방법과 직접 경쟁은 아님 (SAM 기반 vs. augmentation 기반)
- **Relevance**: Medium

---

## Category C — 신규 혈관·구조 특화 논문

### SCNP — CVPR 2026 [Accepted Conference] ⭐

**논문**: Towards High-Quality Image Segmentation: Improving Topology Accuracy by Penalizing Neighbor Pixels  
**CVPR 2026**: Accepted Conference Paper  
**arXiv**: https://arxiv.org/abs/2603.18671  
**Code**: https://jmlipman.github.io/SCNP-SameClassNeighborPenalization  
**Status**: Accepted Conference Paper

#### 방법 요약
- **SCNP** (Same Class Neighbor Penalization): 같은 class 인접 픽셀에 대한 logit penalty
- "가장 poorly classified 이웃 픽셀"에 penalty를 부여 → 모델이 pixel 경계를 개선하도록 강제
- 13 datasets: 다양한 morphology (tubular 포함) + 다양한 modality
- semantic + instance segmentation 모두 적용 가능한 plug-and-play

#### 내 방법과의 관계
- 직접 DG 방법은 아니나, **thin vessel topology 보존**을 위한 loss로 결합 가능
- 내 augmentation과 orthogonal하게 사용 가능 (augmentation + topology loss)
- CVPR 2026 논문이므로 최신 topology 방법 landscape 파악에 중요

---

### FVAC — IJCAI 2025 [Accepted Conference] ⭐

**논문**: Pixel-wise Divide and Conquer for Federated Vessel Segmentation  
**IJCAI 2025**: https://www.ijcai.org/proceedings/2025/540  
**Status**: Accepted Conference Paper

#### 방법 요약
- **FVAC** (Federated Vessel-Aware Calibration):
  - "vessel morphology inconsistency" 문제를 federated learning에서 해결
  - global uncertainty로 각 pixel의 "morphology difficulty"를 추정
  - 어려운 morphology (fine vessels)에 differentiated guidance 제공
- 배경과 혈관의 class imbalance 문제도 동시 해결

#### 내 방법과의 관계
- **방향적 유사성**: "vessel morphology inconsistency → differentiated treatment" 아이디어가 내 ONA와 동일 문제의식
- **차이**:
  - FVAC = federated learning (multi-client), 나 = single-source DG
  - FVAC = uncertainty 기반, 나 = GT radius/observability 기반
  - FVAC = feature-level calibration, 나 = input-space augmentation budget
- 내 Related Work에서 "pixel-level morphology-aware training 계열"로 언급 가능

---

### TopoVST — arXiv 2603.14909 (March 2026) [Preprint]

**논문**: TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking  
**arXiv**: https://arxiv.org/abs/2603.14909  
**Status**: Preprint Only

#### 방법 요약
- Multi-scale sphere graphs + GNN으로 tracking direction + vessel radius를 동시 추정
- Gating-based feature fusion for multi-scale representations
- Geometry-aware weighting scheme for class imbalance
- Wave-propagation skeleton tracking (space-occupancy filtering으로 spurious skeleton 제거)

#### 내 방법과의 관계
- **vessel radius 추정** 방법론 참고: multi-scale sphere graph 기반 radius estimation
- 내 ONA의 "local vessel radius 계산" 구현 참고 가능
- 단, tracking 설정이라 DG와 직접 관련 없음
- **Relevance**: Medium

---

### TOPGUARSEG — SIAM Journal on Imaging Sciences 2026 [Published Journal]

**논문**: Topology-Guaranteed Image Segmentation: Enforcing Connectivity, Genus, and Width Constraints  
**Venue**: SIAM Journal on Imaging Sciences, 2026  
**DOI**: https://epubs.siam.org/doi/abs/10.1137/25M1765870  
**arXiv**: https://arxiv.org/abs/2601.11409  
**Status**: Published Journal Article

#### 방법 요약
- Persistent homology + PDE smoothing으로 local extrema of upper-level sets 수정
- **Width 제약** (thickness, length): topology 구조가 width property도 capture하도록
- Connectivity + genus + width를 동시에 보장하는 segmentation model
- Neural network loss로 통합 가능

#### 내 방법과의 관계
- vessel width (radius)를 topology representation에 직접 포함 → 내 observability 개념의 수학적 기반
- 내 ONA에서 "why thin vessel needs protection"의 이론적 근거로 활용 가능
- **직접 DG 방법 아님** — background 참고

---

### UVSM — IEEE TIP 2025 [Published Journal]

**논문**: Universal Vessel Segmentation for Multi-Modality Retinal Images  
**Venue**: IEEE Transactions on Image Processing, Vol. 34, 2025  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11218739/  
**arXiv**: https://arxiv.org/abs/2502.06987  
**Status**: Published Journal Article

#### 방법 요약
- 다양한 modaliy (CF, MC, OCTA 등) 에서의 retinal vessel segmentation
- Image translation으로 arbitrary modality → Topcon CF 변환 (common anchor space)
- Domain adaptation 방식으로 universal model 구성

#### 내 방법과의 관계
- Multi-modality vessel segmentation의 최신 benchmark (IEEE TIP 2025)
- 내 TOF-MRA 설정과 modality는 다르나 multi-domain vessel seg의 SOTA reference
- **Relevance**: Medium

---

## Category D — 신규 Top-tier Vision 논문

### SoMA — CVPR 2025 Highlight [Accepted Conference]

**논문**: SoMA: Singular Value Decomposed Minor Components Adaptation for Domain Generalizable Representation Learning  
**Venue**: CVPR 2025 (Highlight)  
**arXiv**: https://arxiv.org/abs/2412.04077  
**Code**: https://github.com/ysj9909/SoMA  
**Status**: Accepted Conference Paper

#### 방법 요약
- SVD로 pre-trained weight를 분해 → minor singular components만 fine-tuning
- 핵심 개념: "major singular components = general representation (freeze), minor = task-specific (tune)"
- Pre-trained 모델의 generalization capacity 보존하면서 task-specific 적응

#### 내 방법과의 관계
- Pre-trained model 기반 DG 방법으로 내 방법 (augmentation 기반)과 다른 패러다임
- CVPR 2025 Highlight로서 DG representation 최신 동향 참고
- 내 방법이 SoMA 같은 foundation model과 결합 가능한지 future work로 언급 가능
- **Relevance**: Low

---

### IELDG — arXiv 2508.19604 (August 2025) [Preprint]

**논문**: IELDG: Suppressing Domain-Specific Noise with Inverse Evolution Layers for Domain Generalized Semantic Segmentation  
**arXiv**: https://arxiv.org/abs/2508.19604  
**Status**: Preprint Only

#### 방법 요약
- Diffusion model 기반 augmentation 이미지에서 structural/semantic defect 필터링
- **IEL (Inverse Evolution Layers)**: Laplacian-based priors로 spatial discontinuity + semantic inconsistency 검출
- IELDM: IEL이 통합된 diffusion-based image generation framework

#### 내 방법과의 관계
- 자연영상 DGSS 논문이라 직접 관련 낮음
- "augmentation 과정의 structural fidelity 보장"이라는 개념 방향만 참고 가능
- **Relevance**: Low

---

## Novelty Gap 재확인

Run #8 이후에도 다음 키워드를 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation budget"
- "intra-class vessel radius conditioned appearance perturbation"
- "thin vessel appearance protection during augmentation for domain generalization"
- "continuous augmentation strength based on local vessel radius"

TSIAA가 "instance-level augmentation diversity" 방향을 제시했지만, **동일 class 내 vessel radius에 따른 연속적 augmentation strength conditioning**은 여전히 내 방법의 고유 contribution으로 확인된다.

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA full text 확인: instance-level IAM이 same-class vessel 내 thin/thick를 구분하는지 여부
- [ ] SCNP 실험에서 tubular/retinal vessel 데이터셋 포함 여부 및 성능 확인
- [ ] FVAC 상세 확인: uncertainty-based morphology conditioning mechanism 파악
- [ ] ICLR 2026 proceedings DG/augmentation 논문 탐색
- [ ] NeurIPS 2025 accepted papers에서 추가 topology + DG 논문 탐색
- [ ] "width-aware augmentation" OR "thickness-conditioned augmentation" 신규 검색 시도
