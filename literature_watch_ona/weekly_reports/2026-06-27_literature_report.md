# Literature Watch Report — Run #8 (2026-06-27)

> 연구 주제: Single-source domain generalization for TOF-MRA cerebrovascular segmentation  
> 방법: Continuous-ONA (Continuous Observability-Conditioned Nonlinear Augmentation)  
> 총 신규 논문: **9편** (Published Journal 3편 + Accepted Conference 3편 + Preprint 3편)

---

## 1. 이번 실행 핵심 요약

이번 Run #8에서 발견된 가장 중요한 논문은 **TSIAA (IEEE TMI 2026)**이다.  
"breaks the uniformity of augmentation rules across different structures within an image"라는 표현이 내 Continuous-ONA의 핵심 주장과 거의 동일하다.  
즉시 full text 독해 후 novelty 구분 논거를 강화해야 한다.

또한 CVPR 2026 accepted 논문 **SCNP**와 MICCAI 2026 accepted 논문 **CSWinUNETR**,  
SIAM Imaging Sciences 2026 저널 논문 **TopoGuar**가 신규 발견되었다.

---

## 2. 신규 논문 목록 (우선순위 순)

### P0 — 즉시 독해 필요 (Novelty 위험)

#### ⚠️ TSIAA (IEEE TMI 2026) — **즉시 읽어야 함**

| 항목 | 내용 |
|------|------|
| 제목 | Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation |
| 저자 | Zhengshan Wang et al. |
| Venue | IEEE Transactions on Medical Imaging 2026, vol 45, pp 764–776 |
| Status | **Published Journal Article** |
| IEEEXplore | doc 11146907 |
| Category | A (직접 경쟁) |
| Relevance | **High** |

**핵심 방법:**  
- Instance-level Image Augmenter (IIAG): 복수의 Instance-level Augmentation Modules (IAM)으로 구성  
- 각 IAM은 **learnable constrained Bézier transformation function** 기반  
- Teacher-Student 구조: adversarial augmentation (out-of-source 탐색) + consistent representation learning  
- 실험: prostate MRI (NCI-ISBI13 → 5 sites) + fundus (REFUGE → 3 sites)

**내 방법과의 관계:**  
> "Compared to image-level adversarial augmentation, instance-level adversarial augmentation **breaks the uniformity of augmentation rules across different structures within an image**, thereby providing greater diversity."

이 표현은 내 핵심 novelty claim과 단어 수준에서 겹친다.

**핵심 차이 (구분 논거):**
- **TSIAA**: "instance-level" = semantic instance 단위 discrete Bézier 파라미터. 각 전경 패치/region에 독립적 Bézier 적용. Adversarial 방식으로 탐색.
- **Continuous-ONA**: vessel foreground **단일 class 내**에서 **local radius라는 연속 물리량**을 조건으로 augmentation budget을 smooth하게 조절. Label-image consistency 보호라는 명시적 목표. Adversarial 없음 (annotation-derived).

즉, TSIAA는 서로 다른 semantic instance(object) 간 augmentation 불균일을, 나는 같은 semantic class 내 구조 물리적 속성에 따른 연속 조절을 다룬다.

---

### P1 — 높은 우선순위 (배경/방법 이해)

#### SCNP (CVPR 2026)

| 항목 | 내용 |
|------|------|
| 제목 | Towards High-Quality Image Segmentation: Improving Topology Accuracy by Penalizing Neighbor Pixels |
| 저자 | Juan Miguel Valverde, Dim P. Papadopoulos, Rasmus Larsen, Anders Bjorholm Dahl (DTU) |
| Venue | CVPR 2026 |
| arXiv | 2603.18671 |
| Status | **Accepted Conference Paper** |
| Category | D (Top-tier Vision) |
| Relevance | Medium |

**핵심 방법:**  
- SCNP (Same-Class Neighbor Penalization): 가장 잘못 분류된 이웃 픽셀의 logit을 penalty로 누적  
- 모델이 이웃 픽셀을 개선하기 전에 현재 픽셀부터 개선하도록 강제  
- 13 datasets, semantic + instance segmentation, 3 frameworks에 통합  
- Code: jmlipman.github.io/SCNP-SameClassNeighborPenalization

