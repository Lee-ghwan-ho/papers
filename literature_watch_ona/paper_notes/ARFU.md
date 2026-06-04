# ARFU — Anatomically-Robust and Feature-Unbiased Domain Generalization for Medical Segmentation

## 기본 정보

- **Venue**: Expert Systems with Applications (Elsevier), 2025
- **DOI**: 10.1016/j.eswa.2025.S0957417425033676
- **arXiv**: 미확인
- **Category**: A (직접 경쟁)
- **Relevance**: High
- **Status**: Published Journal Article
- **발견**: Run #7 (2026-06-04)

---

## 핵심 주장

Single-source domain generalization에서 기존 방법들은 두 가지 bias를 해결하지 못한다:
1. **Shape bias**: 과도한 외형 변환이 organ 형태를 왜곡함
2. **Feature bias**: 학습된 feature가 confounding context에 의존

ARFU는 세 가지 모듈로 이를 해결:

### SRG (Shape Regularization-Guided Augmentation)
- 저주파 성분에서 추출한 구조 정보를 regularization으로 사용
- 전체적 외형 변환(global appearance transformation)을 적용하되, anatomical 왜곡 방지
- 즉, "외형을 바꾸되 모양을 보존한다"

### APG (Anatomical Prior-Guided Augmentation)
- 학습 마스크에서 organ shape 분포를 학습
- Organ-specific appearance augmentation + appearance-agnostic anatomical discrimination 학습
- Class-level로 서로 다른 augmentation을 적용하는 mechanism

### FUL (Feature Unbiased Learning)
- Feature 분포에 perturbation을 가하고 frequency domain에서 동적 필터링
- Domain-invariant anatomical representation 학습 강화

---

## 실험

- Cross-modality abdominal segmentation (CT → MRI)
- Cross-sequence cardiac MRI segmentation
- 기준 비교: SLAug, RASS, 기타 SSDG 방법들

---

## 내 연구와의 관계

### 공통점

- "무차별적인 외형 augmentation은 구조를 파괴할 수 있다"는 전제 공유
- 구조 정보를 활용해 augmentation을 guide하는 아이디어 공유
- 내 핵심 주장과 방향 일치: "appearance augmentation should not destroy structure"

### 핵심 차이 (novelty 방어 포인트)

| 항목 | ARFU | Continuous-ONA |
|------|------|----------------|
| 적용 단위 | Class-level (organ vs. organ) | Intra-class (thick vessel vs. thin vessel) |
| 보호 대상 | Organ shape (전체 foreground class의 기하학) | Thin vessel visibility (single class 내 fragile sub-structure) |
| 조절 방식 | Binary (aug 강도가 class마다 다름) | Continuous (vessel radius/observability에 따라 연속 조절) |
| 적용 도메인 | Abdominal CT/MRI, Cardiac MRI | TOF-MRA cerebrovascular |
| 구조 인식 | 저주파 regularization | Local radius / observability score |

### 결론

ARFU는 inter-class 보호 (서로 다른 organ class 간 augmentation 차별화), 나의 Continuous-ONA는 intra-class 보호 (하나의 foreground class인 vessel 내에서 두께에 따른 연속적 조절). ARFU를 인용하여 "class-level 구조 보호를 넘어 intra-class continuous thickness-conditioned 조절로 확장"한다는 서사를 구성할 수 있음.

---

## 인용 전략

Related Work에서 다음과 같이 배치:
> "ARFU [?] showed that indiscriminate augmentation can destroy anatomical shape and proposed organ-level structure-guided augmentation. Our work extends this insight to the intra-class level: within a single vessel class, thin and thick structures have fundamentally different observability, requiring a continuous augmentation budget adjusted by local radius."

---

## 추가 확인 필요

- [ ] SRG의 저주파 추출 방식 상세 확인 (Fourier? GaussianBlur?)
- [ ] APG의 organ-specific aug이 mask 단위인지 아니면 class-conditional feature perturbation인지
- [ ] 실험 결과에서 thin structure (소장, 담관 등)에 대한 분석이 있는지
