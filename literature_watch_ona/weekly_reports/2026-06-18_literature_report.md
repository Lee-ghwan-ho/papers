# Literature Watch Report — Run #8

> 날짜: 2026-06-18  
> 모델: claude-sonnet-4-6  
> 신규 논문: **6편** (Published Journal 1편 + Preprint 5편)

---

## 요약

이번 Run에서는 두 가지 방향에서 신규 논문을 발견했다.

1. **SSDG 직접 경쟁 논문 1편** (arXiv 2603.28463):
   - WAVESDG: 웨이블릿 서브밴드 분리 기반 SSDG (fundus segmentation)

2. **혈관·tubular 구조 특화 논문 5편**:
   - TOPOLORASAM (arXiv 2601.02273, Jan 2026): SAM + LoRA + clDice for thin-structure cross-domain
   - VASOMIM (arXiv 2508.10794, Aug 2025): X-ray angiogram vessel seg용 anatomy-guided MIM
   - VAMAE (arXiv 2604.06583, Apr 2026): OCTA vessel-aware masked autoencoder
   - TUBEMLLLM (arXiv 2603.09217, Mar 2026): vessel-like anatomy topology foundation model
   - TUBENET (Biology Methods, Nov 2025): generalizable 3D vessel segmentation tool

핵심 발견: **VASOMIM이 "vessel-rich region을 우선 마스킹하여 더 큰 training signal 부여"라는 아이디어를 도입**했다. 이는 내 Continuous-ONA의 "resolved vessel → larger augmentation budget" 개념과 방향이 인접하나, 전혀 다른 paradigm(pretraining-time MIM masking vs. training-time augmentation budget)에서 구현된다. 내 동기를 강화하는 선행 근거로 활용 가능.

---

## Category A — 신규 직접경쟁 논문

### WAVESDG — arXiv 2603.28463 (April 2026) ⚠️ 주목

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**저자**: Shramana Dey 외 (Indian Statistical Institute)  
**arXiv**: 2603.28463 (v2: April 27, 2026)  
**Status**: Preprint Only  
**Domain**: Fundus (optic cup/disc segmentation)

#### 방법 요약

**WaveSDG**는 wavelet sub-band decomposition으로 anatomical structure와 domain-specific appearance를 분리하는 SSDG 방법이다.

핵심 모듈 **WISER (Wavelet-based Invariant Structure Extraction and Refinement)**:
- **LL sub-band**: 전역 구조 맥락 (global structural context) → anatomical layout 앵커
  - Low-frequency 성분 = 해부학적 형태 정보 → 보존
- **LH/HL sub-bands**: 방향성 에지 응답 (horizontal/vertical directional edges)
  - Selective enhancement: 구조 경계 강화
- **HH sub-band**: 고주파 성분 (sensor noise + acquisition artifacts)
  - Noise suppression: 도메인 특이 잡음 억제

#### 실험 결과
- Source 1개 → Unseen target 5개 fundus 데이터셋
- 7개 SOTA 방법 능가 (SLAug, RandConv, IBN 등 포함)
- 최고 balanced Dice + 향상된 cross-domain stability

#### 내 방법과의 관계

**공통점**:
- SSDG setting (single source → multiple unseen targets)
- Appearance augmentation의 구조 파괴 문제를 주파수(웨이블릿) 관점에서 접근
- Low-frequency 구조 정보를 augmentation에서 보호하는 전략

**핵심 차이**:
- WAVESDG = **전역 주파수 서브밴드 분리** (image-level, 채널 단위): 어떤 frequency band를 보존/억제할지 결정
- 나 = **intra-image vessel radius별 augmentation budget** (pixel-level, structure-specific): 같은 foreground class 내에서 vessel별로 다른 강도

**구분 논거**:
- WAVESDG는 "어떤 frequency를 얼마나 augment할지"를 전체 이미지 수준에서 결정 → 얇은 혈관과 굵은 혈관을 동일하게 처리
- 나는 "특정 혈관(thin vs. thick)에 얼마나 강한 변형을 허용할지"를 vessel 단위로 결정 → intra-class structural heterogeneity 명시적 모델링

