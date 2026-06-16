# Literature Watch Report — Run #8
**날짜:** 2026-06-16  
**모델:** claude-sonnet-4-6  
**신규 논문:** 5편 (Published Journal 2편 + Accepted Conference 1편 + Preprint 2편)  
**누적 인덱스:** 101편

---

## 이번 실행 요약

Run #8은 2026-06-03 이후 신규 발표된 논문을 5개 Lane × 4 Category로 탐색했다.
주요 발견: TPAMI 최고 tier 신규 논문(SDCL), MICCAI 2025 혈관 SDF 논문(VesselSDF),
IEEE TIP 범용 혈관 분할(UVSM), 최신 wavelet SSDG(WaveSDG), 그리고 cerebrovascular DG
실패를 XAI로 분석한 분석 논문(XAI_DX).

"radius-conditioned augmentation" / "observability-conditioned augmentation" 직접 명시 논문은
이번 실행에서도 발견되지 않아 Continuous-ONA의 핵심 novelty gap이 유지된다.

---

## 신규 발견 논문 5편

### 1. WAVESDG ★★ (Cat A) — SSDG 직접 경쟁
**"Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation"**  
arXiv 2603.28463 | March 2026 | Preprint Only | Rel: High

**방법 요약:**
- WISER(Wavelet-based Invariant Structure Extraction and Refinement) 모듈 제안
- 저주파 sub-band → global anatomy anchor로 정규화
- 고주파 sub-band → directional edge 강화 + noise 억제 (domain-specific artifact 제거)
- 1 source domain + 5 unseen target domain 벤치마크에서 7개 SOTA 방법 대비 최고 balanced Dice + 최저 HD95 달성

**내 방법과의 관계:**
- **공통점:** SSDG 설정, 단일 source에서 unseen domain 일반화
- **차이:** WaveSDG = 전체 이미지 단위 주파수 분해, 나 = 동일 이미지 내 vessel radius별 비선형 appearance augmentation budget 조절. WaveSDG에는 intra-class vessel thickness 이질성 개념이 없음.
- **인용 전략:** "frequency-domain SSDG의 최신 방법으로 baseline 비교 후보; 내 방법은 frequency 축이 아닌 structure-observability 축에서 augmentation을 설계한다"는 구분점으로 활용.

---

### 2. SDCL ★★ (Cat D) — IEEE TPAMI, 최고 tier 아이디어 전이
**"Causal Inference via Style Bias Deconfounding for Domain Generalization"**  
arXiv 2503.16852 | IEEE TPAMI 2025 | Published Journal Article | Rel: Medium

**방법 요약:**
- SCM(Structural Causal Model) 기반: style을 X→Y 인과 경로의 confounding variable로 명시 모델링
- SGEM(style-guided expert module): 훈련 중 style distribution clustering으로 global confound 포착
- BDCL(backdoor causal learning): backdoor adjustment로 style confound를 feature extraction 단계에서 제거
- Multi-domain + single-domain DG 모두에서 자연영상 + 의료영상 SoTA

**내 방법에 적용 가능한 아이디어:**
- **causal narrative 강화:** "style augmentation이 효과적인 이유는 style이 causal pathway의 confound이기 때문" (SDCL 논거)를 인용하여, thin vessel에서 style confound 효과가 더 강하다는 내 주장을 causal framework로 지지 가능.
- style deconfounding vs. style amplification의 이중 전략: thick vessel에 강한 style 변형을 적용하는 이유를 "confound 효과가 충분히 관찰 가능한 구조에서는 제거 가능하지만, fragile structure에서는 confound 제거 시 signal loss가 발생한다"고 causal하게 서술 가능.

---

### 3. UVSM ★ (Cat C) — IEEE TIP, 범용 혈관 분할
**"Universal Vessel Segmentation for Multi-Modality Retinal Images"**  
arXiv 2502.06987 | IEEE Transactions on Image Processing 2025 | Published Journal Article | Rel: Medium

**방법 요약:**
- Universal Vessel Segmentation Model(UVSM): 모달리티별 fine-tuning 없이 모든 retinal modality(CF, MC 등)에서 혈관 분할
- Image translation을 domain adaptation으로 활용: 임의 modality → Topcon Color Fundus 정규화 후 단일 분할 모델 적용
- 기존 CF-only 방법 + fine-tuning 방법 대비 comparable 성능 달성

**내 방법과의 관계:**
- Paradigm이 다름: UVSM = translation-based domain normalization, 나 = training-time SSDG augmentation
- 혈관 분할의 modality-agnostic 표현 학습의 최신 IEEE TIP 기준 논문으로 관련 연구 인용에 활용

