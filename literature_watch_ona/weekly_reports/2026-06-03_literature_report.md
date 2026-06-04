# Literature Watch Report — Run #7

> 날짜: 2026-06-03  
> 모델: claude-sonnet-4-6  
> 신규 논문: **5편** (Published Journal 3편 + Preprint 1편 + Accepted Conference 1편)

---

## 요약

이번 Run에서는 두 가지 주요 방향에서 신규 논문을 발견했다.

1. **SSDG 직접 경쟁 논문 2편** (Pattern Recognition 2025, Expert Systems w/ Applications 2025):
   - DCON (dual-view augmentation + bilevel contrastive learning)
   - ARFU (shape regularization-guided + anatomical prior-guided aug)

2. **Circle of Willis 특화 혈관 분할 논문 2편 + AAAI 2026 DG 논문 1편**:
   - COW_TOPO (Computers in Biology and Medicine 2026)
   - AG-TAL (arXiv 2604.27357, April 2026) — **radius-aware Dice loss** 핵심
   - FLEX-SEG (AAAI 2026) — noise-robust DG for city-scene segmentation

핵심 발견: **AG-TAL이 "radius-aware Dice loss"를 직접 도입**해 소혈관 집중 training을 수행한다. 내 Continuous-ONA의 vessel radius 기반 차별화 아이디어와 개념적으로 인접하나, 목적(loss weighting vs. augmentation budget)과 도메인(CoW multiclass vs. TOF-MRA SSDG DG)이 명확히 분리된다.

---

## Category A — 신규 직접경쟁 논문

### DCON — Pattern Recognition 2025

**논문**: A Hybrid Dual-Augmentation Constraint Framework for Single-source Domain Generalization in Medical Image Segmentation  
**저자**: Ruofan Wang, Jintao Guo, Jian Zhang, Lei Qi, Qian Yu, Yinghuan Shi  
**Venue**: Pattern Recognition, July 2025  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.patcog.2025.xxx (ScienceDirect S0031320325007423)  
**Code**: https://github.com/wrfnj/DCON

#### 방법 요약
- **Dual-view augmentation**: image-level(global-local stylized augmentation with controllability) + feature-level perturbation을 동시에 적용
  - Global-local stylized aug: GIN 계열 global intensity aug + ROI 기반 local aug 조합
  - Feature-level perturbation: feature space에서 domain-variant direction으로 perturbation
- **Bilevel contrastive learning**:
  - Object-focused consistency: 두 dual-view 간 same object의 representation 일치성 강화
  - Cross-individual constraint: batch 내 cross-sample domain-invariant feature 학습
- **Motivation**: "limited stylized variation, small inter-class difference, similar anatomical structure" → 기존 단일 augmentation의 다양성 부족 지적

#### 실험 결과
- Prostate T2-MRI SSDG (6-center): Dice **72.89%** (+2.52% over SLAug)
- 3 datasets에서 기존 SOTA 능가
- Baseline 비교: SLAug, Causality_SDG, RandConv, IBN, RASS 등

#### 내 방법과의 관계
**공통점**: SSDG setting, image-level + feature-level의 이중 augmentation, annotation-region awareness  
**핵심 차이**:
- DCON = 전체 이미지를 "dual-view"로 augment → class-level/image-level diversity
- 나 = 동일 class 내 thin/thick vessel에 **다른 augmentation 강도**를 연속 적용
- DCON에는 intra-class structural observability conditioning 개념이 없음
- DCON은 bilevel contrastive loss를 사용하지만 나는 loss 변경 없이 augmentation만 조절 (POC 기준)

**Novelty 위협도**: Medium — "dual-level augmentation"이라는 큰 방향 유사하나, DCON의 핵심은 feature diversity + contrastive learning. 내 핵심(intra-vessel radius conditioned aug budget)과 구분 명확.

---

### ARFU — Expert Systems with Applications 2025

**논문**: Anatomically-Robust and Feature-Unbiased Domain Generalization for Medical Segmentation  
**Venue**: Expert Systems with Applications, September 28, 2025  
**Status**: Published Journal Article  
**DOI**: ScienceDirect S0957417425033676  
**arXiv**: 미확인

