# Local Gamma Augmentation for Ischemic Stroke Lesion Segmentation on MRI

> **Venue**: Northern Lights Deep Learning (NLDL) Conference 2024
> **Status**: Accepted Conference Paper
> **Category**: B — 방법론 유사
> **Relevance**: High
> **arXiv**: 2401.06893
> **Added**: Run #8 (2026-07-13)

---

## 요약

Ischemic stroke lesion segmentation에서 병변(lesion) 영역에만 국소적으로 gamma-curve intensity
augmentation을 적용하는 기법. 전체 이미지에 균일하게 gamma augmentation을 적용하면 intensity bias가
학습 데이터의 편향을 반영하지 못한다는 문제를 해결하기 위해, **binary lesion mask로 제한된 영역만**
augment한다. 세 개 데이터셋에서 lesion 수준 sensitivity 개선을 보고.

## 내 연구에의 시사점

이번 조사에서 발견한 논문 중 **"이미지 전체에 균일한 augmentation을 적용하면 안 된다"는 정성적 주장을
가장 직접적으로 선취한 선행 연구**다. 2024년 NLDL 논문으로 비교적 오래되었고 지금까지 이 계열의
검색에서 누락되어 있었다.

- 조건 신호가 **binary mask** (lesion vs. background)이며, **continuous**하지 않음
- 조건 대상이 **병리(pathology) 영역**이지 tubular structure의 **관찰 가능성(observability)**이 아님
- 목적이 intensity bias 보정이지, label-image inconsistency 방지나 shortcut 억제가 아님

## Novelty 충돌 위험

**Low-Medium.** 직접적인 방법론 충돌은 아니지만, "region-restricted augmentation"이라는 상위
카테고리에서 반드시 먼저 인용해야 하는 선행 연구다. Related Work에서 이 논문을 인용하지 않으면
심사자가 지적할 가능성이 있음 — "prior work has already restricted augmentation to a binary
region mask; what is new is making the restriction continuous and radius-driven" 형태의
차별화 문장이 필요.

## 대응 전략

> "Local Gamma Augmentation restricts augmentation to a binary lesion mask, a coarse two-level
> policy. Continuous-ONA generalizes this idea along two axes: (1) the conditioning signal is a
> continuous structural observability score rather than a binary mask, and (2) the target is
> geometric fragility of tubular structures rather than pathological status."
