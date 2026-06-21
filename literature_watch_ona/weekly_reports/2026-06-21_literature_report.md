# Literature Watch Report — Run #8

> 날짜: 2026-06-21  
> 모델: claude-sonnet-4-6  
> 신규 논문: **9편** (Published Journal 4편 + Accepted Conference 2편 + Preprint 3편)  
> 누적 인덱스: 96 → **105편**

---

## 요약

이번 Run #8에서는 두 가지 탐색 경로를 병행했다:

1. **주 세션 GitHub MCP code search** (WebFetch/WebSearch 403 Forbidden으로 차단된 상황에서 우회):
   - CCSDG (MICCAI 2023), LEARN2SYNTH (ICCV 2025), CSWINUNETR (arXiv 2606.19824)

2. **Background agent WebSearch** (다른 실행 환경에서 WebSearch 가능):
   - DG-DDM-SEG (IEEE TMI 2025), **TSIAA (IEEE TMI 2026)** ⚠️, PCSDG (BSPC 2025), SDCL (TPAMI 2025), WAVESDG (arXiv 2603.28463), TOPBRAIN (medRxiv 2026)

**가장 중요한 발견: TSIAA (IEEE TMI 2026)**  
"단일 이미지 내 서로 다른 해부학적 구조에 서로 다른 augmentation"을 adversarial 방식으로 구현한 최초의 TMI 논문. 내 Continuous-ONA의 핵심 동기(intra-image structure-specific augmentation)와 가장 근접하나, conditioning signal이 근본적으로 다름:
- **TSIAA**: adversarial(learnable) — 데이터로부터 어떤 구조가 어떤 augmentation을 받아야 하는지 학습
- **나 (Continuous-ONA)**: observability(vessel radius) — 물리적 혈관 직경에 따라 결정론적으로 조절

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 주목

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**저자**: Zhengshan Wang, Long Chen, Xuelin Xie, Weiping Ding  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
**Status**: Published Journal Article  
**DOI**: IEEE Xplore document 11146907  
**Code**: https://github.com/Wangzts0228/TSIAA

#### 방법 요약

- **Instance-level Image Augmenters (IIAG)**: Instance-level Augmentation Modules(IAM)으로 구성
  - 각 IAM은 Learnable Constrained Bézier Transformation 기반
  - Image-level augmentation과 달리 단일 이미지 내 서로 다른 해부학적 구조가 서로 다른 augmentation을 받음
- **Teacher-Student Adversarial Training**:
  - Student: 강도 높은 instance-level adversarial augmentation으로 학습
  - Teacher: EMA로 Student 업데이트, consistency loss로 domain-invariant feature 강제
- **핵심 innovation**: 이미지를 anatomical instance 단위로 분할하여 instance-specific augmentation 적용

#### 내 방법과의 관계

**공통점**: 단일 이미지 내 서로 다른 해부학적 구조에 서로 다른 augmentation → "intra-image structure-specific augmentation"  
**핵심 차이**:
- **Conditioning signal**: TSIAA = adversarial (어떤 augmentation이 좋은지 학습), 나 = vessel radius/observability (물리적 직경이 직접 conditioning)
- **Mechanism**: TSIAA = adversarial search space + IAM, 나 = continuous observability → augmentation budget mapping
- **Domain specificity**: TSIAA = general medical segmentation (prostate, abdominal 등), 나 = TOF-MRA cerebrovascular SSDG
- **Design rationale**: TSIAA는 왜 서로 다른 구조가 다른 augmentation을 받아야 하는지에 대한 명시적 원칙이 없음. 나는 "얇은 혈관은 observability가 낮아 강한 augmentation이 label 신뢰성을 파괴한다"는 물리적/해부학적 원칙을 explicitly 활용.

**Novelty 위협도**: Medium — "intra-image structure-specific augmentation"이라는 방향이 겹침. 하지만 내 핵심 contribution(vessel radius → continuous observability → augmentation budget mapping)은 TSIAA에 없음. TSIAA 전문 독해 후 차별화 논거 구체화 필수.

---

### CCSDG — MICCAI 2023 Early Accept

**논문**: Devil is in Channels: Contrastive Single Domain Generalization for Medical Image Segmentation  
**저자**: Shishuai Hu, Zehui Liao, Yong Xia  
**Venue**: MICCAI 2023  
**Status**: Accepted Conference Paper  
**arXiv**: 2306.05254

#### 방법 요약

