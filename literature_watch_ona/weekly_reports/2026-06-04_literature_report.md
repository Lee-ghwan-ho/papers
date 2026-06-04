# Literature Watch Report — Run #7 (2026-06-04)

> Continuous-ONA 프로젝트 정기 탐색  
> 총 수록 논문: 96편 (신규 5편)

---

## 신규 발견 논문 (5편)

### Accepted / Published (최고 우선순위)

#### 1. ADVERIN — AdverIN: Monotonic Adversarial Intensity Attack (MedIA 2025) ⚠️ Novelty 충돌 주의

- **Venue**: Medical Image Analysis (MedIA), 2025
- **arXiv**: 2304.02720 (April 2023 preprint → MedIA 2025 출판)
- **Category**: B (방법론 유사) + A
- **Relevance**: **High**

**핵심**: Monotonic (단조) 강도 매핑 함수 + adversarial training으로 diverse appearance를 생성, DG 성능 향상. Local intensity order를 보존하면서(=label 무결성 유지) 최대한 어려운 style을 생성.

**내 방법과의 충돌 지점**:
- 동일한 class의 방법: nonlinear, content-order-preserving intensity transformation for DG
- **핵심 차이**: AdverIN은 전체 이미지에 균일하게 적용. 나는 vessel radius에 따라 pixel마다 다른 강도를 연속적으로 적용.
- AdverIN에는 얇은 혈관 보호 개념이 없음. 얇은 혈관도 굵은 혈관과 같은 강도의 intensity 변환을 받음.

**방어 논리**:
> "AdverIN demonstrates the value of monotonic intensity perturbation for DG. However, applying uniform perturbation to all structures ignores the fundamental heterogeneity within a single class: thin vessels have intrinsically lower observability than thick vessels, and aggressive intensity perturbation risks destroying the weak evidence needed to segment them correctly."

---

#### 2. ARFU — Anatomically-Robust and Feature-Unbiased DG (Expert Systems w/ Applications 2025) ⚠️ Novelty 관련

- **Venue**: Expert Systems with Applications (ESWA), 2025
- **DOI**: S0957417425033676
- **Category**: A (직접 경쟁)
- **Relevance**: **High**

**핵심**: SRG (저주파 구조 정보 → 외형 변환 regularization), APG (기관별 외형 증강), FUL (특징 편향 제거). "무차별 외형 변환이 organ shape를 왜곡한다"는 내 동기와 동일한 전제에서 출발.

**내 방법과의 충돌 지점**:
- 공통 전제: "appearance augmentation은 구조를 파괴해서는 안 된다"
- ARFU = inter-class organ-level 보호 (복부, 심장 MRI에서 organ 단위)
- 나 = intra-class continuous thickness-based 보호 (혈관이라는 single foreground class 내부에서 두께에 따라 연속 조절)

**방어 논리**:
> "ARFU extends structure-aware augmentation to the class level. Our work addresses a finer-grained problem: within a single foreground class (cerebrovascular), thin and thick vessels exhibit fundamentally different observability, necessitating intra-class continuous augmentation conditioning."

---

#### 3. DAGBA / MRFFD — Distance-Aware Gaussian Brightness Aug (Neurocomputing 2025)

- **Venue**: Neurocomputing, 2025
- **DOI**: 10.1016/j.neucom.2025.130120
- **Category**: A (직접 경쟁) + B
- **Relevance**: Medium

**핵심**: 다중 수용장 특징 분리(MRFFD) + 이미지 내 위치에 따라 Gaussian brightness aug 강도를 다르게 적용(DAGBA). "spatial position에 따라 augmentation 강도를 다르게 조절"이라는 아이디어 공유.

**내 방법과의 차이**:
- DAGBA: image center-to-pixel 거리 기반 (geometry-free, image-level spatial prior)
- 나: vessel centerline radius / local observability 기반 (structure-aware, annotation-derived)
- DAGBA는 brightness만 조절, 나는 전반적 nonlinear appearance transformation

---

