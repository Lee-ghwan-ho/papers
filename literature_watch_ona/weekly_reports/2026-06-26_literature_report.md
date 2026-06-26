# Literature Watch Report — Run #8

> 날짜: 2026-06-26  
> 모델: claude-sonnet-4-6  
> 신규 논문: **7편** (Published Journal 2편 + Preprint Only 5편)

---

## 요약

이번 Run에서는 두 가지 주요 발견이 있었다.

1. **SSDG 직접경쟁 논문 2편** (Published Journal):
   - **TSIAA (IEEE TMI 2026)**: instance-level Bézier transformation 기반 adversarial augmentation — Bézier라는 동일 변환 함수를 사용하지만 per-image 단위로 내 intra-image vessel conditioning과 level이 다름
   - **PCSDG (Biomedical Signal Processing and Control 2025)**: "structure-aware brightness augmentation (SABA)"를 도입 — "structure-aware augmentation"이라는 용어와 방향이 내 ONA와 가장 근접

2. **혈관·구조 특화 논문 3편 + SSDG freq 논문 1편 + 자연영상 DG 1편** (Preprint Only):
   - WaveSDG, TopoVST, TubeMLLM, TopoLoRA-SAM, IELDG

핵심 takeaway: **TSIAA와 PCSDG가 가장 중요한 신규 발견**이다. TSIAA는 내 방법과 같은 Bézier 기반 SSDG aug를 사용하나 image-level이고 vessel 두께 conditioning이 없다. PCSDG의 SABA는 "구조 정보에 따른 brightness aug 차별화"라는 개념을 처음으로 명시했으나 pixel intensity 기반이어서 vessel geometry-aware conditioning인 내 ONA와 구분된다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 즉시 독해 필요

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**저자**: Zhengshan Wang, Long Chen et al.  
**Venue**: IEEE Transactions on Medical Imaging, Vol 45, pp 764-776, 2026  
**Status**: Published Journal Article  
**IEEE Xplore**: 11146907  
**Code**: https://github.com/Wangzts0228/TSIAA

#### 방법 요약

TSIAA는 single-source domain generalization을 위한 teacher-student 프레임워크다:

- **Instance-level Image Augmenter (IIAG)**:
  - Instance-level Augmentation Modules (IAMs) 구성
  - 각 IAM은 **learnable constrained Bézier transformation function** 기반
  - Per-image(instance) 단위로 nonlinear appearance 변형을 adversarially 탐색
  - Teacher network가 target distribution을 안내, Student network가 generalized representation 학습

- **핵심 문제의식**:
  - 기존 adversarial aug 방법들은 simple structure의 image-level augmenter만 사용 → diversity 부족
  - Out-of-source distribution을 Instance-level에서 탐색해야 함

- **출판 상태**: IEEE TMI 2026, 심사 완료 및 출판 (Vol 45, pp 764-776)

#### 내 방법과의 관계

**공통점**:
- Nonlinear appearance transformation에 Bézier function 사용
- SSDG 설정 (source domain만으로 학습)
- Per-sample adversarial exploration

**핵심 차이**:
| | TSIAA | Continuous-ONA |
|--|-------|----------------|
| Aug 단위 | per-image (image-level uniform) | intra-image vessel 구조 단위 |
| 조건 | 없음 (image 전체에 동일 Bézier) | vessel radius/observability (연속 조건) |
| 얇은 혈관 보호 | 없음 | 핵심 메커니즘 |
| 학습 방식 | adversarial (teacher-student) | fixed observability mapping |
| 동기 | diversity 부족 | label-image inconsistency for thin structures |

**Novelty 위협도**: Medium-High
- Bézier function 사용이 겹치지만, **TSIAA는 image 단위, 나는 intra-image structure 단위**
- TSIAA의 "instance"는 이미지 인스턴스, 나의 conditioning은 혈관 구조 인스턴스
- **구분 논거**: "TSIAA applies Bézier-based nonlinear transformations uniformly across the image. Our method introduces a continuous observability-conditioned augmentation budget that assigns different transformation intensities to vessel structures based on their local radius — a fundamentally different conditioning signal that TSIAA's image-level augmenter does not capture."

---

### WaveSDG — arXiv 2603.28463 (April 2026)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**Venue**: arXiv 2603.28463  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2603.28463

#### 방법 요약

- **WISER (Wavelet-based Invariant Structure Extraction and Refinement) module**:
  - Encoder feature를 wavelet sub-band로 분해
  - Low-frequency component → global anatomy 앵커 (invariant)
  - High-frequency sub-bands → directional edge enhancement + noise suppression
