# Literature Watch Report — Run #8

> 날짜: 2026-07-08 (직전 실행 2026-06-03로부터 약 5주 경과)
> 모델: claude-sonnet-5
> 실행 방식: 5개 병렬 리서치 에이전트 (Cat A / Cat B / Cat C / Cat D / 기준논문 후속탐색)
> 신규 논문: **9편** (Preprint Only 8편 + Published Journal Article 1편)

---

## 요약

이번 Run에서는 5주의 공백기 동안 특히 **vessel radius/thickness를 training 신호로 활용하는 논문이 빠르게 늘어나는 추세**를 확인했다. 3개의 독립 리서치 에이전트가 공통으로 발견한 **MorVess**를 비롯해, radius 개념을 다루는 논문이 이번 Run에서만 3편(MorVess, MARVEL, AC2RUNet 인접) 발견되었다. 이는 "vessel radius가 training에 유의미한 신호"라는 내 핵심 전제를 뒷받침하는 방향이지만, 동시에 **아직 아무도 radius를 augmentation 강도 조절에 사용하지 않았다**는 gap이 계속 좁아지고 있음을 시사한다 — 이번 실행 결과를 반영해 조기에 논문화를 진행할 필요성이 커졌다.

핵심 발견:
1. **MorVess** (arXiv 2606.24214) — vessel thickness map을 auxiliary supervision으로 사용 (loss 계열, AG-TAL과 유사 계열)
2. **MARVEL** (arXiv 2605.25363) — Murray's Law 기반 radius 관계를 topology estimation에 활용 (생물물리학적 근거)
3. **WaveSDG** (arXiv 2603.28463) — Fundus(망막혈관) SSDG 직접 경쟁, feature-space wavelet 분리
4. **TopBrain Challenge** (medRxiv, 2026-05) — TopCoW를 whole-brain으로 확장, vessel caliber measurement 포함 벤치마크

**Novelty gap은 여전히 유지된다**: radius/thickness를 loss weighting(AG-TAL), auxiliary supervision(MorVess), topology consistency(MARVEL)에 활용한 논문은 늘고 있으나, **augmentation strength 조절에 사용한 논문은 이번 Run에서도 발견되지 않았다.**

---

## Category A/C — 신규 혈관·구조 특화 논문 (Radius 관련, 최우선 주의)

### MorVess — arXiv 2606.24214 ⚠️ 3개 에이전트 교차 확인

**논문**: MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network
**Venue**: arXiv, 2026-06-23
**Status**: Preprint Only

#### 방법 요약
- Vessel mask + distance map + **Vessel Thickness Map(VTM)**을 joint 예측
- VTM: centerline 따라 maximal inscribed sphere radius를 propagate하여 계산 — **AG-TAL의 skeleton distance transform과 본질적으로 동일한 계산 원리**
- 2.5D adapter로 3D context + 2D SAM feature 결합
- Cross-dataset 실험 포함 (Parse2022→HiPaS, AIIB2023→ATM2022)이나 SSDG로 formalize 하지는 않음

#### 내 방법과의 관계
**공통점**: radius/thickness를 training 신호로 명시적으로 활용
**핵심 차이**: MorVess = auxiliary supervision target (loss 계열), 나 = augmentation budget 조절. Pulmonary CT (TOF-MRA 아님)
**Novelty 위협도**: Medium — 직접 충돌은 없으나, "radius-aware training" 그룹이 빠르게 채워지고 있어 내 gap을 좁히는 신호. → paper_notes/MORVESS.md 참고

---

### MARVEL — arXiv 2605.25363 (창밖이나 강한 연관성으로 수록)

**논문**: Universal Murray's Law-informed Vessel Tree Segmentation and Topology Estimation
**Venue**: arXiv, 2026-05
**Status**: Preprint Only

#### 방법 요약
- Murray's Law (parent/daughter vessel radius 관계, r_parent³ = Σr_daughter³)를 topology estimation의 consistency constraint로 사용
- 여러 modality/anatomy에 일반화되는 "universal" vessel 표현을 목표

