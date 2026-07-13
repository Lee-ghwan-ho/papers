# MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network

> **Venue**: arXiv preprint
> **Status**: Preprint Only
> **Category**: C — 구조 및 혈관 특화
> **Relevance**: High
> **arXiv**: 2606.24214
> **Added**: Run #8 (2026-07-13)

---

## 요약

Pulmonary CT vessel segmentation을 위해 vessel mask, distance map, **thickness map**(centerline을
따라 propagate된 maximal inscribed-sphere radius)을 joint 예측하는 2.5D SAM-adapter 기반 네트워크.
Thickness map은 centerline consistency와 diameter transition smoothness를 위한 auxiliary
supervision target으로 사용됨.

## 내 연구에의 시사점

이번 조사에서 발견한 논문 중 vessel radius/thickness를 명시적인 신호로 계산한다는 점에서
Continuous-ONA의 observability score 계산과 가장 근접한 사례다.

**핵심 차이**: MorVess는 thickness를 **prediction target (loss supervision)**으로 사용하고,
나는 thickness/observability를 **augmentation strength를 조절하는 control signal**로 사용한다.
즉 둘 다 "vessel radius를 명시적으로 다룬다"는 공통점이 있지만, radius가 소비되는 지점이
loss 단계인지 augmentation 단계인지가 다르다 — cbDice/AG-TAL(loss weighting)과 동일한
구분선이 여기서도 성립한다.

- Domain generalization을 다루지 않음 (in-domain pulmonary CT)
- Fragile structure 보호라는 동기가 없음 (thin vessel을 augmentation으로부터 보호하는 개념 없음)

## Novelty 충돌 위험

**Low.** Radius 계산 방법론(centerline distance transform)은 참고할 가치가 있으나,
augmentation과 무관한 순수 supervision 논문이라 novelty 충돌은 없음.

## 활용 방안

- Radius/thickness map 계산 방식(centerline 기반 maximal inscribed sphere)을 내 observability
  score 계산의 구현 참고로 활용 가능
- Related Work에서 "radius-aware signal이 supervision(MorVess, AG-TAL/cbDice)에서는 이미 활용되고
  있으나, augmentation budget 조절에는 아직 적용되지 않았다"는 gap 논거를 강화하는 근거로 사용
