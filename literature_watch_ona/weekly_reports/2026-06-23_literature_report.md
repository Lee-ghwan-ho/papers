# Literature Watch Report — Run #8
**날짜:** 2026-06-23  
**신규 논문:** 9편 (Published Journal 2편 + Accepted Conference 3편 + Workshop 1편 + Preprint 3편)  
**누적 수록:** 105편

---

## 이번 주 핵심 요약

Run #8의 가장 중요한 발견은 두 편의 IEEE 저널 논문이다. **TSIAA(IEEE TMI 2026)**는 내 방법과 동일한 Bézier 기반 augmentation을 instance-level adversarial 방식으로 확장한 논문으로 즉시 정독이 필요하다. **SDCL(IEEE TPAMI 2025)**는 causal SCM 기반 style deconfounding으로 SLAug를 능가하며, 내 방법의 차별점(causal style 제거 vs 공간적 augmentation budget 조절)을 명확히 서술하는 데 활용할 수 있다.

---

## 신규 논문 목록

### Category A — 직접 경쟁

#### ★★★ TSIAA (IEEE TMI 2026) — 즉시 읽기
**Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation**  
- Venue: IEEE Transactions on Medical Imaging, Vol.45, pp.764–776, 2026  
- DOI: 10.1109/TMI.2026.11146907  
- arXiv: 미확인

**핵심 아이디어:**
- Instance-Level Image Augmenter (IIAG): learnable constrained Bézier 변환 기반 IAM(Instance Augmentation Module) 모듈을 다단계로 스택
- Teacher-Student 구조: teacher가 augmentation 강도를 가이드, student가 augmented 이미지로 학습
- 기존 adversarial aug의 문제("simple structure, image-level, limited diversity")를 instance-level로 해결한다고 주장

**내 방법과의 관계:**
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 단위 | 이미지 전체 (per-instance) | 이미지 내 spatial location (intra-image) |
| Bézier | 이미지 전체에 동일하게 | vessel radius에 따라 spatial로 다르게 |
| Thin vessel 보호 | 없음 | 핵심 기능 |
| Adversarial training | 필요 | 불필요 |
| 설정 | SSDG | SSDG |

→ **ADA(MICCAI 2025) + TSIAA(TMI 2026) + ONA(내 방법)** 3-way 비교 테이블이 필요.

**주의:** TSIAA가 prostate / cardiac / fundus 실험에서 내 데이터셋과 겹치는지 확인 필요.

---

#### ★★★ SDCL (IEEE TPAMI 2025) — 즉시 읽기
**Causal Inference via Style Bias Deconfounding for Domain Generalization**  
- Venue: IEEE Transactions on Pattern Analysis and Machine Intelligence (Early Access 2025)  
- arXiv: 2503.16852

**핵심 아이디어:**
- Structural Causal Model: content ← domain ← style 인과 그래프 구성
- Backdoor adjustment: style을 confounding variable로 처리, do-calculus로 de-confound
- SGEM (Style-Guided Expert Module): 도메인 레이블 없이 style clustering → expert 할당
- BDCL (Backdoor Causal Learning Module): 여러 style group에서 balanced sampling

**내 방법과의 관계:**
- SDCL: style을 전체 이미지 단위에서 causal하게 제거 → domain-invariant content 학습
- ONA: appearance augmentation의 공간적 strength를 vessel radius 기반으로 차등 적용
- 접근법 완전히 다름 → 직접 novelty 충돌 없음
- 내 논문에서 SDCL을 "causal baseline"으로 인용 가능

---

#### WAVESDG (arXiv 2603.28463, April 2026) — Preprint
**Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation**  
- 저자: Shramana Dey, Varun Ajith, Abhirup Banerjee, Sushmita Mitra

**핵심 아이디어:**
- WaveSDG 네트워크: wavelet sub-band decomposition으로 encoder feature를 처리
- WISER (Wavelet-based Invariant Structure Extraction and Refinement): 각 sub-band의 semantic 역할 활용
  - Low-freq sub-band → anatomical structure (domain invariant)
  - High-freq sub-band → domain-specific appearance (augmentation target)
- Fundus optic disc / cup segmentation SSDG

**내 방법과의 관계:** frequency 분리 vs 내 spatial radius 조절 — 완전히 다른 mechanism. 비교 baseline 후보.

---

#### SDGAM (arXiv 2503.06288, March 2025) — Preprint
**Single Domain Generalization with Adversarial Memory**  
- 저자: Hao Yan, Marzi Heidari, Yuhong Guo

**핵심 아이디어:**
- Adversarial memory bank: training/testing feature를 invariant subspace로 투영
- Memory-based feature augmentation: diverse memory feature로 training feature 다양화
- General SDG benchmark 사용 (PACS, Office-Home 등)

**내 방법과의 관계:** feature-space DG (전체 이미지 단위). 의료영상 직접 실험 여부 미확인. 낮은 직접 관련성.

---

#### DomainFlow (STACOM 2024 Workshop) — Workshop Paper
**Single-Source Domain Generalization for Coronary Vessels Segmentation in X-Ray Angiography**  
- 저자: Atwany, M., Lashgari, M., Choudhury, R.P., Grau, V., Banerjee, A.
- Venue: STACOM 2024 (Statistical Atlases and Computational Models of the Heart), Springer 2025
- DOI: 10.1007/978-3-031-87756-8_1

