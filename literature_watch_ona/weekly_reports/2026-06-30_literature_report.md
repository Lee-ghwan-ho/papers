# Literature Watch Report — Run #8
> 날짜: 2026-06-30 | 모델: claude-sonnet-4-6 | 누적 수록: 102편

---

## 이번 실행 요약

- 신규 발견: **6편** (Published Journal 2편 + Accepted Conference 1편 + Preprint 3편)
- 최고 우선순위 신규 논문: **TSIAA (IEEE TMI 2026)** — intra-image non-uniform adversarial augmentation for SSDG
- 내 핵심 novelty gap 상태: **유지** (conservative-for-thin 개념은 직접 다룬 논문 없음)
- 단, TSIAA 출현으로 "intra-image non-uniformity" 자체는 더 이상 novelty claim으로 충분하지 않음 → **positioning 재정비 필요**

---

## 신규 논문 전체 목록

### 최우선 (P0) — 즉시 독해

| KEY | 제목 | Venue | Year | Status | 관련성 |
|-----|------|-------|------|--------|--------|
| TSIAA | Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation | IEEE TMI | 2026 | Published Journal | ⚠️ **High** |

### 높은 우선순위 (P1)

| KEY | 제목 | Venue | Year | Status | 관련성 |
|-----|------|-------|------|--------|--------|
| MAMBA_SEA | Mamba-Sea: Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation | IEEE TMI | 2025 | Published Journal | High |
| ADVST | AdvST: Revisiting Data Augmentations for Single Domain Generalization | AAAI 2024 | 2024 | Accepted Conference | Medium |
| WAVESDG | Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation | arXiv 2603.28463 | 2026 | Preprint Only | Medium |

### 중간 우선순위 (P2)

| KEY | 제목 | Venue | Year | Status | 관련성 |
|-----|------|-------|------|--------|--------|
| TOPOVST | TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking | arXiv 2603.14909 | 2026 | Preprint Only | Medium |
| BREAK_DATA_BARRIER | Breaking the Data Barrier: Robust Few-Shot 3D Vessel Segmentation using Foundation Models | arXiv 2602.23782 | 2026 | Preprint Only | Low |

---

## TSIAA — 핵심 분석 (Novelty 충돌 평가)

### 논문 개요

**Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation**
- 저자: Zhengshan Wang et al.
- 게재: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026
- DOI: 10.1109/TMI.2026.11146907
- Code: https://github.com/Wangzts0228/TSIAA

### 방법 요약

- **IIAG (Instance-level Image Augmenter)**: 여러 IAM(Instance-level Augmentation Module) 구성
- 각 IAM: learnable constrained Bézier transformation function 기반
- **Teacher-Student adversarial framework**: 
  - Student: adversarial augmentation 생성 (harder, more diverse)
  - Teacher: original + augmented features가 일관된 generalized representation 유지
- 4개 SDG task 실험: 제시된 SOTA 초과

### 나의 방법과 겹치는 부분

1. "같은 이미지 내 서로 다른 구조에 서로 다른 augmentation" 아이디어
2. Bézier transformation을 core augmentation function으로 사용
3. SSDG 설정 (single source domain)

### 결정적 차이 — Continuous-ONA의 독자적 contribution

| 비교 항목 | TSIAA | Continuous-ONA |
|-----------|-------|----------------|
| 목적 | Diversity maximization (더 강한 aug 탐색) | Asymmetric protection (thin = 보수적, thick = 공격적) |
| conditioning signal | 없음 (adversarially learned) | Local vessel radius / observability score (explicit physics) |
| thin structure 처리 | 명시적 고려 없음 (동일하게 adversarial) | 의도적으로 conservative (얇을수록 약하게) |
| application domain | 범용 의료영상 (prostate, fundus, polyp 등) | Tubular/vessel 특화 |
| 핵심 문제의식 | "같은 aug 규칙은 diversity가 부족하다" | "강한 aug이 얇은 혈관에서 label-image inconsistency를 만든다" |
| 방향성 | maximize augmentation strength adversarially | regulate augmentation by structure observability |

### 포지셔닝 전략

