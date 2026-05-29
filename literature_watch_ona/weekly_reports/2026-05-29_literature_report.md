# Literature Watch Report — Run #3 (2026-05-29)

> 연구 주제: Continuous-ONA — Single-source DG for TOF-MRA Cerebrovascular Segmentation  
> 검색 범위: Category A–D × 5 Search Lane  
> 신규 발견: **15편** (Accepted Conference 7편 + Published Journal 2편 + Workshop 1편 + Preprint 5편)  
> 누적 수록: **72편**

---

## 1. 이번 Run 핵심 요약

이번 검색에서 가장 주목할 발견은 두 가지다.

**첫째**, 내 "구조별 차별화 증강" 주장과 개념적으로 연결되는 **AGTA (MICCAI 2024 Workshop)** 발견. AGTA는 "tumor boundary에서는 texture를 파괴하면 안 된다"고 주장하며, anatomy segmentation을 이용해 texture augmentation을 anatomy-guided 방식으로 조절한다. 내 방법이 "thin vessel은 강한 appearance 변형으로부터 보호해야 한다"는 주장과 논리 방향이 유사하다. 단, AGTA는 class-level (tumor vs. normal tissue), 나는 single-class 내 continuous structural property 기반이라는 차이가 명확히 존재한다.

**둘째**, **ICRN (IEEE TMI 2024)**의 gamma correction 기반 local style augmentation (LSA)이 foreground와 background를 분리하여 증강 적용한다는 점에서 "annotation-region-specific aug"라는 개념을 공유한다. 내 방법은 같은 foreground 내에서 두께에 따라 다시 분리한다는 점이 핵심 차별점.

**셋째**, **RandDG (Medical Physics 2025)**가 GIN 기반 input-space aug + frequency feature-space perturbation을 동시에 사용하는 frequency-aware SSDG를 발표. 내 method와 baseline 비교 후보가 될 수 있음. 조합이 공학적으로 유사해 보일 수 있으나 RandDG에는 thickness/observability conditioning 개념이 없음.

---

## 2. Category A — 직접 경쟁 신규 논문

### 2.1 FREESDG ★★
- **제목**: Frequency-mixed Single-source Domain Generalization for Medical Image Segmentation
- **Venue**: MICCAI 2023 | **Status**: Accepted Conference
- **arXiv**: 2307.09005
- **핵심 방법**: Mixed frequency spectrum으로 single-source domain augmentation. Self-supervision in domain augmentation으로 context-aware representation 학습.
- **내 연구와의 관계**: RASS, MoreStyle의 직접 선행 논문. ADA, ConStyX가 비교하는 frequency baseline 중 하나. 5개 데이터셋 실험으로 SSDG foundational benchmark.
- **주의점**: 내 baseline 비교 테이블에 FreeSDG를 포함할지 검토 필요.

### 2.2 ICRN ★★★
- **제목**: Invariant Content Representation for Generalizable Medical Image Segmentation
- **Venue**: IEEE TMI 2024 | **Status**: Published Journal
- **PMC**: 11612095
- **핵심 방법**: 
  - Gamma correction 기반 Local Style Augmentation (LSA): foreground/background 각각 별도 gamma 증강
  - Invariant Content Learning (ICL): augmented + source 이미지에서 동일 feature 학습
  - Domain-specific BN 기반 Style Adversarial Learning (SAL)
- **내 연구와의 관계 (High)**: ICRN의 LSA는 "foreground와 background에 다른 gamma 변환 적용"이라는 annotation-region-specific aug 아이디어를 공유한다. 내 방법은 이를 한 단계 더 세분화하여 **single foreground class 내에서도 local radius에 따라 연속적으로 다른 gamma/nonlinear 강도를 적용**한다는 점이 핵심 차별점. ICRN은 class-level binary, 나는 intra-class continuous.
- **주의점**: related work에서 ICRN을 반드시 인용하고 차별화해야 함.

### 2.3 RANDDG ★★
- **제목**: Frequency-aware Domain Randomization for Single-Source Domain Generalization in Medical Image Segmentation
- **Venue**: Medical Physics 2025 | **Status**: Published Journal
- **DOI**: 10.1002/mp.70118
- **핵심 방법**: 
  - Input-space: GIN (Global Intensity Nonlinear augmentation)
  - Feature-space: ULoFT (Uniform Low Frequency spectrum Transform) 필터
  - Consistency loss로 domain invariant representation 학습
- **내 연구와의 관계 (High)**: GIN 기반 input-space aug를 사용한다는 점에서 내 nonlinear augmentation과 pipeline이 유사. 단, RandDG는 모든 픽셀에 동일한 GIN 강도를 적용하고 frequency 기반 feature regularization을 추가하는 구조. 내 방법은 GIN 강도 자체를 vessel의 local radius에 따라 nonuniform하게 조절한다는 점이 핵심 차이.
- **주의점**: Medical Physics라는 venue임에도 방법의 공학적 유사성이 높아 related work에서 명확히 구분 필요.

