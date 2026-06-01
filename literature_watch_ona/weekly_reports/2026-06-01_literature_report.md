# Literature Watch Report — Run #5
**날짜:** 2026-06-01  
**총 신규 발견:** 6편 (Accepted Conference 1 + Published Journal 2 + Workshop 2 + Preprint 1)  
**누적 수록 논문:** 78편

---

## 이번 주 핵심 요약

이번 Run #4에서 가장 중요한 발견은 **COSTA (IEEE TMI 2024)** — 내 연구와 정확히 동일한 TOF-MRA cerebrovascular segmentation 도메인에서 multi-center style heterogeneity를 직접 다룬 논문이다. 이 논문의 존재는 내 방법의 비교 baseline을 재구성하는 데 영향을 줄 수 있다.

또한 **TTDG-MGM (CVPR 2025 main track)** — test-time domain generalization이지만 morphological priors를 활용한다는 점에서 내 방법과 motivation을 공유한다.

---

## 신규 추가 논문

### 1. TTDG-MGM — 즉시 읽기 권장 (P1)
**"Test-Time Domain Generalization via Universe Learning: A Multi-Graph Matching Approach for Medical Image Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | TTDG_MGM |
| Venue | CVPR 2025 (main track) |
| arXiv | 2503.13012 |
| Category | A |
| Status | Accepted Conference Paper |
| Relevance | High |

**핵심 방법:**
- **Universe Embeddings**: 여러 source domain의 morphological prior를 learnable embedding으로 통합
- **Multi-Graph Matching**: 도메인 간 cycle-consistency 보장
- Test-time unsupervised adaptation으로 unseen target에 적용
- SSDG + multi-source DG 두 설정 모두 실험

**내 연구와의 관계:**
- Training-time augmentation (나) vs Test-time adaptation (TTDG-MGM) — 접근 방식이 근본적으로 다름
- "morphological priors" 동기를 공유하지만 이를 활용하는 방식이 상이
- 내 Continuous-ONA 이후 test-time 개선 방법으로 적용 가능
- Novelty 충돌 낮음

---

### 2. COSTA — 즉시 읽기 권장 (P0 수준)
**"COSTA: A Multi-Center TOF-MRA Dataset and a Style Self-Consistency Network for Cerebrovascular Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | COSTA |
| Venue | IEEE Transactions on Medical Imaging |
| Year | 2024 |
| Vol/Pages | 43(12), 4442–4456 |
| DOI | 10.1109/tmi.2024.3424976 |
| Category | A |
| Status | Published Journal Article |
| Relevance | **High** |

**핵심 방법:**
- **COSTA Dataset**: 8개 imaging center, multi-vendor TOF-MRA, 전체 annotated
- **CESAR Network**:
  - Coarse-to-fine architecture (iterative refinement)
  - Automatic Feature Selection: long-range dependency + local context
  - **Style Self-Consistency Loss**: 다양한 center style을 단일 표준 style로 정렬

**내 연구와의 관계:**
- 내 실험 환경과 정확히 동일한 TOF-MRA multi-center 설정
- Style self-consistency는 나의 appearance augmentation 방향과 다름 (나는 augmentation, COSTA는 feature alignment)
- COSTA 데이터셋이 내 평가에 사용 가능할 수 있음 → GitHub 확인 필요 (iMED-Lab/COSTA)
- 비교 baseline 필수 후보

**Novelty 충돌 여부:**
- 충돌 없음. COSTA는 style alignment 방법, 나는 structure-conditioned augmentation
- 다만 COSTA를 reference dataset으로 내 논문에서 cite해야 할 가능성 있음

---

### 3. OVS-Net (P2)
**"Optimized Vessel Segmentation: A Structure-Agnostic Approach with Small Vessel Enhancement and Morphological Correction"**

| 항목 | 내용 |
|------|------|
| KEY | OVS_NET |
| Venue | IEEE Transactions on Image Processing |
| Year | 2025 |
| Vol/Pages | 34, 7168–7179 |
| arXiv | 2411.15251 |
| Category | C |
| Status | Published Journal Article |
| Relevance | Medium |

**핵심 방법:**
- **Macro vessel branch**: ViT backbone + feature/spatial adapters (SAM 기반)
- **Micro vessel branch**: ConvNext + FPN 기반 small vessel enhancement
- **Morphological correction module**: U-Net 기반 topology/connectivity 보정 후처리

**내 연구와의 관계:**
- "overlap-optimized segmentation이 small/fragile vessel을 무시한다"는 논문의 진술은 내 motivation의 직접적인 supporting evidence
- OVS-Net은 inference-time 추가 모듈, 나는 training-time augmentation 방식 — 겹치지 않음
- 내 논문 도입부에서 이 주장을 인용할 수 있음