#### 내 방법과의 관계
**공통점**: radius가 vessel 구조의 핵심 신호라는 전제 공유
**핵심 차이**: MARVEL = topology consistency (구조 예측 정합성), 나 = appearance augmentation budget
**활용 가치**: 생물물리학적 근거(Murray's Law)를 내 observability score의 이론적 정당성 강화에 인용 가능 → paper_notes/MARVEL.md 참고

---

### AC2RUNet — arXiv 2606.12319

**논문**: Anatomically Conditioned Recurrent Refinement for Topology-Aware Circle of Willis Segmentation
**Venue**: arXiv, 2026-06
**Status**: Preprint Only

#### 방법 요약
- Static Stream(불변 해부학적 feature) + Dynamic Stream(반복적 topology 오류 정제)으로 분리
- TopCoW에서 Hausdorff Distance 9.17mm → 4.72mm, Betti number error 0.40 → 0.19로 개선

#### 내 방법과의 관계
같은 CoW/cerebrovascular 영역이나 radius conditioning 없음. Cross-domain generalizability는 future work로 명시. Novelty 충돌 낮음, 벤치마크/비교 대상으로 참고.

---

### TopBrain Segmentation Challenge — medRxiv (2026-05-28/30)

**논문**: TopBrain Segmentation Challenge for Whole Brain Vessel Anatomy
**Venue**: medRxiv
**Status**: Preprint Only

#### 방법 요약
- TopCoW를 whole-brain vasculature로 확장: 90 volumes, 48 landmark vessel classes (동맥+정맥), CTA+MRA
- **Vessel caliber measurement along centerline을 whole-brain 규모에서 최초 보고**
- 신규 "contamination" 및 anatomical-plausibility 지표 도입

#### 내 방법과의 관계
직접 방법론적 경쟁은 아니나, 내 TOF-MRA 평가에 사용 가능한 **radius-aware 벤치마크/데이터셋 후보**. Zenodo 16878417에서 데이터 접근성 확인 필요.

---

## Category A — 신규 SSDG 직접경쟁 논문

### WaveSDG — arXiv 2603.28463 (창밖이나 SSDG 혈관 직접경쟁이라 수록)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation
**Venue**: arXiv, 2026-03/04
**Status**: Preprint Only

#### 방법 요약
- Wavelet 기반 "WISER" 모듈로 encoder feature를 sub-band 분해 → anatomical structure(topology) vs. domain appearance(style)로 분리
- Fundus(망막혈관형 얇은 구조) 대상 SSDG

#### 내 방법과의 관계
**공통점**: 얇은 혈관형 구조의 SSDG라는 동일 문제 설정
**핵심 차이**: WaveSDG = feature-space wavelet 기반 uniform 분리 (구조 크기와 무관하게 동일 처리), 나 = image-space에서 vessel radius에 따라 continuous하게 강도 조절
**Novelty 위협도**: Medium — 같은 문제의 대안적 해법이나 mechanism이 명확히 다름. Related work 비교 필수.

---

### vesselFM-CT — arXiv 2606.09400

**논문**: vesselFM-CT: Segmenting All Blood Vessels in CT Images for System-Level Cardiovascular Analysis
**Venue**: arXiv, 2026-06-08
**Status**: Preprint Only

vesselFM(기존 인덱스 VESSELFM)의 후속작. TubeLoss로 대혈관~미세혈관까지의 극단적 크기 이질성을 처리 — **loss-level scale handling** 사례로 내 augmentation-level 접근과 대조되는 참고 사례.

---

### LIVAS-Net — Electronics (MDPI), 2026

**논문**: LIVAS-Net: A Parameter-Efficient 3D Architecture for Intracranial Artery Segmentation in TOF-MRA
**Venue**: Electronics (MDPI), Vol 15 Issue 11, Article 2450
**Status**: Published Journal Article

내 정확한 modality(TOF-MRA)/task이지만 efficiency(3D Ghost convolution) 중심이며 DG를 다루지 않음. Baseline 아키텍처 참고 수준.

---

## Category B — 신규 방법론 유사 논문

### BTECF — arXiv 2605.13015

**논문**: A General Bézier Tree Encoding Counterfactual Framework for Retinal-Vessel-Mediated Disease Analysis
**Venue**: arXiv, 2026-05
**Status**: Preprint Only

망막 혈관을 interconnected cubic-Bézier segment tree로 인코딩 + diffusion 기반 counterfactual (tortuosity/caliber 축 do-intervention, 배경 텍스처는 고정). Disease biomarker 분석이 목적이며 DG 학습용 augmentation은 아니지만, Bézier 기반 구조-보존 counterfactual generation의 최신 사례로 related work에 인용 가치 있음.

---

## Category C — 기타 낮은 관련성

### SemanticVessel — arXiv 2606.21756

Intracranial vessel(CTA) fine-grained annotation 데이터셋 논문 (20 arterial classes, 4D-CTA 활용). 방법론 논문이 아니므로 낮은 우선순위.

---

## 검토했으나 미수록 (venue/관련성 미달)

- **DASA** (Research Square, non-peer-reviewed) — difficulty-proportional augmentation (hard sample = more augmentation). 내 방법과 정반대 논리 → 향후 motivation section에서 "naive difficulty-proportional augmentation이 왜 thin vessel에는 부적절한가"를 논증하는 대조군으로 활용 가능하나, 정식 출판이 아니라 인덱스에는 미수록.
- **FORGERY_SHORTCUT_SUPP** (딥페이크 탐지) — 의료영상 아님, 미수록
- **ADVAUG_GARLIC** (농업공학 저널) — CV venue 아님, 미수록
- **TOPOTTA_ANOM, TOPOAGENT** — 이름 충돌(기존 TopoTTA와 무관) 및 tangential, 미수록

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation strength"
- "continuous radius-conditioned augmentation budget"
- "intra-class structure-specific nonlinear appearance augmentation"

다만 **radius/thickness를 training 신호로 활용하는 논문의 밀도가 이번 5주 사이 뚜렷이 증가**했다 (MorVess, MARVEL, 그리고 기존 AG-TAL). 이는:
1. 내 핵심 전제("vessel radius는 training에 의미 있는 신호")를 강하게 뒷받침하는 유리한 흐름
2. 동시에 "augmentation에는 아직 적용되지 않았다"는 gap이 향후 몇 달 내 다른 그룹에 의해 채워질 위험이 커지고 있음을 시사 — **논문화 우선순위를 높일 필요**

---

## 다음 Run 우선 탐색 항목

- [ ] MorVess/MARVEL 전문 독해: radius 계산식 상세, 내 observability score 설계와의 직접 비교
- [ ] WaveSDG 전문 독해: WISER module이 기존 인덱스(MixStyleFlow, GRAPHSEG 등)와 완전히 구분되는지 재확인
- [ ] TopBrain Challenge 데이터 접근성 확인 (Zenodo 16878417) — 평가 벤치마크로 활용 가능성
- [ ] AG-TAL, DCON, AADG, ADA, MBFCV 등 기존 기준 논문의 인용 전파 재탐색 (아직 시간 부족으로 후속 논문 미발견)
- [ ] "radius-conditioned augmentation" 계열 논문이 다음 실행에서 나타나는지 최우선 모니터링
