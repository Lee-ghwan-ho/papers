# AADG — Paper Note

**KEY**: AADG  
**제목**: AADG: Automatic Augmentation for Domain Generalization on Retinal Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging (TMI), 2022  
**arXiv**: 2207.13249  
**GitHub**: https://github.com/CRazorback/AADG  
**발견**: Run #6 (2026-06-02)  
**우선순위**: P0 — Novelty 관련 foundational paper, 즉시 독해 필요

---

## 한 줄 요약

Adversarial training + deep RL로 augmentation policy를 자동 탐색하고, Sinkhorn distance 기반 domain diversity를 proxy task로 사용해 retinal DG를 달성한 foundational 논문.

---

## 핵심 방법

### 1. Search Space
- 다양한 appearance augmentation operation들의 조합 및 magnitude를 search space로 정의
- 색상, 대비, 밝기, 기하학적 변환 등 포함

### 2. Proxy Task
- 여러 augmented novel domain 간 **Sinkhorn distance를 최대화** → 다양한 domain을 생성하도록 유도
- 단위 구체(unit sphere) 공간에서 distance 측정

### 3. Optimization
- **Adversarial training**: augmentation policy와 segmentation network가 서로 adversarial
- **Deep RL**: policy gradient로 search space 탐색
- Model-agnostic: learned policy가 다른 아키텍처에도 transfer 가능

### 4. 평가
- 11개 fundus dataset (retinal vessel 4개 + OD/OC 4개 + lesion 3개)
- 2개 OCTA dataset (cross-modality)
- SLAug 등 이후 논문들이 이를 prior work로 인용

---

## 내 Continuous-ONA와의 비교

| 측면 | AADG | Continuous-ONA |
|------|------|----------------|
| 적응 단위 | 이미지 전체 (cross-image policy) | 이미지 내 혈관별 (intra-image, per-structure) |
| 구조 인식 | 없음 (foreground 전체 동일 취급) | 혈관 반경/관찰가능성 기반 continuous conditioning |
| thin vessel 보호 | 없음 | 핵심 기여 (thin vessel = 약한 aug, thick vessel = 강한 aug) |
| Aug 강도 | 전체 policy search (어떤 op + 얼마나) | 동일 augmentation family 내 연속적 강도 조절 |
| DG 설정 | Multi-source (여러 fundus dataset) | Single-source (SSDG) |
| 모달리티 | 2D retinal fundus / OCTA | 3D TOF-MRA |

**핵심 novelty 구분**:
> AADG는 "어떤 augmentation을 얼마나 강하게 적용할지"를 **이미지 단위**로 자동화.  
> Continuous-ONA는 "동일한 augmentation을 같은 이미지 내에서 **구조마다 다른 강도로**" 적용.  
> AADG가 해결하지 못한 문제: 하나의 이미지 안에서 thin vessel과 thick vessel이 공존할 때, 동일한 aug 강도를 적용하면 thin vessel의 visibility evidence가 파괴된다.

---

## 내 논문에서의 활용

### Related Work 서술 예시
> Prior work AADG [ref] automatically searches for augmentation policies that maximize cross-domain diversity via Sinkhorn distance, demonstrating that adaptive augmentation strategies outperform fixed ones. However, AADG treats the entire image as a single unit for policy assignment. Our Continuous-ONA identifies a finer-grained problem: **within a single image**, resolved tubular structures can tolerate stronger appearance interventions, while fragile thin structures require conservative perturbations to preserve their weak but essential structural evidence. This intra-image, structure-conditioned augmentation budget allocation is orthogonal to and complementary with AADG-style cross-image policy search.

### Baseline 검토
- AADG의 search space를 내 setting (TOF-MRA SSDG)에 적용하면 비교 baseline이 될 수 있음
- 단, AADG는 multi-source 설정에 최적화되어 있어 SSDG에 direct 적용은 비효율적일 수 있음

---

## 읽어야 할 내용

- [ ] Table 2: AADG의 retinal vessel segmentation 결과 (DRIVE, CHASE_DB, HRF, STARE)
- [ ] Section 3.2: proxy task 및 Sinkhorn distance 구체적 계산 방법
- [ ] Section 4.1: search space 구성 상세 (내 Bézier/nonlinear aug와 overlap 확인)
- [ ] Ablation: Sinkhorn diversity vs. direct policy gradient 비교
