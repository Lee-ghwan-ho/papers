# Literature Watch Report — Run #8

> 날짜: 2026-06-05  
> 모델: claude-sonnet-4-6  
> 신규 논문: **4편** (Published Journal 2편 + Accepted Challenge Paper 1편 + Preprint 1편)

---

## 요약

이번 Run #8에서는 세 가지 방향에서 신규 논문을 발견했다.

1. **SSDG 직접 경쟁 논문 1편** (IEEE TMI 2025):
   - **Mamba-Sea**: 최초 Mamba 기반 SSDG, Global-to-Local Sequence Augmentation으로 Prostate dataset 최초 90% Dice 돌파 (90.34%)

2. **방법론 유사 논문 1편** (IEEE TIP 2024):
   - **CDSA**: 자연영상 DG에서 inter-class semantic direction + inter-domain style을 교차 결합 (CrossSmooth + CrossVariance)

3. **Circle of Willis 특화 혈관 논문 2편**:
   - **TopCoW Challenge benchmark** (MICCAI 2024 Challenge): 최초 공개 CoW voxel-level annotation dataset + 140+ 팀 참가 벤치마크 논문
   - **TopoCoW Explore** (arXiv 2410.15614): unified CTA+MRA topology framework

핵심 발견: **Mamba-Sea가 동일 SSDG 설정에서 Prostate 90.34% Dice로 현재 SSDG SOTA를 달성**했다. 이는 내 방법과 직접 경쟁하는 새로운 최신 기준점이다. 차이점: Mamba-Sea = 시퀀스 토큰 단위 style perturbation (이미지 단위), 나 = intra-image vessel radius별 augmentation budget. Mamba-Sea에도 thin vessel 보호 개념은 없다.

---

## Category A — 신규 직접경쟁 논문

### MAMBA_SEA — IEEE TMI 2025 ⚠️ 최우선 주의

**논문**: Mamba-Sea: A Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation  
**저자**: Zihan Cheng, Jintao Guo, Jian Zhang, Lei Qi, Luping Zhou, Yinghuan Shi, Yang Gao (Nanjing University)  
**Venue**: IEEE Transactions on Medical Imaging, 2025  
**arXiv**: 2504.17515 (April 24, 2025)  
**IEEE Xplore**: document/10980210  
**Status**: Published Journal Article  
**Code**: https://github.com/orange-czh/Mamba-Sea

#### 방법 요약

Mamba-Sea는 최초로 Mamba(State Space Model) 아키텍처를 SSDG에 적용한 논문이다.

**핵심 구성**:
1. **Global Augmentation**: 이미지 수준에서 cross-site appearance variation을 시뮬레이션하여 domain-specific feature 학습을 억제
2. **Local Sequence Augmentation**: Mamba의 sequential token processing 특성을 활용, 입력 시퀀스 내 random continuous sub-sequence의 style statistics를 모델링 후 resampling → 연속 토큰 블록의 style을 locally perturb
   - 기존 MixStyle 계열은 채널 단위 전역 통계를 섞음. Mamba-Sea는 token sub-sequence 단위로 local style을 교란

**아키텍처**: Mamba (SSM) 기반 encoder + global-to-local sequence augmentation 모듈

#### 실험 결과

| Dataset | Mamba-Sea | Prior SOTA | 개선 |
|---------|-----------|-----------|------|
| Prostate (6-center) | **90.34%** Dice | 88.61% (이전 SOTA) | +1.73% |

- Prostate T2-MRI SSDG (6-center cross-site) 벤치마크에서 **최초 90% Dice 돌파**
- Cardiac, Fundus 등 다양한 modality에서도 평가 (상세 결과 전문 독해 필요)
- 비교 baseline: SLAug, RASS, MoreStyle, ConStyX, DCON 등

#### 내 방법과의 관계

**공통점**:
- 동일한 SSDG 설정 (single source, no target access during training)
- Augmentation 기반 DG (loss 변경 없이 augmentation으로 generalization 달성)
- 동일 벤치마크 데이터셋 (Prostate 6-center)

**핵심 차이**:
- **Mamba-Sea = architecture-level**: Mamba SSM의 token sequence 구조를 활용한 local style perturbation. 전체 이미지의 모든 토큰(혈관/비혈관 포함)에 동일 종류의 augmentation 적용.
- **나의 ONA = augmentation-level**: 동일 CNN/nnUNet 아키텍처 위에서, vessel radius/observability에 따라 augmentation **강도**를 연속적으로 차등 적용
- **근본적 차이**: Mamba-Sea는 "어떤 구조를 얼마나" 다르게 증강할지 결정하지 않음. 나는 thin vessel vs thick vessel 사이의 **intra-class observability** 이질성을 명시적으로 모델링
- Mamba-Sea에는 thin vessel 보호 또는 radius-conditioned augmentation 개념이 없음

**Novelty 위협도**: Medium-High  
동일 SSDG 벤치마크 데이터셋에서 더 높은 Dice를 기록한 최신 baseline. 내 방법이 Mamba-Sea 대비 개선을 보여야 하거나, 적어도 같은 수준을 달성하면서 complementary한 insight를 제공해야 함.

