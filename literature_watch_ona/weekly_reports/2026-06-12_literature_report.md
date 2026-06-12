# Literature Watch Report — Run #8

> 날짜: 2026-06-12  
> 모델: claude-sonnet-4-6  
> 신규 논문: **6편** (Published Journal 3편 + Accepted Conference 1편 + Preprint 2편)

---

## 요약

이번 Run에서는 세 가지 방향에서 신규 논문을 발견했다.

1. **구조 거리/형태 기반 brightness augmentation 논문 2편** (Neurocomputing 2025, Biomedical Signal Processing and Control 2025):
   - MRFFD: Distance-Aware Gaussian Brightness Augmentation (DAGBA) — **⚠️ P0 독해 필요**
   - PCSDG: Structure-Aware Brightness Augmentation (SABA) — 내 방법과 keyword level 유사

2. **혈관 특화 + TOF-MRA 관련 신규 논문 2편**:
   - GrInAdapt (MICCAI 2025): 멀티도메인 retinal vessel structural map segmentation with source-free adaptation
   - XAI_CEREBRO (arXiv 2512.13977): cerebrovascular segmentation의 domain generalization failure 원인 XAI 분석

3. **Wavelet 기반 SSDG + Domain Randomization 논문**:
   - WaveSDG (arXiv 2603.28463): wavelet sub-band 분해로 anatomy-appearance 분리
   - DOMAINRANDOM_RADAI (Radiology: AI 2025): image + feature space combined domain randomization in nnU-Net

핵심 발견: **MRFFD의 DAGBA가 "pixel 위치의 foreground 거리 기반 brightness augmentation 조절"이라는 아이디어를 도입**했다.  
내 Continuous-ONA의 "vessel radius 기반 augmentation budget 조절"과 표면적 유사성이 있으나,  
conditioning variable(경계 거리 vs. intra-vessel radius), augmentation type(brightness only vs. nonlinear appearance family), 그리고 핵심 동기(uneven brightness 보정 vs. fragile vessel 보호)가 모두 다르다.

---

## Category A — 신규 직접경쟁 논문

### MRFFD — Neurocomputing 2025  ⚠️ P0 독해 필요

**논문**: Multi-receptive Field Feature Disentanglement with Distance-Aware Gaussian Brightness Augmentation for Single-Source Domain Generalization in Medical Image Segmentation  
**Venue**: Neurocomputing, 2025  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.neucom.2025.130120

#### 방법 요약

- **MRFFD**: multi-scale (multi-receptive field) feature extraction + channel-level style-structure feature disentanglement  
  - 다양한 크기의 kernel로 fine-grained detail + global context 동시 포착
  - 채널 수준에서 style feature와 structural feature를 분리 → domain-invariant representation 학습

- **DAGBA** (Distance-Aware Gaussian Brightness Augmentation):
  - "pixel의 foreground 구조 및 이미지 경계까지의 거리"를 기반으로 brightness perturbation을 Gaussian 형태로 조절
  - 의도: medical image의 불균일한 brightness distribution을 보정하고 복잡한 brightness variation 시뮬레이션

#### 실험

- Prostate MRI (multi-center SSDG)
- Fundus image segmentation (multi-domain)
- SOTA 대비 유의미한 성능 향상

#### 내 방법과의 관계

**공통점**: spatial distance를 conditioning variable로 사용해 augmentation 강도를 동적 조절  
**핵심 차이**:

| 항목 | MRFFD/DAGBA | Continuous-ONA |
|------|-------------|----------------|
| Distance 측정 대상 | pixel ↔ foreground 경계 (boundary distance) | vessel foreground 내 local radius (intra-class) |
| Augmentation type | brightness only | nonlinear appearance family (intensity curve) |
| Intra-class 이질성 인식 | 없음 | 있음 (thin vs. thick vessel) |
| 핵심 동기 | uneven brightness 보정 | fragile structure 보호 + shortcut 억제 |

