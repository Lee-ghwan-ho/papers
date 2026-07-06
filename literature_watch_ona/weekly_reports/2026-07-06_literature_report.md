# Literature Watch Report — Run #8

> 날짜: 2026-07-06
> 모델: claude-sonnet-5 (4개 병렬 서브에이전트: Lane A+D, Lane B, Lane C, Lane 5 follow-up)
> 신규 논문: **10편** (Published Journal 2편 + Accepted Conference 1편 + Preprint Only 7편)

---

## 요약

Run #7(2026-06-03) 이후 약 한 달간의 신규 문헌을 4개 병렬 에이전트로 조사했다.
이번 Run의 가장 중요한 발견은 **TSIAA (IEEE TMI 2026)** — instance 단위로 augmentation을
다르게 적용하는 SSDG 논문으로, 지금까지 조사한 논문 중 내 Continuous-ONA의 핵심 motivation
("이미지 내에서 augmentation을 균일하게 적용하면 안 된다")과 **가장 근접한 선행 연구**다.
다만 conditioning 메커니즘이 adversarial policy learning이며, vessel radius/observability 같은
명시적 형태학적 신호를 사용하지 않는 것으로 보인다 (원문 미확인, 검증 필요).

그 외에는 vessel radius/thickness를 **loss/supervision**에 사용하는 논문(MorVess, vesselFM-CT,
MARVEL)이 계속 발견되고 있으나, **augmentation strength에 continuous하게 사용하는 논문은
여전히 없다** — 핵심 novelty gap 유지.

---

## ⚠️ 최우선 신규 논문: TSIAA (IEEE TMI 2026)

**논문**: Teacher–Student Instance-Level Adversarial Augmentation
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776
**Status**: Published Journal Article (early access ~2025-09-02)
**DOI**: 10.1109/TMI.2025.3605162

### 방법 요약 (2차 출처 기반, 원문 미확인)

- Learnable constrained **Bézier 변환**을 사용하는 **Instance-level Augmentation Module (IAM)**
- 이미지 내 서로 다른 구조/instance마다 **다른 augmentation을 적용**
- **Teacher-student adversarial loop**으로 augmentation 정책을 학습
- Single-source DG, target domain 정보 미사용
- 4개의 SDG segmentation task에서 평가

### 내 방법과의 관계

**공통점**: "전체 이미지에 uniform augmentation을 적용하면 안 된다"는 문제의식을
정확히 공유하는 첫 번째 논문. Single-source, no target info 세팅도 동일.

**핵심 차이 (잠정, 검증 필요)**:
- TSIAA = **adversarially learned** instance policy (무엇을, 얼마나 augment할지 학습으로 탐색)
- Continuous-ONA = **명시적으로 측정된 vessel radius/observability**에 대한 continuous 함수
- TSIAA에 vessel 고유의 "관찰 가능성" 개념이 있는지 불명확
- TSIAA는 adversarial training 필요 (추가 학습 복잡도), ONA는 loss 불변 + augmentation만 조절하는 lightweight POC

**Novelty 위협도**: **Medium-High (원문 확인 전까지 보수적으로 평가)**. IAM의 "instance" 정의와
augmentation 강도 결정 방식을 원문에서 확인하기 전까지는 확정적 결론 유보.
이번 사이클 최우선 정독 대상 (`paper_notes/TSIAA.md` 참고).

⚠️ **네트워크 제약으로 원문 PDF를 직접 확인하지 못함** (arxiv.org/IEEE Xplore 접근 차단).
다음 실행에서 원문 확보를 최우선 과제로 재시도할 것.

---

## Category C — 신규 혈관·구조 특화 논문

### MorVess (arXiv 2606.24214, 2026-06-23)

Pulmonary vessel segmentation. **Vessel Thickness Map(VTM)** — medial-axis propagation으로
계산되는 continuous thickness map — 을 mask/distance map과 함께 auxiliary supervision target으로 예측,
diameter smoothness와 centerline consistency 강제. Cross-domain 평가 포함 (Parse2022/AIIB2023 →
HiPas/ATM2022 zero-shot).

**관계**: Continuous vessel thickness를 사용한 최신 사례이지만 **augmentation이 아닌 loss/supervision**
측면. "radius가 유용한 continuous 신호"라는 독립적 지지 근거로 활용 가능. Augmentation gap 유지.

### TopBrain Segmentation Challenge (medRxiv, 2026-05-28)

