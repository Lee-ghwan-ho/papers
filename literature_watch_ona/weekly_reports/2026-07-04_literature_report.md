# Literature Watch Report — Run #8

> 날짜: 2026-07-04  
> 모델: claude-sonnet-5  
> 신규 논문: **24편** (Published Journal 4편 + Accepted Conference 6편 + Preprint 14편)  
> 실행 방식: 5개 Search Lane을 병렬 서브에이전트로 동시 수행 (직전 Run #7: 2026-06-03, 약 한 달 공백)

---

## 요약

이번 Run은 지난 실행(Run #7, 2026-06-03) 이후 약 한 달간의 공백을 메우기 위해
5개 Search Lane(직접경쟁 SSDG, 혈관/tubular DG, 증강 방법론, Top-tier Vision,
기준논문 후속연구)을 병렬 서브에이전트로 동시에 수행했다.

**가장 중요한 발견은 TSIAA (IEEE TMI 2026)**로, "이미지 내 서로 다른 구조에
동일한 augmentation rule을 적용하는 것은 최적이 아니다"라는 문장 수준의 주장을
이미 저널 논문으로 출판했다. 이는 지금까지 조사한 논문 중 내 핵심 novelty claim과
가장 근접한 선행연구이며, 최우선으로 전문을 정독하고 차별화 논거를 확정해야 한다.

그 외 ICCV 2025의 **Split-and-Combine**(patch-wise entropy-gated augmentation),
**MSSSeg/StructAug**(구조 복잡도 기반 continuous augmentation 강도 매핑),
**Keep the Core/SAGE-KEEP**(fragility 기반 augmentation 억제)도 mechanism
수준에서 주목할 가치가 있다. 다만 이 네 논문 모두 **radius/observability라는
명시적·해석 가능한 기하학적 신호를 사용하지 않는다**는 점에서, "annotation-derived
continuous vessel radius로 augmentation budget을 조절한다"는 내 핵심 novelty gap은
여전히 유지된다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 경계

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, Issue 2, pp. 764–776  
**Status**: Published Journal Article  
**URL**: https://ieeexplore.ieee.org/document/11146907/  
**Code**: https://github.com/Wangzs0228/TSIAA (검증 필요)

#### 방법 요약
- **핵심 동기**: image-level uniform adversarial augmentation은 suboptimal. Instance-level augmentation이 이미지 내 서로 다른 구조 간 증강 규칙의 균일성을 깨뜨려야 더 큰 다양성을 얻는다.
- **Instance-level Image Augmenter (IIAG)**: 다수의 Instance-level Augmentation Module(IAM), 각각 learnable constrained Bézier transformation
- **Teacher-Student Adversarial Loop**: augmentation search(out-of-source appearance 탐색) ↔ representation learning(teacher-student consistency)을 교대로 수행

#### 내 방법과의 관계 — 상세 비교는 `paper_notes/TSIAA.md` 참조
**공통점**: "구조마다 다른 augmentation rule"이라는 상위 명제, Bézier 기반 nonlinear appearance transform  
**핵심 차이**: 조건 신호가 adversarial하게 학습되는 opaque parameter(TSIAA) vs. annotation에서 직접 계산되는 continuous vessel radius(나). Vessel/tubular 특화 여부 및 thin structure 보호 동기 부재도 차별점.

**Novelty 위협도**: **High** — 즉시 전문/코드 확인 필수.

---

### WaveSDG — arXiv 2603.28463 (2026)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**Status**: Preprint Only

Wavelet sub-band 분해로 anatomical structure와 domain-specific appearance를 분리(WISER 모듈). Fundus(혈관 포함) SSDG 경쟁 방법이나 mechanism이 frequency-domain decoupling으로 내 pixel-space radius-conditioned 접근과 다름. **Novelty 위협도**: Medium.

---

### Revisiting Data Scaling via Topology-Aware Augmentation — arXiv 2511.13883 (2025)

Deformation 기반 topology-aware augmentation이 15개 anatomical task에서 data-scaling law의 error scale을 낮춘다는 실증 연구. 구조 인지 augmentation의 일반적 효용을 지지하는 scaling-law 수준 근거로 인용 가능. **Novelty 위협도**: Low (mechanism 겹침 없음).

---

## Category B — 신규 방법론 유사 논문

### MSSSeg/StructAug — arXiv 2512.23997 (2025)

Differentiable box-counting 기반 multi-scale structural complexity → 이 복잡도에 따라 augmentation 강도를 학습(StructAug) + Persistent Homology Loss. **"Continuous structure signal → continuous aug 강도"라는 내 방법과 동일한 형태의 매핑을 사용하는 유일한 사례** (self-supervised 세팅). 상세는 `paper_notes/MSSSEG.md` 참조. **Novelty 위협도**: High (mechanism 유사, signal/목적/task 상이).

### Keep the Core / SAGE-KEEP — arXiv 2512.15811 (2025)

Adversarial sensitivity 기반 fragility map으로 augmentation을 국소적으로 억제(원본 픽셀 강제 복원). "취약 영역 보호"라는 내 동기와 대칭적. 상세는 `paper_notes/KEEPSAGE.md` 참조. **Novelty 위협도**: High (동기 유사, 신호 정의 상이 — model-centric vs. data-centric).

### BTECF — arXiv 2605.13015 (2026)

Retinal vessel을 Bézier tree로 인코딩해 caliber/tortuosity에 대한 counterfactual do-intervention 수행 (질병 분류기 설명 목적). Radius/caliber 파라미터화 방식이 내 augmentation 설계에 참고 가치. **Novelty 위협도**: Medium (task 완전히 다름 — 분류 설명 vs. segmentation augmentation).

### SADA — arXiv 2510.00434 (2025)

Gradient projection variance(학습 동역학) 기반 per-sample augmentation 강도 조절. Radius 대신 training stability를 관찰가능성 신호로 사용하는 대안적 접근. **Novelty 위협도**: Low-Medium.

---

## Category C — 신규 혈관·구조 특화 논문 (12편)

이번 Run에서 혈관/tubular 구조 관련 논문이 대량으로 발견되었으나, **모두 architecture 또는 loss 설계에 radius/thickness를 사용할 뿐 augmentation budget 조절에는 사용하지 않는다** — 이는 오히려 내 gap을 강화하는 방향의 증거다.

| 논문 | 핵심 내용 | 내 연구와의 관계 |
|------|-----------|-----------------|
| **AC2RUNet** (2606.12319) | CoW segmentation, curriculum + recurrent refinement로 broken thin vessel 문제 해결 | TOF-MRA/CoW 도메인에서 thin vessel 동기 직접 뒷받침 |
| **MorVess** (2606.24214) | Pulmonary vessel, thickness map을 explicit supervision target으로 예측 | Radius를 예측 대상 vs. 내 augmentation 조절 신호 — 상호보완 |
| **MARVEL** (2605.25363) | Murray's law(분기점 radius-flow 관계) biophysical prior | Radius 기반 처리의 생리학적 정당성 근거로 인용 |
| **TopoVST** (2603.14909) | Vessel radius를 GNN skeleton tracking의 조건 신호로 사용 | Radius 계산(sphere graph) 설계 참고 |
| **CSWinUNETR** (2606.19824) | Thin/tortuous 구조 특화 attention backbone | 아키텍처 참고용 |
| **vesselFM-CT** (2606.09400) | TubeLoss로 극단적 vessel scale heterogeneity 처리 | Thick/thin dichotomy를 loss 레벨에서 다룬 사례 |
| **TopoLoRA-SAM** (2601.02273) | SAM+LoRA+clDice, cross-domain 혈관 평가 | Thin-structure + cross-domain 결합 사례 |
| **CorSegRec** (MedIA 2025, 2504.01597) | Coronary artery, post-hoc topology repair로 thin distal vessel 연결 복구 | Thin vessel 연결성 문제의 대안적(post-hoc) 해법 |
| **TPNet** (IEEE TMI 2026) | Pulmonary vessel, tubular-aware prompt-tuning few-shot | Tubular structure를 transfer prior로 활용 |
| **Topology-Guaranteed Seg.** (SIAM J. Imaging Sci. 2026, 2601.11409) | Width(두께)를 topological prior의 1급 속성으로 수학적 정식화 | "두께가 다르면 다르게 취급해야 한다"는 주장의 수학적 지지 근거 |
| **Few-Shot 3D Vessel** (2602.23782) | DINOv3 adapter, few-shot OOD robustness | SSDG vessel 비교군으로 참고 |
| **UniVG** (2604.10737) | 생성 기반 few-shot 2D vascular segmentation data-engine | 합성 데이터 패러다임, 내 방법과 다름 |

---

## Category D — 신규 Top-tier Vision 논문 (6편, 모두 ICCV/ECCV/CVPR 2025-2026)

### Split-and-Combine — ICCV 2025 ⚠️ 주목

Patch-wise 독립 style augmentation + entropy 기반 iterative 강도 조절 + energy 기반 OOD-discrepancy 제어. 공간적으로 국소화된 augmentation 강도 조절이라는 점에서 방법론적으로 가장 가까운 top-tier vision 논문. 상세는 `paper_notes/SPLITCOMBINE.md` 참조. **Novelty 위협도**: Medium-High (조건 신호가 entropy/patch 단위로 vessel 형태와 무관).

### ConstStyle — ICCV 2025

Train/test 샘플을 공통 unified style 공간으로 투영. 강도의 국소적 조절 개념은 없음. **Novelty 위협도**: Low.

### Adapt Foundational Segmentation Models with Heterogeneous Searching Space — ICCV 2025

22개 rule-based + 10개 learning-based 증강 연산의 heterogeneous search space 정의 및 정책 탐색. 내 nonlinear aug family를 여러 연산의 policy-selection으로 확장할 때 참고 가치. **Novelty 위협도**: Low.

### Customizing Domain Adapters for Domain Generalization — ICCV 2025

Domain-specific lightweight adapter 조합. 자연영상 일반 DG, 간접 참고. **Novelty 위협도**: Low.

### SEMIR (ECCV 2026), SCNP (CVPR 2026)

각각 non-medical thin-structure topology preservation(graph minor 표현), 일반 topology accuracy 개선(neighbor-pixel penalization) 논문. 의료/혈관 특화 아니며 augmentation 개념 없음. **Novelty 위협도**: Low.

---

## Novelty Gap 재확인 (Run #8)

- **"radius/observability-conditioned continuous augmentation strength"**: 여전히 직접 명시 논문 없음.
- **근접도 순위**: TSIAA(구조별 비균일 증강, but adversarial opaque 신호) > Split-and-Combine(공간적 국소 강도, but entropy/patch 기반) > MSSSeg(연속 신호→연속 강도 매핑, but fractal complexity·self-supervised)
- 세 논문 모두 "무엇을 조건 신호로 쓰는가"에서 내 방법(annotation-derived continuous vessel radius)과 다르다는 점이 핵심 방어선. TSIAA와의 차별화가 가장 시급함.

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문/코드(github.com/Wangzs0228/TSIAA) 확인 — instance 정의 방식, Bézier 파라미터의 실제 conditioning 신호
- [ ] Split-and-Combine 전문 확인 — entropy 계산 방식, patch 크기, segmentation(특히 thin structure) 실험 포함 여부
- [ ] MSSSeg의 box-counting complexity와 vessel radius의 상관관계 검증 방법 설계
- [ ] DCON (Pattern Recognition 2025) 후속연구 재탐색 (이번 Run에서 검색 실패)
- [ ] AC2RUNet, MorVess, MARVEL, TopoVST 등 radius/thickness-aware 혈관 논문들을 Related Work의 "Motivation 지지 근거" 섹션으로 정리