**Novelty 위협도**: Medium — reviewer가 유사성을 지적할 수 있음. 대응: conditioning variable과 동기의 차이를 related work에서 명시.

---

### PCSDG — Biomedical Signal Processing and Control 2025

**논문**: Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation  
**저자**: Jia-Xuan Jiang, Yuee Li, Zhong Wang  
**Venue**: Biomedical Signal Processing and Control, Vol 99, 2025  
**Status**: Published Journal Article  
**DOI**: 10.1016/j.bspc.2024.106801 (PII: S1746809424008590)  
**Code**: https://github.com/HopkinsKwong/PCSDG

#### 방법 요약

- **PCSDG framework**: pixel-level contrastive single domain generalization
  - Disentanglement module: content-related map + style-related map을 pixel-wise attention으로 분리
  - Contrastive loss: latent space에서 structure/style representation segregation 강화

- **SABA** (Structure-Aware Brightness Augmentation):
  - random brightness variation을 적용하되 anatomical information을 보존
  - "augmentation이 해부학적 구조를 파괴해서는 안 된다"는 원칙

#### 실험

- RIGA+ dataset (optic disc and cup, multi-center)
- 1 source + 5 target domain

#### 내 방법과의 관계

**공통점**: "structure-aware brightness augmentation"이라는 keyword 공유. SSDG에서 appearance aug와 구조 보존을 동시에 달성하려는 방향 유사.  
**핵심 차이**: PCSDG = optic disc/cup에서 class-level boundary 보존 (organ class 구조 보존); 나 = vessel foreground 내 pixel의 local vessel radius에 따라 augmentation strength를 연속적으로 조절. PCSDG에는 intra-class 두께/관찰가능성 개념 없음.  
**Novelty 위협도**: Low-Medium — "structure-aware aug for SSDG"라는 방향은 겹치나, mechanism과 적용 대상이 명확히 다름.

---

### DOMAINRANDOM_RADAI — Radiology: Artificial Intelligence 2025

**논문**: Deep Learning with Domain Randomization in Image and Feature Spaces for Abdominal Multiorgan Segmentation on CT and MRI Scans  
**Venue**: Radiology: Artificial Intelligence, 2025  
**Status**: Published Journal Article  
**DOI**: 10.1148/ryai.240586  
**PubMed**: 40396895

#### 방법 요약

- Image-space domain randomization + feature-space domain randomization을 extended nnU-Net에 결합
- Cross-site + cross-modality (prostate MRI, abdominal CT+MRI) generalization

#### 내 방법과의 관계

- Image + feature combined domain randomization의 최신 사례
- 내 uniform nonlinear aug baseline (GIN 계열)의 벤치마크 맥락 참고
- 직접 경쟁 논문은 아님 (organ segmentation, not vessel; not SSDG framework)
- **Novelty 위협도**: Low

---

## Category C — 신규 혈관·구조 특화 논문

### GrInAdapt — MICCAI 2025

**논문**: GrInAdapt: Scaling Retinal Vessel Structural Map Segmentation Through Grounding, Integrating and Adapting Multi-device, Multi-site, and Multi-modal Fundus Domains  
**arXiv**: 2503.05991  
**Venue**: MICCAI 2025  
**Status**: Accepted Conference Paper

#### 방법 요약

- Source-free multi-target domain adaptation (generalization과 다름: test-time에 target 데이터 일부 활용)
- 3단계: (i) 공통 anchor space로 image registration (Grounding), (ii) multi-view 예측 consensus로 label 정제 (Integrating), (iii) source model을 target에 적응 (Adapting)
- Auxiliary modality (color fundus photography) 통합 가능

#### 내 방법과의 관계

- Domain adaptation (target 데이터 필요) ≠ Domain generalization (target 없음) → 직접 경쟁 아님
- Retinal vessel + multi-domain = 내 연구의 혈관 DG 맥락에서 참고 가능
- **Novelty 위협도**: Low (다른 paradigm)

