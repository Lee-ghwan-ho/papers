# MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network

> **KEY**: MORVESS
> **Venue**: arXiv preprint
> **Status**: Preprint Only
> **Category**: C (구조 및 혈관 특화)
> **Relevance**: High
> **Novelty 충돌**: Low-Medium — radius를 신호로 쓰지만 augmentation이 아닌 supervision 목적

---

## 논문 기본 정보

- **제목**: MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network
- **arXiv**: 2606.24214 (2026년 6월)
- **Task**: Pulmonary vessel segmentation (CT), DG 논문 아님 (in-domain)

---

## 방법 요약 (검색 스니펫 기반)

- Vessel mask, distance map, **thickness map**을 jointly 예측
- Vascular boundary consistency, centerline consistency, smooth diameter transition을 명시적으로 supervise
- Lightweight 2.5D adapter — 3D context와 2D SAM feature를 연결하는 global-local fusion block
- 목표: small-branch recovery, topology integrity

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점
- **Local vessel thickness/radius를 명시적, first-class 신호로 사용**한다는 점에서 내 ONA의 core intuition("혈관 관찰 가능성은 균일하지 않다")과 방향이 일치

### 결정적 차이

| 항목 | MorVess | Continuous-ONA |
|------|---------|-----------------|
| Radius의 역할 | **Supervision target** (thickness map을 예측하도록 학습) | **Augmentation conditioning signal** (radius로 aug 강도를 조절) |
| DG 여부 | 아님 (in-domain 학습) | SSDG (핵심 목표) |
| 대상 | Pulmonary vessel (CT) | Cerebrovascular (TOF-MRA) |
| Loss 변경 | 있음 (thickness map regression loss 추가) | 없음 (augmentation only, POC 기준) |

### Novelty 보호 논거

MorVess는 "radius/thickness를 모델이 예측하도록 지도"하는 반면, ONA는 "radius를 이미 알고 있는 source annotation에서 계산해 augmentation을 조절"한다. **완전히 다른 사용 방식**(supervision vs. augmentation conditioning)이며 목적(topology integrity vs. domain generalization)도 다르다. 다만 "vessel thickness/radius가 vessel segmentation 연구에서 점점 중요한 first-class signal로 부상하고 있다"는 트렌드를 뒷받침하는 근거로 인용 가능.

---

## 메모

- 최근 preprint (2026년 6월), 아직 학회/저널 게재 정보 없음 — 후속 추적 필요
- Thickness map 계산 방식(skeleton distance transform 등)이 내 observability score 계산과 유사할 가능성 — 방법론 디테일 확인 시 참고
