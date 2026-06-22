# Literature Watch Report — Run #8

> 날짜: 2026-06-22  
> 모델: claude-sonnet-4-6  
> 신규 논문: **6편** (Accepted Conference 1편 + Published Journal 1편 + Preprint 4편)

---

## 요약

이번 Run에서는 6편의 신규 논문을 발견했다.

1. **SSDG 직접 경쟁 논문 2편**:
   - WaveSDG (arXiv:2603.28463, April 2026) — wavelet sub-band 기반 SSDG for fundus
   - DG-TTA (Sensors 2025) — GIN aug + SSC descriptor + TTA 조합

2. **혈관 특화 신규 논문 3편** (모두 vessel foundation model 계열):
   - VasoMIM (AAAI 2026) — anatomy-guided masked image modeling for X-ray angiogram
   - BreakDataBarrier (arXiv:2602.23782, Feb 2026) — few-shot 3D vessel with DINOv3
   - UniVG (arXiv:2604.10737, April 2026) — universal few-shot 2D vascular generation

3. **방법론 유사 논문 1편**:
   - MorphGen (arXiv:2509.00311, Sept 2025) — morphology-guided SSDG for histopathology

핵심 발견: WaveSDG가 SSDG에서 wavelet sub-band 분리를 통한 structure-aware 방법을 제시했으나, 내 Continuous-ONA의 핵심인 **intra-vessel radius별 augmentation budget**은 여전히 직접 다룬 논문이 없다.

---

## Category A — 신규 직접경쟁 논문

### WaveSDG — arXiv:2603.28463 (April 2026) ⚠️ 직접 경쟁

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**저자**: Shramana Dey, Varun Ajith, Abhirup Banerjee, Sushmita Mitra  
**Venue**: arXiv:2603.28463, April 27, 2026  
**Status**: Preprint Only  

#### 방법 요약

- **핵심 아이디어**: wavelet sub-band decomposition을 통한 anatomical structure ↔ domain-specific appearance 분리
- **WISER (Wavelet-based Invariant Structure Extraction and Refinement) 모듈**:
  - Encoder feature를 wavelet sub-band로 분해
  - Low-frequency sub-band → 전역 해부 구조 anchor (content 보존)
  - High-frequency sub-band → 방향성 edge 선택적 강화 + noise 억제 (style 분리)
  - Decoder fusion 전에 삽입: U-Net backbone에 plug-in 방식
- **실험**:
  - 1 source domain + 5 unseen target domains
  - Task: optic disc & cup segmentation (fundus images)
  - 7개 SOTA 대비 Dice + HD95 양면에서 일관된 성능 향상

#### 내 방법과의 관계

**공통점**:
- SSDG 설정에서 구조를 보존하면서 domain-specific appearance를 변환
- Structure-aware 원칙 적용

**핵심 차이**:
- WaveSDG = **frequency-domain 분리** (전체 feature map에 동일하게 적용)
- 나 = **pixel-level intra-image vessel radius별 augmentation budget** (같은 이미지 내 얇은/두꺼운 혈관이 서로 다른 강도)
- WaveSDG에는 intra-class vessel observability 차별화 개념 완전 부재
- 적용 도메인: WaveSDG = fundus optic disc/cup, 나 = TOF-MRA cerebrovascular SSDG

**Novelty 위협도**: Low-Medium — SSDG 직접 경쟁이나 방법 paradigm이 다름. Related Work에서 "wavelet-based structure-style separation" 계열로 분류 가능.

---

### DG-TTA — Sensors 2025 (Published Journal)

**논문**: DG-TTA: Out-of-Domain Medical Image Segmentation Through Augmentation, Descriptor-Driven Domain Generalization, and Test-Time Adaptation  
**저자**: Christian Weihsbach, Christian N. Kruse, Alexander Bigalke, Mattias P. Heinrich (University of Lübeck)  
**Venue**: Sensors, Vol. 25(17), Article 5603, September 8, 2025  
**Status**: Published Journal Article  
**arXiv**: 2312.06275 (original Dec 2023)  
**DOI**: 10.3390/s25175603

#### 방법 요약