#### 4. VESSELSDF — VesselSDF: Distance Field Priors for Vascular Network Reconstruction (MICCAI 2025)

- **Venue**: MICCAI 2025, Paper 2121
- **arXiv**: 2506.16556 (June 2025)
- **Category**: C (구조·혈관 특화)
- **Relevance**: Medium

**핵심**: Voxel binary classification 대신 SDF regression으로 혈관 재구성. Gaussian regularizer로 vessel surface에서 먼 곳은 smooth, 가까운 곳은 정밀하게 처리. 혈관의 얇고 연결된 기하학을 연속적으로 표현.

**내 방법에 활용 가능성**:
- SDF 기반 vessel thickness 추정 → 내 observability score의 대안적 구현 방법
- 단, 내 방법은 annotation에서 직접 radius 추출(3D distance transform) → VesselSDF의 계산 방식이 참고됨

---

#### 5. AD-DGCL — Adaptive Disentangled DG Collaborative Learning (Neurocomputing 2025)

- **Venue**: Neurocomputing, 2025
- **DOI**: S0925231225025184
- **Category**: A
- **Relevance**: Low

**핵심**: Semi-supervised + style-content disentanglement + adaptive region-specific loss (small organ 빈도 기반 weight 동적 조절). Multi-organ 3D segmentation에서 소형 organ 성능 개선.

**내 방법과의 관련성**: Adaptive loss weighting for small structures → 내 thin vessel aug conditioning과 개념적으로 근접하지만, loss weighting (training signal 조절)과 augmentation budget 조절은 다른 층위.

---

## Novelty Gap 재확인

이번 탐색에서도 다음 키워드에 해당하는 논문은 발견되지 않았음:

- "vessel radius conditioned augmentation"
- "thickness-conditioned augmentation strength"  
- "observability-conditioned nonlinear augmentation"
- "augmentation budget per vessel size"

→ **Continuous-ONA의 핵심 contribution — 혈관 foreground class 내에서 local radius/observability에 따라 augmentation strength를 연속적으로 조절한다는 아이디어 — 는 여전히 기존 문헌에 없음.**

단, ADVERIN과 ARFU는 내 방법과 부분적으로 겹치는 motivation을 가지므로, 이 두 논문과의 명확한 차별화가 논문 작성 시 반드시 필요함.

---

## 즉시 읽기 목록 업데이트 (P0)

| 우선순위 | KEY | 이유 |
|---------|-----|------|
| ★★★ | **ADVERIN** | Monotonic intensity mapping for DG — 내 방법의 핵심 선행 연구. MedIA 2025. 즉시 구현/실험 파악 필요. |
| ★★★ | **ARFU** | "Appearance aug이 구조를 파괴한다"는 동일 전제. ESWA 2025. novelty 방어 논리 구성 필요. |

---

## 누적 현황

| Run | 날짜 | 신규 | 누적 |
|-----|------|------|------|
| #1 | 2026-05-27 | 40 | 40 |
| #2 | 2026-05-28 | 13 | 53 |
| #3 | 2026-05-29 | 15 | 68 |
| #4 | 2026-05-31 | 8 | 76 |
| #5 | 2026-06-01 | 6 | 82 |
| #6 | 2026-06-02 | 5 | 87 (→ 91 일부 재계산) |
| **#7** | **2026-06-04** | **5** | **96** |

---

## 다음 탐색 방향 제안

1. **ADVERIN 전문 독해**: monotonic function 구현 방식 + arXiv 2023 vs MedIA 2025 차이 파악
2. **ARFU 전문 독해**: SRG 저주파 추출 방식 상세 확인 → 내 방법과의 차이 구체화
3. **DAGBA 전문 독해**: "distance-aware" 구현이 image center 기반인지 확인 (내 radius 기반과 명확히 구분)
4. **"augmentation budget" 키워드**: CVPR 2026 / ICCV 2026 예정 논문 탐색 시작
5. **VesselSDF 상세 확인**: SDF-based thickness estimation이 내 observability score에 적용 가능한지
