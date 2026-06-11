# Literature Watch Report — Run #8 (2026-06-11)

> 연구 주제: Continuous-ONA — TOF-MRA SSDG with observability-conditioned nonlinear augmentation  
> 실행 일자: 2026-06-11  
> 신규 논문: **6편** (Accepted Conference 1편 + Published Journal 2편 + Preprint 3편)  
> 총 누적: **102편**

---

## 1. 이번 Run 핵심 요약

Run #8에서 가장 주목할 발견은 두 가지다.

첫째, **TopoVST** (arXiv 2603.14909)가 GNN으로 vessel radius를 명시적으로 추정한다. 내 Continuous-ONA가 사용하는 radius 개념의 "측정 타당성"을 별도 문헌에서 지지받을 수 있게 됐다.

둘째, **WaveSDG** (arXiv 2603.28463)가 SSDG 문제에서 wavelet sub-band 분리라는 새 방향을 제시했다. fundus 도메인이라 직접 경쟁은 아니지만, 같은 SSDG 공간에서 "frequency separation → feature decoupling"이라는 대안 접근이 나왔음을 인지해야 한다.

**ONA의 핵심 novelty gap은 Run #8 이후에도 유지된다**: "intra-class vessel radius → continuous augmentation budget mapping"을 직접 다룬 논문은 여전히 없다.

---

## 2. 신규 발견 논문 (6편)

### 2-1. Cat A — 직접 경쟁

#### WaveSDG ★★ — Run #8 신규 (P1)

| 항목 | 내용 |
|------|------|
| **제목** | Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation |
| **arXiv** | 2603.28463 |
| **날짜** | March 2026 |
| **Status** | Preprint Only |
| **Venue** | arXiv (conference 투고 미확인) |

**방법 요약**: WaveSDG는 encoder feature를 wavelet sub-band로 분해하는 WISER(Wavelet-based Invariant Structure Extraction and Refinement) 모듈을 제안한다. 저주파 성분은 global anatomy 정보를 anchor하고, 고주파 성분은 방향성 edge를 선택적으로 강화하며 noise를 억제한다. 1 source domain → 5 unseen target domains에서 optic cup/disc segmentation 평가.

**ONA와의 관계:**
- **공통**: SSDG 설정, 구조 정보를 주파수별로 분리해 domain-specific 정보로부터 보호
- **차이**: WaveSDG = feature-level whole-image frequency separation; ONA = pixel-level vessel radius별 augmentation budget 조절. WaveSDG에는 intra-class thin/thick vessel 구분 없음.
- **내 방법 포지셔닝**: ONA는 "frequency domain에서 feature를 분리하는 것"이 아니라 "image space에서 물리적 vessel 관찰가능성(radius)에 따라 augmentation 강도를 연속 조절"한다는 점에서 구별된다.

---

#### AD-DGCL ★★ — Run #8 신규 (P2)

| 항목 | 내용 |
|------|------|
| **제목** | Multi-organ Medical Image Segmentation via Adaptive Disentangled Domain Generalization Collaborative Learning |
| **Journal** | Neurocomputing (Elsevier) |
| **날짜** | October 2025 |
| **Status** | Published Journal Article |
| **DOI** | 10.1016/j.neucom.2025.xxxxx (ScienceDirect pii/S0925231225025184) |

**방법 요약**: AD-DGCL은 세 모듈을 조합한다: ① SSRD — dual encoder로 domain-specific style과 anatomical content를 분리하고 cross-domain contrastive learning으로 style-free representation 학습; ② SCT — synthetic style perturbation + consistency regularization; ③ Adaptive region-aware loss — pixel frequency에 따라 소기관 가중치를 동적 조절. MICCAI FLARE2024 Task 3 (반지도 multi-organ CT) 실험.

**ONA와의 관계:**
- "소기관을 adaptive loss weighting으로 우선 처리"라는 아이디어가 ONA의 "작은/얇은 구조 우선 보호"와 방향이 유사하다.
- 하지만 AD-DGCL = organ-level loss weighting (cross-organ), ONA = intra-vessel radius별 augmentation budget (within single class). 직접 충돌 없음.
- 내 related work에서 "adaptive region weighting 계열"로 언급 가능.

---

#### CQI — Run #8 신규 (P3)

| 항목 | 내용 |
|------|------|
| **제목** | Color-Quality Invariance for Robust Medical Image Segmentation |
| **arXiv** | 2502.07200 |
| **날짜** | February 2025 |
| **Status** | Preprint Only |

**방법 요약**: DCIN(Dynamic Color Image Normalization) — global/local reference 기반 색상 정규화. CQG loss — color+quality variation에 걸쳐 일관된 segmentation을 강제. Fundus SSDG (high-quality → low-quality).

**ONA 관련성**: color/quality shift에 특화된 방법으로, 내 intensity/contrast augmentation과 간접적으로 관련. 직접 경쟁 아님. 참고용.

---

#### DDFP — Run #8 신규 (P3)

| 항목 | 내용 |
|------|------|
| **제목** | Data-dependent Frequency Prompt for Source Free Domain Adaptation of Medical Image Segmentation |
| **Journal** | Knowledge-Based Systems (Elsevier) |
| **arXiv** | 2505.09927 |
| **날짜** | May 2025 |
| **Status** | Published Journal Article |