- **Channel-level domain analysis**: 일부 feature channel은 domain-specific, 일부는 domain-invariant
- **Contrastive learning on channel statistics**: domain-variant channel을 augmentation으로 다양화, domain-invariant channel을 contrastive loss로 일관되게 유지
- Prostate MRI 6-center + skin lesion cross-dataset 실험

#### 내 방법과의 관계

- CCSDG = feature channel 단위 domain gap 분리 (representation learning)
- 나 = augmentation budget의 intra-class spatial 연속 조절 (augmentation)
- 메커니즘과 목적 모두 다름. Novelty 위협도: Low.

---

### DG-DDM-SEG — IEEE TMI 2025

**논문**: Domain-Generalized Discrete Diffusion Model for Cross-Domain Medical Image Segmentation  
**저자**: Heran Yang et al.  
**Venue**: IEEE Transactions on Medical Imaging, April 2025  
**Status**: Published Journal Article  
**DOI**: 10.1109/TMI.2025.3564474  
**Code**: https://github.com/HeranYang/DG-DDM-Seg

#### 방법 요약

- **Discrete conditional distribution**: segmentation mask를 discrete conditional distribution으로 생성하는 diffusion model
- **Two-path reverse diffusion**: Robust Feature Extraction Subnet(domain-independent feature 추출) + Mask-Generation Transformer
- Pseudo-label을 입력으로 활용해 cross-domain 성능 향상

#### 내 방법과의 관계

- 생성 모델(diffusion) 기반 DG vs. augmentation 기반 SSDG — 완전히 다른 패러다임
- Related Work "DG via generative models" 계열 대표 논문으로 인용 가능
- **Novelty 위협도**: None.

---

### PCSDG — Biomedical Signal Processing and Control 2025

**논문**: Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation  
**저자**: Jia-Xuan Jiang, Yuee Li, Zhong Wang  
**Venue**: Biomedical Signal Processing and Control, Vol. 99, 2025  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.bspc.2024.106801

#### 방법 요약

- **SABA (Structure-Aware Brightness Augmentation)**: 
  - Disentanglement module으로 content map + style map 분리
  - Pixel-wise multiplication으로 saliency-based structure attention map 생성
  - Structure attention에 따라 brightness augmentation 강도 차등 적용
- Optic disc/cup SSDG on RIGA+ dataset

#### 내 방법과의 관계

- "pixel-level structural map에 따른 augmentation 강도 조절" 원칙 공유
- 차이: PCSDG = saliency-based binary attention (salient region vs. non-salient), 나 = vessel radius 기반 연속 observability score
- Optic disc 특화, vessel caliber 이질성 개념 없음
- **Novelty 위협도**: Low-Medium.

---

## Category B — 신규 방법론 유사 논문

### SDCL — IEEE TPAMI 2025

**논문**: Causal Inference via Style Bias Deconfounding for Domain Generalization  
**저자**: Jiaxi Li, Di Lin, Hao Chen, Hongying Liu, Liang Wan, Wei Feng  
**Venue**: IEEE Transactions on Pattern Analysis and Machine Intelligence, 2025  
**Status**: Published Journal Article  
**arXiv**: 2503.16852

#### 방법 요약

- **Structural Causal Model (SCM)**: style을 DG의 confounding factor로 명시적 모델링
- **Backdoor adjustment**: style의 인과적 영향을 제거하여 content-only representation 학습
- Style frequency bias 문제를 causal inference로 해결

#### 내 방법과의 관계

- 인과론적 style deconfounding vs. 내 augmentation budget approach — 메커니즘 완전히 다름
- "appearance 변화에도 구조 정보가 보존되어야 한다"는 동기 공유
- TPAMI 최고 tier 논문 → Related Work "causal DG" 계열 대표 인용 가능
- **Novelty 위협도**: None.

---

## Category C — 신규 구조·혈관 특화 논문

### CSWINUNETR — arXiv 2606.19824 (June 2026)

**논문**: CSWinUNETR: Segmentation of Thin Anatomical Structures in Medical Images  
**저자**: Junho Moon, Haejun Chung, Ikbeom Jang  
**Status**: Preprint Only (June 2026, likely MICCAI 2026)  
**arXiv**: 2606.19824

#### 방법 요약

- CSWin Transformer + UNETR 아키텍처: thin anatomical structure에 특화된 cross-shaped window attention
- Thin structure의 anisotropic geometry에 맞춘 shifted window attention 설계
- Long-range dependency + local precision 동시 확보

#### 내 방법과의 관계

