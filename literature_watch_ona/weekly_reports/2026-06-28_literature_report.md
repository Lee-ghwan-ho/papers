# Literature Watch Report — Run #8

> 날짜: 2026-06-28  
> 모델: claude-sonnet-4-6  
> 신규 논문: **5편** (Published Journal 2편 + Accepted Conference 1편 + Preprint 2편)

---

## 요약

이번 Run의 핵심 발견은 **TSIAA (IEEE TMI 2026)**로, "intra-image 구조별 비균일 augmentation"을 adversarial 방향으로 처음 구현한 IEEE TMI 발표 논문이다. 내 Continuous-ONA와 가장 직접적으로 경쟁하는 방법이지만, 방향과 동기가 근본적으로 다르다. 상세한 novelty 구분 논거 작성이 긴급히 필요하다.

추가로 **Mamba-Sea (IEEE TMI 2025)** — Mamba 기반 SSDG 최초 논문으로 Prostate Dice 90%+ — 와 **GraphMorph (NeurIPS 2024)** — branch-level graph-based tubular extraction — 을 새롭게 발견했다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 주의

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**저자**: 미확인 (IEEE Xplore 11146907)  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
**Status**: Published Journal Article  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/

#### 방법 요약

TSIAA는 Instance-level Image Augmenter (IIAG)를 Teacher-Student 프레임워크와 결합한다.

- **Instance-level Augmentation Modules (IAMs)**:
  - learnable constrained Bézier transformation을 각 segment region에 독립적으로 적용
  - 이미지 내 서로 다른 region에 서로 다른 Bézier curve → intra-image non-uniform augmentation
  - Image-level augmentation에서 벗어나 "break the uniformity of augmentation rules across different structures within an image"를 명시적 목표로 설정
- **Adversarial training**:
  - Segmentation model이 어렵다고 판단하는 방향으로 Bézier 파라미터를 최적화 (out-of-source distribution 탐색)
  - Over-augmentation 방지: learnable constrained Bézier를 통해 plausibility 유지
- **Teacher-Student consistency**:
  - Augmented feature(student)와 original feature(teacher) 간 consistency constraint
- **실험**: 4개 SDG task에서 SOTA 능가

#### 내 방법과의 관계 — 상세 비교

| 비교 항목 | TSIAA | Continuous-ONA |
|----------|-------|----------------|
| 핵심 문제의식 | 기존 image-level aug는 intra-image 구조별 다양성 부재 | 기존 uniform aug는 thin vessel의 label-image inconsistency 유발 |
| conditioning 변수 | segment region (공간 패치 단위, binary/patch-level) | vessel radius / observability (연속 scalar, pointwise) |
| augmentation 방향 | adversarial — harder (model이 어렵다고 판단하는 방향으로) | conservative — protective (thin vessel은 더 약하게) |
| thin vessel 처리 | 별도 처리 없음 — adversarial은 thin vessel도 강하게 변형 가능 | 핵심 보호 대상 — radius 작을수록 aug 강도 감소 |
| label-image inconsistency | 문제의식 없음 | 핵심 동기 |
| learning 방식 | adversarial (discriminative loss) | deterministic (analytical radius mapping) |
| 구조 conditioning | patch/region binary partition | continuous radius value per vessel point |

**Novelty 위협도**: High — 같은 방향(intra-image non-uniform aug)이지만, 목적과 메커니즘이 반대.

**내 논문에서의 구분 포인트**:
1. "TSIAA breaks uniformity by making all structures harder adversarially; Continuous-ONA breaks uniformity by protecting fragile structures selectively."
2. TSIAA의 conditioning은 spatial region (where), ONA는 structural observability (how much, conditioned on vessel radius).
3. TSIAA는 thin vessel을 특별히 보호하지 않고 오히려 더 어렵게 만들 수 있다 → label-image inconsistency 위험.

---

### MAMBA_SEA — IEEE TMI 2025

**논문**: Mamba-Sea: A Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, 2025  
**Status**: Published Journal Article  
**arXiv**: 2504.17515 (April 2025)  
**IEEE Xplore**: 10980210

#### 방법 요약

- **Global Augmentation**: 이미지 전체 단위 appearance 다양화 (site variation 시뮬레이션)
- **Local Sequence Augmentation**: Mamba 입력 sequence에서 random continuous sub-sequence를 선택해 해당 token의 style statistics를 resampling
  - Sequence-wise local style perturbation: 일부 token에만 style 변환 적용
- Mamba 아키텍처로 long-range dependency를 linear complexity로 처리
- **최초** Mamba 기반 SSDG 논문
- Prostate Dice 90%+, 기존 SOTA(88.61%) 능가

#### 내 방법과의 관계