**기존 포지션 (유지 가능):**
> "Existing methods either apply uniform augmentation to all structures (SLAug, ConStyX) or seek maximal diversity (TSIAA). Both ignore that thin/fragile vessels cannot tolerate strong perturbations without losing structural evidence. ONA explicitly encodes observability into a continuous augmentation budget, protecting fragile structures while enabling aggressive augmentation for well-resolved ones."

**추가 claim (강화 가능):**
> "TSIAA demonstrates that intra-image non-uniform augmentation is beneficial. We extend this insight with a physics-motivated direction: for tubular structures, the augmentation budget should be constrained by structural observability, not just optimized for diversity."

### 결론

TSIAA는 "intra-image non-uniform augmentation"의 유효성을 IEEE TMI에서 확인해준 논문이다. 이는 내 방법의 전제를 지지하는 동시에, 내 novelty claim을 단순한 "non-uniformity" 수준에서 "observability-driven conservative protection"으로 더 날카롭게 정비해야 함을 의미한다.

---

## Mamba-Sea 분석

**방법**: 
- Global: 다양한 site appearance 시뮬레이션 (inter-domain)
- Local: Mamba 입력 sequence 중 random continuous sub-sequence의 style statistics 재샘플링 (intra-image)

**내 방법과의 관계**:
- 둘 다 "local sub-region 단위로 다른 style aug"를 적용한다는 공통점
- Mamba-Sea의 "sub-sequence"는 Mamba의 scan order에 따른 공간 순서, 내 방법의 "vessel region"은 physics-motivated anatomical unit
- Mamba backbone + DG aug 실험 고려 시 참고할 최신 baseline

---

## WaveSDG 분석

**방법**: WISER(Wavelet-based Invariant Structure Extraction and Refinement) module
- Low-frequency sub-band: global anatomy 고정 (domain-invariant)
- High-frequency sub-band: directional edge 강화 + noise suppression
- Fundus optic disc/cup SSDG에서 7개 SOTA 모두 초과

**내 방법과의 관계**:
- WaveSDG = frequency domain 분리 (low/high-frequency), ONA = spatial domain 분리 (thin/thick vessel)
- 직접 충돌 없음 — 서로 orthogonal한 접근
- 그러나 "구조 정보를 보존하면서 appearance를 분리해야 한다"는 철학은 공유

---

## AdvST 분석

**방법**: Semantics transformations(learnable aug params)의 adversarial learning으로 diverse SSDG 샘플 생성

**관계**: TSIAA의 선행 논문으로 해석 가능. "adversarial augmentation for SDG"의 기초를 제공. 
내 방법과의 핵심 차이: 역시 diversity maximization 방향 (thin vessel protection 없음)

---

## TopoVST 분석

**방법**: Multi-scale sphere graphs + GNN으로 vessel tracking direction + vessel radius 동시 추정

**관계**: 
- vessel radius estimation을 GNN으로 수행한다는 점이 내 observability score 계산 파이프라인과 연결됨
- Wave-propagation skeleton tracking으로 spurious skeleton 억제 → 내 skeleton-distance-transform 기반 radius map 계산에 보완 가능

---

## Novelty Gap 현황 (Run #8 이후)

| 키워드 | 존재 여부 | 비고 |
|--------|-----------|------|
| vessel observability conditioned augmentation | **없음** | 내 핵심 gap 유지 |
| radius-conditioned augmentation budget | **없음** | gap 유지 |
| thin vessel protection augmentation | **없음** | gap 유지 |
| intra-image non-uniform augmentation | **있음 (TSIAA)** | TSIAA에서 확인 — gap 소진, 차별화 필요 |
| intra-class continuous augmentation budget | **없음** | 내 방법의 핵심 독자성 |

---

## 다음 실행을 위한 추적 항목

- [ ] TSIAA full text: "instance" 정의 명확화 (semantic object? region mask?) + IAM의 Bezier space 구성
- [ ] CVPR 2026 proceedings 공개 대기 (2026-07 예정)
- [ ] MICCAI 2026 accepted papers 공개 대기 (2026-08 예정)
- [ ] "conservative augmentation for thin structure" 직접 탐색 지속
- [ ] TopoVST radius estimation GNN 세부 구현 확인 (관찰가능성 score 계산 참고)
