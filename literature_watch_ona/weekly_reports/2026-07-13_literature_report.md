# Literature Watch Report — Run #8

> 날짜: 2026-07-13
> 모델: claude-sonnet-5
> 이전 실행: 2026-06-03 (Run #7) — 약 6주 경과
> 신규 논문: **28편** (Published Journal 5편 + Accepted Conference 11편 + Workshop 1편 + Preprint 11편)
> 실행 방식: Category A/B/C/D + Lane 5(기준논문 후속 탐색) 5개 병렬 서브에이전트, 총 70여 개 검색 쿼리

---

## 요약

6주 만의 정기 탐색으로 예년보다 많은 신규 논문이 발견되었다. 가장 중요한 발견은 다음 세 편이다.

1. **TSIAA (IEEE TMI 2026)** — "이미지 전체에 균일한 augmentation을 적용하는 것은 suboptimal하다"는
   상위 프레이밍이 내 핵심 주장과 정면으로 겹치는 **이번 조사 최우선 경계 논문**. Instance-level
   adversarial Bézier augmentation을 사용하나, 전문 미확인 상태로 즉시 정독이 필요하다.
2. **A3Point (ICLR 2026)** — LiDAR segmentation에서 "augmentation의 안전/위험이 이미지 내 위치마다
   다르다"는 원칙을 독립적으로 재확인한 top-tier vision 논문. 이론적 근거로 인용 가치가 높다.
3. **LOCALGAMMA (NLDL 2024)** — "이미지 전체 균일 augmentation은 문제"라는 아이디어를 binary
   lesion mask 단위로 이미 시도한 2024년 논문. 지금까지의 조사에서 누락되어 있었고, related work
   인용이 필수적이다.

이 외에 vessel radius/thickness를 **loss/supervision**에 활용하는 논문(MorVess, AG-TAL 계열의
연장선)이 계속 발견되고 있으나, **augmentation strength를 radius/observability에 continuous하게
conditioning한 논문은 이번 조사에서도 발견되지 않았다** — 내 방법의 핵심 novelty gap이 유지된다.

---

## Category A — 신규 직접경쟁 논문 (6편)

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 경계

Teacher–Student Instance-Level Adversarial Augmentation for SSDG. Bézier 기반 Instance-level
Augmentation Module로 이미지 내 구조마다 다른 augmentation을 적용. 상세 분석은
`paper_notes/TSIAA.md` 참고. **즉시 전문 확인 필요.**

### AMAP — npj Digital Medicine 2026

Anatomically-guided MAE + Domain-Adaptive Prompting for Cerebral Aneurysm Detection/Segmentation.
TOF-MRA-adjacent cerebrovascular 영역에서 augmentation이 아닌 foundation-model prompting으로
generalization을 달성하는 경쟁 패러다임. Discussion에서 "augmentation-based vs. prompting-based"
대비축으로 활용 가능.

### DomainFlow — STACOM 2024 (MICCAI Workshop)

Coronary vessel SSDG. Binary mask 대신 connectivity/distance-style mask 예측으로 topology 보존.
이미 알고 있는 AngioDG(2025)의 전신 격 워크숍 논문.

### WaveSDG, ROBUST-WT, RobustSurg — 저-중 관련성

Fundus/surgical scene SSDG. 구조 조건부 augmentation 개념 없음. `MASTER_PAPER_INDEX.md` 참고.

---

## Category B — 신규 방법론 유사 논문 (3편)

### LOCALGAMMA — NLDL 2024 ⚠️ 필수 인용 후보

`paper_notes/LOCALGAMMA.md` 참고. Binary lesion mask 제한 gamma augmentation. Continuous/radius
조건이 아니지만 "uniform augmentation의 문제"를 먼저 지적한 선행 연구로 반드시 related work에
포함해야 한다.

### SHORTCUTKD, BEZDIFF — 중-저 관련성

각각 intermediate-layer distillation을 통한 shortcut 억제, Bézier+diffusion 기반 UDA style
transfer. Mechanism이 augmentation-strength conditioning과 다름.

---

## Category C — 신규 구조·혈관 특화 논문 (13편)

### MorVess — arXiv 2606.24214

`paper_notes/MORVESS.md` 참고. Vessel thickness map을 joint prediction target으로 사용하는
가장 근접한 사례. Radius 계산 방법론(centerline maximal inscribed sphere)은 구현 참고 가치 있음.

### AC2RUNet — arXiv 2606.12319

Circle of Willis segmentation. Static/Dynamic stream 분리 + coarse-to-topology curriculum.
Training-time curriculum이 내 augmentation strength conditioning과 결이 유사(spatial이 아닌
temporal 축에서 강도를 조절한다는 점에서).

### 나머지 11편 — Topology/centerline/thin-structure 아키텍처 논문

TopoSculpt(ECCV급 tubular topology), SEMIR(ECCV 2026, graph minors), CSWinUNETR, ContextLoss(ICIP
2025), TopoLoRA-SAM, GraphMorph(NeurIPS 2024), DeformCL(CVPR 2025), CoW Centerline Graphs
데이터셋, 3-stage coronary topology(MedIA), VesselSDF(MICCAI 2025), PASC-Net. 모두 topology
preservation/thin-structure architecture 개선이 목적이며, augmentation strength conditioning과는
무관 — Discussion에서 "tubular structure segmentation의 최신 동향" 문단에 폭넓게 인용 가능.

---

## Category D — 신규 Top-tier Vision 논문 (6편)

### A3Point — ICLR 2026 ⚠️ 이론적 근거로 인용 가치 높음

`paper_notes/A3POINT.md` 참고.

### Sample-Aware RandAugment (SRA) — IJCV 2025

Per-sample(whole-image 단위) augmentation 강도를 sample complexity에 따라 동적 조절. "augmentation
강도의 continuous 조절"이라는 일반-vision 선례로 인용 가치 있으나 intra-image 구조 단위는 아님.

### Flat Minima Perspective — AAAI 2026

Augmentation이 robustness를 개선하는 이유를 flatness/PAC bound로 이론화. "강한 augmentation이 약한
evidence 구조에서 역효과를 낼 수 있다"는 내 동기의 이론적 뒷받침 후보.

### PDAF (ICCV 2025), PAPT-SDG (CVPR 2025), EBiL-HaDS (NeurIPS 2024) — 참고용

각각 probabilistic latent alignment, T2I diffusion 기반 학습된 augmentation policy, evidential
domain difficulty scheduling. Discussion에서 SDG의 최신 대안적 접근으로 폭넓게 인용 가능.

---

## Novelty Gap 재확인

이번 Run에서도 다음을 명시적으로 다룬 논문은 발견되지 않았다:

- "continuous vessel radius/observability-conditioned augmentation strength"
- "intra-vessel structure-specific augmentation budget"
- "fragile tubular structure appearance protection during augmentation"

가장 근접한 두 계열은 여전히 (1) **loss weighting에 radius를 사용하는 계열**(AG-TAL, cbDice,
MorVess의 auxiliary supervision)과 (2) **discrete/binary 단위로 augmentation을 지역 제한하는
계열**(LOCALGAMMA, TSIAA의 instance-level)이다. 두 계열 모두 "continuous + augmentation-strength
+ geometry-driven"이라는 세 조건을 동시에 만족하지 않는다.

다만 **TSIAA(IEEE TMI 2026)**는 이 세 조건 중 "이미지 내 구조마다 다른 augmentation을 적용한다"는
프레이밍을 채택한 최초의 직접 경쟁 논문으로 보이며, 전문 확인 전까지는 novelty 위협도를 확정할 수
없다. 다음 우선순위 작업으로 지정한다.

---

## 다음 Run 우선 탐색 항목

- [ ] **TSIAA 전문 확보 및 정독** (IEEE Xplore doc 11146907) — instance 정의, vessel 실험 포함 여부,
      augmentation 강도와 구조 크기의 상관관계 존재 여부 확인
- [ ] MorVess/AC2RUNet 코드/전문에서 radius·curriculum이 augmentation에도 쓰이는지 재확인
- [ ] MICCAI 2026 proceedings 공개 시 즉시 재탐색
- [ ] ICLR 2026 accepted 전체 목록에서 augmentation-strength-conditioning 관련 논문 추가 스캔
- [ ] arXiv WebFetch 차단(403) 문제로 이번 Run은 검색엔진 snippet 기반 검증에 의존함 —
      다음 실행에서 직접 fetch 가능 여부 재확인
