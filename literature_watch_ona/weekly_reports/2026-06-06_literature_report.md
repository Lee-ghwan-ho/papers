# Literature Watch Report — Run #8
**날짜:** 2026-06-06  
**모델:** claude-sonnet-4-6  
**신규 논문:** 8편 (Accepted Conference 2편 + Published Journal 2편 + Preprint 4편)  
**누적 총계:** 104편  

---

## 1. 핵심 요약

Run #8에서 총 **8편**의 신규 논문을 발굴했다. 최고 우선순위 novelty 경쟁 논문은 **UniFreqSDG (ACM MM 2024)**로, per-image frequency perturbation strength를 학습 가능한 파라미터로 두는 아이디어가 나의 Continuous-ONA와 부분적으로 겹친다. 그러나 UniFreqSDG는 전체 이미지 단위의 frequency-level 조절이고, Continuous-ONA는 intra-image vessel structure 수준의 pixel-level augmentation budget 조절이라는 결정적 차이가 확인된다.

**핵심 novelty gap 재확인**: "radius-conditioned augmentation", "thickness-conditioned augmentation", "observability-conditioned augmentation" 키워드 모두 직접 매칭 논문 없음. **Continuous-ONA의 핵심 claim은 8회 연속 탐색에서 선행 연구 없음으로 확인됨.**

---

## 2. 신규 논문 목록

### Category A — SSDG / Medical Image Segmentation DG

#### ✅ UNIFREQSDG — ACM Multimedia 2024 (Accepted Conference)
**제목:** Universal Frequency Domain Perturbation for Single-Source Domain Generalization  
**DOI:** 10.1145/3664647.3681536  
**Relevance: High**

**방법 요약:**
- LSP (Learnable Spectral Perturbation): LF radius를 학습 가능한 파라미터로 두어 단일 source의 주파수 분포를 적응적으로 확장
- CPR (Content-Preserving Recombination): 증강 전후 feature를 decouple + recombine하여 content 정보 보존
- ADI (Active Domain-variance Inducement) loss: 주파수 공간에서 domain-style feature 분리 강화
- Fundus: +7.47% Dice, Prostate: +4.99% Dice (vs. SOTA)

**내 방법과의 관계:**
- 공통점: augmentation strength를 static하게 고정하지 않고 "조절 가능하게" 만든다는 방향
- **결정적 차이**: UniFreqSDG = per-image(전체 이미지 단위) frequency domain 조절. 나 = intra-image(같은 이미지 내 vessel structure별) spatial/appearance domain 조절.
- UniFreqSDG는 모든 픽셀에 동일한 frequency perturbation 적용 → thin vessel 보호 개념 전무.
- **논문 내 novelty 구분 문장 초안**: "While UniFreqSDG learns a per-image frequency perturbation radius to expand the source distribution, our method conditions augmentation strength on the local observability of each vascular segment within the image, providing intra-image differential treatment that is absent in prior frequency-based approaches."

#### ✅ AEGIS — Pattern Recognition 2025 (Published Journal)
**제목:** Aegis: A Domain Generalization Framework for Medical Image Segmentation by Mitigating Feature Misalignment  
**ScienceDirect:** pii/S0031320325010672 (Sept 2025)  
**Relevance: Medium**

**방법 요약:**
- Style augmentation으로 domain shift를 시뮬레이션
- DAFC (Dual Attention-guided Feature Calibration): source-augmented feature pair 간 implicit alignment constraint
- UFA (Uncertainty-guided Feature Alignment) loss: hard-to-classify pixel 집중

**내 방법과의 관계:**
- image-level uniform style aug 후 feature-level alignment → 구조별 차별 없음
- 내 방법과 추상적 방향은 비슷하나 level이 전혀 다름 (image-level vs. intra-image structure-level)

#### ✅ ADDGCL — Neurocomputing 2025 (Published Journal)
**제목:** Multi-organ Medical Image Segmentation via Adaptive Disentangled Domain Generalization Collaborative Learning  
**ScienceDirect:** pii/S0925231225025184 (Oct 2025)  
**Relevance: Medium**

**방법 요약:**
- SSRD: dual encoder + cross-domain contrastive learning으로 domain-style / anatomical-content 분리
- SCT: synthetic style perturbation + consistency regularization
- Adaptive region-specific loss: pixel frequency 기반으로 small organ 집중

**내 방법과의 관계:**
- "region-specific loss weighting"이라는 개념 공유. 단 weighting 기준이 pixel frequency (빈도 기반) vs. 내 방법은 local vessel radius (구조 관찰가능성 기반)
- semi-supervised + multi-source DG 설정 vs. 내 SSDG 설정: 겹치지 않음

---

### Category D — Top-tier Vision

