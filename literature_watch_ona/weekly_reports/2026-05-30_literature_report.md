# Literature Watch Report — Run #4
**날짜:** 2026-05-30  
**모델:** claude-sonnet-4-6  
**신규 논문:** 13편 (Published/Accepted 12 + Preprint 1)  
**누적 총 수록:** 85편

---

## 이번 Run의 검색 초점

Run #3까지 미탐색으로 표시된 구역 집중 탐색:
- NeurIPS 2025 proceedings (이제 공개)
- CVPR 2025 main track DG 논문
- "vessel diameter / radius / observability conditioned augmentation" 직접 키워드
- DomainDrop / VectorField Transformer 후속 탐색
- Partial volume effect 관련 논문

---

## 최우선 주의 논문 — Novelty 충돌 가능성

### 1. REAUG — Domain Generalization by Rejecting Extreme Augmentations
**Venue:** WACV 2024 | **arXiv:** 2310.06670 | **Status:** Accepted Conference

**핵심 방법:**
augmentation transformation의 uniform sampling에서 강도를 높이되, "너무 극단적인" augmentation을 거부하는 reward function을 사용한다. reward는 augmented sample의 diversity와 consistency를 비교하여 계산되며, 해당 augmentation이 학습에 해로운 경우 자동으로 reject한다.

**내 방법과의 관계:**
- **공통점:** "강한 augmentation이 언제나 좋은 것은 아니다"는 인식. augmentation에 budget 또는 제약을 두는 개념.
- **핵심 차이 (내 novelty 보호 논거):**
  - REAUG의 rejection은 **이미지 전체 단위 (image-level binary)**: 하나의 augmentation이 이 이미지에 적용되어야 하는가 말아야 하는가를 결정한다.
  - 내 Continuous-ONA는 **동일 이미지 내 intra-image 연속적 조절**: 같은 이미지 안에서 얇은 혈관에는 약하게, 두꺼운 혈관에는 강하게 — **공간적, 구조적, 연속적** 차별화.
  - REAUG는 모든 혈관(또는 모든 class)에 같은 augmentation 결정을 적용한다. "어떤 구조는 다른 구조보다 augmentation에 더 취약하다"는 개념이 없다.
  - REAUG는 tubular structure의 observability / vessel radius와 무관하다.
- **결론:** novelty 충돌 위험 있음. Related Work에서 반드시 구분 논거 작성 필요.

---

### 2. GRAPHSEG — Towards Generalizable Retina Vessel Segmentation with Deformable Graph Priors
**Venue:** NeurIPS 2025 | **OpenReview:** zVkbsGlKn9 | **Status:** Accepted Conference

**핵심 방법:**
GraphSeg는 variational Bayesian framework으로, 망막 영상을 **structure-preserved component**와 **structure-degraded component**로 분리한다. 통계적 망막 아틀라스에서 추출한 deformable graph prior를 differentiable alignment를 통해 도메인 불변 표현에 통합. CHASE/DRIVE/HRF에서 domain shift하에서 SOTA 달성.

**내 방법과의 관계:**
- **공통점:** "구조 관찰 가능성"에 따른 분리 처리라는 방향. structure-preserved vs degraded 분리는 내 thin vs thick vessel 분리와 방향이 유사하다.
- **핵심 차이:**
  - GRAPHSEG = feature/representation decomposition 기반 DG: test time에 graph prior 정렬이 필요한 방법.
  - 내 방법 = augmentation 단계에서 radius에 따른 강도 조절: test time에 추가 연산 불필요, training-only 개입.
  - GRAPHSEG는 global 망막 atlas prior를 사용 — anatomy 레벨의 prior. 내 방법은 local vessel radius라는 물리적 측정값을 사용.
  - GRAPHSEG는 domain 간 graph alignment가 핵심 — 나는 augmentation strength conditioning이 핵심.
- **결론:** 고위 NeurIPS 2025 논문. Related Work에서 반드시 언급 필요. 직접 충돌은 없으나 "구조 기반 분리" 방향에서 가장 가까운 최신 논문.

---

### 3. OVSNET — Optimized Vessel Segmentation: A Structure-Agnostic Approach with Small Vessel Enhancement
**Venue:** IEEE TIP 2025 | **arXiv:** 2411.15251 | **DOI:** 10.1109/TIP.2025.3607583 | **Status:** Published Journal

**핵심 방법:**
- Macro vessel extraction: SAM의 ViT encoder backbone + feature/spatial adapter
- Micro vessel enhancement: ConvNext + FPN 기반 소혈관 특화 branch
- Morphological correction: 위상 보존 후처리
- 17개 데이터셋 (multi-modality: retina, CT, X-ray, MRI 등) 벤치마크
- connectivity 34.6% 개선

**내 방법과의 관계:**
- **공통점:** "thin/small vessel을 thick vessel과 다르게 처리해야 한다"는 동기. dual-branch 구조에서 micro/macro 분리는 내 thin/thick 구분과 동기가 동일.
- **핵심 차이:**
  - OVSNET는 segmentation model의 feature extraction architecture에서 구분: network 구조 변경.
  - 내 방법은 augmentation 단계에서 처리: architecture 변경 없이 augmentation 파이프라인에서만 조절.
  - OVSNET는 DG 방법이 아님: 주로 single-domain/multi-modality generalization. 명시적 domain shift 실험 없음.
  - OVSNET는 radius-adaptive가 아니라 structural class (micro/macro)를 binary로 구분.
- **결론:** 내 동기와 가장 직접 관련된 vessel 논문. "모델 구조"가 아닌 "augmentation budget"에서 해결한다는 차이가 핵심.

---

## 주요 신규 발견 요약

### Category A (직접 경쟁)