### 2.4 DCAM (Medium Rel)
- **제목**: D-CAM: Learning Generalizable Weakly-Supervised Medical Image Segmentation from Domain-Invariant CAM
- **Venue**: MICCAI 2025 | **Status**: Accepted Conference
- **방법**: IN + FFT로 domain-invariant pseudo-label (CAM) 생성. Weakly-supervised 설정.
- **내 연구와의 관계 (Medium)**: 설정이 다름 (weakly-supervised). 그러나 domain-invariant CAM 기반 pseudo-label 생성 파이프라인은 thin vessel 탐지의 불확실성 측정에 참고 가능.

### 2.5 WFEX (Medium Rel)
- **제목**: Addressing Label Scarcity and Domain Shift in Medical Image Segmentation
- **Venue**: MICCAI 2025 | **Status**: Accepted Conference
- **방법**: Wavelet Frequency Exchange (WFE) + Learnable Parametric Feature Network (LPFN, Parametric Spline layers). Semi-supervised + UDA.
- **내 연구와의 관계 (Medium)**: Semi-supervised + UDA 설정이므로 내 SSDG와 다름. 단, Parametric Spline activation layers를 feature network에 사용하는 아이디어는 내 nonlinear mapping 구현 참고.

---

## 3. Category B — 방법론 유사 신규 논문

### 3.1 AGTA ★★★ (novelty 충돌 가능성 있음)
- **제목**: Improving Single-Source Domain Generalization via Anatomy-Guided Texture Augmentation for Cervical Tumor Segmentation
- **Venue**: MICCAI 2024 Workshop (CMMCA) | **Status**: Workshop Paper
- **DOI**: 10.1007/978-3-031-73360-4_8
- **핵심 방법**: 
  - 기존 방법들이 texture를 무차별 파괴하는 것을 비판
  - Anatomy segmentation을 가이드로 사용하여 texture augmentation을 anatomy-aware하게 조절
  - Tumor boundary 등 중요한 texture cue를 보존하면서 DG 달성
- **내 연구와의 관계 (High)**: 
  - **논리 방향 유사**: "structure-specific texture 보호"는 내 "fragile vessel 보호" 주장과 방향이 동일.
  - **핵심 차이**: AGTA는 서로 다른 anatomical class 간(tumor vs. 주변 조직), 나는 단일 foreground class 내 continuous structural observability 기반. AGTA는 texture feature 보존이 목적, 나는 label-image inconsistency 방지가 목적. AGTA는 binary (class-level), 나는 continuous (radius-level).
  - **Workshop 논문**이므로 영향력 낮으나 개념적 선례로 인용 가능.
- **노트 파일**: `paper_notes/AGTA.md` 참조.

### 3.2 XDOMAINMIX (Medium Rel)
- **제목**: Cross-Domain Feature Augmentation for Domain Generalization
- **Venue**: IJCAI 2024 | **Status**: Accepted Conference
- **arXiv**: 2405.08586 | **GitHub**: NancyQuris/XDomainMix
- **핵심 방법**: Feature를 class-generic, class-specific, domain-generic, domain-specific 4가지로 분해. Cross-domain feature aug은 domain-specific component를 swap하면서 class-specific은 보존.
- **내 연구와의 관계 (Medium)**: Feature-space augmentation (image-space ❌). Domain generalization for natural images. 그러나 "class-specific vs domain-specific 분리"라는 개념은 내 content-preserving aug의 이론적 근거로 인용 가능.

### 3.3 STRUCSTYLE (Preprint)
- **제목**: Structure-Aware Stylized Image Synthesis for Robust Medical Image Segmentation
- **arXiv**: 2412.04296 | **Status**: Preprint Only
- **핵심 방법**: Diffusion model + Structure-Preserving Network로 structure-aware one-shot image stylization. Domain shift에 robust.
- **내 연구와의 관계 (Medium)**: Diffusion 기반 stylization이므로 내 방법과 구현 다름. 단, "structure를 보존하면서 style을 변경"이라는 목적 공유. 참고용.

---

## 4. Category C — 구조 및 혈관 특화 신규 논문

### 4.1 DSUSNAKE (Medium Rel, Preprint)
- **제목**: Dynamic Snake Upsampling Operator and Boundary-Skeleton Weighted Loss for Tubular Structure Segmentation
- **arXiv**: 2505.08525 | **Status**: Preprint Only
- **핵심 방법**: 
  - 동적 snake upsampling: serpentine path를 따라 subpixel-level feature 복원
  - Skeleton-to-boundary increasing weighted loss: mask class ratio와 distance field로 main body vs boundary weight 조절
- **내 연구와의 관계 (Medium)**: Loss와 upsampling 개선이 주목적. 내 augmentation-only 설정과 보완 관계. 내 thin vessel 탐지 motivation 강화에 참고.