- **적용**: Fundus 이미지의 optic disc/cup SSDG
- **결과**: 1 source domain → 5 unseen target domains, 7개 SOTA 방법 능가 (Dice + Hausdorff 모두)

#### 내 방법과의 관계

- **방향 차이**: WaveSDG = frequency-domain structure/style 분리 (전체 이미지 단위), 나 = spatial-domain vessel radius conditioned augmentation budget
- 겹치는 부분: 구조 정보를 보존하면서 스타일을 변환하는 기조
- **직접 충돌 없음** — 서로 다른 표현 공간 (frequency vs. spatial, fundus vs. cerebrovascular)

---

## Category B — 신규 방법론 유사 논문

### PCSDG — Biomedical Signal Processing and Control 2025 ⚠️

**논문**: Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation  
**저자**: Jia-Xuan Jiang, Yuee Li, Zhong Wang  
**Venue**: Biomedical Signal Processing and Control, Vol 99, 2025  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.bspc.2024.106801  
**Code**: https://github.com/HopkinsKwong/PCSDG

#### 방법 요약

**PCSDG framework**:
1. **Pixel-level Contrastive SSDG (PCSDG)**:
   - Shallow feature extraction → contrastive learning
   - Style/structure representation disentanglement (pixel-level)
   - Segmentation은 structure representation만 사용

2. **SABA (Structure-Aware Brightness Augmentation)**:
   - Pixel grayscale 값 기반 brightness curve 초기화
   - Brightness factor를 truncated Gaussian으로 랜덤화
   - → 구조 정보에 따른 brightness augmentation 차별화
   - "첫 번째 픽셀 수준 대조 분리 + attention mechanism을 SSDG에 통합"

#### 내 방법과의 관계

**공통점**:
- "Structure-aware augmentation"이라는 용어와 개념을 명시적으로 사용
- SSDG 설정, annotation 기반 구조 정보 활용

**핵심 차이**:
| | PCSDG/SABA | Continuous-ONA |
|--|-----------|----------------|
| 구조 신호 | pixel grayscale intensity | vessel radius / observability score |
| 대상 구조 | optic disc/cup (organ) | cerebrovascular (tubular network) |
| Aug 종류 | brightness curve | nonlinear appearance (intensity mapping) |
| Conditioning | pixel brightness value | local vessel radius (geometry) |
| 연속성 | 연속적이나 geometry 무관 | vessel radius에 직접 연동 |
| 보호 개념 | 없음 (brightness만 조절) | thin vessel의 label-image inconsistency 방지 |

**Novelty 위협도**: Medium
- "structure-aware brightness augmentation" 용어가 내 방법의 마케팅 언어와 겹침
- 그러나 SABA의 conditioning은 pixel grayscale → brightness뿐, vessel geometry 기반이 아님
- **구분 논거**: "SABA conditions augmentation on pixel intensity, not on structural geometry. Our method conditions on locally-estimated vessel radius — a fundamentally different signal that reflects the observability of tubular structures. SABA cannot distinguish thin from thick vessels unless they have different intensities, which is not always the case in TOF-MRA."

---

## Category C — 신규 혈관·구조 특화 논문

### TopoVST — arXiv 2603.14909 (March 2026)

**논문**: Toward Topology-fidelitous Vessel Skeleton Tracking  
**arXiv**: https://arxiv.org/abs/2603.14909 (submitted March 16, 2026)  
**Status**: Preprint Only

#### 방법 요약

- **Multi-scale sphere graphs**: 입력 영상을 다중 스케일 구 그래프로 샘플링
- **GNN**: vessel tracking direction + **vessel radius** 동시 추정
- **Geometry-aware weighting**: directional loss에 기하학적 가중치 포함으로 class imbalance 완화
- **Wave-propagation skeleton tracking**: space-occupancy filtering으로 spurious skeleton 제거

#### 내 방법과의 관계

- **vessel radius 추정 방법론**이 내 observability score 계산에 직접 참고 가능
- DG 논문이 아니나 vessel topology + radius 추정의 최신 방법론 제공
- **활용 방향**: 내 논문에서 "local vessel radius 계산" 방법을 설명할 때 TopoVST의 multi-scale radius estimation 접근을 reference로 활용 가능

---

### TubeMLLM — arXiv 2603.09217 (March 2026)

**논문**: A Foundation Model for Topology Knowledge Exploration in Vessel-like Anatomy  
**저자**: Yaoyu Liu, Minghui Zhang et al. (Shanghai Jiao Tong University)  
**arXiv**: https://arxiv.org/abs/2603.09217 (MICCAI 2026 extended submission)  
**Status**: Preprint Only