---

### XAI_CEREBRO — arXiv 2512.13977 (Preprint)

**논문**: XAI-Driven Diagnosis of Generalization Failure in State-Space Cerebrovascular Segmentation Models: A Case Study on Domain Shift Between RSNA and TopCoW Datasets  
**저자**: Youssef Abuzeid, Shimaa El-Bana, Ahmad Al-Kabbany  
**arXiv**: 2512.13977, December 2025  
**Status**: Preprint Only

#### 방법 요약

- XAI(설명가능한 AI)를 사용하여 cerebrovascular segmentation 모델이 domain shift 시 왜 실패하는지 진단
- State-Space Model(UMamba)이 target domain에서 attention을 진짜 혈관 구조가 아닌 spurious correlation으로 옮기는 현상 발견
- RSNA cerebrovascular dataset → TopCoW dataset 간 domain shift 분석

#### 내 방법과의 관계

- 직접 경쟁 논문 아님 (DG 방법이 아닌 분석 논문)
- **내 연구에의 활용**: TOF-MRA domain shift가 얼마나 심각한지 동기 강화 근거로 활용 가능
  - "기존 모델이 domain shift 시 spurious correlation에 의존한다" → "특히 thin vessel은 appearance evidence가 약하므로 더 취약하다"는 나의 주장을 강화
- **Novelty 위협도**: 없음

---

## Category A (Preprint) — WaveSDG

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463, March 31, 2026  
**Status**: Preprint Only

#### 방법 요약

- **WISER** (Wavelet-based Invariant Structure Extraction and Refinement) module:
  - Encoder feature에 wavelet sub-band decomposition 적용
  - Low-frequency sub-band: global anatomy anchoring (구조 정보 보존)
  - High-frequency sub-band: directional edges 강화 + noise 억제
- "anatomy topology를 포착하지 못하고 appearance와 anatomy를 분리 못하는 기존 SSDG의 한계" 극복이 목적

#### 내 방법과의 관계

- 방향 유사: anatomy/structure와 domain-specific appearance를 분리하여 DG 향상
- **핵심 차이**: WaveSDG = wavelet decomposition based feature-level structure-appearance separation; 나 = pixel-level vessel radius conditioned augmentation budget
- Optic disc/cup segmentation에 특화 (혈관 아님)
- **Novelty 위협도**: Low — mechanism 완전히 다름

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class vessel thickness augmentation strength"
- "thin vessel appearance protection during domain generalization augmentation"

**MRFFD/DAGBA** 가 "distance-conditioned brightness augmentation"이라는 개념을 처음 제안했으나:  
→ conditioning variable이 "경계 거리"(background-side)이고, augmentation type이 "brightness only"로 제한됨  
→ 내 ONA의 핵심인 "vessel foreground 내 intra-class radius → nonlinear appearance budget" 개념과는 명확히 분리됨

**핵심 gap 여전히 유지됨**:  
> Within the vessel foreground, the augmentation strength should vary continuously with vessel radius/observability. Thin vessels need conservative perturbations; thick vessels can tolerate aggressive transformations.

---

## 다음 Run 우선 탐색 항목

- [ ] **MRFFD 전문 독해**: DAGBA의 거리 계산 공식 상세 (skeleton distance transform 사용 여부, gaussian sigma 결정 방식)
- [ ] **PCSDG 전문 독해**: SABA가 실제로 어떻게 구조를 "aware"하는지 공식 확인
- [ ] ICLR 2026 DG 관련 논문 openreview.net 직접 탐색 (ICLR 2026 decisions 공개 상태 확인)
- [ ] CVPR 2026 accepted papers 공개 시 DG/segmentation/augmentation 관련 논문 탐색
- [ ] Radiology:AI 2025-2026에서 추가 DG 논문 탐색 (DOMAINRANDOM_RADAI 기반)
- [ ] "vessel radius conditioned" 또는 "observability-weighted augmentation" 키워드 재탐색
