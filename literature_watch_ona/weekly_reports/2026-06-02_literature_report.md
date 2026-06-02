# Literature Watch Report — Run #6
**날짜**: 2026-06-02  
**실행 모델**: claude-sonnet-4-6  
**신규 논문**: 5편 (Accepted Conference 2 + Published Journal 1 + Preprint 2)  
**누적 수록**: 91편  

---

## 1. 이번 실행 핵심 요약

Run #6에서는 다음을 중점적으로 탐색했다:

- MICCAI 2025 open access 포털에서 미탐색 DG 논문 재탐색
- arXiv 2605/2606 최신 논문 (2026년 5–6월)
- 미수록 foundational paper (AADG, CDDSA) 평가
- 기존 인덱스 항목의 publication status 업데이트

**주요 발견**: 
1. MICCAI 2025에서 MixStyleFlow 발견 (normalizing flows 기반 domain style generation)
2. AADG (IEEE TMI 2022) — 미수록 상태였던 foundational augmentation policy learning 논문 수록
3. arXiv 2605에서 VesselSim (3D vessel simulation + domain randomization, May 2026) 발견
4. INVCAUSAL (arXiv 2411.05223)의 실제 publication: WACV 2025 (기존 인덱스 "Preprint Only" → 실제 발표 확인)

---

## 2. 신규 논문 상세

### [A-1] MixStyleFlow ★★ MICCAI 2025 — Cat A

| 항목 | 내용 |
|------|------|
| **KEY** | MIXSTYLEFLOW |
| **제목** | MixStyleFlow: Domain Generalization in Medical Image Segmentation using Normalizing Flows |
| **저자** | Reza Safdari, Mohammad-Ali Nikouei Mahani, Mohamad Koohi-Moghadam, Kyongtae Tyler Bae (U Hong Kong) |
| **Venue** | MICCAI 2025, Paper 0571-Paper3460 |
| **Status** | Accepted Conference |
| **URL** | https://papers.miccai.org/miccai-2025/0571-Paper3460.html |

**핵심 방법**: Normalizing Flows를 사용해 도메인 feature style의 분포를 **명시적**으로 학습·모델링한 뒤, 이를 MixStyle과 결합해 feature channel dimension을 따라 원본 통계와 생성된 스타일을 mix. 기존 MixStyle이 단순히 training batch 내 instance 간 통계를 swap하는 것에 비해, normalizing flow로 학습된 더 풍부한 스타일 공간에서 샘플링 가능.

**적용 도메인**: Prostate MRI cross-site, Fundus optic disc/cup segmentation.

**내 연구와의 관계**:
- **공통점**: Feature style 다양화를 통한 DG 개선이라는 방향
- **차이점**: MixStyleFlow = 전체 feature map 단위 uniform style mixing. 나 = 같은 이미지 내 혈관별 관찰 가능성에 따른 **pixel-level, structure-conditioned** 강도 조절. MixStyleFlow는 혈관 두께나 observability에 따라 augmentation을 차별화하지 않음. TOF-MRA 실험 없음.
- **활용**: 내 논문의 방법론 비교 후보 (baseline 또는 related work). Normalizing flow 기반 스타일 모델링을 내 방법에 통합할 수 있는지 탐색 가능.

---

### [A-2] DAGMRI ★★ MIDL 2025 — Cat A/B

| 항목 | 내용 |
|------|------|
| **KEY** | DAGMRI |
| **제목** | Data-Agnostic Augmentations for Unknown Variations: Out-of-Distribution Generalisation in MRI Segmentation |
| **저자** | Puru Vaish, Felix Meister, Tobias Heimann, Christoph Brune, Jelmer M. Wolterink |
| **Venue** | MIDL 2025 (accepted full paper) |
| **arXiv** | 2505.10223 (May 2025) |
| **Status** | Accepted Conference |

**핵심 방법**: MixUp과 Auxiliary Fourier Augmentation(AFA)을 nnU-Net 훈련 파이프라인에 통합. "feature separability와 compactness를 동시에 향상"시켜 OOD 일반화를 개선. Cardiac cine MRI + Prostate MRI에서 평가.

**내 연구와의 관계**:
- MixUp/AFA는 이미지 전체에 uniform하게 적용. 나 = 구조 단위 conditional.
- nnU-Net 기반 평가라는 점에서 내 baseline (nnU-Net)과 동일한 실험 환경. 비교 참고 가능.
- MIDL 2025는 보조 venue이나, nnU-Net + Fourier aug의 실질적 효과를 정량화한 점에서 참고 가치.

---

### [B-1] AADG ★★★ IEEE TMI 2022 — Cat B (Foundational)

| 항목 | 내용 |
|------|------|
| **KEY** | AADG |
| **제목** | AADG: Automatic Augmentation for Domain Generalization on Retinal Image Segmentation |
| **저자** | (github: CRazorback) |
| **Venue** | IEEE Transactions on Medical Imaging (TMI), 2022 |
| **arXiv** | 2207.13249 |
| **Status** | Published Journal |
| **GitHub** | https://github.com/CRazorback/AADG |

**핵심 방법**: 
- Augmentation policy search space를 정의하고 adversarial training + deep RL(강화학습)로 최적 policy 탐색
- **Proxy task**: Multiple augmented domain 간 Sinkhorn distance를 최대화 → 훈련 데이터의 domain diversity를 최대화
- 11개 공개 fundus dataset (retinal vessel, OD/OC, lesion)으로 검증. OCTA cross-modality 포함.
- Learned policy가 model-agnostic하고 다른 모델에 transfer 가능함을 실험적으로 검증.

