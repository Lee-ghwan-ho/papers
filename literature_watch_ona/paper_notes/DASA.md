# DASA: Difficulty-Aware Sample Allocation for Adaptive Data Augmentation

> **KEY**: DASA
> **Venue**: Research Square (preprint server, 동료평가 전)
> **Status**: Preprint Only
> **Category**: D (Top-tier Vision — 아이디어 전이, 단 venue 신뢰도 낮음)
> **Relevance**: High (mechanism), Low (venue 신뢰도)
> **Novelty 충돌**: Medium — segmentation에서 continuous difficulty → continuous augmentation 강도 매핑을 시도한 가장 근접한 사례

---

## 논문 기본 정보

- **제목**: Difficulty-Aware Sample Allocation for Adaptive Data Augmentation in Semantic Segmentation (DASA)
- **Venue**: Research Square (rs-10044069/v1) — **동료평가를 거치지 않은 preprint 서버**
- **Task**: Semantic segmentation (Oxford-IIIT Pet, binary Pascal VOC), U-Net/DeepLabV3/SegFormer-B0

---

## 방법 요약 (검색 스니펫 기반)

- Architecture-agnostic 프레임워크
- Prediction ambiguity + training loss + class rarity + boundary complexity를 결합한 단일 정규화된 **difficulty score**를 sample 단위로 계산
- 이 difficulty score를 sample-specific augmentation 강도로 매핑

---

## 내 연구(Continuous-ONA)와의 관계

### 구조적 유사성

| 항목 | DASA | Continuous-ONA |
|------|------|-----------------|
| 핵심 메커니즘 | Continuous difficulty score → continuous augmentation 강도 | Continuous radius/observability → continuous augmentation 강도 |
| Task | Semantic segmentation | Medical (vessel) segmentation, SSDG |
| 조건화 granularity | **Sample-level** (이미지 전체 단위) | **Intra-image, intra-class, structure-level** (같은 이미지 안의 혈관마다 다름) |
| Difficulty/observability 신호 | Prediction ambiguity, loss, class rarity, boundary complexity (model-dependent, training 중 변화) | Source annotation에서 계산되는 local radius (model-independent, 고정) |
| SSDG 여부 | 아님 (일반 semantic segmentation, in-domain) | SSDG (domain shift 명시적 대상) |

### 결정적 차이

1. **Granularity**: DASA는 "이 이미지 전체를 얼마나 세게 augment할지"를 결정하는 sample-level 방법이다. 내 방법은 "이 이미지 안에서도 이 혈관 부분은 세게, 저 혈관 부분은 약하게"를 결정하는 **intra-image, structure-level** 방법이다. 이는 근본적으로 다른 문제 스케일이다.
2. **신호의 성격**: DASA의 difficulty score는 모델의 현재 예측/loss에 의존하는 **model-dependent, dynamic** 신호다. 내 radius/observability는 annotation에서 한 번 계산되는 **model-independent, static** 신호다. 이는 SSDG에서 특히 중요한 차이: model-dependent 신호는 학습 초기/후기에 따라 다르게 작동할 수 있고, target domain 없이 학습되는 SSDG 세팅에서 재현성/해석 가능성이 떨어진다.
3. **Venue 신뢰도**: Research Square는 동료평가 전 preprint 서버이며, 방법론의 엄밀성이 검증되지 않았다. 인용은 하되 "unreviewed preprint"임을 명시해야 한다.

### 인용 활용 방향

> "DASA (unreviewed preprint) proposes mapping a continuous difficulty score to continuous augmentation strength in semantic segmentation, but at the whole-sample level using model-dependent signals (prediction ambiguity, loss). Continuous-ONA instead operates at the intra-image, intra-class structural level, using a static, annotation-derived local radius signal that does not depend on model state — a distinction that matters specifically for single-source domain generalization, where no target-domain feedback is available to calibrate a dynamic difficulty signal."

---

## 메모

- 동료평가 전 preprint이므로 최상위 추천 목록에는 포함하지 않음 (규칙 준수)
- Peer review 통과 여부를 다음 run에서 추적할 것
- "sample-level 대 structure-level" 구분을 IDEA_COMPARISON_TABLE의 비교 축("Aug. Target")에 명시적으로 반영함
