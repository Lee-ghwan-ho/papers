# Literature Watch Report — Run #8 (2026-06-09)

> 실행 일자: 2026-06-09  
> 기준 기록: Run #7 (2026-06-03, 96편)  
> 신규 발견: **5편** (Accepted Conference 4편 + Preprint 1편) + Preprint 보조 목록 추가 1편  
> 누적 총계: 101편  

---

## 요약 (Executive Summary)

Run #8에서 총 5편의 신규 논문이 메인 인덱스에 추가되었다.

- **Cat A (직접 경쟁)**: WaveSDG — wavelet 기반 SSDG로 anatomy/appearance 분리. 주파수 domain에서 내 방법의 "구조 보존" 개념에 인접하지만 메커니즘 완전히 다름.
- **Cat B (방법론 유사)**: FedGIN (MICCAI 2025), DG-TTA (Sensors 2025) — 두 논문 모두 GIN 기반 비선형 강도 증강의 효과성을 다른 설정(federated, TTA)에서 재확인.
- **Cat C (혈관 특화)**: VesselGPT (MICCAI 2025 oral), VesselSDF (MICCAI 2025) — 혈관 기하학 모델링의 새로운 방향; DG 직접 연관성은 낮음.
- Preprint 보조: IELDG (자연영상 DG 세그멘테이션, 확산모델 기반).

**Novelty Gap 재확인**: "vessel radius/observability conditioned augmentation budget" 키워드에 직접 대응하는 논문은 Run #8에서도 발견되지 않았다. Continuous-ONA의 핵심 novelty는 유지된다.

---

## Category A — 신규 발견 (직접 경쟁)

### ⭐ WaveSDG (arXiv 2603.28463) — **즉시 읽기 권장**

**Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation**  
Shramana Dey, Abhirup Banerjee, Varun Ajith, Sushmita Mitra (Indian Statistical Institute)  
arXiv: 2603.28463 | Posted: March 31, 2026 | Status: Preprint Only

**핵심 방법**:  
- **WISER 모듈**: Wavelet-based Invariant Structure Extraction and Refinement
  - 저주파 서브밴드 → 글로벌 해부학적 구조 고정 (anatomy anchor)
  - 고주파 서브밴드 → 방향성 엣지 강화 + 도메인 노이즈 억제
- 1개 source → 5개 unseen target dataset (optic disc / cup segmentation)
- 7개 SOTA 방법과 비교, balanced Dice 및 95th percentile HD에서 최상

**내 방법과의 관계**:
- **공통점**: SSDG에서 anatomy-appearance 분리를 목표로 함
- **핵심 차이**:
  - WaveSDG = **글로벌 이미지** 수준 wavelet 주파수 분리. "어떤 주파수가 구조다"라는 전역적 가정에 기반.
  - 내 Continuous-ONA = **로컬 구조** 단위 (intra-image vessel-specific). 같은 이미지 내에서 혈관마다 augmentation budget이 다름. radius/observability라는 명시적 공간 지표 사용.
  - WaveSDG는 fundus optic disc (단일 구조, 크고 균일한 형태) 대상 — 혈관 두께 이질성 문제를 다루지 않음.
- **novelty 충돌 없음**. WaveSDG를 related work로 인용하며 구분 가능.

---

## Category B — 신규 발견 (방법론 유사)

### FedGIN (MICCAI 2025 LNCS 16135)

**FedGIN: Federated Learning with Dynamic Global Intensity Non-linear Augmentation for Organ Segmentation Using Multi-modal Images**  
Nagaraju S.D., Moradi A., Abrahamsen B.S., Elschot M. (Norwegian university)  
arXiv: 2508.05137 | MICCAI 2025 (LNCS 16135) | Status: Accepted Conference Paper

**핵심**:
- GIN 증강을 Federated Learning에 통합해 CT+MRI multi-modal organ segmentation
- "Dynamic GIN": federated round마다 GIN 파라미터를 동적으로 조정
- Limited-data scenario: MRI에서 12–18% Dice 향상
- Complete dataset: 30% Dice 향상 over MRI-only baseline

**내 방법과의 관계**:
- 내 방법이 GIN 계열 비선형 augmentation을 "uniform nonlinear aug" baseline으로 사용하는 근거를 강화
- Federated 설정이므로 SSDG와 패러다임 다름 — 직접 경쟁 아님
- GIN이 modality-agnostic한 domain gap을 효과적으로 줄일 수 있음을 보여주는 추가 증거

### DG-TTA (MDPI Sensors, Sep 2025)

**DG-TTA: Out-of-Domain Medical Image Segmentation Through Augmentation, Descriptor-Driven Domain Generalization, and Test-Time Adaptation**  
Christian Weihsbach, Alexander Bigalke, Christian N. Kruse, Mattias P. Heinrich (Uni Lübeck)  
arXiv: 2312.06275v3 | MDPI Sensors 25(17), 5603, 2025 | Status: Published Journal Article

