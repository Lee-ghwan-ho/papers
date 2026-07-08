# MARVEL: Universal Murray's Law-informed Vessel Tree Segmentation and Topology Estimation

> **KEY**: MARVEL
> **Venue**: arXiv 2605.25363 (~May 2026) — Preprint Only
> **Status**: Preprint Only
> **Category**: C (구조·혈관특화)
> **Relevance**: High
> **Novelty 충돌**: Low — mechanism(생물물리 법칙 기반 topology estimation)과 목적(augmentation 아님)이 명확히 분리됨

---

## 논문 기본 정보

- **제목**: Universal Murray's Law-informed Vessel Tree Segmentation and Topology Estimation
- **문제**: Vessel tree segmentation + topology estimation, 다양한 modality/anatomy에 걸친 universal한 vessel 표현
- **Task**: Segmentation + topology consistency (DG framing은 명시적이지 않음)
- **날짜**: 검색 시점 기준 2026년 5월경 preprint로 확인 — 본 프로젝트의 검색 윈도우(2026-06 이후)보다 다소 이른 시점이나, 개념적 연관성이 매우 높아 수록

---

## 핵심 아이디어

- **Murray's Law**: 혈관 분기에서 parent vessel radius와 daughter vessel radii 사이에 성립하는 생물물리학적 관계 (r_parent^3 = Σ r_daughter^3, 유체역학적 에너지 최소화 원리에서 유도)
- 이 법칙을 vessel tree topology estimation의 **prior/consistency constraint**로 사용하여, 서로 다른 modality/anatomy에 걸쳐 "universal"하게 작동하는 vessel 표현을 목표로 함
- Radius 관계를 구조적 정합성(topology correctness) 검증/보정에 활용

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점 (주의 필요)

| 항목 | MARVEL | Continuous-ONA |
|------|--------|----------------|
| Radius의 중심적 역할 | Murray's Law로 radius 관계를 topology consistency에 직접 사용 | Local radius/observability를 augmentation budget에 직접 사용 |
| "Universal"/일반화 지향 | 여러 modality/anatomy에 일반화되는 vessel 표현 목표 | Single-source DG로 unseen target에 일반화 목표 |
| 이론적 근거 | 생물물리학 법칙(Murray's Law) | 관찰 가능성(observability) 개념 — 아직 명시적 물리 법칙에 근거하지 않음 |

### 결정적 차이

| 항목 | MARVEL | Continuous-ONA |
|------|--------|----------------|
| **Mechanism** | Topology estimation/consistency (구조 예측 정합성) | Augmentation budget (appearance perturbation 강도) |
| **Radius의 용도** | 혈관 분기 구조가 물리 법칙을 만족하는지 검증 | 혈관이 얼마나 강한 augmentation을 견딜 수 있는지 결정 |
| **DG framing** | 명시적이지 않음 (universal representation 지향) | SSDG로 명시적 formalize |
| **Loss/Aug 여부** | Topology/구조 예측 관련 (loss 계열) | 순수 augmentation (loss 불변) |

---

## 내 연구에서의 활용

1. **이론적 근거 보강**: 혈관의 radius가 단순 기하학적 속성이 아니라 Murray's Law 같은 **생물물리학적 최적화 원리**에서 유래한다는 점은, "radius가 vessel의 구조적/기능적 중요도를 나타내는 의미 있는 신호"라는 내 observability score의 이론적 정당성을 강화하는 데 인용 가능
2. **Related Work 포지셔닝**: "radius를 활용한 선행 연구" 그룹에 AG-TAL(loss), MorVess(supervision), MARVEL(topology consistency)을 함께 묶고, augmentation에 적용한 사례가 없다는 gap을 강조하는 근거로 활용
3. **Future work 언급 가능성**: Murray's Law 기반 radius 예측을 내 observability score 계산의 보조 신호로 사용하는 확장 아이디어 (직접 구현 필요는 없음, 논의 수준)

---

## 인용 전략

> "The relationship between vessel radius and vascular function is not merely descriptive but grounded in physical optimality principles such as Murray's Law, which recent work (MARVEL) leverages for topology-consistent vessel tree estimation. This supports our premise that local radius is a meaningful proxy for structural significance — one that, to date, has been used for topology/supervision purposes but not for modulating augmentation strength."

---

## 메모

- 검색 창(2026-06 이후) 바로 이전 시점의 논문이나, novelty 충돌 위험도가 높아 예외적으로 수록
- 다음 실행에서 정식 venue 확정 여부 및 후속 인용 확인 필요
- Murray's Law 수식과 radius 계산 세부 방법론은 전문 확인 필요 (현재 snippet 기반 요약)
