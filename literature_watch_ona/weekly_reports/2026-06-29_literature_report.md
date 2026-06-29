# Literature Watch Report — Run #8

> 날짜: 2026-06-29  
> 모델: claude-sonnet-4-6  
> 신규 논문: **11편** (Published Journal 4편 + Accepted Conference 2편 + Preprint 5편)

---

## 요약

이번 Run #8 (Run #7로부터 26일 경과)의 핵심 발견은 다음 두 가지다.

1. **IEEE TMI 2026에 SSDG + 인스턴스별 Bézier 기반 적응 증강 논문(TSIAA) 출판**: 내 방법과 가장 가까운 선행 연구로, ADA(MICCAI 2025)보다 더 높은 venue. 그러나 per-image 단위 적응 vs. 내 intra-image vessel-pixel 단위 공간적 조절이라는 granularity 차이가 명확히 존재.

2. **ICLR 2026, CVPR 2026 논문 탐색 완료**: 직접 경쟁 논문 없음 확인. YPILEARN(ICLR 2026)는 TTA 설정, PEPR(CVPR 2026)는 event camera 자연영상.

추가로 혈관 특화 신규 논문 4편(TOPOVST, AMAP, UVSM, RLAD)과 Mamba 기반 SSDG 논문(MAMBA_SEA), wavelet 기반 SSDG(WAVESDG) 등을 발견했다.

**Novelty Gap 재확인**: "vessel observability conditioned augmentation", "radius-conditioned augmentation budget", "intra-image structural heterogeneity augmentation"을 명시적으로 다룬 논문은 여전히 없음.

---

## Category A — 신규 직접경쟁 논문 (5편)

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 주의

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
**Status**: Published Journal Article  
**ieeexplore**: doi.org/document/11146907  

#### 방법 요약

- **IIAG** (Instance-Level Image Augmenter): 여러 IAM (Instance-level Augmentation Module)으로 구성
  - IAM = **learnable constrained Bézier transformation function** 기반
  - per-instance (이미지별로 독립적) augmentation 파라미터 동적 결정
- **Teacher-Student 구조**:
  - Teacher: student를 최대한 혼란시키는 augmentation을 adversarial하게 탐색
  - Student: augmented image에서 분절 학습
- **Over-augmentation 방지**: label-preserving constraint로 Bézier 변형 범위 제한

#### 내 방법과의 관계

