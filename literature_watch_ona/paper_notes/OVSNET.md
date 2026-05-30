# OVSNET — Optimized Vessel Segmentation: A Structure-Agnostic Approach with Small Vessel Enhancement and Morphological Correction

**Venue:** IEEE Transactions on Image Processing (TIP), Vol. 34, pp. 7168-7179  
**arXiv:** 2411.15251  
**DOI:** 10.1109/TIP.2025.3607583  
**PubMed:** 41032568  
**Authors:** Song, Huang, et al.  
**Status:** Published Journal  
**발견:** Run #4 (2026-05-30)  
**Category:** C  
**Rel:** High

---

## 핵심 방법

OVS-Net: dual-branch architecture for multi-modality vessel segmentation.

### 아키텍처 구성
1. **Macro vessel extraction module:**
   - SAM의 ViT encoder backbone 기반
   - Feature adapter + Spatial adapter로 혈관 도메인에 맞게 파인튜닝
   - 굵은 혈관의 전체적 구조 포착

2. **Micro vessel enhancement module:**
   - ConvNext + FPN 기반 convolutional network
   - 소혈관(얇은 혈관) 특화 feature 추출
   - Multi-scale feature fusion

3. **Morphological correction module:**
   - 후처리 기반 위상 보존 및 connectivity 강화

### 실험 규모
- 17개 데이터셋 (retina, CT angiography, X-ray angiography, MRI 등 multi-modality)
- 6개 SAM-based 방법 + 17개 expert model과 비교
- connectivity 34.6% 개선

---

## 내 Continuous-ONA와의 관계

### 공통점 (동기의 직접적 일치)
- **"얇은 혈관과 굵은 혈관을 서로 다르게 처리해야 한다"** 는 핵심 동기가 완전히 일치.
- Dual-branch 구조의 micro/macro 분리 = 내 thin/thick 분리와 같은 motivation.
- Small vessel의 connectivity 보존이 중요한 문제임을 실험으로 입증.

### 핵심 차이

| 항목 | OVSNET | 내 Continuous-ONA |
|------|--------|-------------------|
| 개입 지점 | Model architecture (segmentation network) | Data augmentation (training pipeline) |
| Thin/thick 구분 | Binary (micro / macro branch) | Continuous (local radius에 따른 연속적 강도 조절) |
| DG 초점 | Multi-modality generalization (명시적 DG 실험 없음) | Single-source domain generalization (명시적 타겟) |
| Source annotation 활용 | 학습 시 label 사용 (표준) | label에서 local radius 추출 → augmentation strength 조절 |
| Training cost | 두 branch를 별도로 학습 | 기존 nnUNet architecture 유지 (augmentation만 변경) |
| Test time | 두 branch 동시 inference | 기존 inference 동일 |

### 내 주장에서 OVSNET를 언급하는 방식

> "Recent work has recognized the importance of treating thin and thick vessels differently. OVS-Net [IEEE TIP 2025] introduces a dual-branch architecture with separate macro- and micro-vessel extraction modules, achieving significant improvements in connectivity for small vessels across multiple modalities. However, this approach requires architectural modifications to the segmentation model. Our Continuous-ONA embodies the same motivation — that thin vessels must be handled differently from thick vessels — but operates entirely within the augmentation pipeline: without changing the backbone architecture, we modulate augmentation strength continuously based on local vessel radius, protecting fragile thin vessel structures from excessive appearance perturbations."

---

## 읽을 때 확인할 것

- [ ] Micro vessel enhancement에서 "얇은 혈관"의 정의/기준이 있는가? (radius threshold?)
- [ ] TOF-MRA나 3D brain vessel 데이터셋이 포함되어 있는가?
- [ ] Domain generalization 실험이 있는가? (cross-dataset evaluation)
- [ ] Connectivity 34.6% 개선: 얇은 혈관과 굵은 혈관 각각의 결과가 있는가?
- [ ] Morphological correction module의 수식: clDice 계열인가?
