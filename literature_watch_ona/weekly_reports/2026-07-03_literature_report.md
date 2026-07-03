# Literature Watch Report — Run #8

> 날짜: 2026-07-03
> 모델: claude-sonnet-5
> 직전 실행: 2026-06-03 (약 1개월 공백)
> 신규 논문: **33편** (Published Journal Article 14편 + Preprint Only 15편 + Accepted Conference Paper 3편 + Official Proceedings Paper 1편)

---

## 요약

이번 Run은 Category A/B/C/D를 독립 subagent로 병렬 조사하는 방식으로 진행했다. 가장 중요한 발견은 **TSIAA (IEEE TMI 2026)**로, "이미지 내 서로 다른 구조에 균일한 augmentation 규칙을 적용하면 안 된다"는 문제의식을 명시적으로 다룬 published journal paper다. 이는 Continuous-ONA의 핵심 문제의식과 거의 동일한 문장으로 표현되어 있어, 현재까지 발견된 논문 중 가장 강한 novelty 충돌 후보로 판정했다.

일반 vision 영역에서는 **IPF-RDA**(local importance score 기반 continuous augmentation 조절)와 **DASA**(segmentation에서 continuous difficulty score 기반 augmentation 조절, 미검증 preprint)가 mechanism 구조상 Continuous-ONA와 가장 근접했다. 두 논문 모두 조건화 신호가 "anatomical/geometric radius"가 아니라 "학습된 중요도/난이도"라는 점에서 명확히 구분되지만, related work에서 반드시 인용하고 차별화해야 한다.

---

## Category A — 신규 직접경쟁 논문 (13편)

### ⚠️ TSIAA — IEEE TMI 2026 (최우선)

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776, 2026 (early access Sept 2025)
**Status**: Published Journal Article
**DOI**: 10.1109/TMI.2025.3605162
**Code**: https://github.com/Wangzs0228/TSIAA

Instance-level Image Augmenter가 여러 learnable constrained Bézier 모듈로 구성되어, 이미지 내 서로 다른 구조(instance)마다 다른 augmentation 파라미터를 adversarial teacher-student loop로 학습한다. "breaks the uniformity of augmentation rules across different structures within an image"라는 저자들의 표현이 Continuous-ONA의 핵심 주장과 사실상 동일하다.

**잠정 차이** (전문 미확인, 다음 세션 최우선 과제):
- 강도 결정 메커니즘: adversarial optimization(학습, black-box) vs. 내 방법(source annotation에서 계산되는 local radius, 명시적·결정론적)
- Fragile structure 보호라는 명시적 동기 유무 불명 — adversarial 방향이 "어려운 구조에 더 강한 aug"일 가능성도 있어, 내 방법과 정반대 방향일 수 있음
- Instance 정의(개별 장기 단위인지, 혈관 내부 위치별 세분화까지 포함하는지)가 불명

**Novelty 위협도**: **High** — 전문 확인 전까지 확정 불가. paper_notes/TSIAA.md 참조.

### FSDA-DG — Medical Image Analysis 2025

Global broad region과 semantics-guided local region으로 나누어 서로 다른 augmentation을 적용하는 binary 2-tier 방식(arXiv 2311.02583). ICRN, AGTA와 함께 "binary region-split augmentation" 계열로 분류하여 내 continuous radius conditioning과 대조.

### 기타 신규 (11편)
WISER(wavelet 기반 SSDG fundus), Mamba-Sea(TMI, global-to-local 2-tier augmentation), SDCL(causal style deconfounding), ADDGCL(small organ adaptive loss weighting), RLAD(retinal vessel layout-aware generation), CQINV/GENEVAL/NEUROVASCU/PMDG/FGMLDG/CONDISR(낮은 관련성, P3 등록)

---

## Category C — 신규 혈관·구조 특화 논문 (12편)

### MorVess — arXiv 2606.24214 (2026)

Pulmonary vessel segmentation에서 vessel mask + distance map + **thickness map**을 jointly 예측. Local vessel thickness를 명시적 first-class training signal로 사용한다는 점에서 ONA와 방향은 유사하나, radius를 **supervision target**으로 쓴다는 점(loss 관점)에서 ONA의 **augmentation conditioning**(radius를 aug 강도 조절에 사용) 접근과 명확히 구분된다. paper_notes/MORVESS.md 참조.

### SEMIR — ECCV 2026