**공통점**: learnable Bézier transformation for SSDG, over-augmentation 방지 의식  
**핵심 차이**:
- TSIAA = **per-image** adversarial augmentation (이미지 하나 전체에 하나의 Bézier curve)
- 나 = **intra-image vessel-pixel** augmentation (동일 이미지 내 thin/thick 혈관이 다른 강도)
- TSIAA는 혈관 두께나 observability 개념이 없음
- TSIAA는 adversarial signal(student's error)이 기준, 나는 vessel radius/observability가 기준

**Novelty 위협도**: Medium-High → paper_notes/TSIAA.md 참조

---

### WAVESDG — arXiv 2603.28463 (April 2026)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2603.28463

#### 방법 요약

- **WaveSDG** 네트워크: encoder feature에 **WISER module** 적용
  - WISER = Wavelet-based Invariant Structure Extraction and Refinement
  - LL sub-band (저주파): 전역 해부학 구조 고정 → domain-invariant anatomy anchor
  - LH/HL/HH sub-band (고주파): directional edge response를 선택적으로 강화 + noise 억제
- Fundus optic disc/cup SSDG, 1 source → 5 unseen target
- 7개 SOTA 대비 최고 성능

#### 내 방법과의 관계
- WAVESDG = frequency domain 전체 영상 단위 decomposition
- 나 = spatial domain 혈관별 radius 기반 conditioning
- 방향 다름: 직접 충돌 없음

---

### MAMBA_SEA — arXiv 2504.17515 (April 2025)

**논문**: Mamba-Sea: A Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2504.17515

- Mamba SSM을 DG에 처음 적용한 논문
- **Global sequence augmentation**: Mamba state space 전체에 domain style perturbation
- **Local sequence augmentation**: local window 단위 appearance perturbation
- 내 방법과 다른 architecture paradigm (SSM vs. augmentation pipeline); SSDG 비교 baseline 후보

---

### PMDG — arXiv 2505.23173 (May 2025)

**논문**: Pseudo Multi-Source Domain Generalization: Bridging the Gap Between Single and Multi-Source Domain Generalization  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2505.23173

- 단일 소스에서 style transfer + data augmentation으로 pseudo-domain 생성
- MDG (multi-source) 알고리즘을 SSDG에 적용 가능하게 만드는 bridge 방법
- 내 방법과 다른 paradigm (pseudo-domain multiplexing); SSDG 방법론 배경 참고용

---

### AD_DGCL — Neurocomputing Vol. 659 (January 2026)

**논문**: Multi-organ Medical Image Segmentation via Adaptive Disentangled Domain Generalization Collaborative Learning  
**Venue**: Neurocomputing, Vol. 659, January 2026  
**Status**: Published Journal Article  
**DOI**: ScienceDirect S0925231225025184

- **SSRD** (Semi-Supervised Representation Disentanglement): domain-specific style vs. anatomical content 분리
- **SCT** (Style-induced Consistency Training): synthetic style perturbation + consistency loss
- Adaptive loss: small organ (소기관)에 더 큰 weight → 내 thin vessel 보호 motivation과 표면적 유사
- 그러나 organ-level (전체 기관 단위), multi-source semi-supervised setting → 내 방법과 설정 다름

---

## Category C — 신규 혈관·구조 특화 논문 (4편)

### AMAP — npj Digital Medicine 2025

**논문**: Anatomically-Guided Masked Autoencoder with Domain-Adaptive Prompting for Multimodal Cerebral Aneurysm Detection and Segmentation  
**Venue**: npj Digital Medicine (Nature Publishing Group), 2025  
**Status**: Published Journal Article  
**DOI**: 10.1038/s41746-025-02188-8

- TOF-MRA + CTA 모두 활용한 뇌혈관 aneurysm detection/segmentation
- **Domain-adaptive prompting**: 도메인별 prompt를 학습하여 modality gap 해소
- **Boundary-aware contrastive generalization**: 혈관 경계에서의 domain invariance 강화
- 내 TOF-MRA 연구와 동일한 해부학적 도메인 (CoW, cerebral vessels)
- 분절 목표 다름 (aneurysm vs. vessel DG), 그러나 같은 데이터 도메인 → Related Work 참조

---

### TOPOVST — arXiv 2603.14909 (March 2026)

**논문**: TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2603.14909

- **Multi-scale sphere graph** 기반 vessel skeleton tracking
- GNN으로 tracking direction과 **vessel radius를 동시 추정**
- **geometry-aware weighting scheme**: 가는 혈관에서의 class imbalance 완화
- Wave-propagation 기반 skeleton tracking으로 spurious skeleton 억제

**내 방법과의 관계**:
- radius estimation이 핵심 구성 요소 → 내 observability score 계산 방식과 비교 가능
- Hessian 기반 vs. sphere graph 기반 radius 추정 비교 흥미
- DG 목적 아님, 내 연구에서 radius 계산 방법론 참고 가능

---

### UVSM — IEEE TIP Vol. 34 (2025)

**논문**: Universal Vessel Segmentation for Multi-Modality Retinal Images  
**Venue**: IEEE Transactions on Image Processing, Vol. 34, pp. 7903–7918, 2025  
**Status**: Published Journal Article  
**arXiv**: https://arxiv.org/abs/2502.06987  
**DOI**: ieeexplore IEEE TIP 2025

- Color Fundus, Multi-Color SLO 등 여러 retinal modality에서 단일 universal model
- per-modality fine-tuning 없이 cross-modality generalization
- 내 방법과 다른 paradigm (modality-universal architecture vs. SSDG augmentation)
- 단, multi-modality universal model 논문의 generalization 전략 참고 가능

---

### RLAD — arXiv 2503.01190 (March 2025)

**논문**: Enhancing Retinal Vessel Segmentation Generalization via Layout-Aware Generative Modelling  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2503.01190

- **RLAD** (Retinal Layout-Aware Diffusion): 실제 혈관 layout에 조건화된 diffusion 기반 image synthesis
- paired retinal image + vessel segmentation mask 생성 (vessel layout 고정, 나머지 변환)
- **REYIA dataset** 도입: 586 manually segmented retinal images
- RLAD-generated data로 vessel segmentation generalization +8.1%
- 내 방법과 다른 paradigm (generative augmentation vs. nonlinear transformation)

---

## Category D — Top-tier Vision 신규 논문 (2편)

### YPILEARN — ICLR 2026

**논문**: You Point, I Learn: Online Adaptation of Interactive Segmentation Models for Handling Distribution Shifts in Medical Imaging  
**Venue**: ICLR 2026  
**Status**: Accepted Conference Paper  
**arXiv**: https://arxiv.org/abs/2503.06717  
**OpenReview**: openreview.net/forum?id=n0vHjCiLD2

- 사용자 click을 통해 test-time에 segmentation model을 distribution shift에 적응
- **Post-Interaction** + **Mid-Interaction** adaptation (click 완료 후/중간에 파라미터 업데이트)
- 5개 fundus + 4개 brain-MRI 데이터셋 실험
- 설정: test-time adaptation (target domain 필요), 나: SSDG (target 없이 학습)
- 직접 경쟁 아님, ICLR 2026 venue coverage

---

### PEPR — CVPR 2026

**논문**: Privileged Event-based Predictive Regularization for Domain Generalization  
**Venue**: CVPR 2026  
**Status**: Accepted Conference Paper  
**arXiv**: https://arxiv.org/abs/2602.04583

- Event camera를 privileged modality로 활용한 domain-invariant representation 학습
- LUPI (Learning Using Privileged Information) 패러다임을 DG에 적용
- 자연영상 semantic segmentation (driving scene)
- 내 연구와 직접 관련 없음, CVPR 2026 DG 트렌드 추적용

---

## Novelty Gap 재확인

Run #8에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation" → 없음
- "radius-conditioned augmentation budget" → 없음 (AG-TAL: radius × loss, 나: radius × aug)
- "intra-class vessel pixel-level augmentation strength" → 없음
- "thin vessel appearance protection in domain generalization" → 없음 (L2CP는 TTA 설정)

**TSIAA가 가장 가까운 경쟁 논문이지만**, per-image unit vs. intra-image spatial unit의 차이로 핵심 novelty gap은 유지된다.

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문 독해: IAM Bézier 구조 + teacher-student 학습 방식 상세 → ADA와의 구분 + 내 방법과의 구분 논거 확립
- [ ] TOPOVST: radius estimation 방식 상세 → 내 observability score와 비교 가능성
- [ ] WAVESDG: WISER module의 sub-band별 처리 방식 상세
- [ ] MAMBA_SEA: Mamba SSM 기반 DG의 sequence augmentation 방식
- [ ] AMAP: domain-adaptive prompting 메커니즘 → SSDG에 응용 가능성
- [ ] arXiv 2606.x 논문 추가 탐색 (June 2026 직전 논문들 확인)
- [ ] MICCAI 2026 early acceptance 공개 시 재탐색
- [ ] IJCAI 2026 accepted list 공개 시 탐색