#### 방법 요약

- **MLLM + topological priors**: natural language prompt로 topology prior를 명시적 통합
- **TubeMData benchmark**: 15 vessel-like datasets, 2 imaging modalities
- **Adaptive loss weighting**: topology-critical regions 강조

#### 내 방법과의 관계

- Foundation model 패러다임 (내 lightweight SSDG와 다름)
- 15개 혈관 데이터셋 포함 → 내 연구 데이터셋 범위 참고 가능
- DG 설정 아님

---

### TopoLoRA-SAM — arXiv 2601.02273 (January 2026)

**논문**: Topology-Aware Parameter-Efficient Adaptation of Foundation Segmenters for Thin-Structure and Cross-Domain Binary Semantic Segmentation  
**arXiv**: https://arxiv.org/abs/2601.02273  
**Status**: Preprint Only  
**Code**: https://github.com/salimkhazem/Seglab

#### 방법 요약

- **SAM + LoRA**: ViT encoder에 Low-Rank Adaptation 주입 (5.2% 파라미터만 학습)
- **Spatial convolutional adapter**: 도메인 특화 local feature
- **Optional clDice topology supervision**: topology-aware 미세조정
- **대상**: thin structure (retinal vessels: DRIVE/STARE/CHASE DB1) + cross-domain adaptation

#### 내 방법과의 관계

- Cross-domain thin-structure 분할이라는 점에서 관련성 있음
- PEFT 방식이므로 내 SSDG augmentation 방식과 직접 경쟁하지 않음
- clDice + cross-domain = 내 Cat C 배경 보완

---

## Category D — 신규 Top-tier Vision 논문

### IELDG — arXiv 2508.19604 (August 2025)

**논문**: IELDG: Suppressing Domain-Specific Noise with Inverse Evolution Layers for Domain Generalized Semantic Segmentation  
**저자**: Qizhe Fan, Chaoyu Liu, Zhonghua Qiao, Xiaoqin Shen  
**arXiv**: https://arxiv.org/abs/2508.19604  
**Status**: Preprint Only

#### 방법 요약

- **Inverse Evolution Layers (IELs)**: Laplacian-based priors로 공간적 불연속성 + 의미론적 불일치 강조
- **IELDM**: 확산 기반 생성 모델 (생성 과정에 IEL 통합)
- **IELFormer**: 구조 안내 segmentation head
- **대상**: 자연영상 Domain Generalized Semantic Segmentation (Cityscapes → 다른 도시)

#### 내 방법과의 관계

- 자연영상 DGSS 논문 — 의료영상 직접 관련 없음
- "공간적 불연속성을 강조"하는 아이디어가 혈관 edge/boundary 활용에 간접 참고 가능
- **Category D 낮은 우선순위**

---

## Novelty Gap 재확인

이번 Run #8에서도 다음 개념을 직접 다룬 논문은 없었다:
- **"vessel observability conditioned augmentation"** → 없음
- **"radius-conditioned augmentation budget"** (augmentation 측) → 없음
- **"thin vessel appearance protection during SSDG augmentation"** → 없음

가장 근접한 논문들의 구분:
- **TSIAA**: Bézier가 겹치나 image-level uniform, vessel radius conditioning 없음
- **PCSDG**: "structure-aware aug" 방향이 겹치나 pixel intensity 기반, vessel geometry 무관
- **AG-TAL** (Run #7): radius를 loss에 활용하나 augmentation에는 없음

**결론**: Continuous-ONA의 핵심 gap ("intra-class continuous vessel radius → augmentation budget mapping") 은 Run #8에서도 유지된다.

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA full text 독해: IAM Bézier function 수식 + adversarial training 방식 → 내 방법과 구분 논거 작성
- [ ] PCSDG full text 독해: SABA curve function 공식 + 실험 설정 → Related Work에서 "structure-aware aug" 계보 정리
- [ ] WaveSDG full text: WISER module 세부 구조, frequency vs. spatial aug 비교 분석
- [ ] MICCAI 2026 early accept list 탐색 (Awesome-MICCAI-2026 GitHub: ambicuity/Awesome-MICCAI-2026)
- [ ] IJCAI 2026 accepted papers (2026.ijcai.org/accepted-papers/) DG/segmentation 탐색
- [ ] TopoVST radius 추정 공식 상세 확인: GNN에서 radius를 어떻게 supervision하는지