**내 연구 적용 가능성:**  
Thin vessel topology 평가 시 SCNP를 loss 보완으로 활용할 수 있음.  
"구조 topology를 명시적으로 보호"하는 최신 CVPR 논문으로 related work 인용 가능.

---

#### TOPOGUAR (SIAM Journal on Imaging Sciences 2026)

| 항목 | 내용 |
|------|------|
| 제목 | Topology-Guaranteed Image Segmentation: Enforcing Connectivity, Genus, and Width Constraints |
| 저자 | Wenxiao Li et al. |
| Venue | SIAM Journal on Imaging Sciences 2026 |
| arXiv | 2601.11409 |
| DOI | 10.1137/25M1765870 |
| Status | **Published Journal Article** |
| Category | C (구조·혈관 특화) |
| Relevance | Medium |

**핵심 방법:**  
- Width-aware persistent homology: vessel thickness와 length를 topological energy에 통합  
- PDE smoothing + persistent homology로 local extrema 수정  
- Variational 모델 + deep neural networks 모두 적용 가능

**내 연구 적용 가능성:**  
> "single-pixel-width lines achieve topological connectivity but compromise blood perfusion analysis"  

이 진술은 내 "얇은 혈관의 label-image inconsistency" 주장의 외부 지지 근거로 인용 가능.  
Width 유지가 의학적으로 중요하다는 것을 수학적 topology 관점에서 뒷받침.

---

#### AD_DGCL (Neurocomputing 2026)

| 항목 | 내용 |
|------|------|
| 제목 | Multi-organ Medical Image Segmentation via Adaptive Disentangled Domain Generalization Collaborative Learning |
| Venue | Neurocomputing 2026 |
| ScienceDirect | pii/S0925231225025184 |
| Status | **Published Journal Article** |
| Category | A (직접 경쟁) |
| Relevance | Medium |

**핵심 방법:**  
- SSRD: dual encoder로 domain-specific style ↔ anatomical content 분리 + cross-domain contrastive learning  
- SCT: synthetic style perturbation + consistency regularization  
- **Adaptive region-specific loss**: pixel frequency에 따라 small organ에 동적 loss weight 부여

**내 방법과의 관계:**  
"small organ은 더 많은 주의가 필요하다"는 방향이 내 "thin vessel은 augmentation 보호가 필요하다"와 개념 유사.  
단, AD-DGCL = semi-supervised multi-organ (CT), loss-side 조절. 나 = single-vessel SSDG, aug-side 조절.

---

### P2 — 중간 우선순위

#### WAVESDG (arXiv:2603.28463, March 2026) — Preprint Only

| 항목 | 내용 |
|------|------|
| 제목 | Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation |
| Venue | arXiv (preprint) |
| arXiv | 2603.28463 |
| Status | **Preprint Only** |
| Category | A (직접 경쟁) |
| Relevance | Medium |

WISER module: LL sub-band = global anatomy 고정, LH/HL/HH = directional edge enhancement + noise suppression.  
Fundus optic disc/cup, 1 source → 5 unseen target. 직접 경쟁 없음 (wavelet vs. radius conditioning).

---

#### CSWUNETR (arXiv:2606.19824, MICCAI 2026)

| 항목 | 내용 |
|------|------|
| 제목 | CSWinUNETR: Segmentation of Thin Anatomical Structures in Medical Images |
| 저자 | Junho Moon, Haejun Chung, Ikbeom Jang |
| Venue | MICCAI 2026 |
| arXiv | 2606.19824 |
| Status | **Accepted Conference Paper** |
| Category | C (혈관·tubular 특화) |
| Relevance | Medium |

Cross-shaped stripe self-attention + cyclic shifts. Retinal vessels, cerebral vasculature, facial wrinkles 적용.  
Architecture 관련 논문 (aug 방법과 직접 경쟁 없음). Related work 인용 가능.

---

#### VesselSDF (arXiv:2506.16556, MICCAI 2025)

