# Literature Watch Report — Run #8 (2026-06-25)

> 검색 범위: 2026-06-03 이후 신규 논문  
> 신규 수록: **11편** (Published Journal 1 + Accepted Conference 5 + Preprint 5)  
> 누적 총계: 107편  
> 담당 모델: claude-sonnet-4-6

---

## 최우선 주의 — Novelty 충돌 경보 ⚠️

### TSIAA (IEEE TMI 2026) — 즉시 독해 필수

**Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation**  
Zhengshan Wang, Long Chen et al. | IEEE TMI Vol.45, pp.764–776 | DOI: 10.1109/TMI.2025.3605162

**충돌 위험 수준: HIGH**

논문에서 명시한 구절:
> "breaks the uniformity of augmentation rules across different structures within an image, thereby providing greater diversity"

이는 내 ONA의 핵심 주장("tubular structures should not receive a uniform augmentation budget")과 **표면적으로 동일한 주장**처럼 보인다.

**그러나 핵심 차이는 명확하다:**

| | TSIAA | Continuous-ONA |
|---|---|---|
| 비균일화 단위 | Semantic instance (anatomical object 간) | Single class 내 local spatial radius |
| 조절 기준 | Adversarial student loss → 학습된 Bézier param | GT annotation → local radius / observability score |
| Thin vessel 보호 | 없음 (adversarial → thin에도 max aug 적용 가능) | 명시적 보호 (observability 낮을수록 conservative aug) |
| Intra-class 이질성 | 반영 없음 | 핵심 contribution |
| 적용 대상 | Multi-organ (prostate, fundus, skin) | Single tubular foreground (vessel) |

**내 논문 대응 전략:**
- TSIAA를 Related Work에서 "TSIAA가 inter-instance non-uniformity의 효과를 증명했다면, 우리는 intra-class spatial non-uniformity로 확장한다"는 방향으로 포지션
- TSIAA의 IEEE TMI 2026 발표가 내 관점의 중요성을 지지하는 근거로 활용 가능
- paper_notes/TSIAA.md에 세부 차이 분석 기록

---

## 이번 주 신규 논문 요약

### Category A — SSDG / Medical Segmentation DG

#### 1. TSIAA (IEEE TMI 2026) ⚠️
- **Published Journal Article** | Rel: **High**
- 위 최우선 섹션 참조

#### 2. WaveSDG (arXiv 2603.28463, April 2026)
- **Preprint Only** | Rel: High
- Shramana Dey, Varun Ajith, Abhirup Banerjee, Sushmita Mitra (ISI)
- **WISER module**: wavelet sub-band를 semantic 역할별 분리 (저주파=global anatomy, 고주파=domain style)
- Encoder feature를 wavelet sub-band별로 처리 → domain-stable anatomical topology 추출
- Optic cup/disc 분할, 5개 unseen domain, 7개 SOTA 능가
- 내 방법과 mechanism 다름 (wavelet freq decomp vs. spatial radius budget) 직접 충돌 없음

#### 3. MAMBA_SEA (arXiv 2504.17515, April 2025)
- **Preprint Only** | Rel: Medium
- Mamba SSM 기반 global-to-local sequence augmentation for generalizable medical segmentation
- SSM의 long-range dependency를 augmentation policy에 활용

#### 4. SegMoTE (CVPR 2026 Oral, arXiv 2602.19213)
- **Accepted Conference Paper** | Rel: Medium
- Token-level Mixture of Experts in SAM-based framework
- MedSeg-HQ (소규모 데이터) 학습 → zero-shot cross-domain 일반화
- 내 방법과 paradigm 완전히 다름 (large foundation model MoE vs. lightweight SSDG aug)
- CVPR 2026 최신 의료영상 DG 방향 파악용

---

### Category B — 방법론 유사

#### 5. SGDC (arXiv 2602.23496, Feb 2026)
- **Preprint Only** | Rel: Medium
- Bo Shi, Wei-ping Zhu, M.N.S. Swamy (Concordia Univ.)
- Structurally-Guided Dynamic Convolution: structure-extraction branch → dynamic kernel & gating signal 생성
- "average-pooled features suppress high-frequency structural details" 문제 해결
- Dermatology/pathology 실험 (혈관 DG 아님) — 구조 기반 feature modulation 방향 참고용

---

### Category C — 구조 및 혈관 특화

#### 6. GraphMorph (NeurIPS 2024, arXiv 2502.11731) ⭐
- **Accepted Conference Paper (NeurIPS 2024)** | Rel: Medium
- Zhao Zhang, Ziwei Zhao, Dong Wang, Liwei Wang
- Branch-level features for tubular structure extraction
- Graph Decoder (branch-level feature → predicted graph) + SkeletonDijkstra (centerline mask)
- Morph Module로 predicted graph와 segmentation mask를 정렬
- **topological accuracy** 향상 — 내 thin vessel connectivity 동기와 연결 가능

