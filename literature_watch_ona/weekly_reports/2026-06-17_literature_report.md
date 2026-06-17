# Literature Watch Report — Run #8

> 날짜: 2026-06-17  
> 모델: claude-sonnet-4-6  
> 신규 논문: **6편** (Published Journal 2편 + Accepted Conference 1편 + Preprint 3편)

---

## 요약

이번 Run에서는 5개 카테고리 × 5개 Lane의 체계적 탐색과 함께 June 2026 arXiv
신규 논문 탐색을 수행했다. ICML 2026 (Seoul, July 6-11) 및 ICLR 2026 published
proceedings를 탐색했으나, 이번 주제와 직접 관련된 논문은 찾기 어려웠다.

이번 Run의 핵심 발견:

1. **DG-TTA (Sensors 2025)**: 기존 Run에서 "재확인"으로만 처리되었던 GIN+TTA 논문을
   공식 인덱싱. GIN uniform augmentation baseline과의 비교 기준점으로 활용 가능.

2. **SMILEUHURA (ISBI 2023 Challenge / arXiv 2024)**: 7T TOF MRA 소혈관 분절 challenge
   결과 — 내 thin vessel 동기의 직접적 경험적 근거. 관찰 가능성이 MRI 해상도 (7T vs 3T)
   및 혈관 크기에 의존함을 실증.

3. **GENIE (ICML 2025)**: DG optimizer로 OSGR 균등화. 범용 DG optimizer로 기존
   SSDG methods와 통합 가능. 내 ONA와 orthogonal 관계 — 이론적 보완 가능성.

4. **URVSM (IEEE TIP 2025)**: 다중 모달리티 망막혈관 분절에서 image translation 기반
   modality-agnostic approach. 혈관 DG 방법론 다양성 reference.

5. **BREAKDATA (arXiv 2026)**: TopCoW + Lausanne OOD 평가가 포함된 few-shot 3D
   vessel foundation model. 데이터셋 연결점 확인.

6. **RETHINK_DG_MRI (arXiv 2025)**: MRI sequence heterogeneity에서의 DG 재해석.
   센터 shift보다 어려운 sequence shift 관점 제시.

---

## Category A — 신규 직접경쟁 논문

### DG-TTA — Sensors 2025

**논문**: DG-TTA: Out-of-Domain Medical Image Segmentation Through Augmentation,
Descriptor-Driven Domain Generalization, and Test-Time Adaptation  
**저자**: Christian Weihsbach, Christian N. Kruse, Alexander Bigalke, Mattias P. Heinrich  
**소속**: Institute of Medical Informatics, University of Lübeck  
**Venue**: Sensors, Vol. 25(17):5603, September 8, 2025  
**Status**: Published Journal Article  
**DOI**: 10.3390/s25175603  
**arXiv**: 2312.06275 (December 2023, updated)

#### 방법 요약

- **GIN intensity augmentation**: Generalized Intensity Normalization — nonlinear
  gamma/brightness/contrast 변환으로 input data manifold를 확대
- **SSC descriptor**: Self-Supervised Correspondence descriptor로 domain-invariant
  local feature 추출
- **Test-Time Adaptation**: 실제 inference 시 target domain data를 활용한 TTA
- 두 전략은 "orthogonal"로 설명: GIN = input manifold 확대, SSC = descriptor 공간 풍부화

#### 실험 결과
- CT → MRI abdominal: +46.2, +28.2 Dice point (p < 0.001)
- CT → MRI spine: +72.9 Dice point
- CT → MRI cardiac: +14.2, +55.7 Dice point
- 5개 공개 데이터셋, 3D CT + MRI

#### 내 방법과의 관계

**공통점**: GIN nonlinear augmentation을 사용 — 내 "Uniform nonlinear augmentation"
baseline과 직접 비교 가능  
**핵심 차이**:
- DG-TTA = uniform GIN aug (전체 이미지 단일 강도) + target-data TTA
- 나 = structure-conditioned ONA (혈관 두께별 다른 강도) + pure SSDG (target 불필요)
- DG-TTA는 test-time target access가 필수; 내 방법은 inference-time target 없이 작동

**Novelty 위협도**: Low — paradigm이 완전히 다름 (DG+TTA vs. SSDG). GIN baseline
comparison에서 DG-TTA 인용 가능.

---