- **공통점**: SSDG 설정, augmentation 기반 generalization
- **핵심 차이**: Mamba-Sea = feature-space sequence token augmentation (feature level, sub-sequence random), 나 = input-space structure-conditioned appearance augmentation (pixel/voxel level, radius-based continuous)
- Mamba-Sea는 혈관 구조 두께 차이에 의한 conditioning 개념 없음
- **Novelty 충돌 없음**: 경쟁 방법으로 비교 필요

---

### WAVESDG — arXiv 2603.28463 (April 2026) [Preprint Only]

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**Venue**: arXiv preprint (April 27, 2026)  
**Status**: Preprint Only  
**arXiv**: 2603.28463

#### 방법 요약

- **WaveSDG** with **WISER (Wavelet-based Invariant Structure Extraction and Refinement)** module
- Wavelet decomposition으로 feature를 sub-band 분리:
  - LL sub-band (저주파) → anatomical structure (domain-invariant)
  - LH, HL sub-band (중주파) → horizontal + vertical edge responses
  - HH sub-band (고주파) → sensor noise + acquisition artifacts (domain-specific)
- LL를 구조 보존에, LH/HL/HH를 style perturbation에 활용

#### 내 방법과의 관계

- 주파수 기반 structure-appearance 분리 (기존 RASS, MoreStyle, FreeSDG 계열)
- fundus image 대상, TOF-MRA 아님
- 나와 mechanism 다름 (wavelet 주파수 분리 vs. vessel radius conditioned spatial aug)
- **Novelty 충돌 없음**: 같은 SSDG 문제의 다른 접근법

---

## Category C — 신규 혈관·구조 특화 논문

### GRAPHMORPH — NeurIPS 2024

**논문**: GraphMorph: Tubular Structure Extraction by Morphing Predicted Graphs  
**저자**: Zhao Zhang, Ziwei Zhao, Dong Wang, Liwei Wang  
**Venue**: NeurIPS 2024 (38th Annual Conference)  
**Status**: Accepted Conference Paper  
**arXiv**: 2502.11731  
**NeurIPS**: neurips.cc/virtual/2024/poster/94063

#### 방법 요약

- **핵심 아이디어**: pixel-level classification을 벗어나 branch-level 특성 학습
- **Graph Decoder**: multi-scale feature → branch-level feature 학습 + tubular structure graph 생성
- **Morph Module**: SkeletonDijkstra 알고리즘으로 예측된 graph에 맞는 centerline mask 생성
  - Centerline을 graph topology에 "morphing"하여 topologically faithful skeleton 추출
- 혈관 분할 + 도로망 추출 등 다양한 tubular 구조에 적용
- DG는 직접 다루지 않음

#### 내 방법과의 관계

- Cat C 핵심 NeurIPS 2024 논문 — related work에 언급 가능
- Branch-level representation이 내 vessel radius 기반 observability 계산과 개념적으로 연결 가능
- DG 관련성 없음; 내 방법의 baseline/비교군은 아님

---

### TOPOVST — arXiv 2603.14909 (March 2026) [Preprint Only]

**논문**: TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking  
**저자**: Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu  
**Venue**: arXiv (under review)  
**Status**: Preprint Only  
**arXiv**: 2603.14909

#### 방법 요약

- Multi-scale sphere graph + GNN으로 vessel skeleton 추적
- Vessel radius를 tracking direction과 jointly 추정 (geometry-aware weighting)
- Wave-propagation-based skeleton tracking algorithm (spurious skeleton 억제)
- Gating-based feature fusion mechanism (multi-scale representation 강화)

#### 내 방법과의 관계

- Vessel radius 추정 방법이 내 ONA의 observability score 계산 참고 가능
- DG와 직접 관련 없음; Cat C 보조 참고 논문

---

## Novelty Gap 재확인

이번 Run #8에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "radius-conditioned augmentation budget" → 여전히 없음
- "vessel observability conditioned augmentation" → 여전히 없음
- "thin vessel appearance protection during augmentation" → 여전히 없음

**TSIAA가 가장 가까운 논문**이지만:
- TSIAA는 adversarial (harder) 방향, 내 ONA는 protective (conservative) 방향
- TSIAA의 intra-image conditioning은 region binary, 내 ONA는 radius-continuous
- TSIAA에는 label-image inconsistency 문제의식 없음

**내 핵심 novelty gap은 Run #8에서도 유지된다.**

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문 독해: IAM segment region 정의 방법 + adversarial optimization 수식 상세 → related work에서 구분 논거 작성
- [ ] MAMBA_SEA 전문 독해: local sub-sequence 정의 + prostate 실험 수치 상세
- [ ] MICCAI 2026 preprint 탐색 (Awesome-MICCAI-2026 GitHub 활용)
- [ ] WACV 2026 open access: DG/SSDG 의료영상 세그멘테이션 추가 논문 탐색
- [ ] "non-uniform augmentation medical image" OR "spatially adaptive augmentation DG" 키워드로 2026 신규 탐색
- [ ] ConStyX arXiv(2506.10675) 발표 시기 재확인 (CONSTYX와 동일 논문인지 확인)
