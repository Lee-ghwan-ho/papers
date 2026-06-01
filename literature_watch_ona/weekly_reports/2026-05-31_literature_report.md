# Literature Watch Report — Run #4 (2026-05-31)

> 연구 주제: Continuous-ONA — Single-source DG for TOF-MRA Cerebrovascular Segmentation  
> 검색 범위: Category A–D × 5 Search Lane  
> 신규 발견: **8편** (Accepted Conference 5편 + Preprint 3편)  
> 누적 수록: **80편**

---

## 1. 이번 Run 핵심 요약

이번 검색의 가장 중요한 발견은 세 가지다.

**첫째**, CVPR 2025에서 **vesselFM**과 **TTDG-MGM**이 발견되었다. vesselFM은 3D blood vessel 분할을 위한 foundation model로 TOF-MRA를 포함한 4가지 modality에서 zero-shot DG를 달성했다. 이는 내 lightweight SSDG 접근과 paradigm이 다르지만, vessel DG 분야의 CVPR 2025 기준 논문으로 반드시 related work에 포함해야 한다.

**둘째**, MICCAI 2025의 **L2CP**는 이번 Run에서 가장 개념적으로 중요한 발견이다. 이 논문은 "retinal vessel의 thin nature를 명시적으로 활용"하여, morphological closing으로 thin vessel을 제거한 vessel-free 배경을 test-time training에 사용한다. Thin vessel이 domain appearance에 취약하다는 전제를 내 방법과 독립적으로 공유하지만, 방법론은 test-time adaptation (target 필요)으로 근본적으로 다르다.

**셋째**, ICML 2025에서 **LangDAug**가 발견되었다. ICML이라는 top-tier venue에서 의료영상 DG augmentation을 다룬 논문으로, EBM + Langevin dynamics를 이용한 이론적으로 정당화된 multi-source DG augmentation을 제안한다. Multi-source 설정이므로 내 SSDG와 설정은 다르나, ICML 논문으로서 내 method 비교 풀에 포함해야 한다.

---

## 2. Category A — 직접 경쟁 신규 논문

### 2.1 TTDG-MGM ★ (Medium Rel)
- **제목**: Test-Time Domain Generalization via Universe Learning: A Multi-Graph Matching Approach for Medical Image Segmentation
- **Venue**: CVPR 2025 | **Status**: Accepted Conference
- **arXiv**: 2503.13012 | **GitHub**: https://github.com/Yore0/TTDG-MGM
- **핵심 방법**:
  - Learnable universe embeddings에 morphological prior 통합
  - Multi-source training + unsupervised test-time adaptation
  - Multi-graph matching으로 source domain 간 cycle-consistency 보장
  - Retinal fundus segmentation + polyp segmentation benchmark SOTA
- **내 연구와의 관계 (Medium)**: Test-time adaptation 패러다임으로 내 training-time SSDG와 근본적으로 다름. 그러나 CVPR 2025의 medical DG 논문으로 related work에서 언급 필요.
- **주의점**: Morphological prior를 DG에 통합한다는 개념은 내 local radius signal과 유사한 직관. 단, 내 방법은 test-time 정보를 전혀 사용하지 않음.

### 2.2 LANGDAUG ★★ (Medium Rel)
- **제목**: LangDAug: Langevin Data Augmentation for Multi-Source Domain Generalization in Medical Image Segmentation
- **Venue**: ICML 2025 | **Status**: Accepted Conference
- **arXiv**: 2505.19659 | **OpenReview**: LB5F02kwAv
- **핵심 방법**:
  - Energy-Based Model (EBM)을 contrastive divergence로 학습
  - Langevin dynamics로 source domain 간 intermediate 샘플 생성
  - Theoretical analysis: Rademacher complexity를 data manifold의 intrinsic dimensionality로 상한
  - Fundus segmentation + 2D MRI prostate segmentation 실험
- **내 연구와의 관계 (Medium)**:
  - ICML 2025 (top-tier), 의료영상 DG augmentation → 주요 경쟁 논문으로 citation 필요
  - Multi-source 설정으로 내 single-source SSDG와 설정 다름
  - Langevin dynamics를 통한 domain interpolation = 나의 radius-conditioned 강도 조절과 접근 방식 완전히 다름
  - 내 방법은 single foreground class 내 structural property 기반, LangDAug는 domain 간 경계 샘플 생성
- **주의점**: ICML 2025 논문이므로 related work에서 반드시 포함해야 함. Theoretical analysis 방식은 내 방법의 이론적 분석에 참고 가능.

