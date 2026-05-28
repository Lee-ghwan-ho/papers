# Literature Watch Report — Run #2
**날짜**: 2026-05-28  
**검색 범위**: 2025–2026 우선, 2024 보조  
**신규 수록 논문**: 13편  
**누적 총 논문**: 57편

---

## 이번 실행 요약

Run #2에서 가장 중요한 발견은 **ADA (MICCAI 2025)**다.  
"Content feature에 따라 Bezier augmentation 파라미터를 per-sample adaptive하게 조절한다"는 아이디어로,
내 Continuous-ONA의 핵심 아이디어("augmentation 강도를 구조 특성에 따라 다르게")와 표면적으로 유사하다.

그러나 ADA는 **image-level global adaptive aug** (전체 이미지 하나의 Bezier param)이고,  
내 방법은 **structure-level intra-image local adaptive aug** (같은 이미지 내 혈관마다 다른 budget)이다.  
이 granularity의 차이와 "thin vessel label-image consistency" 문제의식이 핵심 차별점이다.

또한 **MBFCV (MICCAI 2025)**가 혈관 두께를 Multi-Branch Feature Extractor로 명시적으로 모델링했으나,
이는 few-shot inference-time 설정이고 내 방법(SSDG + training-time augmentation conditioning)과 접근이 전혀 다르다.

---

## 신규 논문 목록 (13편)

### Accepted Conference Papers / Published Journal

| KEY | 제목 | Venue | Cat | Rel | 핵심 메모 |
|-----|------|-------|-----|-----|-----------|
| **ADA** | ADA: Adaptive Aug Framework for SSDG in Medical Image Seg | MICCAI 2025 | A | **High** | Learnable Bezier Remap per-sample. **직접 경쟁 — 즉시 독해 필요** |
| **MBFCV** | Multi-Branch Framework for Cross-Domain Vessel Seg via Few-Shot | MICCAI 2025 | A/C | High | MBFE로 혈관 두께 구분. Few-shot 설정. |
| **GLCP** | GLCP: Global-to-Local Connectivity Preservation for Tubular Seg | MICCAI 2025 | C | Medium | IMS + DAR 모듈. Local discontinuity 탐지. |
| **CF_SEG** | CF-Seg: Counterfactuals Meet Segmentation | MICCAI 2025 | B | Low | Disease-free counterfactual generation → segmentation |
| **SPATIAL_TOPO** | Topology-Preserving Seg with Spatial-Aware Persistent Feature Matching | ICCV 2025 Workshop | C | Medium | Spatial-Aware Topological Loss. Tubular structure. |
| **FDGP** | Frequency-Based Federated Domain Generalization for Polyp Seg | ICASSP 2025 | B | Low | Fourier thresholding in federated setting |
| **GFSA** | Generative Feature Style Augmentation for DG in Medical Image Seg | Pattern Recognition 2025 | A | Medium | VAE 기반 feature style generation. Prostate/spinal MRI. |

### Preprint Only (별도 후보)

| KEY | 제목 | arXiv | Cat | Rel | 핵심 메모 |
|-----|------|-------|-----|-----|-----------|
| **CRISP** | CRISP: Rank-Guided Iterative Squeezing for Robust Medical Seg | 2604.05409 (2026) | A | Medium | Rank stability of positive regions. Parameter-free. Lung vessel CT 포함. |
| **FMS2** | FMS²: Unified Flow Matching for Segmentation and Synthesis | 2603.13659 (2026) | C | Low | Flow matching for thin structures. Cross-domain generalization. |
| **SPOCKMIP** | SPOCKMIP: Vessel MRA Segmentation with MIP Loss | 2407.08655 (2024) | C | Medium | TOF-MRA 특화. MIP projection as training loss. 연속성 향상. |
| **FPGM** | Frequency Prior Guided Matching: Aug for Generalizable Polyp Seg | 2508.06517 (2025) | B | Low | Learned edge frequency prior → spectral perturbation |
| **BOUNDLESS** | Boundless Across Domains: Adaptive Feature Blending + DCAR | 2411.14883 (2024) | A | Low | AFB + Dual Cross-Attention Regularization |
| **DRIPS** | DRIPS: Domain Randomisation for Perivascular Spaces Seg | medRxiv 2025 | C | Low | Physics-inspired synthetic data for PVS seg DG |

---

## Novelty Check 요약

### Continuous-ONA의 핵심 Gap — Run #2 이후도 유지됨

1. **Intra-class continuous conditioning**: ADA도 per-sample이지, intra-image per-structure가 아님.  
   MBFCV도 두께를 구분하지만 feature representation 수준이고 augmentation budget이 아님.  
   → **단일 foreground class 내에서 구조별로 연속적으로 aug budget을 다르게 배분하는 방법은 여전히 없다.**

2. **Label-image consistency for fragile structures**: ADA, MBFCV 모두 thin vessel의 label-image inconsistency를 문제로 정의하지 않음.  
   → **내 방법의 핵심 문제의식은 여전히 고유함.**

3. **Source annotation 기반 observability**: ADA는 learned content feature (implicit), 내 방법은 local radius/vesselness (explicit, interpretable).

### 위험도 업데이트

| 논문 | 위험도 | 설명 |
|------|--------|------|
| ADA | 🔴 중위험 | Bezier adaptive aug 아이디어 공유. Granularity 차이로 방어 가능. |
| MBFCV | 🟡 저위험 | 혈관 두께 모델링 공유. 설정(few-shot vs SSDG)이 완전히 다름. |
| SRCSM | 🔴 중위험 | (기존) Semantic-aware class별 RC. 같은 class 내 차이와 명확히 구분 필요. |
| FIESTA | 🔴 중위험 | (기존) Uncertainty-guided aug strength. Model-centric vs data-centric 차이. |

---

## ADA와의 직접 비교 분석

| 비교 항목 | ADA (MICCAI 2025) | Continuous-ONA |
|-----------|-------------------|----------------|
| **적응 단위** | Image-level (per-sample) | **Structure-level (per-vessel-segment)** |
| **조건 신호** | Content feature (learned, global) | **Local observability (annotation-derived, local)** |
| **Aug budget 분포** | 이미지마다 하나의 global budget | **같은 이미지 내 연속적으로 다른 budget** |
| **얇은 구조 보호** | ❌ | ✅ |
| **Label-image consistency** | ❌ | ✅ |
| **Bezier parameterization** | Learnable (gradient-driven) | Observability-conditioned (geometry-driven) |

→ **같은 Bezier aug 계열이지만 전혀 다른 conditioning 방식과 목적.**  
→ ADA의 존재는 "Bezier aug는 이미 MICCAI 2025에 있다"는 사실을 보여주지만,  
   "구조별 local conditioning"과 "thin vessel protection"은 ADA에 없다.

---

## 다음 실행 (Run #3)에서 추가 탐색할 항목

- [ ] ADA full text (Paper 0315) 직접 확인 — LBR이 global인지 local인지 확인 필수
- [ ] UniDDG (2501.04741) — "One image as one domain" 아이디어
- [ ] Anatomy-Guided Texture Augmentation for Cervical Tumor (MICCAI 2024)
- [ ] Causality-inspired Latent Feature Aug (arXiv 2406.05980)
- [ ] ICCV 2025 full accepted list 추가 탐색
- [ ] NeurIPS 2025 proceedings 추가 탐색
- [ ] SLAug/SRCSM/ADA를 모두 비교하는 systematic review 존재 여부
