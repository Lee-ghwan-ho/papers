# TSIAA — Paper Note

**제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026  
**IEEE Xplore**: Document 11146907  
**Status**: Published Journal Article  
**Category**: A (직접 경쟁)  
**Relevance**: High — Bézier 기반 SSDG 직접 경쟁  
**Run**: #8 (2026-06-20)

---

## 핵심 내용

### 문제 의식
- 기존 adversarial-based DG 방법은 augmenter 구조가 단순 (image-level 적용)
- Image-level 단위 augmentation은 다양성이 제한됨 → over-simple augmentation
- SDG에서 out-of-source distribution을 탐색하는 데 한계

### 제안 방법
- **IIAG (Instance-level Image Augmenter)**: 여러 IAM(Instance-level Augmentation Module)로 구성
- **IAM**: learnable constrained Bézier transformation function 기반
  - 각 sample(instance)에 대해 Bézier curve 파라미터를 동적으로 조절
  - image-level이 아니라 instance-level에서 다양성 확장
- **Teacher-Student 구조**:
  - Teacher: 원본 이미지로부터 stable prediction 생성
  - Student: 적대적으로 증강된 이미지로 학습 → teacher의 예측을 따르도록 유도
  - 이를 통해 over-augmentation을 teacher supervision으로 억제

---

## 내 Continuous-ONA와의 관계 분석

### 공통점
- **Bézier transformation 사용**: 둘 다 learnable nonlinear intensity transformation 계열
- **Over-augmentation 인식**: TSIAA = teacher-student로 억제, 나 = radius별 conservative/aggressive 조절
- **SSDG 설정**: 동일한 single-source domain generalization 환경

### 핵심 차이 (나의 novelty 방어 근거)

| 구분 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 적용 단위 | per-sample (전체 이미지 단위) | per-structure (혈관 내 위치별) |
| 강도 조절 근거 | adversarial teacher loss 최대화 | local vessel radius / observability |
| 보호 대상 | 없음 (teacher supervision으로 전체 억제) | thin/fragile vessel 구조 선택적 보호 |
| 공간 해상도 | 이미지 전체 단일 파라미터 | spatial map (pixel-level observability) |
| intra-image heterogeneity | 고려 안 함 | 핵심 전제 |

### 결론
- TSIAA는 **sample-level adversarial diversity**에 집중, 내 방법은 **structure-level conservative/aggressive budget**에 집중
- 같은 이미지 안에서 얇은 혈관과 굵은 혈관이 서로 다른 augmentation strength를 받아야 한다는 개념: TSIAA에 없음
- ADA(MICCAI 2025)와 함께 Bézier 계열로 분류하되, 둘 다 내 intra-class spatial conditioning과 구별됨

---

## 실험 정보
- 공개 정보 기준: 프로스타 MRI, 망막 안저, 심장 MRI 등 다중 데이터셋
- 비교 baseline: SLAug, ADA, ConStyX 등과 비교 예상

---

## 내 논문에서의 활용
- Related Work에 ADA와 함께 "Bézier-based adaptive augmentation" 묶어 서술
- 차이 논거: "TSIAA and ADA adjust Bézier parameters at the sample level, while our ONA conditions augmentation strength on intra-image vessel observability — the same image receives spatially heterogeneous perturbation budgets."