### 4.2 SDFTOPONET (Low Rel, Preprint)
- **제목**: SDF-TopoNet: A Two-Stage Framework for Tubular Structure Segmentation via SDF Pre-training and Topology-Aware Fine-Tuning
- **arXiv**: 2503.14523 | **Status**: Preprint Only
- **핵심 방법**: SDF 보조 학습으로 topology 정보 인코딩 후, Dynamic Adapter + 정제된 topology loss로 fine-tuning.
- **내 연구와의 관계 (Low)**: 독립적 논문이지만 SDF 기반 vessel representation → observability score 계산에 일부 참고 가능.

---

## 5. Category D — Top-tier Vision 신규 논문

### 5.1 ADAL ★★
- **제목**: Adversarial Data Augmentation for Single Domain Generalization via Lyapunov Exponent-Guided Optimization
- **Venue**: ICCV 2025 | **Status**: Accepted Conference
- **arXiv**: 2507.04302
- **핵심 방법**: Lyapunov exponent 기반 learning rate 변조. "카오스의 가장자리"에서 훈련을 유지하여 SDG 일반화 향상. PACS, OfficeHome, DomainNet에서 최대 9.47% 향상.
- **내 연구와의 관계 (Medium)**: 자연영상 classification, 의료영상 아님. 그러나 "augmentation 강도 조절의 이론적 근거"로서 dynamical systems theory 활용이 내 augmentation budget 조절의 theoretical motivation에 참고 가능. "Edge of chaos" = "fragile structure의 임계점" 비유.

### 5.2 DPMFORMER (Low Rel)
- **제목**: Exploiting Domain Properties in Language-Driven Domain Generalization for Semantic Segmentation
- **Venue**: ICCV 2025 | **Status**: Accepted Conference
- **arXiv**: 2512.03508
- **핵심 방법**: Domain-aware prompt learning (CLIP) + texture perturbation + domain-robust consistency learning.
- **내 연구와의 관계 (Low)**: 자연영상 DG segmentation. 내 방법에 직접 적용 어려움. Language-driven DG 계열 배경 이해용.

---

## 6. Preprint-Only 후보 요약

| KEY | 제목 (요약) | arXiv | Cat | 중요도 |
|-----|------------|-------|-----|--------|
| UNIDDG | Rethinking DG: One Image as One Domain | 2501.04741 | A | ★★ |
| STRUCSTYLE | Structure-Aware Stylized Image Synthesis | 2412.04296 | B | ★ |
| DSUSNAKE | Dynamic Snake Upsampling + BSW Loss | 2505.08525 | C | ★ |
| SDFTOPONET | SDF-TopoNet for tubular segmentation | 2503.14523 | C | ☆ |
| BISDG | Bi-Level Optimization for SDG | 2604.06349 | D | ☆ |
| CLFA | Causality-inspired Latent Feature Aug | 2406.05980 | D | ★ |

---

## 7. Novelty Gap 재확인

이번 Run에서 탐색한 15편의 신규 논문 중 **내 핵심 gap을 직접 메우는 논문은 없다.**

| Gap | 현황 |
|-----|------|
| Single foreground class 내 continuous radius 기반 aug budget 조절 | 여전히 미존재 |
| Thin vessel label-image inconsistency를 명시적으로 aug 설계에 반영 | 여전히 미존재 |
| Source annotation만으로 계산한 local observability 기반 aug conditioning | 여전히 미존재 |

AGTA와 ICRN이 가장 개념적으로 근접하지만:
- AGTA: class-level (tumor vs. 주변), binary, workshop paper
- ICRN: foreground/background binary, gamma correction

모두 내 방법의 "single foreground class 내 continuous structural property 기반 aug"와는 granularity가 다르다.

---

## 8. 다음 Run에서 추가 탐색이 필요한 구역

- [ ] ICRN 전문 독해: LSA 구현 상세 (gamma 범위, 적용 방식)
- [ ] AGTA 전문 독해: anatomy segmentation 어떻게 사용하는지 상세 확인
- [ ] RANDDG 전문 독해: GIN 파라미터 범위 및 consistency loss 구조 확인
- [ ] NeurIPS 2025 proceedings 정식 발행 확인 (Dec 2025 → 온라인 게재 여부)
- [ ] MICCAI 2025 검색 추가: domain generalization 키워드 더 세분화
- [ ] TopBrain 2025 challenge: whole brain vessel segmentation dataset으로 사용 가능성 검토
- [ ] "observability" 또는 "vessel diameter/radius conditioned" augmentation 키워드로 추가 검색

---

## 9. 커밋 정보

- Branch: `claude/amazing-allen-FB8zd`
- 수정 파일: MASTER_PAPER_INDEX.md, READING_QUEUE.md, SEARCH_LOG.md, weekly_reports/2026-05-29_literature_report.md, paper_notes/AGTA.md
