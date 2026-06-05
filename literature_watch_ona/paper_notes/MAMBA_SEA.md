# Paper Note: MAMBA_SEA

> **발견**: Run #8 (2026-06-05)  
> **우선순위**: P0 — 즉시 읽어야 할 논문 (내 POC 직접 경쟁 baseline)

---

## 기본 정보

| 항목 | 내용 |
|------|------|
| **제목** | Mamba-Sea: A Mamba-based Framework with Global-to-Local Sequence Augmentation for Generalizable Medical Image Segmentation |
| **저자** | Zihan Cheng, Jintao Guo, Jian Zhang, Lei Qi, Luping Zhou, Yinghuan Shi, Yang Gao |
| **소속** | Nanjing University (Yinghuan Shi 그룹) |
| **Venue** | IEEE Transactions on Medical Imaging (IEEE TMI) |
| **Year** | 2025 |
| **arXiv** | 2504.17515 (April 24, 2025) |
| **IEEE** | ieeexplore.ieee.org/document/10980210 |
| **Code** | https://github.com/orange-czh/Mamba-Sea |
| **Status** | Published Journal Article |
| **Cat** | A — 직접 경쟁 |
| **Rel** | High |

---

## 핵심 주장

> 기존 DG 방법들은 CNN/Transformer 아키텍처를 사용하며 global 또는 이미지 단위 style aug에 집중한다. Mamba의 sequential token modeling 능력을 활용하면, **local sub-sequence 단위**로 style을 perturbation하여 더 다양한 domain variation을 시뮬레이션할 수 있다.

---

## 방법 요약

### Global Augmentation
- Cross-site appearance variation을 시뮬레이션
- domain-specific feature 학습 억제
- 기존 GIN/RandConv 계열과 유사한 역할

### Local (Sequence-wise) Augmentation
- Mamba의 scanning 과정에서 입력 시퀀스를 random continuous sub-sequence로 분할
- 각 sub-sequence의 style statistics (mean, variance)를 모델링 후 resampling
- 연속된 token block의 local appearance를 독립적으로 변환
- **MixStyle 계열과의 차이**: MixStyle은 채널 전체의 global statistics를 mix. Mamba-Sea는 token 위치 기반의 local statistics를 perturb → 공간적으로 불균일한 style 교란

### Architecture
- Mamba (State Space Model) 기반 encoder
- 기존 CNN/Transformer baseline과 아키텍처 완전 교체

---

## 실험 결과

### Prostate T2-MRI SSDG (6-center)

| Method | Dice (%) |
|--------|---------|
| SLAug | ~85.xx |
| RASS | ~86.xx |
| DCON | 88.61 (이전 최고) |
| **Mamba-Sea** | **90.34** ← 최초 90% 돌파 |

- 기존 SOTA 대비 +1.73% 이상
- 상세 비교 표는 전문 독해 후 채울 것

### 기타 데이터셋 (전문 독해 후 업데이트)
- Cardiac segmentation
- Fundus vessel segmentation
- 추가 확인 필요

---

## 내 방법과의 비교 분석

### 공통점 (경쟁 관계)
1. Single-source domain generalization (no target access)
2. Augmentation 기반 DG (loss 변경 없이 augmentation만으로 generalization)
3. 동일 벤치마크 (Prostate 6-center SSDG)
4. 같은 연구 그룹 일부 (Jintao Guo는 DCON 공동저자이기도 함)

### 근본적 차이 (내 novelty 보호)
| 항목 | Mamba-Sea | 내 Continuous-ONA |
|------|-----------|------------------|
| 증강 단위 | 이미지 내 token sub-sequence | 이미지 내 개별 voxel/vessel |
| 증강 조건 | 공간적 연속성 (random sub-seq) | vessel radius/observability (structure-specific) |
| 얇은 혈관 보호 | ❌ 없음 | ✅ thin vessel에 보수적 aug |
| 두꺼운 혈관 증강 | ❌ 구분 없음 | ✅ thick vessel에 강한 aug |
| Intra-class이질성 처리 | ❌ 없음 | ✅ 연속적 radius에 따라 차등 |
| 아키텍처 의존성 | Mamba 필수 (CNN 불가) | Architecture-agnostic (nnUNet 등에 직접 적용) |
| 적용 레벨 | Token 단위 style stat | Pixel-level appearance transformation |

### Novelty 위협도 평가: **Medium-High**

- 동일 SSDG 벤치마크에서 더 높은 Dice를 달성한 최신 SOTA
- 하지만 내 방법의 핵심 claim인 "intra-class radius-conditioned augmentation budget"은 Mamba-Sea에 없음
- Mamba-Sea는 thin vessel vs thick vessel을 구분하지 않음

---

## 대응 전략

### 1. Complementary 포지셔닝
- Mamba-Sea = 아키텍처 기반 generalization (SSM의 sequence 특성 활용)
- 내 방법 = augmentation 기반 structural awareness (vessel observability 활용)
- 두 방법은 **서로 보완적** — 내 aug를 Mamba-Sea에 적용하면 추가 개선 가능성

### 2. Architecture-agnostic 강조
- Mamba-Sea는 Mamba 아키텍처로 완전히 교체해야 함
- 내 방법은 기존 nnUNet, UNet 등에 plug-in 형태로 바로 적용 가능
- "architectural innovation 없이 augmentation만으로 comparable한 thin vessel 성능을 달성" 주장 가능

### 3. TOF-MRA/Vessel 특화 강점
- Mamba-Sea는 Prostate, Cardiac, Fundus 위주 평가 (voxel seg)
- 내 방법은 **cerebrovascular / TOF-MRA** 특화 — thin vessel이 핵심인 도메인에서 차별화
- Prostate는 blob-like structure이므로 thin vessel 보호의 차이가 덜 드러날 수 있음

### 4. POC 비교 대상 추가
- 현재 비교: Baseline nnUNet / GLA / Uniform nonlinear aug / Binary thickness / Continuous-ONA
- **Mamba-Sea를 추가 비교 대상으로 포함하거나 supplementary에서 언급** 검토

---

## 미확인 사항 (전문 독해 필요)

- [ ] sequence augmentation의 sub-sequence window 크기 및 선택 방식
- [ ] style statistics 샘플링 방법 (어떤 분포에서 resampling?)
- [ ] fundus 및 cardiac 실험 결과 상세
- [ ] Mamba-Sea vs DCON 직접 비교 실험 포함 여부
- [ ] 3D 볼륨 데이터에서의 적용 방식 (TOF-MRA는 3D)
- [ ] thin/small vessel segmentation 관련 ablation이 있는지

---

## 관련 논문 연결

- 같은 저자 그룹 (Shi 그룹): **DCON** (Pattern Recognition 2025, Run #7에서 발견)
- 동일 벤치마크 경쟁: **SLAug, RASS, MoreStyle, ConStyX, DCON, ADA**
- Mamba DG 계열: 본 논문이 선구자 — 후속 연구 주시 필요