#### 7. TopoVST (arXiv 2603.14909, March 2026)
- **Preprint Only** | Rel: Medium
- Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu
- Multi-scale sphere graphs + GNN: **tracking direction + vessel radii 동시 추정**
- Geometry-aware weighting (class imbalance 해결) in directional loss
- Wave-propagation 기반 skeleton tracking (spurious segment 억제)
- **Vessel radii 추정이 primary task** → 내 local observability 계산의 관련 방법론

#### 8. DUALADAPTER_CURV (CVPR 2026 Oral)
- **Accepted Conference Paper (CVPR 2026 Oral)** | Rel: Medium
- Kai Zhu et al.
- Dual-level Adapter Boosting Prompt-free Curvilinear Structure Segmentation
- 혈관, 신경 등 curvilinear 구조를 prompt 없이 분할하는 dual-level adapter
- arXiv 미공개 (CVPR 2026 Oral ID 40317)
- Cat C에서 직접 경쟁 가능성 있음 — 전문 독해 필요

#### 9. BREAKDATABARRIER (arXiv 2602.23782, Feb 2026)
- **Preprint Only** | Rel: Low
- Kirato Yoshihara et al.
- DINOv3 기반 few-shot 3D vessel segmentation framework
- TopCoW (in-domain) + Lausanne (OOD) 실험: nnU-Net 대비 50% 상대 개선
- 내 방법과 paradigm 다름 (foundation few-shot vs. SSDG aug training)

---

### Category D — Top-tier Vision 아이디어 전이

#### 10. GENIE (ICML 2026, arXiv 2606.16301) ⭐
- **Accepted Conference Paper (ICML 2026)** | Rel: Medium
- Sumin Cho, Dongwon Kim, Kwangsu Kim
- One-Step Generalization Ratio Guided Optimization (OSGR)
- 각 파라미터의 generalization 기여도 정량화 + gradient alignment 평가 → preconditioning factor로 DG 최적화
- 소수 파라미터가 optimization을 dominate하는 문제(= spurious correlation shortcut) 방지
- General DG optimizer (의료영상 아님) — 내 augmentation과 직접 결합 여지 검토

#### 11. SDCL (arXiv 2503.16852, March 2025)
- **Preprint Only** | Rel: Low
- Jiaxi Li et al.
- Structural Causal Model (SCM) + backdoor adjustment for style deconfounding in DG
- Style-guided expert module (stratification) + back-door causal learning module
- 자연영상 DG, 내 방법과 direct overlap 없음 — 이론적 참고용

---

## Novelty Gap 종합 재확인 (Run #8 이후)

8번의 정기 탐색을 통해 확인된 결과:

| 검색 키워드 | 결과 | ONA와의 관계 |
|------------|------|-------------|
| "vessel observability conditioned augmentation" | **없음** | ONA 핵심 gap 유지 ✅ |
| "radius-conditioned augmentation budget" | **없음** | ONA 핵심 gap 유지 ✅ |
| "intra-class continuous augmentation strength" | **없음** | ONA 핵심 gap 유지 ✅ |
| "thin vessel protection augmentation" | **없음** | ONA 핵심 gap 유지 ✅ |
| "non-uniform augmentation across structures" | TSIAA (inter-class) | 다른 granularity — 차별점 명확 |
| "vessel radius aware training" | AG-TAL (loss), TopoVST (geometry) | augmentation에는 없음 |

**결론**: Continuous-ONA의 핵심 contribution — "단일 foreground class 내 local vessel radius/observability에 따른 continuous augmentation budget 조절" — 은 Run #8 이후에도 기존 발표 논문에 없음.

---

## 다음 Run 탐색 우선순위

1. **DUALADAPTER_CURV 전문 접근** — CVPR 2026 proceedings에서 전문 확인 (curvilinear DG와 내 방법 overlap 평가)
2. **TSIAA 전문 독해** — IIAG "instance" 정의 범위, thin vessel 관련 분석 있는지 확인
3. **IJCAI 2026** — August 15 이후 accepted list 공개 예정
4. **MICCAI 2026** — 2026년 8월 이후 proceedings 공개 예정
5. **arXiv 2606–2607** — July 2026 이후 추가 preprint 탐색
6. **TopoVST 인용 논문** — vessel radius estimation 방법론 계보

---

## 주목할 연구 동향 (Run #8 관찰)

1. **CVPR 2026은 foundation model 기반 일반화가 주류**: SegMoTE(SAM+MoE), SD-FSMIS(Stable Diffusion), MedCLIPSeg(CLIP), R²-Seg(BiomedParse) 등. 경량 SSDG augmentation 방법(내 방향)과 접근법 완전히 다름.

2. **Instance-level non-uniform augmentation의 등장** (TSIAA, IEEE TMI 2026): inter-class non-uniformity는 증명됨. intra-class (내 ONA)로 내려가는 다음 단계가 자연스럽게 열림.

3. **Vessel skeleton/topology 관련 활발한 연구** (TopoVST, GraphMorph): vessel radius 추정과 topological continuity가 독립적 연구 주제로 발전 중 — 내 observability 계산의 이론적 배경 강화 가능.

4. **ICML 2026에서 DG 최적화 이론 연구** (GENIE): gradient-level DG analysis가 심화되는 추세.
