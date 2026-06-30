# TSIAA — Paper Note

> **KEY**: TSIAA  
> **Run**: #8 (2026-06-30)  
> **Priority**: P0 ⚠️ (Novelty 충돌 위험)

---

## 서지 정보

- **제목**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation
- **저자**: Zhengshan Wang, Long Chen, et al.
- **게재지**: IEEE Transactions on Medical Imaging
- **연도/권호**: 2026, Vol. 45, pp. 764–776
- **DOI**: 10.1109/TMI.2026.11146907 (IEEE Xplore: 11146907)
- **Code**: https://github.com/Wangzts0228/TSIAA
- **Status**: Published Journal Article (IEEE TMI 2026)

---

## 핵심 주장

> "Compared to image-level adversarial augmentation, instance-level adversarial augmentation **breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity."

→ 이 문장은 내 Continuous-ONA의 동기와 직접 겹친다.

---

## 방법 요약

### IIAG (Instance-level Image Augmenter)
- 여러 IAM (Instance-level Augmentation Module)으로 구성
- 각 IAM: **learnable constrained Bézier transformation** 기반
- image-level augmenter와 달리 구조마다 다른 augmentation 파라미터를 독립적으로 학습

### Teacher-Student Adversarial Learning
- **Student**: adversarial aug 생성기 — out-of-source, plausible augmented data 탐색 (diversity maximize)
- **Teacher**: original + augmented feature 간 consistent generalized representation 유지
- 두 모델을 alternating update: aug 생성 ↔ representation 학습

### 실험
- 4개 SDG task (추정: prostate, fundus, polyp, cardiac)
- 기존 SOTA 대비 significant improvement

---

## 내 방법과의 비교

### 공통점
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 설정 | SSDG | SSDG |
| 기반 transformation | Bézier curve | Bézier/spline nonlinear mapping |
| 핵심 아이디어 | intra-image 비균일 augmentation | intra-image 비균일 augmentation |
| target | generic medical image | tubular vessel |

### 결정적 차이점

**1. 방향성의 차이 (가장 중요)**
- TSIAA: diversity MAXIMIZE (adversarial → 더 강한 aug 탐색)
- ONA: 얇은 혈관에는 CONSERVATIVE (radius ↓ → aug strength ↓)

TSIAA는 augmentation을 "더 강하게, 더 다양하게" 만드는 방향으로 최적화한다. 반면 ONA는 얇은 혈관에 대해 **의도적으로 약하게** 적용한다.

**2. Conditioning signal의 차이**
- TSIAA: conditioning 없음 — adversarially learned (implicit)
- ONA: local vessel radius / observability score (explicit, physics-motivated)

ONA의 radius conditioning은 해석 가능하다(interpretable). TSIAA의 instance-level augmentation은 무엇을 기준으로 다르게 하는지 명시적이지 않다.

**3. 문제 의식의 차이**
- TSIAA: "image-level uniform augmentation → diversity 부족"
- ONA: "uniform augmentation → thin vessel에서 label-image inconsistency (annotation에는 혈관인데 영상에서는 안 보임)"

ONA의 문제 의식은 vessel physics(partial volume effect)에서 출발한다.

**4. Application domain**
- TSIAA: generic medical image segmentation (prostate, fundus, polyp 등)
- ONA: tubular structure 특화 (TOF-MRA cerebrovascular)

---

## 포지셔닝 전략

### Related work에서 인용 방법

> "TSIAA [ref] demonstrates that applying uniform augmentation rules across all structures within an image is suboptimal, and proposes adversarial instance-level augmentation to improve diversity. However, diversity alone does not address the fundamental challenge in tubular structure segmentation: excessive augmentation applied to thin/fragile vessels corrupts structural evidence, creating label-image inconsistency. Continuous-ONA instead introduces observability as an explicit conditioning factor, deliberately constraining augmentation for fragile structures while encouraging stronger augmentation for well-resolved ones."

### 논문의 novelty 재정비

기존 claim: "intra-image 비균일 augmentation이 필요하다" → TSIAA로 인해 이 claim 자체가 novel하지 않음

**강화된 claim**:
> "We identify that for tubular structures, the augmentation budget should asymmetrically depend on structural observability: thin, fragile vessels require conservative perturbations to preserve weak structural evidence, while well-resolved vessels can tolerate — and benefit from — stronger appearance variation. This observability-driven asymmetry is absent in both uniform augmentation methods (SLAug, ConStyX) and diversity-driven methods (TSIAA, AdvST)."

---

## 미확인 사항 (full text 독해 필요)

- [ ] "instance"의 정의: semantic object instance(전체 구조 마스크 기반)인가, 아니면 다른 granularity인가?
- [ ] 4개 SDG task 실험 구성 상세 (dataset, 비교 baseline)
- [ ] IAM의 Bézier parameter space 구성 (내 Bézier mapping과 overlap 정도)
- [ ] thin/thick structure에서 각각 어떤 aug가 적용되는지 확인 (보완 관계 파악)
- [ ] ablation: instance-level vs. image-level 비교 실험 상세

---

## 결론

TSIAA는 내 방법의 가장 직접적인 경쟁 논문이다. IEEE TMI 2026에 게재된 강력한 관련 논문으로, 반드시 Related Work에 포함해야 한다.

그러나 TSIAA와 ONA는 **방향이 반대**다:
- TSIAA: more diverse, more adversarial
- ONA: more conservative for fragile, more aggressive for robust

이 asymmetry야말로 ONA의 핵심 contribution이며, TSIAA의 존재가 이 asymmetry를 더 선명하게 만들어준다.
