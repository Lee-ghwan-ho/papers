# TSIAA — Teacher-Student Instance-Level Adversarial Augmentation

> **KEY**: TSIAA  
> **Category**: A (직접경쟁 — SSDG Medical Image Segmentation)  
> **Venue**: IEEE Transactions on Medical Imaging, Vol 45, pp 764-776, 2026  
> **Status**: Published Journal Article  
> **IEEE Xplore**: 11146907  
> **Code**: https://github.com/Wangzts0228/TSIAA  
> **Novelty 위협도**: ⚠️ Medium-High

---

## 논문 기본 정보

**제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**저자**: Zhengshan Wang, Long Chen et al.  
**출판**: IEEE TMI 2026, Vol 45, Pages 764-776

---

## 핵심 기여

### 문제의식

기존 adversarial augmentation 기반 SSDG 방법들의 한계:
- Augmenter 구조가 단순하고 image-level에서만 작동
- Augmented image diversity가 제한적
- Out-of-source distribution을 충분히 커버하지 못함

### 제안 방법

**TSIAA = Teacher-Student Instance-level Adversarial Augmentation**

1. **Instance-level Image Augmenter (IIAG)**:
   - Instance-level Augmentation Modules (IAMs)으로 구성
   - 각 IAM = **learnable constrained Bézier transformation function**
   - Bézier 파라미터를 per-image(instance) 단위로 adversarially 학습
   - 복수의 IAM을 연결하여 다양한 nonlinear appearance variation 생성

2. **Teacher-Student 학습**:
   - Teacher network: 현재 best model로 target distribution 방향 안내
   - Student network: adversarial augmented samples에서 domain-generalizable representation 학습
   - Adversarial 목표: segmentation을 가장 어렵게 만드는 appearance 변형 탐색

---

## 실험 결과

- 실험 데이터셋: 의료영상 다중 분절 (prostate, cardiac, fundus 포함 추정)
- Baseline 대비 개선 확인 (상세 수치: full text 독해 필요)
- 7개 데이터셋 실험

---

## 내 Continuous-ONA와의 비교

### 공통점

| 공통 요소 | TSIAA | ONA |
|-----------|-------|-----|
| Bézier 기반 nonlinear appearance | ✓ | ✓ |
| SSDG 설정 | ✓ | ✓ |
| Learnable transformation | ✓ | ✗ (fixed schedule) |

### 핵심 차이

| 차이 차원 | TSIAA | Continuous-ONA |
|-----------|-------|----------------|
| **Aug 단위** | per-image (image-level uniform) | intra-image vessel 구조 단위 |
| **Conditioning 신호** | 없음 (adversarial exploration) | vessel radius/observability (명시적 구조 정보) |
| **Thin vessel 보호** | 없음 | 핵심 메커니즘 |
| **학습 방식** | adversarial (teacher-student) | fixed observability-strength mapping |
| **동기** | augmentation diversity 부족 | label-image inconsistency for thin structures |
| **Aug 강도 조절** | per-image 단위 scalar | 연속적 intra-image spatial map |

### 결정적 구분

**TSIAA의 "instance-level"** = 이미지 인스턴스(샘플) 단위 → 여전히 하나의 이미지 전체에 uniform하게 적용  
**내 ONA의 "structure-conditioned"** = 동일 이미지 내 혈관 각각의 관찰 가능성에 따라 공간적으로 다른 강도 적용

> TSIAA applies Bézier-based nonlinear transformations uniformly across each image instance. Our method introduces a **spatially-varying, continuous observability-conditioned augmentation budget** that assigns different transformation intensities to vessel structures based on their local radius — a fundamentally different conditioning signal that TSIAA's image-level augmenter does not capture.

---

## Related Work에서 활용 방안

1. **Bézier transformation 계보**: TSIAA → 기존 Bézier-based SSDG의 연장선. 내 방법이 Bézier를 사용하더라도 per-image가 아닌 per-vessel-observability 적용임을 명확히.
2. **Adversarial aug 계보**: TSIAA, AADG (TMI 2022), ADAL (ICCV 2025) — 모두 image-level diversity 탐색. 내 방법은 intra-image structural diversity 조절로 orthogonal.
3. **"Instance"의 의미 주의**: TSIAA의 instance = 이미지 샘플, 나의 conditioning = 혈관 구조 geometry.

---

## TODO

- [ ] Full text 독해: IAM Bézier function 수식 상세 (제약 조건 어떻게 두는가)
- [ ] 실험 데이터셋 확인: prostate, cardiac, fundus vs. vessel
- [ ] 비교 baseline 목록 확인: SLAug, ADA 대비 성능 차이
- [ ] TSIAA의 diversity 지표와 내 observability 지표 비교 가능성 검토
