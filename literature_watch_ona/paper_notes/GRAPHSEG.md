# GRAPHSEG — Towards Generalizable Retina Vessel Segmentation with Deformable Graph Priors

**Venue:** NeurIPS 2025 (Poster)  
**OpenReview:** https://openreview.net/forum?id=zVkbsGlKn9  
**NeurIPS page:** https://neurips.cc/virtual/2025/poster/115045  
**Authors:** Ke Liu, Shangde Gao, Yichao Fu, Shangqi Gao  
**Status:** Accepted Conference  
**발견:** Run #4 (2026-05-30)  
**Category:** A (직접 경쟁) + D (Top-tier Vision)  
**Rel:** High

---

## 핵심 방법

GraphSeg: variational Bayesian framework for generalizable retinal vessel segmentation.

1. **Image Decomposition:** 망막 영상을 두 컴포넌트로 분리:
   - **Structure-preserved component:** domain-invariant vessel 구조 정보
   - **Structure-degraded component:** domain-specific appearance 정보
2. **Deformable Graph Prior:** 통계적 망막 아틀라스에서 추출한 그래프 prior. differentiable alignment을 통해 통합. unsupervised energy function으로 학습.
3. **Cross-domain Generalization:** CHASE / DRIVE / HRF 3개 벤치마크에서 domain shift하에서 SOTA.

---

## 내 Continuous-ONA와의 관계

### 공통점
- "구조 관찰 가능성(structure observability)"에 따른 분리 처리라는 방향이 유사.
- 얇은 혈관의 구조 정보가 appearance variation에 의해 손상될 수 있다는 문제의식과 연결됨.
- 망막 혈관 DG = 내 TOF-MRA DG와 동일 problem 계열.

### 핵심 차이

| 항목 | GRAPHSEG | 내 Continuous-ONA |
|------|----------|-------------------|
| 개입 단계 | Feature/representation space (decomposition) | Augmentation/input space |
| 방법 | Variational Bayesian + graph prior alignment | Radius-conditioned nonlinear appearance transform |
| Test time | Graph prior alignment 필요 | 추가 연산 없음 (augmentation-only 개입) |
| Domain information | Atlas prior (anatomy-level 사전 지식) | Source annotation의 local radius (측정값) |
| 구조 단위 | 전체 이미지의 structure/style 분리 | 동일 이미지 내 혈관별 관찰 가능성에 따른 budget |

### 내 주장에서 GRAPHSEG를 언급하는 방식

> "GraphSeg [NeurIPS 2025] achieves domain generalization for retinal vessel segmentation by decomposing images into structure-preserved and structure-degraded components, guided by an anatomical graph prior. While this demonstrates the importance of structure-aware representation for vessel DG, GraphSeg operates at the representation level and requires test-time alignment with a statistical atlas prior. Our Continuous-ONA instead addresses the problem at the augmentation level: by assigning conservative appearance perturbations to fragile thin vessels and stronger perturbations to robustly-visible thick vessels within the same training image. This requires no test-time overhead and no additional anatomical atlas."

---

## 읽을 때 확인할 것

- [ ] arXiv ID 확인 (OpenReview에만 있고 arXiv preprint 여부 불명)
- [ ] 구체적인 structure-preserved / structure-degraded 분리 수식
- [ ] variational Bayesian formulation: evidence lower bound (ELBO)의 각 항이 무엇인가?
- [ ] 어떤 baseline과 비교했는가? (SLAug, RASS, HESSIAN_VF와 비교했는가?)
- [ ] 얇은 혈관 (thin vessel) 성능 분석이 있는가?
- [ ] TOF-MRA나 3D vessel segmentation으로 확장 가능성
