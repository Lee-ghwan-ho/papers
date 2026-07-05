# Literature Watch Report — 2026-07-05 (Run #8)

## 요약

- 검색 쿼리: 30개 (Lane A/B/C/D + 5개 기준 논문 follow-up 체인)
- 신규 발견: **8편**
  - Published Journal Article: 2편 (ADVERIN, TOPOWIDTH)
  - Workshop Paper: 1편 (DOMAINFLOW_CORONARY)
  - Preprint Only: 5편 (WAVESDG, RLAD_RETINA, BTECF, CURVSEGFLOW, MTFLOW)
- 마지막 실행(Run #7, 2026-06-03) 이후 약 1개월 공백 탐색

## 가장 중요한 발견: AdverIN (7회 실행 누락 보완)

**AdverIN — Monotonic Adversarial Intensity Attack for Domain Generalization in Medical Image Segmentation**
(Medical Image Analysis, 2025; arXiv 2304.02720; 원 논문은 2023년부터 arXiv에 존재)

- 원본 intensity 순서를 국소적으로 보존하는 **monotonic intensity mapping**을 adversarial하게 학습하는 방법. Code: github.com/NUBagciLab/AdverIN.
- Continuous-ONA가 속한 "monotonic/label-preserving nonlinear appearance transform" 계열의 foundational 논문임에도 불구하고 Run #1–#7 동안 색인되지 않았던 **중대한 문헌 공백**.
- **Continuous-ONA와의 관계**: AdverIN은 이미지 전체에 걸쳐 균일한 강도로 매핑을 적대적으로 최적화한다. Radius/observability 등 intra-class 구조 신호에 따른 조건화는 전혀 없다. 즉, novelty를 위협하지 않지만 — "monotonic nonlinear intensity transform이 이미 SSDG에서 효과적인 baseline"이라는 사실을 뒷받침하므로, Continuous-ONA는 이 baseline 대비 **thin vessel 보존 효과의 개선**을 실험적으로 입증해야 한다.
- **조치**: Related Work 필수 인용 + baseline 비교 후보로 즉시 전문 정독 (READING_QUEUE P1 등재).

## 기타 신규 발견 (Novelty 충돌 낮음, 참고용)

| KEY | 요지 | 관계 |
|-----|------|------|
| BTECF | 망막 혈관을 Bézier segment tree로 인코딩해 질병 counterfactual 생성 | "vessel segment를 독립 perturbation 단위로 인코딩"하는 기술적 아이디어가 유사하나, 목적(질병 설명)과 축(기하학적 형태)이 다름 |
| WAVESDG | Wavelet sub-band 분해 기반 SSDG (fundus) | 전역 wavelet-band 분리, intra-class 조건화 없음 |
| RLAD_RETINA | Layout-preserving diffusion 생성 augmentation (retinal vessel DG) | Vessel 구조 전체 보존 + 나머지 다양화 — thin/thick 구분 없음 |
| CURVSEGFLOW / MTFLOW | Curvilinear/microtubule 구조의 noise-robust flow-matching segmentation | Architecture/inference 방법, DG/augmentation과 무관하나 "thin structure의 불균형적 취약성" 동기 보강 |
| TOPOWIDTH | Vessel width를 topological energy 제약에 포함하는 loss/PDE 방법 | Mechanism이 loss/constraint이며 augmentation과 무관 |
| DOMAINFLOW_CORONARY | AngioDG의 동일 저자 선행 workshop 논문 (STACOM 2024) | 이미 색인된 AngioDG로 superseded, 계보 기록용 |

## Novelty Gap 재확인

**"Intra-class continuous vessel radius/observability 기반 augmentation budget"** — Run #1부터 Run #8까지 8회 연속 탐색에서 직접 명시한 논문 없음. AG-TAL(loss weighting), L2CP(test-time thin vessel 제거), DCON/ADA(per-sample adaptivity), AdverIN(uniform monotonic mapping), BTECF(counterfactual segment 조작) 모두 인접한 축에서 근접하지만, "continuous radius → augmentation strength, source-only, training-time, label-preserving"의 결합은 여전히 비어 있다.

## 다음 실행 우선 확인 사항

- [ ] AdverIN 전문 정독: mask 연산 정의, 적대적 최적화 절차 상세, baseline 실험 설계에 포함
- [ ] BTECF Bézier segment encoding이 observability score parameterization에 참고 가능한지 확인
- [ ] MICCAI 2026 / CVPR 2026 accepted paper list 공개 시 재탐색