**내 연구와의 관계 (P0 수준 독해 필요)**:
- **공통 방향**: "augmentation을 data에 적응적으로 맞추는 것이 중요하다"는 아이디어
- **핵심 차이**: AADG = **cross-image** policy optimization (어떤 augmentation op을 이미지 전체에 적용할지 탐색). 나 = **intra-image** structure-conditioned continuous modulation (같은 이미지 내 혈관마다 다른 강도). AADG는 한 이미지 내 thin/thick vessel 간 augmentation 강도 차등화 개념이 없음.
- **활용**: 내 논문 related work에서 "augmentation policy automation" 흐름의 대표 prior work로 명시 필요. 내 방법이 AADG와 직교하는 새로운 dimension을 다룬다는 것을 강조.
- 2022년 논문이지만 SSDG augmentation 분야의 사실상 baseline으로 여러 recent paper가 인용. **반드시 즉시 읽기.**

---

### [C-1] VesselSim ★ arXiv 2605.26277 — Cat C (Preprint)

| 항목 | 내용 |
|------|------|
| **KEY** | VESSELSIM |
| **제목** | VesselSim: Learning 3D Blood Vessel Segmentation Without Expert Annotations |
| **저자** | Erin Rainville, Melissa Ananian, Tristan Mirolla, Hassan Rivaz, Yiming Xiao (Concordia U) |
| **arXiv** | 2605.26277 (May 2026) |
| **Status** | Preprint Only |

**핵심 방법**:
1. **Stochastic geometry-driven vascular simulation**: recursive branching, curvature-controlled growth, collision-aware topology
2. **Domain-randomized intensity synthesis**: 16,500 anatomically plausible 3D angiographic volumes 생성
3. 3D U-Net을 완전 합성 데이터로만 훈련 → real annotated data 없이 real vessel DG 수준 달성

**내 연구와의 관계**:
- Paradigm 차이: VesselSim = 합성 데이터로 학습, 나 = 실제 source data + SSDG augmentation
- domain-randomized intensity synthesis scheme → 내 nonlinear aug 설계에 참고 가능 (어떤 intensity variation이 혈관 morphology를 보존하는가)
- vesselFM과 직접 경쟁. TOF-MRA 포함 여부 확인 필요.

---

### [A-3] FreqAdapSAM — arXiv 2605.09925 (Preprint)

| 항목 | 내용 |
|------|------|
| **KEY** | FREQADAPSAM |
| **제목** | Frequency Adapter with SAM for Generalized Medical Image Segmentation |
| **저자** | Phuoc-Nguyen Bui et al. (Sungkyunkwan U, Korea) |
| **arXiv** | 2605.09925 (May 2026) |
| **Status** | Preprint Only |

**핵심 방법**: SAM 기반 DG에 frequency adapter 추가. 기존 SAM-DG 방법이 공간 도메인에만 집중하는 것의 한계를 지적하고 frequency discrepancy 해결을 추가.

**내 연구와의 관계**: Cat A 직접경쟁이나 낮은 relevance. SAM 기반이고 vessel 특화 아님. Preprint이므로 별도 후보 목록에만 수록.

---

## 3. 기존 논문 상태 업데이트

### INVCAUSAL (arXiv 2411.05223) — Status Correction

| 기존 상태 | 실제 상태 |
|-----------|-----------|
| Preprint Only | WACV 2025 Accepted Conference (pp. 3592-3602) |

WACV 2025 Open Access Repository에서 공식 확인: "Generalizable Single-Source Cross-Modality Medical Image Segmentation via Invariant Causal Mechanisms", IEEE WACV 2025.

*주의*: 인덱스 수정 규칙에 따라 메인 인덱스는 수정하지 않음. 다음 주요 정리 시에 반영 권장.

---

## 4. 내 Continuous-ONA Novelty Gap 확인

이번 Run #6 후에도 다음을 확인:

| 검색 키워드 | 결과 |
|-------------|------|
| "vessel radius conditioned augmentation" | **없음** → gap 유지 |
| "intra-class thickness conditioned augmentation strength" | **없음** → gap 유지 |
| "observability-conditioned appearance augmentation" | **없음** → gap 유지 |
| "thin vessel protection augmentation DG" | **없음** (L2CP는 test-time) → gap 유지 |
| "augmentation budget per structure" | **없음** (BucketAugment는 per-image policy) → gap 유지 |

AADG가 "augmentation policy automation"이라는 아이디어를 공유하지만, intra-image, intra-class, continuous structure-conditioned 차원에서 내 방법은 완전히 새로운 dimension.

---

## 5. 다음 Run 탐색 권장 사항

- [ ] AADG 전문 독해 + paper note 작성 (P0)
- [ ] MixStyleFlow 전문 독해 (P1)
- [ ] VesselSim: TOF-MRA 포함 여부 + domain randomization 세부
- [ ] BucketAugment (IEEE OJEMB 2024): augmentation policy search paradigm 독해 가치 평가
- [ ] MICCAI 2025 추가 DG 논문 탐색 (GrInAdapt 등 vessel DA 논문)
- [ ] CVPR 2026 / ICCV 2026 arXiv submission 탐색 시작