### RETHINK_DG_MRI — arXiv 2507.23110

**논문**: Rethink Domain Generalization in Heterogeneous Sequence MRI Segmentation  
**Venue**: arXiv, July 30, 2025  
**Status**: Preprint Only  
**arXiv**: 2507.23110

#### 방법 요약

- MRI sequence variation (venous-to-out-of-phase)이 center shift보다 훨씬 심한 DG
  challenge라는 재해석
- 기존 DG methods와 대형 segmentation models (SAM, nnUNet 등)이 sequence shift에
  취약함을 실증
- Semi-supervised pretraining 방식이 현재 SOTA를 압도

#### 내 방법과의 관계

**공통점**: single-center training → multi-center/multi-sequence generalization 문제  
**핵심 차이**:
- RETHINK_DG_MRI = sequence shift (different MRI contrast mechanisms)
- 나 = multi-center appearance shift in TOF-MRA (same sequence, different scanners/protocols)

---

## Category C — 신규 혈관·구조 특화 논문

### SMILEUHURA — ISBI 2023 Challenge / arXiv 2411.09593 ⚠️ 동기 직접 지지

**논문**: SMILE-UHURA Challenge -- Small Vessel Segmentation at Mesoscopic Scale
from Ultra-High Resolution 7T Magnetic Resonance Angiograms  
**Venue**: ISBI 2023 challenge (Cartagena de Indias, Colombia); arXiv preprint Nov 2024  
**Status**: Preprint Only (challenge paper, journal review 예정)  
**arXiv**: 2411.09593

#### 방법 요약

- **배경**: 소혈관 병리 (Cerebral Small Vessel Disease)가 뇌혈액 공급의 핵심 취약점
- **데이터**: 7T TOF MRA — 표준 3T에서 보이지 않는 소혈관을 가시화
- **Challenge**: ISBI 2023에서 진행, 16개 제출 방법 + 2 baseline 비교
- **성과**: Dice up to **0.838 ±0.066** (held-out test), 0.716 ±0.125 (separate 7T dataset)
- **데이터셋**: 공개 annotated 7T ToF MRA dataset

#### 내 연구에의 활용 (High Priority) ⭐

이 논문은 내 Continuous-ONA의 **핵심 동기를 실증적으로 지지**한다:

1. **Observability는 resolution-dependent**: 3T에서 보이지 않는 소혈관이 7T에서 가시화
   → 같은 "혈관 foreground" 내에서도 **관찰 가능성이 구조 크기에 따라 극도로 다름**
2. **Thin vessel segmentation difficulty는 실재한다**: Dice 0.716–0.838은 standard 3T
   대혈관 분절 (≥0.90)에 비해 훨씬 낮음 → thin/small vessel의 관찰 가능성 취약성 실증
3. **7T vs. 3T**: "MRI 해상도 × 혈관 두께"가 observability를 결정하는 요인
   → 내가 주장하는 "local vessel radius ∝ observability" 가설과 일치

**Related Work / Introduction 서술 예시**:
> "Even at 7T MRI, the segmentation of small cerebral vessels remains challenging
> (SMILE-UHURA, Dice 0.716–0.838), compared to large vessel segmentation (>0.90),
> suggesting that observability varies dramatically within the vascular foreground
> depending on vessel caliber."

---

### URVSM — IEEE Transactions on Image Processing 2025

**논문**: Universal Vessel Segmentation for Multi-Modality Retinal Images  
**저자**: Bo Wen, Anna Heinke, Akshay Agnihotri, Dirk-Uwe Bartsch, William Freeman,
Truong Nguyen, Cheolhong An  
**소속**: University of California, San Diego  
**Venue**: IEEE Transactions on Image Processing, published 2025  
**Status**: Published Journal Article  
**arXiv**: 2502.06987

#### 방법 요약

- 다중 모달리티 (Color Fundus, Multi-Color SLO, SLO-Red, SLO-Blue, CSLO, FA, FAF)
  → 단일 모델로 분절
- **핵심 방법**: image translation으로 arbitrary modality를 Topcon CF 스타일로 변환 후 통일
- Topology-aware feature 통합
- 최초의 modality-agnostic 망막혈관 분절 모델

#### 내 방법과의 관계

다른 paradigm (modality translation-based, 나는 augmentation-based SSDG)이나 IEEE TIP
published vessel DG 논문으로 관련 연구 coverage에 포함.

