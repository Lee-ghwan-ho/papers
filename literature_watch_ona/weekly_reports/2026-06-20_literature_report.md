# Literature Watch Report — Run #8

**날짜**: 2026-06-20  
**이전 실행**: 2026-06-03 (Run #7)  
**신규 논문**: 5편 (Published Journal 1 + Accepted Conference 1 + Preprint 3)  
**누적 인덱스**: 101편 (Run #7 기준 96편)

---

## 이번 실행 핵심 요약

### 가장 중요한 발견

**TSIAA (IEEE TMI 2026)** — Bézier 계열 SSDG 두 번째 출판 논문 등장.  
ADA(MICCAI 2025)에 이어 learnable Bézier transformation 기반 SSDG가 IEEE TMI 2026에도 게재됨. 이 흐름은 내 nonlinear appearance augmentation 방향이 맞다는 것을 확인시켜 주지만, 동시에 Bézier 계열 within-image spatial conditioning의 미탐색 영역(내 ONA)을 더욱 명확하게 대비시켜 준다.

---

## 신규 논문 목록

### Category A — 직접 경쟁

#### ✅ TSIAA — Published Journal Article (최고 우선순위)

| 항목 | 내용 |
|------|------|
| 제목 | Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation |
| Venue | IEEE Transactions on Medical Imaging |
| Year | 2026, Vol. 45, pp. 764–776 |
| Status | **Published Journal Article** |
| Relevance | **High** |
| IEEE Xplore | doc. 11146907 |

**핵심 아이디어**:
- IIAG (Instance-level Image Augmenter): IAM 모듈들로 구성, 각 IAM은 learnable constrained Bézier transform
- Teacher-Student adversarial 구조: teacher가 원본 이미지로 stable prediction → student는 adversarially augmented 이미지로 teacher를 따라 학습
- "over-augmentation" 문제를 teacher supervision으로 억제
- per-sample (전체 이미지 단위) 적응적 Bézier 파라미터

**내 방법과의 관계**:
- **공통**: Bézier nonlinear aug, SSDG, over-augmentation 인식
- **차이**: TSIAA = sample-level adversarial diversity. 나 = intra-image vessel-radius-conditioned spatial budget. 동일한 이미지 내 thin/thick 혈관의 이질적 처리 개념이 TSIAA에는 없음.
- ADA + TSIAA = Bézier 계열 경쟁. 둘 다 paper에서 구분 필요.

---

### Category C — 구조/혈관 특화

#### ✅ GRAPHMORPH — Accepted Conference (NeurIPS 2024)

| 항목 | 내용 |
|------|------|
| 제목 | GraphMorph: Tubular Structure Extraction by Morphing Predicted Graphs |
| Venue | NeurIPS 2024 |
| arXiv | 2502.11731 |
| Status | **Accepted Conference Paper** |
| Relevance | Medium |
| NeurIPS | Poster 94063 |

**핵심 아이디어**:
- Pixel-level classification 대신 **branch-level feature learning**
- Graph Decoder: 멀티스케일 feature에서 graph 생성 (tubular structure의 branch endpoint 예측)
- Morph Module: SkeletonDijkstra 알고리즘으로 두 endpoint 간 최적 centerline 경로 탐색
- Post-processing: topologically accurate centerline → false positive 분할 결과 대폭 억제
- 혈관 + 도로 네트워크 양쪽 실험

**내 방법과의 관계**: 학습 시점 augmentation vs. 추론 시점 topology 보정으로 겹치지 않음. Branch connectivity 보존 참고.

---

#### ✅ TOPOVST — Preprint Only

| 항목 | 내용 |
|------|------|
| 제목 | TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking |
| arXiv | 2603.14909 |
| 제출일 | 2026-03-16 |
| Status | **Preprint Only** |
| Relevance | Medium |

**핵심 아이디어**:
- Multi-scale sphere graphs로 혈관 이미지를 샘플링 → GNN으로 tracking direction + vessel radius 동시 추정
- Geometry-aware weighting scheme: 방향 loss에 삽입 → class imbalance(thin vs. thick) 완화
- Wave-propagation skeleton tracking: space-occupancy filtering으로 spurious skeleton 제거
- Vessel radius를 학습 과정에서 명시적으로 추정 → thin vessel discontinuity 완화

**내 방법과의 관계**: TopoVST의 "geometry-aware weighting"이 vessel 두께 불균형을 명시적으로 고려한다는 점에서 내 관찰 가능성 조절 동기와 방향 일치. 단, DG 설정이 아님.

---

#### ✅ TUBEMLM — Preprint Only (낮은 우선순위)

| 항목 | 내용 |
|------|------|
| 제목 | TubeMLLM: A Foundation Model for Topology Knowledge Exploration in Vessel-like Anatomy |
| arXiv | 2603.09217 |
| 제출일 | 2026-03-10 |
| Status | **Preprint Only** |
| Relevance | Low |

**핵심 아이디어**:
- MLLM(Multimodal LLM)으로 vessel-like anatomy의 topology 이해 + 생성 통합
- Natural language prompting으로 topological prior 주입
- TubeMData: topology-centric task 멀티모달 벤치마크
- Zero-shot X-ray angiography: Dice 67.50%, β₀ error 1.21 달성

**내 방법과의 관계**: Foundation model 접근 (내 lightweight SSDG와 paradigm 다름). 배경 섹션 vessel topology 이해의 최신 동향 참고.

---

## Preprint-Only 별도 후보 논문

#### WaveSDG (arXiv 2603.28463) — April 2026 preprint

- 제목: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation
- WISER 모듈: Low-freq = global anatomy anchor, High-freq = 방향 edge 선택 강화 + noise 억제
- 1 source → 5 unseen target domain, optic disc/cup 분할, 7개 SOTA 능가
- **내 방법과의 차이**: 전체 이미지 단위 frequency 분리 vs. 내 intra-image vessel-specific budget. 직접 경쟁은 아님.

---

## Novelty Gap 현황 (Run #8 기준)

| 키워드 | 발견 여부 | 비고 |
|--------|-----------|------|
| "observability-conditioned augmentation" | ❌ 없음 | Run #1~#8 통틀어 없음 |
| "radius-conditioned augmentation budget" | ❌ 없음 | AG-TAL은 loss에만 사용 |
| "thickness-conditioned appearance transform" | ❌ 없음 | 여전히 없음 |
| "intra-class vessel-specific augmentation" | ❌ 없음 | 없음 |

> **결론**: Continuous-ONA의 핵심 novelty ("intra-image vessel observability → augmentation budget") 는 Run #8 이후에도 선행 논문 없음 확인.

---

## 경쟁 논문 지형도 업데이트 (2026-06-20 기준)

### Bézier 계열 SSDG (가장 주의해야 할 경쟁군)
- CAUSALITY_SDG (IEEE TMI 2022) — Bézier intensity transform 최초 도입
- ADA (MICCAI 2025) — Learnable Bézier Remap, per-sample adaptive
- TSIAA (IEEE TMI 2026) — Bézier + adversarial teacher-student, instance-level

→ 이 세 논문과의 차이 논거: "All three methods apply Bézier transformations at the sample level. ONA is the first to decompose the augmentation budget within a single image according to vessel-specific observability."

### Thin-vessel 보호 맥락 논문군
- AGTA (MICCAI 2024W) — class-level binary texture protection
- L2CP (MICCAI 2025) — morphological closing으로 thin vessel 제거 후 test-time copy-paste
- OVS_NET (IEEE TIP 2025) — small vessel enhancement separate branch
- TopoVST (arXiv 2603) — geometry-aware weighting for class imbalance
- 나의 ONA — training-time, intra-class continuous radius conditioning

→ ONA는 이 중 유일하게 "training-time SSDG + continuous intra-class augmentation budget" 조합.

---

## 다음 실행 탐색 우선순위

1. MICCAI 2026 논문 발표 후 vessel/DG 관련 즉시 탐색
2. ICLR 2026 openreview에서 DG/augmentation 논문 직접 필터링
3. TSIAA 전문 독해: ADA와의 차이 및 실험 구성 상세 확인
4. WaveSDG venue 확인 (MICCAI 2026 제출 여부)
5. "partial volume effect" 관련 논문 재탐색