#### ✅ DEPTHFORGE — ICCV 2025 (Accepted Conference)
**제목:** Stronger, Steadier & Superior: Geometric Consistency in Depth VFM Forges Domain Generalized Semantic Segmentation  
**arXiv:** 2504.12753 | **GitHub:** SY-Ch/DepthForge  
**Relevance: Low**

**방법 요약:**
- DINOv2/EVA02 (visual) + Depth Anything V2 (depth) 통합
- "visual cues are susceptible to domain shift; geometry (depth) remains stable"
- Depth-awareness learnable tokens으로 geometry-consistent feature 학습
- Urban scene DGSS, 자연영상 전용

**내 방법에 적용 가능한 아이디어:**
- "geometry > appearance in robustness" 논리는 내 "vessel structure (observability) > domain-specific intensity" 주장과 구조적으로 유사
- 내 관찰가능성 score (Hessian / local radius)를 "geometry anchor"로 포지셔닝하는 narrative 강화에 활용 가능

---

### Preprint-Only 후보 논문

#### ⭐ XAICEV — arXiv:2512.13977 (Dec 2025)
**제목:** XAI-Driven Diagnosis of Generalization Failure in State-Space Cerebrovascular Segmentation Models  
**Relevance: Medium**

- UMamba 기반 cerebrovascular segmentation의 도메인 이전 실패를 XAI로 진단
- RSNA CTA Aneurysm → TopCoW Circle of Willis CT: Dice **0.8604 → 0.2902** (catastrophic failure)
- 원인: Z-resolution 차이 + background noise 분포 차이
- **내 연구의 motivation 지지**: cerebrovascular domain shift는 trivial하지 않음을 실증

#### BREAKVESSEL — arXiv:2602.23782 (Feb 2026)
**제목:** Breaking the Data Barrier: Robust Few-Shot 3D Vessel Segmentation using Foundation Models  
**Relevance: Medium**

- DINOv3 + lightweight 3D Adapter + multi-scale 3D Aggregator
- TopCoW(in-domain) + Lausanne(OOD), 5-shot Dice 43.42% (+30% relative)
- Few-shot paradigm (not SSDG) — 병행 참고용

#### UNIVG — arXiv:2604.10737 (April 2026)
**제목:** Generative Data-engine Foundation Model for Universal Few-shot 2D Vascular Image Segmentation  
**Relevance: Medium**

- 혈관 이미지 compositionality 학습, generative foundation model for vascular seg
- 2D vascular 전반 (retinal, coronary, cerebral)
- Few-shot paradigm — foundation model 접근법 현황 파악용

#### WAVESDG — arXiv:2603.28463 (March 2026)
**제목:** Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**Relevance: Medium**

- WISER module: LL (global anatomy) / LH+HL (edge) / HH (noise) 분리
- LL sub-band로 global structural context 고정, HH sub-band 억제
- Fundus (optic disc+cup), 1 source → 5 unseen targets
- **내 방법과의 차이**: WaveSDG = feature-level wavelet decomposition, 나 = pixel-level structure-conditioned augmentation. WaveSDG는 vessel thickness를 다루지 않음.

---

## 3. Novelty Gap 종합 분석 (Run #8 기준)

| 핵심 novelty 구성 요소 | 선행 연구 | 상태 |
|----------------------|-----------|------|
| intra-class vessel radius → augmentation budget | 없음 | ✅ 유지 |
| observability-conditioned augmentation | 없음 | ✅ 유지 |
| continuous (vs. binary) structural conditioning | 없음 | ✅ 유지 |
| thin vessel 보호 + thick vessel 강화 동시 | 없음 | ✅ 유지 |
| 가장 가까운 경쟁: per-image aug strength 조절 | ADA(MICCAI 2025), UniFreqSDG(ACM MM 2024) | 차이 명확 |
| 가장 가까운 경쟁: radius를 loss weighting에 활용 | AG-TAL(arXiv 2026) | 완전히 다른 mechanism |

---

## 4. 다음 실행 우선 탐색 방향

1. **UniFreqSDG 전문 독해**: LSP의 LF radius 학습 방식 — inner radius vs. outer radius 정의, 내 continuous radius와의 차이 논증 강화
2. **ICLR 2026 openreview.net 직접 탐색**: 현재 available papers 중 medical DG/augmentation 관련 확인
3. **arXiv 2606.xxxxx 탐색**: June 2026 첫 2주 논문 공개 후 재탐색 (목표: Run #9)
4. **DG-TTA (Sensors 2025)**: GIN + SSC descriptor 조합 — 낮은 tier 저널이나 GIN baseline 구현 참고
5. **IELDG (arXiv:2508.19604)**: Inverse Evolution Layers for DGSS 구조 확인 — 자연영상이지만 noise-structure 분리 관점 참고 가능