---

### BREAKDATA — arXiv 2602.23782

**논문**: Breaking the Data Barrier: Robust Few-Shot 3D Vessel Segmentation using
Foundation Models  
**저자**: Kirato Yoshihara, Yohei Sugawara, Yuta Tokuoka, Lihang Hong  
**소속**: University of Osaka + Preferred Networks, Inc.  
**Venue**: arXiv preprint, February 27, 2026  
**Status**: Preprint Only  
**arXiv**: 2602.23782

#### 방법 요약

- **Foundation model adaptation**: DINOv3 (2D pretrained) → 3D 혈관 분절
- **3D Adapter**: volumetric consistency
- **Multi-scale 3D Aggregator**: hierarchical feature fusion
- **Z-channel embedding**: 2D pretrain → 3D 적용 gap 해소
- 5-shot Dice 43.42% (+30% 상대 개선 over nnU-Net 33.41%)

#### 실험

- **In-domain**: TopCoW (ToF MRA 기반 Circle of Willis 분절)
- **Out-of-distribution**: Lausanne dataset
- TOF-MRA 기반 few-shot 성능 — 내 COSTA/TopCoW 실험 설계와 연결점 있음

---

## Category D — 신규 Top-tier Vision 논문

### GENIE — ICML 2025

**논문**: One-Step Generalization Ratio Guided Optimization for Domain Generalization  
**저자**: Sumin Cho, Dongwon Kim, Kwangsu Kim  
**Venue**: ICML 2025 (poster, icml.cc/virtual/2025/poster/45152)  
**Status**: Accepted Conference Paper  
**arXiv**: 2606.16301 (June 15, 2026)

#### 방법 요약

- **OSGR (One-Step Generalization Ratio)**: 각 파라미터가 (1) loss 감소에 얼마나
  기여하는지 + (2) gradient alignment가 얼마나 좋은지를 동시에 측정하는 지표
- **GENIE optimizer**: OSGR을 preconditioning factor로 사용하여 일부 파라미터가
  전체 optimization을 지배(spurious correlation)하는 것을 방지
- **이론적 보장**: SGD 수렴 속도 유지 + OSGR 균등화 동시 달성
- **범용성**: 기존 DG/single-DG 방법과 통합 시 추가 성능 향상

#### 내 방법과의 관계

- GENIE는 optimizer 수준의 DG 방법 — 내 augmentation strategy와 **orthogonal**
- 내 ONA augmentation + GENIE optimizer를 동시에 사용하면 추가 이득 가능
- DG training의 이론적 관점: "파라미터 단위 domain invariance"라는 분석 방법
- 관련 Work에서 "optimization-level DG" 방법으로 언급 가능

---

## Novelty Gap 재확인 — Run #8

이번 Run에서도 다음 개념을 직접 명시한 논문은 발견되지 않았다:

| 개념 | 상태 |
|------|------|
| vessel observability conditioned augmentation | ❌ 없음 |
| radius-conditioned augmentation budget | ❌ 없음 |
| intra-class augmentation strength differential | ❌ 없음 |
| thin vessel appearance protection during SSDG augmentation | ❌ 없음 |
| continuous morphology-conditioned nonlinear appearance transform | ❌ 없음 |

**Continuous-ONA의 핵심 novelty gap은 Run #8에서도 유지된다.**

---

## 다음 Run 우선 탐색 항목

- [ ] ICML 2026 (Seoul, July 6-11, 2026) published papers 중 DG/augmentation → 7월 이후
- [ ] MICCAI 2026 accepted papers (예상 7월 발표) — TOF-MRA, vessel DG, SSDG 관련 확인
- [ ] SMILEUHURA 전문 독해: 7T MRA 소혈관 challenge 상위 방법들의 특징 파악
  → 내 thin vessel 동기 논거로 활용할 구체적 수치 확인
- [ ] GENIE OpenReview 전문 독해: OSGR 수학적 정의 + 실험 setup (의료영상 포함 여부)
- [ ] DG-TTA 전문 독해: SSC descriptor가 GIN aug와 결합하는 방식 상세 확인
- [ ] 새로운 키워드 탐색: "partial volume effect augmentation" / "resolution-dependent observability"
  / "mesoscopic vessel segmentation domain shift"