**핵심 아이디어:**
- Connectivity mask prediction: binary segmentation 대신 vessel 연결성 mask 예측
- Connectivity mask는 domain-invariant spatial relationship을 포착한다고 주장
- Coronary vessel segmentation에서 domain shift 대응

**내 방법과의 관계:** vessel segmentation SSDG 직접 경쟁. 내 방법은 appearance augmentation, DomainFlow는 prediction target 변경. Workshop tier.

---

### Category C — 혈관 및 Tubular 특화

#### VASOMIM (AAAI 2026) — Accepted Conference
**VasoMIM: Vascular Anatomy-Aware Masked Image Modeling for Vessel Segmentation**  
- Venue: AAAI 2026  
- arXiv: 2508.10794

**핵심 아이디어:**
- Anatomy-guided masking: vessel-containing patch를 우선 mask → 혈관 representation 강화
- Anatomical consistency loss: original ↔ reconstructed 사이 vascular semantic 일관성 유지
- X-ray angiogram 3개 dataset SOTA

**내 방법과의 관계:** self-supervised pretraining 방향 (supervised SSDG인 내 방법과 보완적). VasoMIM으로 pretrain → ONA로 SSDG 학습 pipeline 가능성.

---

#### VASSELSDF (MICCAI 2025) — Accepted Conference
**VesselSDF: Distance Field Priors for Vascular Network Reconstruction**  
- Venue: MICCAI 2025 (Paper 2121)  
- arXiv: 2506.16556
- 저자: Salvatore Esposito 외, University of Edinburgh / UBC

**핵심 아이디어:**
- Binary label 대신 Signed Distance Field (SDF) 회귀
- Adaptive Gaussian regularizer: vessel 표면 근처 정밀, 멀수록 smooth
- Thin vessel connectivity 및 기하학 fidelity 향상 (floating segment 제거)

**내 방법과의 관계:**
- SDF output = 암묵적 vessel radius (SDF 값 = 표면까지 거리)
- ONA의 observability score를 SDF 기반으로 계산하는 pipeline 가능:  
  `radius_map = |SDF| → observability = f(radius_map)`
- 보완적 활용 가능성 높음

---

#### TOPOVSST (arXiv 2603.14909, March 2026) — Preprint
**TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking**  
- 저자: Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu (under review)

**핵심 아이디어:**
- Multi-scale sphere graph: 입력 이미지를 샘플링 + GNN으로 tracking direction + vessel radius 동시 추정
- Geometry-aware weighting: directional loss에 geometry 기반 가중치 → class imbalance 완화
- Wave-propagation skeleton tracking: space-occupancy filtering으로 spurious skeleton 제거

**내 방법과의 관계:** vessel radius 추정 방법 참고 가능. 내 ONA의 radius-based observability score 계산에 TopoVST 방식 통합 가능성.

---

#### FGOSNET (arXiv 2603.28503, March 2026) — Preprint
**Bridging the Geometry Mismatch: Frequency-Aware Anisotropic Serialization for Thin-Structure SSMs**  
- 저자: Jin Bai 외 6인 (under review)

**핵심 아이디어:**
- SSM의 isotropic raster scanning이 anisotropic thin structure에서 geometry mismatch 발생
- Frequency-Geometric Disentangling: stable topology carrier + directional high-freq band 분리
- Frequency-aligned scanning: direction-consistent trace 보존
- 91.3% mIoU, 97.1% clDice on DeepCrack (80 FPS, 7.87 GFLOPs)

**내 방법과의 관계:** thin structure의 anisotropic 특성을 주파수 관점에서 명시적으로 다룸. 내 ONA와 orthogonal하지만 thin structure 처리 방식 참고 가치.

---

## 전체 Novelty Gap 재확인 (Run #8)

| 키워드 | 상태 |
|--------|------|
| "vessel observability conditioned augmentation" | 여전히 없음 ✅ |
| "radius-conditioned augmentation budget" | 여전히 없음 ✅ |
| "intra-image augmentation strength by vessel radius" | 여전히 없음 ✅ |
| Per-sample Bézier adaptive aug (전체 이미지) | ADA(MICCAI25) + TSIAA(TMI26) 있음 → 구분 필요 |
| Thin vessel 보호 개념 (training-time SSDG) | L2CP(test-time) 있으나 training-time SSDG 없음 ✅ |
| Causal style removal for DG | SDCL(TPAMI25) 있음 → 내 aug budget 조절과 구분 명확 |

**핵심 주장 유지:** TSIAA/ADA는 모두 전체 이미지 단위 적응. 내 ONA만이 동일 이미지 내 spatial location별로 vessel radius에 따라 augmentation budget을 연속적으로 조절한다.

---

## 다음 우선 독해 목록 (P0 추가)

1. **TSIAA** — IEEE TMI 2026, ieeexplore doc 11146907: 실험 데이터셋 + IAM 구성 상세
2. **SDCL** — IEEE TPAMI 2025, arXiv 2503.16852: medical seg ablation + SGEM expert 수

---

## 다음 Run 탐색 예정 구역

- [ ] MICCAI 2026 papers.miccai.org 공개 후 DG/vessel 논문 탐색 (9월 예상)
- [ ] CVPR 2026 accepted list 공개 후 DG/augmentation 논문 탐색
- [ ] ICLR 2026 proceedings (April 2026 발표) 직접 탐색
- [ ] "connectivity mask" / "DomainFlow" 후속 citation 탐색
- [ ] VesselSDF + SDF-based radius computation pipeline 실현 가능성 검토
