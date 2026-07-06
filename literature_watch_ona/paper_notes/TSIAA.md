# TSIAA: Teacher–Student Instance-Level Adversarial Augmentation

> **KEY**: TSIAA
> **Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776 (early access ~2025-09-02)
> **Status**: Published Journal Article
> **Category**: A/B (직접경쟁 + 방법론 유사)
> **Relevance**: **High**
> **Novelty 충돌**: ⚠️ Medium — 가장 근접한 선행 연구, 원문 미확인 상태

---

## ⚠️ 확인 필요 사항 (최우선)

이 노트는 **원문 PDF를 직접 확인하지 못한 상태**에서 검색 스니펫/색인 초록을 기반으로 재구성되었다
(세션 네트워크 정책상 arxiv.org 및 IEEE Xplore 직접 접근이 403/차단됨).
서지정보(DOI, volume/page)와 메커니즘 설명은 2차 출처 기반이므로,
**Related Work 작성 전 반드시 원문을 직접 확보하여 검증**할 것.

- DOI: 10.1109/TMI.2025.3605162
- IEEE: https://ieeexplore.ieee.org/document/11146907/
- Code (보고됨, 미확인): https://github.com/Wangzs0228/TSIAA

---

## 논문 기본 정보 (2차 출처 기반, 검증 필요)

- **제목**: Teacher–Student Instance-Level Adversarial Augmentation (for Single-Source Domain Generalization in Medical Image Segmentation — 정확한 부제 미확인)
- **핵심 아이디어**: Learnable constrained Bézier 변환을 사용하는 **Instance-level Augmentation Module (IAM)** — 이미지 내 서로 다른 구조/instance마다 다른 augmentation을 적용
- **학습 방식**: Teacher-student adversarial loop (adversarial training으로 augmentation 정책 탐색)
- **설정**: Single-source DG, target domain 정보 미사용
- **실험**: 4개의 SDG segmentation task에서 평가 (구체적 데이터셋 미확인)

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점 (novelty 충돌 위험 요인)

| 항목 | TSIAA | Continuous-ONA |
|------|-------|-----------------|
| Augmentation 단위 | **Instance-level** (이미지 내 구조별) | **Intra-class structure-level** (혈관별 local radius) |
| 전체 이미지 uniform augmentation 탈피 | ✅ | ✅ |
| Single-source, no target info | ✅ | ✅ |
| Bézier 계열 nonlinear transform | ✅ | ✅ (nonlinear appearance transform family) |

이 두 논문은 "**하나의 이미지 안에서 augmentation을 균일하게 적용하면 안 된다**"는
motivation을 공유하는 것으로 보인다 — 지금까지 조사한 논문 중 가장 근접한 사례.

### 결정적 차이 (검증 필요, 잠정)

| 항목 | TSIAA | Continuous-ONA |
|------|-------|-----------------|
| **Conditioning 신호** | 학습된 adversarial policy (무엇이 instance인지, 강도를 얼마로 할지 모두 학습) | **명시적으로 측정된** local vessel radius/observability score |
| **연속성** | Instance 단위 discrete 분할로 추정됨 (원문 미확인) | Radius/observability에 대한 **continuous** 함수 |
| **구조 특화 신호** | 혈관 고유의 "관찰 가능성(observability)" 개념 없음 | Vessel-specific: thin=fragile/저관찰, thick=resolved/고관찰 |
| **Fragile structure 보호 논리** | 명시적 언급 없음 (adversarial loss가 암묵적으로 유도할 가능성) | Label-image inconsistency 방지가 명시적 설계 목표 |
| **Task** | 일반 medical segmentation SDG (4개 task) | TOF-MRA cerebrovascular 특화 |

### 잠정 결론

TSIAA는 "instance마다 다른 augmentation" 이라는 **더 넓은 문제의식**을 다루지만,
그 방법론이 **adversarial policy learning**이라는 점에서 나의 **explicit morphological
conditioning (radius/observability)**과 메커니즘이 근본적으로 다르다.
그러나 "instance-level differential augmentation"이라는 상위 개념을 공유하므로,
Related Work에서 **가장 비중 있게 다뤄야 할 비교 대상**이다.

가능한 차별화 논거:
1. TSIAA는 augmentation 정책을 adversarial training으로 "발견"하는 반면,
   Continuous-ONA는 도메인 지식(vessel radius가 관찰 가능성과 직결된다는 해부학적 사실)을
   augmentation 설계에 **직접 주입**한다 — 학습이 필요 없고 해석 가능하다.
2. TSIAA에 vessel-specific radius/observability 개념이 있는지 불명확 — 만약 없다면,
   "tubular structure의 fragile/resolved 구분"이라는 나의 핵심 주장은 여전히 미개척.
3. Adversarial training 방식은 additional training complexity(teacher-student, adversarial loop)를
   요구하는 반면, ONA는 "loss 변경 없이 augmentation 효과만" 검증하는 lightweight POC.

---

## Action Items

- [ ] **최우선**: 원문 PDF 확보 (기관 접근, Sci-Hub 등 합법적 경로, 또는 저자에게 직접 요청 검토)
- [ ] IAM의 "instance" 정의 확인: 혈관 구조 단위인지, 일반적 해부학적 영역/객체 단위인지
- [ ] Bézier 파라미터가 어떻게 instance별로 달라지는지 — 학습된 값인지 규칙 기반인지
- [ ] ADA (MICCAI 2025, Learnable Bezier Remap)와의 관계 확인 — 동일 연구 그룹/계열 논문인지
- [ ] 실험 데이터셋에 vessel segmentation이 포함되는지 확인 (TOF-MRA/혈관 벤치마크와 직접 비교 가능성)
- [ ] 코드 저장소(GitHub) 확인 가능 시 실제 IAM 구현 검토

---

## 메모

- Run #8 (2026-07-06) 신규 발견. 기존 known-list에 없었음.
- Published Journal (IEEE TMI) — preprint가 아닌 정식 출판 논문이므로 인용 시 최상위 우선순위.
- 이번 사이클 최우선 정독 대상으로 READING_QUEUE P0에 등재.