#### 방법 요약
- **핵심 문제의식**: 기존 SSDG 방법들이 "appearance bias"만 다루고 (1) organ shape bias, (2) feature bias (confounding context)를 무시
- **SRG (Shape Regularization-Guided Augmentation)**: low-frequency 구조 정보를 regularizer로 활용한 global appearance transformation
  - 증강 중에도 low-frequency 해부학적 형태가 유지되도록 제약
- **APG (Anatomical Prior-Guided Augmentation)**: anatomical prior 기반 shape-aware augmentation
  - 형태 변화가 해부학적으로 plausible한 범위 내에서만 이루어지도록 제한
- **Feature bias 완화**: uncertainty perturbation + frequency filtering으로 network의 confounding context 의존 억제

#### 실험
- Cross-modality abdominal segmentation (CT → MRI)
- Cross-sequence cardiac MRI (bSSFP → LGE)

#### 내 방법과의 관계
**공통점**: augmentation 강도를 구조 정보(low-frequency content)에 따라 제약, appearance-only augmentation의 한계 지적  
**핵심 차이**:
- ARFU = organ-level shape bias 방지 (전체 organ shape 보존)
- 나 = vessel class 내 radius별 augmentation budget (intra-class)
- ARFU는 thin/thick vessel 구분 개념 없음; 나는 tubular structure의 observability 이질성에 초점

**Novelty 위협도**: Low-Medium — "structure-aware augmentation"이라는 방향이 겹치나, ARFU는 organ-level, 나는 intra-vessel 수준. 출발 문제의식(shape bias)도 다름.

---

## Category C — 신규 혈관·구조 특화 논문

### COW_TOPO — Computers in Biology and Medicine 2026

**논문**: Topology-Aware Multiclass Segmentation of the Circle of Willis from MRA and CTA Images  
**저자**: University of Girona (VICOROB)  
**Venue**: Computers in Biology and Medicine, Vol 204, March 2026  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.compbiomed.2026.111516

#### 방법 요약
- Adapted **post-processing block for topology refinement** (topology-aware 후처리)
- Multi-modality: MRA + CTA from TopCoW 2024 challenge dataset
- In-domain: Dice 0.90 (MRA), 0.88 (CTA)
- **Out-of-domain MRA**: Dice 0.81
- TopCoW 2024 hidden test 1위 달성

#### 내 방법과의 관계
- DG를 직접 다루지는 않으나 CoW의 multi-center out-of-domain 성능을 보고
- 내 TOF-MRA 데이터와 해부학적으로 인접 (Circle of Willis는 TOF-MRA의 주요 구조)
- 내 연구의 application scope 확장 또는 discussion에서 CoW 분절 언급 시 참고 가능

---

### AG-TAL — arXiv 2604.27357 (April 2026) ⚠️ 주목

**논문**: AG-TAL: Anatomically-Guided Topology-Aware Loss for Multiclass Segmentation of the Circle of Willis Using Large-Scale Multi-Center Datasets  
**저자**: Jialu Liu et al.  
**Venue**: arXiv 2604.27357, April 30, 2026  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2604.27357

#### 방법 요약 — **radius-aware Dice loss 핵심**

AG-TAL은 3가지 topology-aware loss를 통합:

1. **Radius-Aware Dice Loss (RAD)**:
   - GT vascular radius를 localized weighting으로 Dice loss에 통합
   - 소혈관일수록 더 큰 penalty weight → 소혈관 집중 학습 유도
   - Small artery 성능 +1.05–3.09% over SOTA

2. **Breakage-Aware clDice (BAC)**:
   - Group convolution으로 효율적 local connectivity 보존
   - 기존 clDice의 3D multiclass 계산 비용 문제를 group conv로 해결

3. **Adjacency-Aware Co-occurrence Loss (AAC)**:
   - 인접 동맥 간 anatomical adjacency prior 통합
   - 이웃 동맥 경계에서의 inter-class misclassification 감소

