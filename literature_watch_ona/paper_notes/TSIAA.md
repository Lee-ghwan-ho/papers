# TSIAA — Teacher-Student Instance-Level Adversarial Augmentation

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging (IEEE TMI), 2026  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Code**: https://github.com/Wangzts0228/TSIAA  
**Status**: Published Journal Article  
**Category**: A (직접경쟁)  
**Relevance**: **HIGH ⚠️ Novelty 충돌 위험**

---

## 방법 요약

### 핵심 문제의식
기존 adversarial augmentation 기반 SSDG 방법은 **image-level** 증강에 그쳐 다양성이 제한된다.  
특히 over-augmentation을 피하기 위해 단순한 구조의 augmenter를 쓰면, 같은 이미지 내 다른 구조에 동일한 augmentation 규칙을 적용하게 된다.

> "Compared to image-level adversarial augmentation, **instance-level adversarial augmentation breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity."

### 제안 방법: TSIAA (Teacher-Student Instance-level Adversarial Augmentation)

**구성 요소:**

1. **IIAG (Instance-level Image Augmenter)**:
   - 여러 IAM (Instance-level Augmentation Module)으로 구성
   - 각 IAM은 **learnable constrained Bézier transformation**을 기반으로 함
   - 이미지 내 서로 다른 instance/structure에 대해 독립적인 Bézier 파라미터 사용
   - → 동일 이미지 내 다른 구조가 서로 다른 augmentation을 받음

2. **Teacher-Student 학습**:
   - Adversarial 방식: IIAG는 segmentation 모델이 어렵게 인식하는 방향으로 augmentation을 생성
   - Student: augmented image로 학습 (harder samples)
   - Teacher: original image로 학습 (EMA update)
   - original과 augmented feature 사이의 consistency 강제 → domain-invariant representation 학습

3. **과제**: 4개 SDG task (cardiac, prostate, fundus retina, skin lesion)에서 SOTA 능가

---

## ⚠️ 내 방법 (Continuous-ONA)과의 관계

### 공통점 (novelty 위협 요소)
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 핵심 주장 | "uniform augmentation이 문제" | "uniform augmentation budget이 문제" |
| 증강 단위 | instance 수준 (구조별 다른 규칙) | intra-class vessel 수준 (radius별 다른 강도) |
| 기반 변환 | learnable Bézier curve | nonlinear intensity mapping (Bézier 또는 spline) |
| 학습 패러다임 | adversarial + teacher-student | augmentation-only (POC 기준 loss 변경 없음) |

### 핵심 차이점 (novelty 보호 논거)

**1. 증강 강도 결정 원리:**
- TSIAA: **adversarial** — segmentation model이 가장 어려워하는 augmentation을 찾음 (모델 반응 기반)
- 나: **observability-conditioned** — GT vessel radius/observability라는 구조적 property 기반 (모델 무관)

**2. 증강의 공간적 단위:**
- TSIAA: different *semantic instances* (서로 다른 객체 클래스 간 다른 augmentation)
- 나: *동일 class 내* 다른 vessel locations (intra-class, within-vessel spatial conditioning)
  - TSIAA는 thin vessel vs. thick vessel을 하나의 vessel class 내에서 구분하지 않음
  - 나는 같은 "vessel" class에서도 local radius에 따라 augmentation budget을 연속적으로 조절

**3. 증강 강도의 구조 의존성:**
- TSIAA: 구조적 observability (radius, visibility)와 무관. 학습 손실에 반응하는 adversarial policy.
- 나: 구조의 **물리적 관찰 가능성** (vessel radius, vesselness)에 직접 조건부. thin vessel일수록 appearance perturbation을 줄여 label-image consistency 보호.

**4. 핵심 주장의 층위:**
- TSIAA: "같은 이미지 내 다른 *semantic* 구조에 다른 augmentation" (cross-class)
- 나: "같은 class 내 다른 *geometrical* 구조에 다른 augmentation" (intra-class)

### 결론: Novelty 보호 논거

> TSIAA demonstrates that inter-structure augmentation diversity improves SSDG over image-level methods.
> Continuous-ONA addresses a finer-grained, orthogonal challenge: within a single tubular class, structures
> vary in observability (radius, vesselness), and a uniform augmentation budget treats fragile thin vessels
> identically to well-resolved thick vessels — creating label-image inconsistency specifically for the
> most clinically critical sub-structures.

**Novelty 위협도**: **Medium** (아이디어 방향 유사하나 granularity와 원리가 다름)  
**대응 전략**: Related Work에서 TSIAA를 "instance-level adversarial diversity"의 대표로 인용하고,  
나의 방법이 "같은 instance 내 intra-class observability conditioning"이라는 추가 차원을 다룬다고 명시.

---

## 실험 결과 요약
- 4개 SDG task에서 기존 SOTA 능가
- 비교 baseline: SLAug, ADA, ConStyX 등 포함 (확인 필요)
- Bézier-based augmenter를 instance-level로 확장한 것이 핵심

---

## 내 연구에서의 활용 방안

1. **Related Work**: "adversarial instance-level augmentation 계열"로 소개 (SLAug → ADA → TSIAA)
2. **차별화 논거**: inter-structure vs. intra-class observability conditioning
3. **실험 baseline**: TSIAA를 baseline 비교군에 추가 고려 (코드 공개)
4. **인용 필수**: 내 방법이 "uniform augmentation이 문제"라는 주장을 할 때 TSIAA를 인용하며 레벨 차이 강조

---

> 마지막 업데이트: 2026-06-24 (Run #8)