- **두 단계 구성**:
  1. Domain-generalized pre-training: SSC descriptor + GIN intensity augmentation 조합
  2. Test-time adaptation: augmentation-descriptor 조합을 이용한 consistency scheme
- **SSC (Shape Space Correspondence) descriptor**: cross-domain invariant structural representation
- **GIN (Gray Level Image Normalization)**: input-space nonlinear intensity augmentation
- **실험**: 5개 CT + MRI 데이터셋, abdominal (+46% Dice), spine (+73%), cardiac (+14%) 개선

#### 내 방법과의 관계

- **참고 가치**: GIN aug의 효과를 Published Journal에서 체계적으로 입증 → 내 uniform nonlinear aug baseline 구성 근거
- **핵심 차이**: DG-TTA = TTA (test-time target 데이터 필요), 나 = pure SSDG (source-only)
- **직접 충돌 없음**

---

## Category C — 신규 혈관·구조 특화 논문

### VasoMIM — AAAI 2026

**논문**: Vascular Anatomy-Aware Masked Image Modeling for Vessel Segmentation  
**저자**: De-Xing Huang et al.  
**Venue**: AAAI 2026 (preliminary); 확장 버전 arXiv:2602.11536 (XA-170K 포함)  
**Status**: Accepted Conference Paper  
**arXiv**: 2508.10794 (Aug 2025)

#### 방법 요약

- **핵심 문제**: 기존 MIM은 vessel-background 클래스 불균형으로 혈관 representation 학습 실패
- **두 가지 핵심 설계**:
  1. **Anatomy-guided masking strategy**: vessel patch를 우선 마스킹 → 모델이 혈관 의미를 강제 학습
  2. **Anatomical consistency loss**: 원본 이미지 ↔ 재구성 이미지 간 혈관 구조 일관성 강화
- X-ray angiogram 분석에 특화; 3개 downstream 데이터셋 SOTA
- 확장 버전에서 XA-170K (최대 X-ray angiogram pre-training dataset) 공개

#### 내 방법과의 관계

- **관련성**: vessel-specific design에서 anatomy-guided 원칙 공유
- **차이**: VasoMIM = X-ray angiogram self-supervised pre-training (DG 아님), 나 = TOF-MRA SSDG training-time augmentation
- **활용 가능성**: 내 연구의 "vessel-aware training"의 최근 동향으로 Related Work 언급 가능

---

### BreakDataBarrier — arXiv:2602.23782 (Feb 2026)

**논문**: Breaking the Data Barrier: Robust Few-Shot 3D Vessel Segmentation using Foundation Models  
**Venue**: arXiv:2602.23782, February 27, 2026  
**Status**: Preprint Only  

#### 방법 요약

- DINOv3 기반 Vision Foundation Model + 3D Adapter (volumetric consistency) + multi-scale 3D Aggregator (hierarchical fusion) + Z-channel embedding (2D→3D gap 해소)
- **실험 데이터셋**:
  - TopCoW (in-domain, CoW TOF-MRA + CTA)
  - Lausanne (out-of-distribution)
- **결과**: 5개 training sample으로 OOD 설정 nnUNet 대비 +50% relative improvement (Dice)

#### 내 방법과의 관계

- **관련 데이터셋**: TopCoW + Lausanne = 내 연구 TOF-MRA 도메인과 직접 인접
- **차이**: few-shot foundation model vs. SSDG augmentation. Paradigm 완전히 다름.
- **시사점**: foundation model 방식으로 이미 OOD 문제에 어느 정도 대응 → 내 lightweight SSDG aug 방법의 차별화 포인트(annotation efficiency + no target data required) 재확인

---

### UniVG — arXiv:2604.10737 (April 2026)

**논문**: Generative Data-engine Foundation Model for Universal Few-shot 2D Vascular Image Segmentation  
**저자**: Rongjun Ge et al. (Southeast University, University of Macau)  
**Venue**: arXiv:2604.10737, April 12, 2026  
**Status**: Preprint Only  

#### 방법 요약

- 혈관 이미지의 compositionality를 학습하는 generative foundation model
- Universal few-shot 2D vascular segmentation 목표

#### 내 방법과의 관계