**대응 전략**:
1. 내 방법은 구조 분리 가능성(architecture-agnostic) 측면 강조: Mamba-Sea처럼 아키텍처를 교체할 필요 없이 기존 nnUNet 위에 바로 적용 가능
2. Thin vessel의 특수성을 DG 맥락에서 명시적으로 다루는 유일한 방법으로 포지셔닝
3. MAMBA_SEA도 POC baseline에 추가하거나 comparison table에 포함 검토

---

## Category B — 신규 방법론 유사 논문

### CDSA — IEEE TIP 2024

**논문**: Inter-Class and Inter-Domain Semantic Augmentation for Domain Generalization  
**Venue**: IEEE Transactions on Image Processing, 2024  
**DOI**: 10.1109/TIP.2024.3354420  
**Status**: Published Journal Article

#### 방법 요약

- **CrossSmooth**: inter-class semantic direction을 샘플링하여 feature space augmentation 수행. 서로 다른 class 간 semantic interpolation으로 intra-class diversity 확장.
- **CrossVariance**: inter-domain style statistics를 수집하여 domain-variant style을 cross-domain으로 교환
- Spectral perturbations + contrastive learning과 결합

#### 실험
- 자연영상 DG benchmark: PACS (89.3%), Office-Home (73.0%), Digits-DG, DomainNet
- Semantic segmentation: GTA→Cityscapes, SYNTHIA, Mapillary

#### 내 방법과의 관계
- **공통점**: semantic label 정보를 활용한 class-specific augmentation 방향
- **차이**: CDSA = inter-class (서로 다른 class 간 direction), 나 = intra-class (동일 vessel class 내 radius별 budget)
- 의료영상 아님. 내 방법의 "class-conditioned" vs "intra-class structure-conditioned" 구분 논거로 활용 가능

---

## Category C — 신규 혈관·구조 특화 논문

### TOPCOW_CHALLENGE — MICCAI 2024 Challenge

**논문**: Benchmarking the CoW with the TopCoW Challenge: Topology-Aware Anatomical Segmentation of the Circle of Willis for CTA and MRA  
**Venue**: MICCAI 2024 (Challenge proceedings / associated workshop)  
**arXiv**: 2312.17670  
**Status**: Accepted Challenge Paper  
**URL**: cowbenchmark.github.io

#### 방법 요약
- 최초 공개 CoW voxel-level annotation dataset: 200 paired MRA+CTA (동일 환자), 13 vessel class
- 140+ 등록 팀, 4개 대륙 참가
- 주요 결과: 상위 팀 class-average Dice >90%, detection F1 >85%, variant classification balanced acc >85%
- 공개 데이터: Zenodo records로 dataset + 상위 알고리즘 공개

#### 내 방법과의 관계
- AG-TAL, COW_TOPO, TOPOCOW_EXPLORE 등이 모두 이 benchmark를 기준으로 평가
- 내 TOF-MRA 연구와 해부학적으로 인접 (Circle of Willis는 TOF-MRA의 핵심 구조)
- 내 연구의 downstream application 또는 데이터셋 확장 논의 시 reference로 활용

---

### TOPOCOW_EXPLORE — arXiv 2410.15614 (Preprint Only)

**논문**: Topology-Aware Exploration of Circle of Willis for CTA and MRA: Segmentation, Detection, and Classification  
**저자**: Minghui Zhang, Xin You, Hanxiao Zhang, Yun Gu (Shanghai Jiao Tong University)  
**arXiv**: 2410.15614 (October 2024)  
**Status**: Preprint Only

#### 방법 요약
- Unified framework: CTA + MRA를 동시에 처리 (independent intensity preprocessing + joint resampling)
- Topology-aware loss: CoW topology completeness 강화
- Complement topology-aware refinement: 동일 class 내 connectivity 강화
- TopCoW24 challenge 참가 방법으로 추정

#### 내 방법과의 관계
- DG를 직접 다루지 않으나 CoW MRA 구조의 topology 처리 방식 참고 가능
- AG-TAL, COW_TOPO와 같은 계열의 CoW 특화 방법

---

## Novelty Gap 재확인

이번 Run #8에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class structure-specific augmentation strength"
- "thin vessel appearance protection during augmentation"
- "continuous observability based augmentation"

Mamba-Sea가 새로운 SSDG SOTA baseline이 됨에 따라, 내 POC 비교 대상에 추가하고 내 방법이 thin vessel에서 어떤 보완적 개선을 보이는지 실험적으로 확인하는 것이 중요하다.

---

## 다음 Run 우선 탐색 항목

- [ ] Mamba-Sea 전문 독해: sequence-wise local aug 상세 (sub-sequence window, style stat 샘플링), fundus/cardiac 실험 결과 비교
- [ ] Mamba-Sea GitHub 코드: sequence augmentation 모듈 구현 확인
- [ ] CDSA 전문 독해: CrossSmooth semantic direction 샘플링 방식 → 내 radius direction과 비교
- [ ] DG-EBF @CVPR 2026 workshop proceedings: 의료영상 DG 직접 경쟁 논문 탐색
- [ ] TopBrain 2025 challenge (Zenodo 15084013): whole-brain vessel annotation 데이터셋 → 내 연구 확장 가능성
- [ ] ICLR 2026 openreview 직접 탐색: medical image DG augmentation 논문
- [ ] "vessel observability conditioned augmentation" 키워드 재탐색 (Run #8에서도 없음 → gap 유지)