**핵심**:
- SSC (Self-Supervised Contrastive) descriptor + GIN 강도 증강 조합 → DG 사전학습
- TTA (Test-Time Adaptation)로 각 unseen scan에 추가 적응
- CT→MRI: abdominal +46%, spine +73%, cardiac +55% Dice 향상
- 공개 데이터셋 5개 (2012–2022), 3D CT + MRI

**내 방법과의 관계**:
- GIN + TTA라는 조합은 내 방법과 패러다임 다름 (나 = training-time SSDG aug)
- Sensors = 낮은 tier. 방법 독창성보다는 기존 GIN+descriptor의 결합 논문.
- 내 baseline 설계 시 GIN의 CT-MRI 교차 효과 근거로 참고

---

## Category C — 신규 발견 (혈관 특화)

### VesselGPT (MICCAI 2025 oral, arXiv 2505.13318)

**VesselGPT: Autoregressive Modeling of Vascular Geometry**  
Paula Feldman, Martin Sinnona, Viviana Siless, Claudio Delrieux, Emmanuel Iarussi  
arXiv: 2505.13318 | MICCAI 2025 (oral) | Status: Accepted Conference Paper

**핵심**:
- VQ-VAE로 혈관 구조를 이산 vocabulary에 임베딩
- GPT-2로 혈관 트리 자기회귀 생성
- B-spline 혈관 단면 표현으로 세밀한 형태 유지
- 최초의 autoregressive 혈관 생성 모델

**내 방법과의 관계**: DG/augmentation 연구 아님. 혈관 기하학 생성 패러다임의 최신 방향 파악용.

### VesselSDF (MICCAI 2025, arXiv 2506.16556)

**VesselSDF: Distance Field Priors for Vascular Network Reconstruction**  
arXiv: 2506.16556 | MICCAI 2025 | Status: Accepted Conference Paper

**핵심**:
- 혈관 분할을 SDF(Signed Distance Field) 회귀로 재정의
- 연속적 기하 표현 → 가지치는 혈관 연속성 보존
- 2-stage network + Gaussian regularization

**내 방법과의 관계**: SDF 표현이 thin vessel 연속성을 implicit하게 모델링. 내 observability score 계산에 SDF 개념 응용 가능성 탐색 가능. 직접 DG 연구 아님.

---

## Preprint 보조 목록 추가

### IELDG (arXiv 2508.19604, Aug 2025) — Cat D 후보

**IELDG: Suppressing Domain-Specific Noise with Inverse Evolution Layers for Domain Generalized Semantic Segmentation**  
arXiv: 2508.19604 | Posted: August 27, 2025 | Status: Preprint Only

**핵심**:
- DGSS (자연영상 domain generalized semantic segmentation) 대상
- Laplacian 기반 Inverse Evolution Layer(IEL)를 확산 모델 생성 과정에 통합
- IEL: 공간적 불연속성과 의미 불일치를 하이라이트 → 결함 이미지 필터링
- IELDM (고품질 aug 데이터 생성) + IELFormer (구조 가이드 아키텍처)

**내 방법과의 관계**: 자연영상 DG 세그멘테이션 (의료영상 아님). "Laplacian 구조 보존"이라는 개념이 내 관찰 가능성 개념과 방향 유사하나 메커니즘 완전히 다름. 참고 수준.

---

## Novelty Gap 분석 (Run #8 기준)

| 검색 키워드 | 결과 |
|------------|------|
| vessel radius conditioned augmentation | **없음** |
| thickness-conditioned augmentation domain generalization | **없음** |
| observability conditioned augmentation | **없음** |
| intra-class augmentation budget vessel | **없음** |
| continuous augmentation strength vessel segmentation DG | **없음** |

→ **Continuous-ONA의 핵심 클레임은 Run #8 이후에도 선행 논문 없음**

---

## 기준 논문 Follow-up 상태

| 기준 논문 | 주목할 후속 / 경쟁 논문 | 상태 |
|-----------|------------------------|------|
| SLAug (AAAI 2023) | ADA, DCON, SRCSM, WAVESDG | 모두 indexed |
| RASS (MICCAI 2024) | MoreStyle, FreeSDG | 모두 indexed |
| ConStyX (MICCAI 2025) | arXiv 2506.10675 확인됨 (동일 논문 preprint) | 기존 인덱스 유지 |
| clDice (CVPR 2021) | cbDice, GLCP, SkelRecall, DSUSNAKE | 모두 indexed |
| Hessian VF (MedIA 2024) | AG-TAL (radius-aware loss) | indexed |

---

## 다음 실행 탐색 우선 구역

- [ ] WAVESDG 전문 독해: WISER 모듈 구현 상세 + 5-target 실험 결과 테이블
- [ ] IJCAI 2026 accepted list 공개 여부 확인
- [ ] MICCAI 2026 deadline 전후 arXiv 제출 논문 급증 예상 → 7월 이후 재탐색
- [ ] "vessel-scale conditioned augmentation" 키워드로 Google Scholar alert 설정 권장
- [ ] VesselSDF의 SDF 표현 → 내 observability score 계산에 응용 가능성 검토
- [ ] FedGIN GitHub 코드 확인: Dynamic GIN 파라미터 조정 방식 상세