- Few-shot foundation model 방식 — 내 SSDG augmentation과 paradigm 다름
- 혈관 분할의 generative foundation model 계열의 최신 사례로 참고

---

## Category B — 신규 방법론 유사 논문

### MorphGen — arXiv:2509.00311 (Sept 2025)

**논문**: MorphGen: Morphology-Guided Representation Learning for Robust Single-Domain Generalization in Histopathological Cancer Classification  
**저자**: Syed Farhan Alam Zaidi et al. (Chung-Ang University)  
**Venue**: arXiv:2509.00311, September 2025  
**Status**: Preprint Only  

#### 방법 요약

- **핵심 동기**: pathologist가 domain-invariant morphological cue(핵 비대, 불규칙 윤곽, chromatin 질감, 공간 구조)에 의존 → 이를 명시적으로 모델링
- **방법**:
  - 조직병리 이미지 + augmentation + nuclear segmentation mask를 supervised contrastive learning에 통합
  - 이미지 ↔ 핵 마스크의 latent representation 정렬: staining artifact보다 진단적 morphological feature에 집중
  - SWA (Stochastic Weight Averaging)로 OOD robustness 강화
- 단, 분류(classification) 태스크에 적용, segmentation 아님

#### 내 방법과의 관계

- **공통 방향**: morphological prior를 domain-invariant 학습에 명시적으로 통합
- **핵심 차이**:
  - MorphGen = cell-level nuclear morphology (히스토패스), 나 = vessel-level radius/observability (MRI/TOF-MRA)
  - MorphGen = 분류 태스크, 나 = segmentation
  - MorphGen에는 intra-class 내 연속적 augmentation budget 개념 없음
- **참고 가치**: "domain-invariant morphological feature learning"의 최신 SSDG 사례로 Related Work 언급 가능

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class structure-specific augmentation strength"
- "thin vessel appearance protection during augmentation"
- "continuous observability-conditioned nonlinear augmentation"

**WaveSDG가 "structure-aware SSDG"의 방향을 공유하지만, 내 핵심 claim("같은 class 내 얇은 혈관과 두꺼운 혈관은 다른 augmentation budget을 받아야 한다")은 어떤 논문에서도 직접 다루지 않는다.**

---

## 신규 논문 총괄

| KEY | 제목 (요약) | Venue | Status | Cat | Rel |
|-----|------------|-------|--------|-----|-----|
| WAVESDG | Decoupling Wavelet Sub-bands for SSDG in Fundus (WaveSDG) | arXiv 2026 | Preprint | A | High |
| DGTAA | DG-TTA: Out-of-Domain Medical Seg via Aug+Descriptor+TTA | Sensors 2025 | Journal | A | Medium |
| VASOMIM | VasoMIM: Vascular Anatomy-Aware MIM for Vessel Seg | AAAI 2026 | Conference | C | Medium |
| BREAKDATABARRIER | Breaking Data Barrier: Few-Shot 3D Vessel Seg (Foundation) | arXiv 2026 | Preprint | C | Medium |
| UNIVG | UniVG: Generative Foundation for Few-shot 2D Vascular Seg | arXiv 2026 | Preprint | C | Medium |
| MORPHGEN | MorphGen: Morphology-Guided SSDG for Histopathology | arXiv 2025 | Preprint | B | Low-Med |

---

## 다음 Run 우선 탐색 항목

- [ ] WaveSDG full text: WISER module 상세, sub-band별 역할 공식 → 내 augmentation design과 비교
- [ ] VasoMIM AAAI 2026 full paper: anatomy-guided masking threshold + consistency loss 공식
- [ ] BreakDataBarrier 전문: OOD Lausanne 실험 상세, 내 TOF-MRA 데이터와 데이터셋 overlap 확인
- [ ] ICLR 2026 openreview.net 직접 탐색: DG/augmentation/medical accepted paper 탐색
- [ ] CVPR 2026 accepted paper 목록 공개 시 즉시 탐색
- [ ] Topo-R1 (arXiv:2603.13054) "Detecting Topological Anomalies via VLMs" — Cat C 추가 여부 검토
- [ ] AG-TAL 전문 독해 (priority from Run #7): GT radius 계산 방식 상세