---

### 4. VESSELSDF ★ (Cat C) — MICCAI 2025, SDF 기반 혈관 기하
**"VesselSDF: Distance Field Priors for Vascular Network Reconstruction"**  
arXiv 2506.16556 | MICCAI 2025 (papers.miccai.org/miccai-2025/1003-Paper2121.html) | Accepted Conference Paper | Rel: Medium

**방법 요약:**
- Voxel binary 분류 대신 SDF(Signed Distance Field) regression으로 혈관 재구성 패러다임 전환
- 각 복셀의 값 = nearest vessel surface까지의 signed distance → 연속적인 기하 표현
- Adaptive Gaussian regularizer: vessel surface 근방은 정밀, 원거리는 smooth → floating artifact 제거
- Sparse CT slice에서 continuous vessel geometry 재구성; SOTA 대비 유의미한 개선

**내 방법에 적용 가능한 아이디어:**
- **SDF → vessel radius proxy:** SDF 절댓값 = vessel centreline에서의 거리 = local radius. 내 observability score 계산을 Euclidean Distance Transform(EDT) 대신 SDF 기반으로 구성하는 대안.
- VesselSDF의 continuous SDF representation은 내가 사용하는 "skeleton EDT = radius map" 개념과 수학적으로 동일하므로, 참고 문헌으로 인용 가능: "vessel radius는 skeleton EDT(SDF)의 값으로 계산한다 [VesselSDF, COSTA, AG-TAL]"

---

### 5. XAI_DX (Cat A, Preprint) — Cerebrovascular DG 실패 분석
**"XAI-Driven Diagnosis of Generalization Failure in State-Space Cerebrovascular Segmentation Models: A Case Study on Domain Shift Between RSNA and TopCoW Datasets"**  
arXiv 2512.13977 | December 2025 | Preprint Only | Rel: Medium

**내용 요약:**
- UMamba (State-Space Model)를 RSNA CTA Aneurysm(source) → TopCoW CoW CT(target)에 적용
- Dice 0.8604 (source) → 0.2902 (target): 심각한 DG failure
- XAI(Explainable AI) 방법으로 실패 원인 진단: Z-resolution 차이 + background noise 분포가 주요 confound
- SDG (Single-Domain Generalization)이 practical한 대안임을 강조

**내 연구에의 활용:**
- **동기 부여 근거:** cerebrovascular segmentation에서 inter-site domain shift가 실제로 severe함을 수치로 증명 → 내 SSDG 연구의 필요성 지지
- 방법 논문이 아니므로 baseline 비교 대상이 아님. Introduction/Motivation 섹션에 인용.

---

## Novelty Gap 상태 (Run #8 기준)

| 키워드 | 상태 |
|--------|------|
| radius-conditioned augmentation | **없음 (유지)** |
| observability-conditioned augmentation | **없음 (유지)** |
| intra-class augmentation budget vessel | **없음 (유지)** |
| thickness-conditioned appearance transform | **없음 (유지)** |
| vessel radius → augmentation strength (continuous) | **없음 (유지)** |

→ **Continuous-ONA의 핵심 novelty gap은 Run #8 이후에도 완전히 유지됨.**

---

## 이번 실행에서 검토했으나 수록 보류한 논문

| 논문 | 이유 |
|------|------|
| VesselGPT (MICCAI 2025, arXiv 2505.13318) | 자기회귀 혈관 기하 생성 모델, DG와 무관 |
| GenEval (arXiv 2603.12369) | DR grading + fMRI 분류, segmentation DG와 무관 |
| MGC-net (Neurocomputing 2025) | Semi-supervised multi-source DG, secondary venue, SSDG 아님 |
| AD-DGCL (Neurocomputing 2025) | Semi-supervised, secondary venue, SSDG 아님 |
| DG-TTA (Sensors 2025) | 2023 arXiv, SSC+GIN 기존 방법 조합, 독자적 기여 낮음 |
| ConStyX arXiv 2506.10675 | 기존 CONSTYX (MICCAI 2025)의 arXiv 버전, 동일 논문 |

---

## 다음 실행 우선 탐색 목표

1. ICML 2026 proceedings (July 2026 예정) — DG/augmentation 관련 논문
2. ECCV 2026 papers (예정) — DG/tubular/vessel 관련 논문
3. WAVESDG 전문 독해: WISER 모듈의 고주파 sub-band 처리가 내 nonlinear aug와 어떻게 다른지 상세 분석
4. SDCL 전문 독해: backdoor adjustment 수식 → thin vessel causal narrative에 인용 방식 검토
5. "vessel observability conditioned augmentation" 키워드 지속 탐색