**Novelty 위협도**: Low-Medium — 주파수 기반 구조 보존이라는 방향 유사하나, global sub-band vs. local structure-specific budget이라는 근본적 차이 존재. WAVESDG는 fundus (optic disc/cup, 2D), 나는 cerebrovascular (TOF-MRA, 3D). 겹침 없음.

---

## Category C — 신규 혈관·구조 특화 논문

### TOPOLORASAM — arXiv 2601.02273 (January 2026)

**논문**: TopoLoRA-SAM: Topology-Aware Parameter-Efficient Adaptation of Foundation Segmenters for Thin-Structure and Cross-Domain Binary Semantic Segmentation  
**arXiv**: 2601.02273 (January 5, 2026)  
**Status**: Preprint Only  
**Code**: https://github.com/salimkhazem/Seglab

#### 방법 요약
- SAM ViT encoder에 LoRA 주입 (Low-Rank Adaptation)
- Lightweight spatial convolutional adapter
- Optional topology-aware supervision: differentiable clDice
- 5.2% parameters (~4.9M) 학습

#### 실험
- Retinal vessel: DRIVE, STARE, CHASE_DB1 (best retina-average Dice)
- Polyp: Kvasir-SEG
- SAR: SL-SSDD (cross-domain)

#### 내 방법과의 관계
- Cross-domain + thin-structure + topology preservation 방향
- 내 방법과 설정이 다름: TOPOLORASAM = fine-tuning adaptation, 나 = training-time SSDG augmentation
- clDice topology supervision과 thin vessel 처리 방식 참고 가능

---

### VASOMIM — arXiv 2508.10794 (August 2025) ⚠️ 주목

**논문**: VasoMIM: Vascular Anatomy-Aware Masked Image Modeling for Vessel Segmentation  
**저자**: De-Xing Huang 외 (Institute of Automation, Chinese Academy of Sciences)  
**arXiv**: 2508.10794 (August 2025)  
**Status**: Preprint Only  
**Domain**: X-ray angiogram vessel segmentation

#### 방법 요약

VasoMIM은 X-ray angiogram vessel seg를 위한 anatomy-guided MIM pretraining framework이다.

두 가지 핵심 구성:
1. **Anatomy-guided masking strategy**: vessel-containing patch를 우선적으로 마스킹 → vessel reconstruction에 집중
2. **Anatomical consistency loss**: original과 reconstructed image 간 vascular semantic 일치성 강화

#### 내 방법과의 관계 (High Interest)

**방향 인접성**:
> VasoMIM: "vessel-rich region(=thick, well-resolved vessels)을 우선 마스킹 → 더 큰 reconstruction training signal"
> 내 ONA: "resolved vessel(=thick) → larger augmentation budget 허용"

두 방법 모두 "vessel의 관찰 가능성/크기에 따라 training effort를 차별 배분"한다는 공통 직관을 가진다.

**핵심 차이**:
- VASOMIM = pretraining에서 **어떤 위치를 마스킹(가릴)지** 결정 (masked-then-reconstruct)
- 나 = augmentation에서 **어떤 vessel에 더 강한 appearance change를 허용할지** 결정 (augment-then-segment)
- VASOMIM은 DG를 직접 다루지 않음 (pretraining strategy, not SSDG)

**활용 방향**:
- Related Work에서 "vessel anatomy-aware training effort allocation이 이미 MIM 측면에서 탐색됨"을 언급 가능
- "pretraining-level allocation (VASOMIM) vs. augmentation-level allocation (내 ONA)"의 상보성 주장 가능

---

### VAMAE — arXiv 2604.06583 (April 2026)

**논문**: VAMAE: Vessel-Aware Masked Autoencoders for OCT Angiography  
**arXiv**: 2604.06583 (April 2026)  
**Status**: Preprint Only  
**Domain**: OCTA vessel segmentation