#### 실험
- Large-scale multi-center CoW dataset (unified annotation)
- 6 independent datasets: Dice 74.46–81.17% (all CoW arteries)
- Small arteries: +2.20–9.98% over other methods

#### 내 방법과의 관계

**직접 관련성 (High)**:
- **공통 핵심 개념**: "vessel radius를 training 신호 조절에 활용" — 내 ONA도 radius/observability를 augmentation budget 조절에 사용
- **목적 분리**: AG-TAL = **loss weighting** (radius-aware penalty), 나 = **augmentation budget** (radius-conditioned appearance perturbation)
- **설정 분리**: AG-TAL = single-domain closed-set training (no DG), 나 = SSDG
- **구조 분리**: AG-TAL = CoW multiclass artery labeling, 나 = cerebrovascular binary SSDG

**내 연구에의 활용**:
- Related Work에서 "radius-aware training이 이미 loss에서 탐색되고 있음"을 언급 가능
- "loss-level radius weighting"(AG-TAL) vs. "augmentation-level radius conditioning"(나)의 상보성 주장 가능
- RAD의 radius 계산 방식 (GT skeleton distance transform) 참고 가능

**Novelty 보호**: 완전히 다른 mechanism (loss vs. augmentation) + 다른 task setting (closed-set vs. SSDG) → 충돌 없음

---

## Category D — 신규 Top-tier Vision 논문

### FLEX-SEG — AAAI 2026

**논문**: Do We Need Perfect Data? Leveraging Noise for Domain Generalized Segmentation  
**Venue**: AAAI 2026 (40th Annual AAAI Conference on Artificial Intelligence)  
**Status**: Accepted Conference Paper  
**arXiv**: 2511.22948 (November 2025)  
**Code**: https://github.com/VisualScienceLab-KHU/FLEX-Seg

#### 방법 요약
- **Problem**: diffusion model로 생성된 augmentation 이미지와 GT mask 간 misalignment 문제 → 기존 방법은 이를 error로 처리하고 제거
- **FLEX-Seg의 관점**: misalignment를 robust learning의 기회로 전환
- **구성**:
  - **Granular Adaptive Prototypes**: boundary 특성을 multi-scale로 capture
  - **Uncertainty Boundary Emphasis**: prediction entropy 기반 학습 가중치 동적 조절
  - **Hardness-Aware Sampling**: challenging example 점진적 집중

#### 실험
- ACDC (nighttime driving), Dark Zurich: +2.44%, +2.63% mIoU over SOTA
- 5 real-world datasets에서 일관된 개선

#### 내 방법과의 관계
- **자연영상 city-scene DG** 논문 — 의료영상 직접 관련 없음
- **Uncertainty Boundary Emphasis**: prediction entropy로 학습 강도 조절 → 내 observability-conditioned aug와 방향 유사하나 완전히 다른 mechanism
- **Category D 참고 수준** (Low relevance)
- AAAI 2026 DG 논문으로서 venue coverage 측면에서 추적 가치 있음

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class structure-specific augmentation strength"
- "thin vessel appearance protection during augmentation"

AG-TAL이 radius를 loss에 활용하는 첫 번째 논문이라는 점은, **반대로 augmentation 쪽에서 radius를 활용하는 my ONA의 gap을 더 명확히 보여준다**.

---

## 다음 Run 우선 탐색 항목

- [ ] AG-TAL 전문 독해: GT radius 계산 방식 (skeleton-based distance transform vs. local maxima) 파악 → 내 observability score 계산 방법론과 비교
- [ ] DCON 전문 독해: bilevel contrastive loss + stylized aug 상세 → 내 방법과 차별화 논거 작성
- [ ] ICLR 2026 proceedings에서 DG/augmentation 관련 논문 추가 탐색 (search 성공률 낮았음)
- [ ] "Topology-Aware Exploration of Circle of Willis" (arXiv 2410.15614) — TopCoW challenge 관련 방법, Cat C 추가 여부 검토
- [ ] Pattern Recognition, Expert Systems w/ Applications 등 보조 venue SSDG 논문 추가 탐색