TopCoW를 whole-brain 48-class 혈관으로 확장한 새 challenge/benchmark. CTA+MRA 90 volumes,
**per-vessel caliber(반지름) ground truth** 포함. Menze lab 계열. Challenge report: "smaller and
more complex vessels remain the true bottleneck" — 내 motivation을 독립적으로 재확인.

**관계**: 직접적인 방법론 논문은 아니나, 내 TOF-MRA 연구와 해부학적으로 밀접한 신규 평가
리소스. 향후 벤치마크 활용 가능성 검토 가치 있음.

### vesselFM-CT (arXiv 2606.09400)

CVPR 2025 vesselFM의 CT 확장판. **TubeLoss**로 대동맥부터 미세 mesenteric branch까지의 radius
이질성을 처리하는 iterative training scheme.

**관계**: Radius를 loss 설계 축에서 다룸. Foundation model paradigm으로 내 lightweight SSDG와
데이터 요구사항이 근본적으로 다름.

### CSWinUNETR (arXiv 2606.19824, MICCAI 2026 accepted)

Cross-shaped stripe self-attention + dynamic snake convolution 기반 thin anatomical structure
분할 아키텍처 (retinal vessel, cerebral vasculature, thin facial structure).

**관계**: 순수 아키텍처 논문 — augmentation/DG 실험 없음. Thinness를 attention geometry로
처리, radius conditioning 없음.

### TubeMLLM (arXiv 2603.09217)

Vessel-like anatomy를 위한 topology-aware multimodal foundation model (language-based
topological prior + segmentation/generation).

**관계**: Topology-first foundation model 방향. Augmentation과 무관.

### MARVEL (arXiv 2605.25363)

Murray's Law 기반 physics-informed loss — parent/daughter vessel radius bifurcation 관계를
radius-specific exponent로 모델링. Multi-source/multi-modality (8개 데이터셋).

**관계**: AG-TAL과 같은 축 (radius → loss weighting/constraint). Augmentation 축과는 직교.

---

## Category A/B — 신규 방법론 논문

### WaveSDG (arXiv 2603.28463, ~2026-03)

Wavelet sub-band decomposition으로 anatomical structure와 domain-specific appearance를 분리하는
SSDG 방법 (fundus, vessel 포함).

**관계**: SLAug/RASS의 Bezier/frequency perturbation과는 다른 구조 분리 전략. Radius conditioning 없음.

---

## Category D — 신규 Top-tier Vision 논문 (참고용)

### GPDG (Frontiers of Computer Science, 2026-06-15)

Domain을 latent "environment" 분포의 샘플로 재정의하는 이론적 DG 프레임워크
(Gaussian Process 기반 meta-function 학습).

**관계**: 의료영상/증강과 무관. "단일 invariant mapping의 한계"라는 이론적 motivation
인용 후보 (Low relevance).

### Low-Frequency Shortcuts in Texture-Driven Visual Learning (arXiv 2606.03493)

Texture-driven domain에서 low-frequency shortcut 의존성 분석 — texture-driven 분류 문제에서
low-freq component pruning이 OOD robustness를 개선함을 보임.

**관계**: 내 "thick vessel의 intensity shortcut 억제" motivation을 지지하는 방계 근거로 활용 가능
(Low-Medium relevance).

---

## Novelty Gap 재확인

- **"continuous radius/observability-conditioned augmentation strength"**: Run #8에서도 명시적으로
  이를 구현한 논문 없음.
- 가장 근접한 두 축:
  1. **TSIAA** — instance-level differential augmentation (adversarial policy, radius 미사용)
  2. **MorVess / MARVEL / vesselFM-CT** — vessel radius를 loss/supervision에 사용 (augmentation 미사용)
- 두 축 모두 내 방법과 정확히 겹치지 않는다. **핵심 gap 유지** — 단, TSIAA는 mechanism이
  다르더라도 "동일 문제의식"을 다루는 첫 사례이므로 Related Work에서 비중 있게 다뤄야 함.

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA 원문 PDF 확보 및 정독** — instance 정의, IAM 강도 결정 메커니즘, ADA(MICCAI25)와의 관계
- [ ] TopBrain 데이터셋 접근성 및 caliber ground truth 포맷 확인
- [ ] MorVess VTM 계산 방식과 내 observability score 계산법 비교
- [ ] MICCAI 2026 정식 accepted list 공개 시 (통상 7~8월) 재탐색
- [ ] arXiv/IEEE Xplore 직접 접근이 막힌 네트워크 정책 확인 — 원문 확인 대체 경로 마련 (예: 기관 프록시, 저자 요청)