### 2.3 L2CP ★★ (High Rel)
- **제목**: Test-Time Training with Local Contrast-Preserving Copy-Pasted Image for Domain Generalization in Retinal Vessel Segmentation
- **Venue**: MICCAI 2025 (Paper 3277) | **Status**: Accepted Conference
- **SpringerLink**: https://link.springer.com/chapter/10.1007/978-3-032-04981-0_58
- **Authors**: Gu, Y.; Sun, Z.; Liu, Z.; Xu, Y.
- **핵심 방법**:
  - **Morphological closing으로 thin vessel 구조를 배경에 inpaint** → vessel-free 이미지 생성
  - Vessel-free target 이미지 = target domain style 유지 + vessel 없음
  - Source 이미지의 vessel local contrast map을 vessel-free target에 paste
  - 이 synthetic 이미지로 test-time training 수행
- **내 연구와의 관계 (High)**:
  - **공통 전제**: thin vessel의 특수성을 DG에서 명시적으로 인식 (이번 Run 최중요 발견)
  - **논리 방향 유사**: "thin vessel은 appearance에 특별히 취약하다"는 전제 공유
  - **핵심 차이**: L2CP = test-time adaptation (target domain image 필요), 나 = training-time SSDG (target 불필요)
  - L2CP는 thin vessel 구조를 제거해서 target style을 분리하는 것이 목적. 나는 thin vessel에 약한 aug budget을 주어 label-image inconsistency를 방지하는 것이 목적.
  - **활용**: 내 논문에서 thin vessel 취약성의 independent evidence로 인용 가능. "L2CP도 thin vessel의 특수성을 인식했지만 test-time adaptation에서 다루었다. 우리는 training-time SSDG에서 이를 최초로 다룬다"는 논리 가능.
- **노트 파일**: `paper_notes/L2CP.md` 참조.

---

## 3. Category C — 구조 및 혈관 특화 신규 논문

### 3.1 VESSELFM ★★ (High Rel for Cat C)
- **제목**: vesselFM: A Foundation Model for Universal 3D Blood Vessel Segmentation
- **Venue**: CVPR 2025 | **Status**: Accepted Conference
- **arXiv**: 2411.17386 | **GitHub**: https://github.com/bwittmann/vesselFM
- **핵심 방법**:
  - **3 data sources**: 17 annotated datasets + domain randomization + flow matching 생성 데이터
  - Domain randomization: 3D vessel 특화 정교한 scheme
  - Flow matching: mask- and class-conditioned 3D medical image 생성
  - 4가지 modality (brain MRA, coronary CTA, retinal vessel, liver vessel)
  - Zero/one/few-shot DG에서 SOTA
- **내 연구와의 관계 (High for Cat C, Medium for direct competition)**:
  - TOF-MRA cerebrovascular 분야의 CVPR 2025 기준 논문
  - Foundation model paradigm vs. 내 lightweight SSDG: 데이터 요구사항 근본 다름
  - Domain randomization component = global-level, 내 방법 = intra-image local-level
  - **내 논문 positioning**: "vesselFM 같은 대규모 foundation model이 있어도, 단일 site의 단일 도메인 데이터만 존재하는 임상 환경에서는 SSDG가 여전히 중요하다"
- **노트 파일**: `paper_notes/VESSELFM.md` 참조.

### 3.2 GRAPHSEG ★ (Medium Rel)
- **제목**: Towards Generalizable Retina Vessel Segmentation with Deformable Graph Priors
- **Venue**: NeurIPS 2025 (Poster 115045) | **Status**: Accepted Conference
- **OpenReview**: https://openreview.net/forum?id=zVkbsGlKn9
- **핵심 방법**:
  - Variational Bayesian framework
  - Retinal image → structure-preserved + structure-degraded components 분해
  - Statistical retinal atlas에서 도출한 deformable graph prior
  - Differentiable alignment + unsupervised energy function
  - CHASE/DRIVE/HRF에서 domain shift 조건 우수 성능
- **내 연구와의 관계 (Medium)**:
  - NeurIPS 2025의 retinal vessel DG 논문 → related work에서 언급
  - Structure-preserved/degraded 분해는 내 "관찰 가능한 구조 vs. fragile 구조" 이분법과 개념 유사
  - 단, GRAPHSEG는 retinal atlas에 의존 (특정 해부 구조), 나는 annotation 기반 local radius 계산 (일반성 높음)

---

## 4. Preprint-Only 후보 요약

| KEY | 제목 (요약) | arXiv | Cat | 중요도 |
|-----|------------|-------|-----|--------|
| DROPGEN | Why Invariance Not Enough for Biomedical DG | 2604.02564 | A | ★ |
| VESSHAPE | VessShape: Few-shot vessel seg via shape priors | 2510.27646 | C | ★ |
| SDAIRM | Semantic Aug + IRM for medical DG | 2502.05593 | A | ☆ |