#### 방법 요약
- Vesselness + skeleton 기반 anatomically informed masking
- Vascular connectivity + branching pattern에 집중
- Multi-target reconstruction (appearance + structure + topology)
- OCTA-500 benchmark에서 limited-label setting SOTA

#### 내 방법과의 관계
- VASOMIM과 유사한 방향 (선택적 vessel-aware masking)
- OCTA = retinal microvasculature, 내 TOF-MRA와 modality 다름
- DG를 직접 다루지 않음 (낮은 관련성)

---

### TUBEMLLLM — arXiv 2603.09217 (March 2026)

**논문**: TubeMLLM: A Foundation Model for Topology Knowledge Exploration in Vessel-like Anatomy  
**arXiv**: 2603.09217 (March 13, 2026)  
**Status**: Preprint Only

#### 방법 요약
- MLLM + explicit natural language topology prompting + visual alignment
- TubeMData benchmark (topology-centric multimodal tasks)
- Adaptive loss weighting strategy emphasizing topology-critical regions
- Color fundus photography: β₀ error 37.42 → 8.58

#### 내 방법과의 관계
- Topology-critical region에 adaptive loss weighting → 내 radius-conditioned aug budget과 개념적 유사성 (다른 mechanism)
- Foundation model approach (내 SSDG-specific method와 규모 다름)
- 낮은 직접 관련성

---

### TUBENET — Biology Methods and Protocols, Nov 2025

**논문**: tUbeNet: a generalizable deep learning tool for 3D vessel segmentation  
**Venue**: Biology Methods and Protocols (Oxford Academic), Vol 10(1), bpaf087  
**DOI**: 10.1093/biomethods/bpaf087  
**저자**: Natalie A Holroyd 외 (UCL Walker-Samuel Lab)  
**Status**: Published Journal Article  
**PMC**: PMC12679403

#### 방법 요약
- 3D CNN trained on multi-modality data (CT, optical imaging, photoacoustic imaging)
- Varied training set으로 cross-modality feature 학습
- Human-in-the-loop training + 0.3% fine-tuning volume으로 new modality 적응
- DICE 0.81~0.98 across diverse applications

#### 내 방법과의 관계
- vesselFM/VesselSim과 유사한 3D vessel generalization 방향
- Foundation model approach, SSDG augmentation과 다른 paradigm
- 낮은 tier venue (Biology Methods and Protocols); 직접 경쟁 아님

---

## Novelty Gap 재확인 (Run #8)

Run #8에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "intra-class radius-conditioned augmentation budget"
- "thin vessel appearance protection during augmentation"
- "continuous augmentation strength by tubular structure observability"

**새로운 관련 발견 (동기 강화)**:
- VASOMIM: "vessel-rich region에 더 큰 training signal" → 내 ONA 동기와 방향 인접
- WAVESDG: "구조 정보(LL)를 appearance augmentation에서 보호" → 내 "fragile structure 보호" 주장과 방향 유사
- 두 논문 모두 내 핵심 mechanism (intra-class continuous radius-conditioned augmentation)을 다루지 않음

**결론**: Continuous-ONA의 핵심 novelty gap (intra-vessel radius → augmentation budget의 연속적 mapping)은 Run #8에서도 유지된다.

---

## 다음 Run 우선 탐색 항목

- [ ] WAVESDG 전문 독해: WISER module의 LL/LH/HL/HH 처리 방식 상세 → 내 방법과 구분 논거 구체화
- [ ] VASOMIM 전문 독해: anatomy-guided masking이 thin vs. thick vessel에 미치는 효과 차이 → 내 동기 지지 근거로 활용 가능성
- [ ] ICML 2026 proceedings (공개 예정) DG/augmentation 관련 논문 탐색
- [ ] MICCAI 2026 accepted list 공개 시 vessel/DG 논문 탐색
- [ ] SPIRONet (arXiv 2406.19749) AAAI 2026 실제 acceptance 여부 재확인
- [ ] "vessel-specific augmentation intensity" OR "pixel-level augmentation budget" 키워드 추가 탐색
- [ ] Vesselpose (OpenReview 02CZZogtcB) 향후 conference acceptance 추적