| 항목 | 내용 |
|------|------|
| 제목 | VesselSDF: Distance Field Priors for Vascular Network Reconstruction |
| 저자 | Salvatore Esposito, Daniel Rebain, Arno Onken, Changjian Li, Oisin Mac Aodha |
| Venue | MICCAI 2025 |
| arXiv | 2506.16556 |
| Status | **Accepted Conference Paper** |
| Category | C (혈관·tubular 특화) |
| Relevance | Medium |

SDF regression으로 smooth tubular geometry 포착. Adaptive Gaussian regularizer로 floating segment 제거.  
CT sparse slices 기반 재구성. 내 TOF-MRA SSDG와 데이터/task 다름.

---

#### TOPOVST (arXiv:2603.14909, March 2026) — Preprint Only

| 항목 | 내용 |
|------|------|
| 제목 | TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking |
| 저자 | Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu |
| Venue | arXiv |
| arXiv | 2603.14909 |
| Status | **Preprint Only** |
| Category | C (혈관·tubular 특화) |
| Relevance | Medium |

Multi-scale sphere graphs + GNN으로 tracking direction + **vessel radii** 동시 추정.  
Wave propagation 기반 skeleton tracking. 내 observability score 계산 참고 가능.

---

#### PMDG (arXiv:2505.23173, May 2025) — Preprint Only

| 항목 | 내용 |
|------|------|
| 제목 | Pseudo Multi-Source Domain Generalization: Bridging the Gap Between Single and Multi-Source Domain Generalization |
| 저자 | Shohei Enomoto (NTT) |
| Venue | arXiv |
| arXiv | 2505.23173 |
| Status | **Preprint Only** |
| Category | A (직접 경쟁) |
| Relevance | Medium |

Single source에서 style transfer + aug로 pseudo multi-domain 생성, MDG 알고리즘 적용.  
내 방법과 직접 경쟁 없음 (다른 DG 패러다임). 일반 DG (non-medical).

---

## 3. Novelty Gap 현황 (Run #8 기준)

| Gap 항목 | 현황 |
|----------|------|
| "radius-conditioned augmentation budget" | 직접 명시 논문 없음 ✅ |
| "intra-class continuous augmentation conditioning" | 직접 명시 논문 없음 ✅ |
| "label-image consistency for thin vessel" | 직접 명시 논문 없음 ✅ |
| "observability-conditioned nonlinear appearance aug" | 직접 명시 논문 없음 ✅ |
| TSIAA의 "uniformity 파괴" 표현 유사 | 차이: discrete instance vs. continuous radius — 구분 가능 ⚠️ |

---

## 4. IDEA_COMPARISON_TABLE 업데이트 필요 항목

| 방법 | 신규 비교 항목 |
|------|---------------|
| **TSIAA** | Aug. Target: Instance-level (semantic patch) / Aug. Condition: Adversarially learned Bézier / Structure Signal: ❌ / Label Safety: ❌ (목적이 diversity 극대화) |
| **SCNP** | Loss-based topology accuracy. Aug 방법 아님. |
| **TOPOGUAR** | Width-aware topology loss. 내 thin vessel 보호 동기 지지. |

---

## 5. 다음 실행을 위한 미탐색 구역

- [ ] TSIAA full text: "instance"의 구체적 구현 확인 (per-pixel? per-semantic-segment? per-patch?)
- [ ] SCNP (CVPR 2026): neighbor penalization이 thin vessel topology에 미치는 effect 수치 확인
- [ ] WaveSDG: WISER 모듈 세부 구현 및 sub-band별 처리 방식
- [ ] AD-DGCL: pixel frequency 기반 adaptive loss weight 공식
- [ ] TopoVST: radius 추정의 신뢰도 및 계산 비용
- [ ] MICCAI 2026 추가 accepted 논문: DG + vessel 관련 탐색
- [ ] ECCV 2026 submission deadline / call for papers 확인
- [ ] "augmentation budget" 또는 "augmentation intensity" + "vessel" + "radius" OR "diameter" 키워드 재탐색