**주의**: SSDG가 아닌 **Source-Free DA** (target data 필요). 내 problem setting과 다름. BN 통계 재조정 + frequency prompt learning으로 target domain adaptation.

---

### 2-2. Cat C — 혈관·Tubular 특화

#### TopoVST ★★ — Run #8 신규 (P2) ⭐ ONA 연결

| 항목 | 내용 |
|------|------|
| **제목** | TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking |
| **arXiv** | 2603.14909 |
| **날짜** | March 2026 |
| **Status** | Preprint Only |
| **저자** | Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu |

**방법 요약**: Multi-scale sphere graph를 구성해 GNN으로 **tracking direction + vessel radius를 동시 추정**한다. Wave-propagation 기반 skeleton tracking이 space-occupancy filtering으로 spurious segment를 억제한다.

**ONA 연결 (중요):**

TopoVST는 vessel radius를 GNN으로 명시적으로 추정하며 이를 skeleton tracking의 핵심 출력으로 사용한다. 내 Continuous-ONA에서 "local vessel radius"를 augmentation budget의 입력으로 사용하는 것과 동일한 물리량에 기반한다.

**활용 방안**:
1. ONA 논문 introduction/motivation에서: "vessel radius는 skeleton tracking(TopoVST)에서도 핵심 물리량으로 취급됨이 최근 입증됨 → 이를 augmentation budget에 활용하는 ONA는 자연스러운 확장"
2. radius 추정 방법론 참고: annotation이 없을 때 GNN 기반 radius 추정을 ONA의 semi-supervised 또는 annotation-free 확장에 적용 가능

---

#### GrInAdapt ★★ — Run #8 신규 (P1)

| 항목 | 내용 |
|------|------|
| **제목** | GrInAdapt: Scaling Retinal Vessel Structural Map Segmentation Through Grounding, Integrating and Adapting Multi-device, Multi-site, and Multi-modal Fundus Domains |
| **Venue** | MICCAI 2025 (Paper 0903) |
| **arXiv** | 2503.05991 |
| **날짜** | 2025 |
| **Status** | Accepted Conference |

**방법 요약**: Source-free multi-target domain adaptation 3단계 프레임워크.
- Grounding: 다중 도메인 영상을 공통 anchor space에 등록
- Integrating: multi-view 예측을 통합해 label consensus 달성
- Adapting: source model을 target domain들에 적응

Multi-device + multi-site + multi-modal (fundus + OCTA) fundus 혈관 분할.

**ONA 관련성**: Source-free DA (target data 필요) vs. SSDG (target 불필요)로 problem setting이 다름. 혈관 DG 분야의 병렬 연구로 인지.

---

## 3. 현재 Novelty Gap 상태 (Run #8 기준)

| Gap 항목 | 상태 |
|---------|------|
| "vessel radius-conditioned augmentation" | **없음** ✅ ONA의 핵심 gap |
| "observability-conditioned augmentation budget" | **없음** ✅ |
| "intra-class continuous augmentation strength" for SSDG | **없음** ✅ |
| thin vessel 보호 + thick vessel 강한 aug 조합 | **없음** ✅ |
| TOF-MRA SSDG 직접 실험 + radius-aware | **없음** ✅ |

**결론**: Run #8 탐색에서도 Continuous-ONA의 핵심 novelty("단일 foreground class 내 vessel radius에 따른 continuous augmentation budget 조절")를 직접 다룬 논문은 발견되지 않았다.

---

## 4. 다음 Run을 위한 탐색 제안

- [ ] arXiv 2606 (June 2026) 신규 논문: 6월 하반기 업로드 논문 탐색
- [ ] CVPR 2026 proceedings 공개 확인 (6월 중 예상)
- [ ] MICCAI 2026 accepted list (7월 이후)
- [ ] ICLR 2026 proceedings 공식 공개 후 DG/aug 논문 탐색
- [ ] TopoVST 전문 독해: radius 추정 GNN 구조 + wave-propagation 상세
- [ ] WaveSDG 전문 독해: WISER 저주파/고주파 분리 방식 구체화
- [ ] AD-DGCL 전문 독해: adaptive region-aware loss 공식 + small organ weighting 세부

---

## 5. 업데이트된 파일 목록

| 파일 | 변경 내용 |
|------|----------|
| MASTER_PAPER_INDEX.md | 96편 → 102편, Cat A/C에 6편 추가, Preprint 목록 3편 추가 |
| SEARCH_LOG.md | Run #8 탐색 기록 추가 |
| READING_QUEUE.md | P1에 2편(WAVESDG, GRINADAPT), P2에 2편(AD_DGCL, TOPOVST), P3에 2편(CQI, DDFP) 추가 |
| paper_notes/TOPOVST.md | 신규 작성 (radius 추정 → ONA 연결 분석) |
| paper_notes/WAVESDG.md | 신규 작성 (SSDG 경쟁 논문 분석) |
| weekly_reports/2026-06-11_literature_report.md | 이 보고서 |
