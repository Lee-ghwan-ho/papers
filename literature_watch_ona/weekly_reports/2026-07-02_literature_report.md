# Literature Watch Report — Run #8

> 날짜: 2026-07-02
> 모델: claude-sonnet-5
> 신규 논문: **22편** (Published Journal 3편 + Accepted Conference 1편 + Workshop 1편 + Preprint 17편)

---

## 요약

지난 실행(Run #7, 2026-06-03)으로부터 약 한 달간의 공백을 4개 병렬 리서치 에이전트(Cat A/B/C/D)로 커버했다. 가장 중요한 발견은 두 갈래다.

1. **⚠️ 최우선 위험 논문 발견 — TSIAA (IEEE TMI 2026)**: "이미지 내 서로 다른 구조에 동일한 augmentation을 적용하면 안 된다"는 상위 주장을 명시적으로 선점한 첫 논문. 내 핵심 claim과 상위 개념 수준에서 거의 동일하여 즉시 정독 및 Related Work 차별화 서술이 필요하다.

2. **Radius-aware training 트렌드의 급성장**: AG-TAL(Run #7)에 이어 VESSELFM_CT(TubeLoss), MorVess(thickness map supervision), TopoVST(radius-weighted directional loss) 3편이 추가로 발견되었다. 모두 **loss/supervision** 측면에서 vessel radius를 사용하며, **augmentation budget** 측면에서 사용하는 논문은 여전히 없다 — 내 novelty gap은 유지되지만 인접 연구 커뮤니티가 빠르게 채워지고 있어 경쟁 압박이 증가하고 있다.

3. **Top-tier Vision에서 "curvilinear/flow-matching" 서브트렌드 부상**: SACM(CVPR 2026 Oral), CurvSegFlow, CrackSegFlow, R2DCURVE 등 2026년 상반기에 집중적으로 등장한 클러스터. Continuous-ONA와 직접 경쟁하지는 않으나(대부분 architecture/decoder 측 혁신), vessel/curvilinear segmentation을 다루는 연구 커뮤니티 자체가 top-tier venue에서 활발해지고 있음을 시사.

---

## Category A — 신규 직접경쟁 논문 (3편)

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 주의

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation
**Venue**: IEEE Transactions on Medical Imaging, Vol. 45, pp. 764–776
**Status**: Published Journal Article
**Code**: github.com/Wangzs0228/TSIAA

Bézier 변환 기반 Instance-level Augmentation Module(IAM)을 여러 개 조합해 이미지 내 서로 다른 구조마다 다른 augmentation 규칙을 adversarial teacher-student 학습으로 적용. 논문 원문: *"Instance-level adversarial augmentation breaks the uniformity of augmentation rules across different structures within an image."*

**내 방법과의 관계**: 상위 주장 수준에서 거의 동일 — discrete instance-adversarial 조건화(TSIAA) vs. continuous radius-conditioned 조건화(Continuous-ONA)로 구분. 상세 분석은 `paper_notes/TSIAA.md` 참조.

**Novelty 위협도**: **High** — 최우선 정독 대상.

---

### WAVESDG — arXiv 2603.28463 (Preprint Only)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation

Wavelet 기반 Invariant Structure Extraction and Refinement(WISER) 모듈로 저주파(전역 해부구조)와 고주파(경계/노이즈) sub-band를 분리해 구조/외형을 decouple. Fundus optic cup/disc 대상.

**내 방법과의 관계**: Frequency-domain structure/appearance decoupling 계열의 SSDG 경쟁 방법. Radius conditioning 없음 — 위협도 Medium.

---

### ROBUSTWT — arXiv 2606.03069 (Preprint Only)

**논문**: ROBUST-WT: Robust Uncertainty-aware Segmentation Transform via Whitening and Training Enhancements

기존 whitening-transform 기반 DG 파이프라인(WT-PSE, TMI 2024)에 대한 engineering follow-up. Global augmentation 추가 및 loss 조정. 위협도 Low.

---

## Category B — 신규 방법론 논문 (1편)

### NOISEUNET — arXiv 2606.04427 (Preprint Only)

**논문**: Implicit Fuzzification via Bounded Noise Injection for Robust Medical Image Segmentation

U-Net skip connection에 bounded perturbation을 주입해 경계 불확실성을 다룸. Domain generalization augmentation이 아닌 boundary robustness 목적 — 위협도 Low.

---

## Category C — 신규 구조·혈관 특화 논문 (9편)

### 최우선 주의: Radius-aware loss/supervision 트렌드 3편

**VESSELFM_CT** (arXiv 2606.09400, Preprint) — vesselFM의 CT 후속작. TubeLoss로 혈관 tree 전체의 radius 이질성을 loss에서 명시적으로 처리.

**MORVESS** (arXiv 2606.24214, Preprint) — MorVess: pulmonary vessel segmentation. Distance map + **thickness map**을 auxiliary supervision으로 명시적으로 예측.

**TOPOVST** (arXiv 2603.14909, Preprint) — TopoVST: GNN으로 vessel radius를 공동 추정해 directional loss weighting에 사용 (skeleton tracking).

세 논문 모두 loss/supervision 측면의 radius 활용이며, augmentation budget 측면은 여전히 공백 — Continuous-ONA gap 유지 근거로 활용 가능하나, 경쟁 압박 증가 신호로 해석해야 함.

### 기타 구조/혈관 논문

- **AC2RUNET** (arXiv 2606.12319) — Circle of Willis topology-aware recurrent refinement (EUSIPCO 2026 self-reported, 미검증)
- **TOPOLORA_SAM** (arXiv 2601.02273) — SAM을 LoRA + differentiable clDice로 thin-structure에 적응
- **TOPOWIDTH** (SIAM J. Imaging Sciences 2026) — Persistent homology + PDE 기반 width 보존 변분 프레임워크
- **COWCENTERLINE** (ScienceDirect 2026, arXiv 2510.13720) — CoW centerline graph 데이터셋 + baseline
- **TOPOFIELD** (arXiv 2602.02186) — Pulmonary tree topology repair, DG 아님
- **SEMIR** (arXiv 2606.24935, ECCV 2026 self-reported) — 비의료 데이터 대상 topology-preserving graph minor

---

## Category D — 신규 Top-tier Vision 논문 (9편)

### SACM — CVPR 2026 Oral

**논문**: Dual-level Adapter Boosting Prompt-free Curvilinear Structure Segmentation (Segment Anything Curve Model)

SAM 기반 curvilinear structure segmentation. Block-level internal adapter(local structure refinement) + external adapter(cross-domain alignment) 분리 설계로 12개 데이터셋에서 18장만으로 강한 cross-domain 일반화.

**내 방법과의 관계**: Architecture 방법이라 augmentation과 직접 경쟁하지 않음. "local vs. global 조건화 분리"라는 설계 철학은 참고 가치.

### MAPJITTER — CVPR 2026 Workshop (DG-EBF)

**논문**: Magnitude-Aware Phase Jittering for Domain-Generalized Semantic Segmentation

Frequency phase를 magnitude-aware하게 jitter — "구조는 보존하며 appearance만 perturb"라는 목표가 철학적으로 Continuous-ONA와 동일. Spatial-domain(내 방법) vs. frequency-domain(MAPJITTER) 접근 비교 가치.

### Curvilinear/Flow-Matching 서브트렌드

**CurvSegFlow** (arXiv 2606.21608), **R2DCURVE** (arXiv 2606.23486, ECCV 2026 self-reported), **CrackSegFlow** (arXiv 2601.03637) — 2026년 상반기에 vessel/curvilinear 구조 분할을 위한 flow-matching 기반 방법이 클러스터로 등장. 대부분 synthesis/decoder 혁신이며 augmentation-strength conditioning과 무관하지만, 인접 연구 커뮤니티의 활발한 성장을 시사.

### 기타

- **CAUSALTUNE** (arXiv 2512.16567) — VFM의 causal frequency band 분리
- **BRIDGECAUSAL** (arXiv 2604.26820) — Front-door adjustment 기반 causal DG (object detection)
- **EVOAUG** (arXiv 2602.03123) — 생성모델 + 진화 알고리즘 기반 augmentation policy 자동 탐색
- **MAXPOOLSHAPE** (arXiv 2601.05599) — Max-pool dilation으로 shape bias 유도 (classification)

---

## 중복 발견 처리 (QA)

이번 Run에서 여러 에이전트가 "신규"로 보고했으나 실제로는 이미 인덱싱된 논문과 동일하여 제외한 항목: SARCS(=SRCSM), MDBVFD/VESSELDIS(=MULTIDOMAIN_BRAIN), WAVERNET(=WAVERNETV), SEMDIRFA(=SEMDIR), FMS2(중복 재발견), BISDG(중복 재발견), LEASGD(=ADAL), DGSSA(중복 재발견), FASAM(=Run #6에서 이미 낮은 관련성으로 기각된 FA-SAM). 상세는 SEARCH_LOG.md 참조.

---

## Novelty Gap 재확인

- **"vessel observability conditioned augmentation strength (continuous)"**: 여전히 직접 명시 논문 없음 — gap 유지
- **TSIAA의 등장으로 "구조별 비균일 augmentation"이라는 상위 개념은 더 이상 완전히 비어있지 않음** — 내 논문은 이 상위 개념을 인정하고, tubular structure에서 continuous/geometry-grounded 방식으로 최초 구체화했다는 하위 차별점을 명확히 주장해야 함
- Radius-aware training 트렌드가 loss 측면(AG-TAL, VESSELFM_CT, MorVess, TopoVST — 4편)에서 급성장 중, augmentation 측면은 0편 유지

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문 독해 최우선 (paper_notes/TSIAA.md 참조)
- [ ] VESSELFM_CT의 TubeLoss 공식이 continuous radius weighting인지 확인
- [ ] SACM, R2DCURVE의 실제 venue accept 여부 재확인 (다음 Run에서 공식 proceedings 공개 여부 체크)
- [ ] AC2RUNET(EUSIPCO 2026), SEMIR(ECCV 2026) self-reported venue의 공식 accept 확정 여부
- [ ] "curvilinear/flow-matching" 서브트렌드 성장 추적 — 후속 논문 다수 예상
- [ ] MICCAI 2026 accepted papers list 공개 시 즉시 전수 탐색 (이번 Run 기준 아직 미공개)