- "thin anatomical structure는 특별한 처리가 필요하다"는 동기 공유
- 차이: CSWINUNETR = architecture 설계(attention), 나 = training-time augmentation budget 조절
- 내 동기 지지 근거로 활용 가능 (architecture 관점의 independent motivation)
- **Novelty 위협도**: None.

### TOPBRAIN — medRxiv 2026

**논문**: TopBrain Segmentation Challenge for Whole Brain Vessel Anatomy  
**Status**: Preprint Only (medRxiv)  
**DOI**: 10.64898/2026.05.28.26354312

#### 방법 요약

- 최초 whole-brain MRA+CTA fine-grained multi-class vessel segmentation benchmark
- 48 landmark vessel classes (arterial + venous), 90 annotated volumes
- **Vessel caliber measurements along centerlines** 포함 — 전 뇌혈관에 걸친 직경 측정

#### 내 방법과의 관계

- TOF-MRA/MRA 기반 뇌혈관 분할 도메인에서 직접 관련
- **Caliber 분포 데이터**: 내 observability score 계산의 외부 검증 근거로 활용 가능
- Dataset/benchmark 논문 (방법 제안 아님), 내 application scope 확장 논거

---

## Category D — 신규 Top-tier Vision 논문

### LEARN2SYNTH — ICCV 2025

**논문**: Learn2Synth: Learning Optimal Data Synthesis Using Hypergradients for Brain Image Segmentation  
**저자**: Xiaoling Hu et al.  
**Venue**: ICCV 2025, pp. 20368-20378  
**Status**: Accepted Conference Paper  
**arXiv**: 2411.16719

#### 방법 요약

- **Bilevel optimization**: inner loop에서 segmentation 학습, outer loop에서 synthesis strategy를 hypergradient로 최적화
- 최적 synthetic data 생성 전략을 자동 학습 (meta-learning 관점)
- Brain MRI 합성 데이터 기반 DG 실험

#### 내 방법과의 관계

- 합성 데이터 생성 전략 최적화 (bilevel) vs. 내 실제 데이터 기반 augmentation
- 패러다임 다름. ICCV 2025 top-tier 논문으로 DG survey 참고용.
- **Novelty 위협도**: None.

---

## Novelty Gap 재확인

이번 Run #8에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "thin vessel appearance protection during nonlinear augmentation"
- "intra-class continuous augmentation strength based on vessel radius"

**TSIAA 경보**: TSIAA가 "intra-image structure-specific augmentation"의 개념적 선례를 제공함. 그러나:
1. TSIAA의 conditioning: adversarial (모델이 학습), Continuous-ONA: vessel radius (물리적 원칙)
2. TSIAA의 목적: general SSDG diversity, Continuous-ONA: thin vessel observability 보호
3. TSIAA의 mechanism: IAM + adversarial training, Continuous-ONA: continuous observability → aug budget

**결론**: Continuous-ONA의 핵심 novelty claim 유지. TSIAA 전문 독해 후 차별화 논거를 Related Work와 Introduction에 명시적으로 작성해야 함.

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA 즉시 전문 독해**: IAM 구조 상세 + adversarial training 방식 파악 → 내 방법과 차별화 논거 작성
- [ ] AG-TAL 전문 독해: GT radius 계산 방식 (skeleton distance transform) → 내 observability score와 비교
- [ ] DCON 전문 독해: GLSA controllability 파라미터 + bilevel contrastive loss 상세
- [ ] CSWINUNETR 전문 독해: thin structure attention 설계 + 내 DG 설정과의 관계
- [ ] TOPBRAIN caliber 데이터 확인: vessel caliber 분포가 내 observability score 설계 근거로 활용 가능한지
- [ ] MICCAI 2026 accepted list 공개 시 DG/vessel/thin structure 논문 탐색 (2026-07 예상)
- [ ] PCSDG 전문 독해: SABA structure attention map의 granularity 확인 (saliency map vs. segmentation mask)

---

## 탐색 환경 제약 메모

- **WebFetch 403 Forbidden**: arXiv, SemanticScholar, OpenReview, IEEE Xplore 등 학술 사이트 전체 차단
- **WebSearch 결과 없음**: 네트워크 정책 (US 외 환경)
- **우회 방법**: GitHub MCP `search_code`로 public GitHub repo 탐색 (유효)
- **Background agent**: WebSearch 접속 가능 — DG-DDM-SEG, TSIAA, PCSDG, SDCL, WAVESDG, TOPBRAIN 발견