### DROPGEN (arXiv 2604.02564)
- "Why Invariance is Not Enough for Biomedical Domain Generalization and How to Fix It"
- Foundation model representation + source intensities 조합
- Architecture-agnostic, loss-agnostic, 3D biomedical segmentation
- SSDG 및 few-shot 모두 지원
- "Invariance alone is not sufficient" → 내 observability conditioning이 invariance-only 접근의 한계를 보완한다는 논리 지원

### VESSHAPE (arXiv 2510.27646)
- "VessShape: Few-shot 2D Blood Vessel Segmentation by Leveraging Shape Priors from Synthetic Images"
- 합성 데이터셋 (절차적 tubular geometry + 다양한 texture)으로 shape bias 유도
- CNN의 texture bias 문제를 vessel 분야에서 다룸
- 내 방법의 thin vessel shape preservation 동기와 간접 연결

---

## 5. Category D 최신 Top-tier 요약

이번 Run에서 발견된 top-tier venue 논문:

| KEY | Venue | 핵심 기여 | 내 연구 연관성 |
|-----|-------|-----------|--------------|
| TTDG_MGM | CVPR 2025 | Universe learning + morphological prior + multi-graph matching | Medium |
| LANGDAUG | ICML 2025 | EBM + Langevin dynamics DG augmentation (이론 분석 포함) | Medium |
| GRAPHSEG | NeurIPS 2025 | Deformable graph prior + structure decomposition for vessel DG | Medium |
| VESSELFM | CVPR 2025 | 3D vessel foundation model (17 datasets + domain rand + flow matching) | High (Cat C) |

---

## 6. Novelty Gap 재확인

이번 Run에서 탐색한 8편의 신규 논문 중 **내 핵심 gap을 직접 메우는 논문은 없다.**

| Gap | 현황 |
|-----|------|
| Single foreground class 내 continuous radius 기반 aug budget 조절 | **여전히 미존재** |
| Thin vessel label-image inconsistency를 training-time SSDG aug에 명시적 반영 | **여전히 미존재** (L2CP는 test-time) |
| Source annotation만으로 계산한 local observability 기반 aug conditioning | **여전히 미존재** |

특히 L2CP (MICCAI 2025)가 thin vessel의 특수성을 DG에서 명시적으로 인식했지만,
이는 **test-time adaptation**이므로 training-time SSDG에서의 gap은 여전히 유효하다.

오히려 L2CP의 존재는 "thin vessel이 domain appearance에 특별히 취약하다"는 내 전제를
독립적인 MICCAI 2025 논문이 확인해주는 **지지 근거**가 된다.

---

## 7. 논문 전략 업데이트

### Related Work 구조 추가 논문
- **"Vessel DG foundation model"** 단락: vesselFM (CVPR 2025)
- **"Test-time adaptation for vessel DG"** 단락: L2CP (MICCAI 2025), TopoTTA (ICCV 2025)
- **"Training-time augmentation for medical DG"** 단락: LangDAug (ICML 2025), ADA (MICCAI 2025)
- **"Graph/topology priors for vessel DG"** 단락: GraphSeg (NeurIPS 2025), HarmonySeg (ICCV 2025)

### 주장 강화 근거
- L2CP: "Thin vessel의 특수성은 독립적인 MICCAI 2025 연구가 인식함"
- vesselFM: "Foundation model 접근도 있지만, single-site 임상 환경의 SSDG는 여전히 필요"
- GRAPHSEG: "Structure-preserved/degraded 분해라는 개념이 vessel DG에서 유효함을 NeurIPS 2025이 확인"

---

## 8. 다음 Run에서 추가 탐색이 필요한 구역

- [ ] vesselFM domain randomization scheme 상세: thin vessel 전용 randomization 있는지 확인
- [ ] LangDAug single-source 적용 가능성 확인 (multi-source 설정에서 single-source로 reduce 가능?)
- [ ] GraphSeg full text (OpenReview PDF): structure-preserved/degraded 분해의 vessel size dependency
- [ ] L2CP morphological closing scale: thin/thick 경계 파라미터 확인
- [ ] CVPR 2025 전체 목록 추가 확인: 의료영상 DG 관련 추가 논문 있을 수 있음
- [ ] ICCV 2025 전체 목록 확인 (403 forbidden으로 접근 실패, 다른 방법 필요)
- [ ] "vessel observability" 또는 "local vessel radius augmentation" 키워드로 재탐색

---

## 9. 커밋 정보

- Branch: `claude/wonderful-dirac-ZJJxh`
- 수정 파일: MASTER_PAPER_INDEX.md, READING_QUEUE.md, SEARCH_LOG.md, IDEA_COMPARISON_TABLE.md, weekly_reports/2026-05-31_literature_report.md, paper_notes/L2CP.md, paper_notes/VESSELFM.md