| KEY | Venue | 핵심 내용 | Rel |
|-----|-------|-----------|-----|
| GRAPHSEG | NeurIPS 2025 | Structure-preserved/degraded 분리 + deformable graph prior for retinal vessel DG | High |
| BUCKETAUG | IEEE OJEMB 2024 | Q-learning 기반 augmentation policy for CT DG | Medium |
| DOMRAND_IMG_FEAT | Radiology AI 2025 | image+feature 동시 domain randomization for multiorgan DG | Medium |
| FASAM | IEEE SMC 2025 | Fully automated SAM for SSDG (lower venue) | Low |

### Category B (방법론 유사)

| KEY | Venue | 핵심 내용 | Rel |
|-----|-------|-----------|-----|
| REAUG | WACV 2024 | Reward function으로 extreme augmentation reject → augmentation budget 개념 | **High** |
| DFA | ACM MM 2024 | Uncertainty-guided hard cross-domain feature 생성 + adversarial causal masking | Medium |
| SDCL | IEEE TIP 2025 | Style as confounder → backdoor causal learning + style-guided expert module | Medium |
| SCSD | AAAI 2025 | Semantic consistency + style diversity for DGSS (natural image) | Medium |
| STYLIZING_VIT | ISBI 2026 | Anatomy-preserving weight-shared ViT attention for DG (medical classification) | Low |

### Category C (혈관·tubular 특화)

| KEY | Venue | 핵심 내용 | Rel |
|-----|-------|-----------|-----|
| OVSNET | IEEE TIP 2025 | Dual-branch macro/micro vessel extraction + 17-dataset benchmark | **High** |
| COW_TOPOLOGY | CBM 2026 | nnUNet + Skeleton Recall + topology post-proc for CoW MRA/CTA; TopCoW 2024 winner | Medium |

### Category D (Top-tier Vision 아이디어 전이)

| KEY | Venue | 핵심 내용 | Rel |
|-----|-------|-----------|-----|
| GRAPHSEG | NeurIPS 2025 | (Cat A에도 포함) 구조 분리 기반 DG representation | High |
| SCSD | AAAI 2025 | (Cat B에도 포함) semantic consistency + style diversity for DGSS | Medium |
| DOMRAND_NEURO | IEEE SPM 2025 | Neuroimage domain randomization: intensity randomization from anatomy maps | Low |

### Preprint 후보

| KEY | arXiv | 핵심 내용 | Rel |
|-----|-------|-----------|-----|
| ASAUG | 2505.23438 | Entropy-based adaptive aug strength per instance (semi-supervised, not DG) | Medium |

---

## 주요 발견 없음 — 내 방법의 탐색 Gap 재확인

이번 run에서도 다음 키워드에 대한 직접 논문이 없음을 재확인:
- "vessel radius conditioned augmentation"
- "observability conditioned augmentation"
- "intra-image structure-adaptive augmentation budget"
- "thin vessel protection during augmentation"

이는 내 Continuous-ONA가 다루는 문제가 **현재까지 명시적으로 다루어지지 않은 gap**임을 3 run 연속으로 확인한 것이다.

---

## 이번 Run 이후 내 논문에 미치는 영향

### Related Work 업데이트 필요 항목
1. **REAUG** — "augmentation이 해로울 수 있다"는 인식 측면에서 반드시 인용하고 차이를 명시할 것
2. **GRAPHSEG** — NeurIPS 2025 최신 vessel DG. Related Work에서 언급 필수
3. **OVSNET** — "thin vessel을 별도 처리"하는 motivation 공유 논문으로 Related Work 인용 가능

### Baseline 추가 검토 후보
- REAUG: augmentation rejection 기반 baseline으로 추가 고려 가능

### 아이디어 강화에 활용 가능
- GRAPHSEG의 "structure-preserved / structure-degraded" 분리 개념 → 내 방법이 augmentation 단계에서 동일 논리를 구현한다는 점을 서술에 활용
- OVSNET의 micro vessel enhancement 동기 → 내 thin vessel 보호 동기의 supplementary evidence로 인용

---

## 다음 Run 탐색 권장 항목

- [ ] GRAPHSEG arXiv ID 확인 (OpenReview에서만 확인됨)
- [ ] REAUG full text 확인: reward function 수식 및 augmentation rejection 기준 상세
- [ ] NeurIPS 2025 전체 proceedings에서 추가 DG/augmentation 논문 탐색
- [ ] CVPR 2025 openaccess 직접 확인 (2025년 6월 이후 공개 여부)
- [ ] TopBrain 2025 challenge (whole brain vessel) dataset 및 관련 논문 탐색
- [ ] COW_TOPOLOGY 인용 후속 및 TopCoW 2024 challenge proceedings

---

*Sources:*
- [GRAPHSEG @ NeurIPS 2025](https://neurips.cc/virtual/2025/poster/115045)
- [REAUG @ WACV 2024](https://openaccess.thecvf.com/content/WACV2024/papers/Aminbeidokhti_Domain_Generalization_by_Rejecting_Extreme_Augmentations_WACV_2024_paper.pdf)
- [OVSNET @ IEEE TIP](https://ieeexplore.ieee.org/document/11185323/)
- [SCSD @ AAAI 2025](https://ojs.aaai.org/index.php/AAAI/article/view/32668)
- [DFA @ ACM MM 2024](https://dl.acm.org/doi/10.1145/3664647.3680652)
- [SDCL @ IEEE TIP](https://ieeexplore.ieee.org/iel8/34/11474534/11344809.pdf)
- [ASAug arXiv](https://arxiv.org/abs/2505.23438)
- [COW_TOPOLOGY @ CBM](https://pubmed.ncbi.nlm.nih.gov/41637822/)
