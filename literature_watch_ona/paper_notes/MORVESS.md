# MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network

> **KEY**: MORVESS
> **Venue**: arXiv 2606.24214 (June 23, 2026) — Preprint Only
> **Status**: Preprint Only
> **Category**: C (구조·혈관특화)
> **Relevance**: High
> **Novelty 충돌**: Low-Medium — mechanism(loss/supervision vs. augmentation)과 modality(pulmonary CT vs. TOF-MRA)가 다르지만, radius 계산 방식이 겹침

---

## 논문 기본 정보

- **제목**: MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network
- **저자**: Fuyou Mao, Yifei Chen, Beining Wu, Lixin Lin, Jinnan Dai, Zhiling Li, Yilei Chen, Yaqi Wang, Hao Zhang, Yan Tang, Huiyu Zhou, Feiwei Qin
- **문제**: Pulmonary vessel segmentation — 특히 small/peripheral vessel의 detection 및 continuity
- **Task**: Supervised segmentation + cross-dataset generalization 실험 포함 (완전한 SSDG 세팅은 아님)
- **Data**: Parse2022 → HiPaS venous subset, AIIB2023 → ATM2022 peripheral subset (cross-domain 평가)

---

## 핵심 방법

1. **Joint prediction**: vessel mask + distance map + **Vessel Thickness Map (VTM)**을 동시 예측
2. **VTM 계산**: centerline(skeleton)을 따라 maximal inscribed sphere radius를 propagate하여 각 voxel의 국소 두께를 추정 — **AG-TAL의 skeleton distance transform과 본질적으로 동일한 radius 계산 원리**
3. **아키텍처**: 2.5D adapter로 3D context와 2D SAM feature를 연결, global-local fusion block
4. **목적**: centerline consistency + smooth diameter transition을 auxiliary task로 학습 → small vessel recovery 향상

---

## 실험 결과 (요약)

- Dice/clDice/HD95에서 baseline 대비 개선, 특히 small vessel recovery
- Cross-domain 평가(Parse2022→HiPaS, AIIB2023→ATM2022)에서도 일관된 개선 — domain shift 하에서도 thickness-aware supervision이 도움이 됨을 시사

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점 (주의 필요)

| 항목 | MorVess | Continuous-ONA |
|------|---------|----------------|
| Radius 활용 목적 | auxiliary supervision (thickness map 예측) | augmentation budget conditioning |
| Radius 계산 방식 | centerline propagate maximal inscribed sphere | (동일 계열 방식 사용 가능 — observability score 구현 참고) |
| 근거 | "vessel thickness 정보가 topology/continuity에 중요" | "vessel thickness가 label-image consistency에 중요" |
| Cross-domain 언급 | 실험적으로 확인 (하지만 SSDG framing 없음) | SSDG를 정식 문제 설정으로 다룸 |

### 결정적 차이

| 항목 | MorVess | Continuous-ONA |
|------|---------|----------------|
| **Mechanism** | Loss/auxiliary supervision target (모델이 thickness map을 예측하도록 학습) | Augmentation budget (input-space perturbation 강도 조절) |
| **Modality** | Pulmonary CT | TOF-MRA cerebrovascular |
| **DG framing** | Cross-dataset 실험은 있으나 SSDG로 formalize 안 함 | SSDG로 명시적으로 설정 |
| **Fragile structure 보호** | 없음 (모든 구조를 동일하게 잘 예측하려는 supervision) | 명시적으로 얇은 구조의 augmentation을 약화 |

---

## 내 연구에서의 활용

1. **Related Work 인용 근거 강화**: "radius/thickness 정보를 training에 활용한 선행 연구가 최근 급증하고 있음 (AG-TAL: loss weighting, MorVess: auxiliary supervision, MARVEL: topology estimation) — 그러나 이들 모두 **augmentation**에는 적용되지 않았다"는 gap 주장을 3개 논문으로 뒷받침 가능
2. **Observability score 계산 방법론 참고**: MorVess의 VTM 계산식(centerline propagate maximal inscribed sphere radius)은 내 ONA의 local radius/observability score 구현 시 baseline 계산법으로 활용 가능
3. **Cross-domain 실험 설계 참고**: Parse2022→HiPaS, AIIB2023→ATM2022 같은 cross-dataset 평가 프로토콜이 내 TOF-MRA multi-center 평가 설계에 참고가 될 수 있음

---

## 인용 전략

Related Work에서 다음과 같이 포지셔닝:

> "Recent work has increasingly recognized the value of vessel radius/thickness information during training — as loss weighting (AG-TAL), as an auxiliary supervision target (MorVess), or as a topological consistency prior (MARVEL). However, none of these leverage radius/observability to modulate the *augmentation* process itself, leaving open the question of how much appearance perturbation a given structure can safely tolerate during single-source domain generalization."

---

## 메모

- 3개의 독립 리서치 에이전트(Cat A, Cat B, Cat C)가 모두 이 논문을 교차 발견 — 검색 신뢰도 높음
- arXiv preprint — venue 확정 시 status 업데이트 필요
- VTM 계산식 수식이 논문에 명시되어 있는지 전문 확인 필요 (현재는 abstract/snippet 기반 요약)
