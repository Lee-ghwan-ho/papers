# Literature Watch Report — Run #8
**날짜**: 2026-06-15  
**이전 실행**: 2026-06-03 (Run #7)  
**신규 논문**: **5편** (Published Journal 2편 + Accepted Conference 1편 + Preprint 2편)  
**누적 수록**: 101편

---

## 이번 실행 요약

Run #7 이후 12일간의 공백을 커버. 5개 search lane × 4 카테고리 탐색.  
가장 중요한 신규 발견: **TSIAA (IEEE TMI 2026)** — 내 방법과 가장 직접적으로 경쟁하는 Published Journal 논문.

---

## 신규 발견 논문 (5편)

### 1. TSIAA ⭐ 최우선 주의

**제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE TMI, Vol. 45, pp. 764–776, 2026  
**Status**: Published Journal Article  
**Category**: A (직접 경쟁)  
**Relevance**: High

**핵심 내용**:
- Instance-level Image Augmenter (IIAG): learnable constrained Bézier transformation으로 per-sample hard examples 생성
- Teacher → hard augmented view 생성 (adversarial) / Student → domain-invariant feature 학습
- 기존 adversarial aug의 image-level 단순 구조 → over-augmentation 문제 해결하겠다는 동기

**내 연구와의 관계**:
- 공통점: Bézier 계열 nonlinear transformation + SSDG 설정
- **핵심 차이**: TSIAA = image 전체 단일 aug strength (intra-image 구조 구분 없음) vs. ONA = vessel radius/observability별 공간적으로 연속 조절. TSIAA는 thin vessel의 label-image inconsistency 문제를 다루지 않음.
- 내 ONA의 독립적 novelty 유지: "intra-image 공간적 augmentation budget 조절"

**실험**: Prostate MRI (6 centers) + Retinal Fundus (4 centers) — TOF-MRA 없음

---

### 2. WaveSDG

**제목**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463 (March 2026)  
**Status**: Preprint Only  
**Category**: A  
**Relevance**: High

**핵심 내용**:
- WISER (Wavelet-based Invariant Structure Extraction and Refinement) module
- LL sub-band = 해부학적 구조 보존 / LH, HL = 방향성 에지 / HH = 도메인 노이즈
- wavelet decomposition으로 anatomy-appearance 분리 → 구조는 보존하면서 style만 변환

**내 연구와의 관계**:
- 방향: structure-appearance decoupling → 내 방법과 동일한 "구조 보존" 동기
- **차이**: WaveSDG = frequency domain에서 sub-band 분리 / ONA = spatial domain에서 vessel radius별 budget 조절
- Fundus 데이터 (retinal vessel), 내 TOF-MRA와 도메인 다름

---

### 3. AEGIS

**제목**: Aegis: A Domain Generalization Framework for Medical Image Segmentation by Mitigating Feature Misalignment  
**Venue**: Pattern Recognition (ScienceDirect, September 2025)  
**Status**: Published Journal Article  
**Category**: A  
**Relevance**: Medium

**핵심 내용**:
- Style augmentation → augmented feature 생성
- DAFC (Dual Attention-Guided Feature Calibration): source와 augmented feature 간 interaction
- UFA loss (Uncertainty-Guided Feature Alignment): domain shift로 인한 segmentation discrepancy를 uncertainty weighting으로 alignment
- 3 benchmarks: Prostate + Fundus + Cardiac

**내 연구와의 관계**:
- 유사점: uncertainty 개념 활용 (FIESTA와도 유사 방향)
- 차이: Aegis = feature-space alignment constraint (image-level uniform style aug 기반) / ONA = input-space 내에서 spatial varying aug budget

---

### 4. CDG (Continuous Domain Generalization)

**제목**: Continuous Domain Generalization  
**Venue**: NeurIPS 2025  
**arXiv**: 2505.13519  
**Status**: Accepted Conference  
**Category**: D (Top-tier Vision)  
**Relevance**: Low

**핵심 내용**:
- NeuralLio (Neural Lie Transport Operator): Lie Group theory로 parameter manifold에서 domain transition을 기하학적 연속성 보존
- "연속적 latent factor (시간, 지리, 사회경제적 맥락) 조합에 의해 정의되는 unseen domain으로의 generalization"
- 의료영상 아님 (remote sensing, traffic 등)

**내 연구와의 관계**:
- "Continuous" 개념의 이론적 배경 참고 (내 "continuous observability" 명칭과 공명)
- 직접적 방법 전이는 어려움 (parameter space vs. input space)

---

### 5. RLAD

**제목**: Enhancing Retinal Vessel Segmentation Generalization via Layout-Aware Generative Modelling  
**arXiv**: 2503.01190 (March 2025)  
**Status**: Preprint Only  
**Category**: C (혈관 특화)  
**Relevance**: Medium

**핵심 내용**:
- Diffusion model에 vascular structure layout (A/V map, OD/OC) 조건 결합
- 다양한 도메인의 synthetic retinal image 생성 → 최대 +8.1% cross-domain improvement
- RLAD-generated data로 vessel segmentation DG 강화

**내 연구와의 관계**:
- Cat C 참고용: 생성 모델 기반 DG의 retinal vessel 사례
- 내 방법은 non-generative (augmentation based)이므로 orthogonal

---

## Novelty Gap 현황 (Run #8 기준)

| 개념 | 기존 가장 가까운 논문 | gap |
|------|---------------------|-----|
| Intra-image vessel radius → augmentation budget | AG-TAL (loss weighting), ADA (per-sample Bézier) | aug에서는 없음 |
| Thin vessel label-image inconsistency 보호 | L2CP (morphological closing concept) | DG training에서는 없음 |
| Continuous spatial aug budget (thick→thin 연속) | FIESTA (uncertainty-based), TSIAA (per-sample) | 공간적으로 연속인 것은 없음 |
| Nonlinear intensity aug + radius conditioning 동시 | 없음 | 완전 gap |

→ **Run #8 이후에도 내 핵심 novelty gap 유지됨**

---

## 다음 실행 시 탐색 우선순위

1. CVPR 2026 proceedings 공개 여부 확인 (6월 개최 예정)
2. MICCAI 2026 early accept 목록 (August 2026)
3. TSIAA와 ADA의 ablation 비교: Bézier parameter optimization 방식 차이
4. ICLR 2026 의료영상 DG 논문 직접 openreview.net 탐색
5. "augmentation budget" + "thin structure" 조합 키워드 재탐색 (새 preprint 유입 가능)