---

### 4. Domain Game (P2, Workshop)
**"Domain Game: Disentangle Anatomical Feature for Single Domain Generalized Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | DOMAIN_GAME |
| Venue | MICCAI 2024 Workshop (CMMCA) |
| DOI | 10.1007/978-3-031-73360-4_5 |
| arXiv | 2406.02125 |
| Category | A |
| Status | Workshop Paper |
| Relevance | Medium |

**핵심 방법:**
- Geometric transformation에 대한 민감도 차이로 anatomical feature vs domain-specific feature를 분리
- Anatomical feature는 geometric transform에 민감 → 이를 task-relevant feature로 활용
- AGTA와 같은 MICCAI 2024 CMMCA Workshop volume에 수록

**내 연구와의 관계:**
- Feature-level disentanglement (Domain Game) vs augmentation-level structural conditioning (나) — 다른 접근
- AGTA와 같이 내 방법과 동일한 workshop에서 나온 논문이지만 방향이 다름
- Novelty 충돌 낮음

---

### 5. MoSE (P3, Workshop)
**"Mixture-of-Shape-Experts (MoSE): End-to-End Shape Dictionary Framework to Prompt SAM for Generalizable Medical Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | MOSE |
| Venue | CVPR 2025 Workshop (DG-EBF) |
| arXiv | 2504.09601 |
| Category | A |
| Status | Workshop Paper |
| Relevance | Low |

**핵심 방법:**
- Shape dictionary learning + Mixture-of-Experts gating
- SAM encoder guidance로 sparse activation
- Shape priors를 SAM prompt로 변환 → generalizable segmentation

**내 연구와의 관계:**
- SAM 기반 접근, 나는 augmentation 기반 — 방향 다름
- Shape prior 활용 아이디어는 참고 가능하나 낮은 우선순위

---

### 6. FL-AugDG (Preprint 후보, B급)
**"Federated Learning for Cross-Modality Medical Image Segmentation via Augmentation-Driven Generalization"**

| 항목 | 내용 |
|------|------|
| KEY | FL_AUGDG |
| arXiv | 2602.20773 |
| Year | 2026 |
| Category | B |
| Status | Preprint Only |
| Relevance | Medium |

**핵심 내용:**
- Federated + centralized 설정에서 GIN, spatial aug, frequency aug, normalization 비교
- GIN이 모든 설정에서 일관되게 최상 성능 (pancreas: 0.073 → 0.437)
- GIN의 범용 우수성을 re-confirm하는 paper

**내 연구와의 관계:**
- 내 uniform nonlinear augmentation baseline 구성 시 GIN 선택의 근거로 활용 가능
- "GIN은 이미 광범위하게 검증된 baseline"임을 뒷받침하는 citation

---

## Novelty 충돌 분석 (이번 Run 기준)

| 논문 | 충돌 가능성 | 핵심 구분점 |
|------|------------|------------|
| TTDG-MGM | **낮음** | test-time adaptation vs 내 training-time augmentation |
| COSTA | **없음** | feature alignment vs 내 structure-conditioned aug |
| OVS-Net | **없음** | inference-time dual-branch vs 내 training-time aug |
| Domain Game | **낮음** | geometric sensitivity disentanglement vs 내 radius-conditioned aug |

기존 high-priority 충돌 목록 (ADA, MBFCV, SRCSM, AGTA, ICRN)에 변화 없음.

---

## 내 연구 novelty 유지 확인

이번 Run #4에서도 다음은 확인되었다:

> **"vessel foreground 내에서 local observability/radius에 따라 nonlinear augmentation strength를 연속적으로 조절한다"**  
> → 직접적으로 이 아이디어를 구현한 논문은 여전히 발견되지 않음.

COSTA가 TOF-MRA 연구자들에게 중요한 reference가 될 수 있으나, style alignment 방법론적으로는 내 방법과 겹치지 않는다.

---

## 다음 Run 탐색 제안

- [ ] COSTA GitHub(iMED-Lab/COSTA) dataset 접근 가능성 + 데이터 규모 확인
- [ ] CVPR 2025 open access에서 augmentation budget / structural DG 추가 탐색
- [ ] NeurIPS 2025 medical imaging 논문 추가 탐색
- [ ] TTDG-MGM 상세 독해: universe embedding의 morphological prior 정의 방식
- [ ] OVS-Net 독해: "small vessel이 overlap metric에서 무시된다" 정량적 분석 확인