Pixel lattice 대신 thin-structure connectivity를 보존하는 parameterized graph minor 표현. Power line/crack/lane marking 등 cross-domain thin-structure에서 단일 파이프라인으로 검증한 top-tier venue 논문. Representation-level topology 보존이며 augmentation과 무관.

### TOF-MRA/cerebrovascular 동일 도메인 논문 (2편)

- **EI-Seg** (Medical Image Analysis 2026): MIP 기반 tri-plane 표현으로 효율적 3D cerebrovascular segmentation — 내 실험 도메인과 정확히 일치, DG 논문은 아니나 baseline/평가 참고용
- **LIVAS-Net** (Electronics/MDPI 2026): TOF-MRA intracranial artery segmentation, parameter-efficient 3D architecture

### 기타 신규 (8편)
FGOS-Net, MARVEL(Murray's Law), VesselTok, Topology-Guaranteed Segmentation(SIAM), CoroBench(MICCAI 2026), Coronary 3-stage(MedIA 2025), PulmTree, AirwayMulti

---

## Category D — 신규 Top-tier Vision 논문 (8편)

### IPF-RDA — arXiv 2509.16678 (TPAMI 심사 중 추정)

일반 vision(classification/re-ID)에서 local region의 class-discriminative "importance score"로 augmentation 강도를 continuous하게 조절하는 information-preserving 프레임워크. **Mechanism 구조가 Continuous-ONA와 가장 유사한 general-vision 논문**. 차이: task(classification vs. segmentation), 조건화 신호(학습된 discriminativeness vs. 물리적 vessel radius), 보호 방향(중요한 것 보호 vs. 취약한 것 보호 — 서로 다른 축). paper_notes/IPFRDA.md 참조.

### DASA — Research Square (미검증 preprint)

Segmentation에서 continuous difficulty score(prediction ambiguity + loss + class rarity + boundary complexity) → continuous augmentation 강도 매핑. Segmentation 세팅에서는 가장 근접한 mechanism이나 sample-level(전체 이미지 단위) 조건화이고 동료평가 전 preprint. paper_notes/DASA.md 참조.

### SensAug — ICML 2025

모델 sensitivity 측정 기반 augmentation severity 정책(arXiv 2406.01425). "모델 민감도가 augmentation 강도를 결정한다"는 메타 원칙은 유사하나 조건화 신호가 anatomical structure가 아닌 global per-corruption-type sensitivity. Ablation 설계 참고용(radius-conditioning이 실제 thin-vessel sensitivity와 상관관계가 있는지 검증하는 방법론 차용 가능).

### 이론적 배경 논문 (2편)
- **Flat Minima Perspective on Augmentations** (AAAI 2026): label-preserving augmentation이 robustness를 개선하는 이유를 loss landscape flattening으로 설명 — "왜 thin vessel에 강한 aug를 주면 안 되는가"에 대한 이론적 근거로 motivation 섹션에 활용 가능
- **Geometry of Invariant Learning** (arXiv 2602.14423): augmentation의 generalization gap을 정보이론적으로 분해

### 기타 신규 (3편)
Sample-aware RandAugment(IJCV 2025), SADA(gradient-guided influence estimation), A3MDA(hardness-driven UDA augmentation)

---

## Novelty Gap 재확인

이번 Run에서 처음으로 "이미지 내 서로 다른 구조에 비균일 augmentation을 적용해야 한다"는 문제의식을 **명시적으로 공유하는 published journal paper(TSIAA)**를 발견했다. 그러나 여전히 다음 조건을 **모두** 만족하는 논문은 발견되지 않았다:

- Segmentation (dense prediction) 세팅
- Single-source domain generalization
- 하나의 foreground class(vessel) **내부**에서
- **연속적인, source annotation에서 직접 계산되는 물리적 radius/observability**로
- Augmentation **강도**를 조절하며
- **얇은 구조를 보호하는 방향**(fragile structure protection)이 명시적 목표

TSIAA, IPF-RDA, DASA는 각각 이 조건 중 일부만 만족한다. 다음 세션에서 TSIAA 전문 확인이 완료되면 이 gap 진술을 다시 검증해야 한다.

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA 전문 확인 (최우선)**: instance 정의, augmentation 강도 결정 메커니즘, thin structure 관련 실험 유무
- [ ] "Hallucinated DG Network" venue 불일치 해소 (기존 HALLUDG 항목과 동일 논문 여부 재확인)
- [ ] IPF-RDA TPAMI 심사 결과 추적
- [ ] DASA 동료평가 통과 여부 추적
- [ ] MICCAI 2026 정식 accepted list 공개 시 재탐색
