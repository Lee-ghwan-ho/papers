# Literature Watch Report — 2026-06-13 (Run #8)

> 작성일: 2026-06-13  
> 검색 윈도우: 2026-06-04 이후 신규 (arXiv 2606.XXXXX) + 2026-03~05 catch-up  
> 신규 논문: **5편** (Preprint Only 5편)  
> 누적 인덱스: **101편**

---

## 1. Executive Summary

Run #8은 2026년 6월 초 기준 신규 arXiv 2606 논문이 극히 적은 시기에 해당하며,
직접적인 SSDG 의료영상 신규 논문(2606.XXXXX)은 ASFOSDA(domain adaptation) 1편에 불과하다.
대신 3월~5월 arXiv에 등재된 3편의 혈관 특화 논문(WaveSDG, VesselTok, VesselPose)이
이번 catch-up 탐색에서 발견되었다.

**핵심 발견**: WaveSDG(arXiv 2603.28463)는 Continuous-ONA와 동일한 동기("구조와 외양을 분리해야 SSDG가 개선된다")를 명시하는 논문이지만, mechanism이 feature-space wavelet decomposition으로 완전히 다르다.

**Novelty gap 유지**: "vessel radius/observability conditioned augmentation budget" 개념을 직접 다루는 논문은 Run #8에서도 발견되지 않았다.

---

## 2. 신규 논문 상세

### 2.1 WAVESDG — ⚠️ Novelty 관련 주의 (P1)

**제목**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463 | **게재**: Preprint (March 31, 2026)  
**Category**: A (SSDG vessel) / B (structure-conditioned)  
**Relevance**: **High**

**내용 요약**

WISER(Wavelet-based Invariant Structure Extraction and Refinement) 모듈을 제안.
인코더 feature를 wavelet sub-band로 분해하고 각 sub-band에 서로 다른 역할을 부여:
- 저주파 sub-band → 해부학적 구조(anatomy) 앵커링
- 고주파 방향성 sub-band → 경계/엣지 디테일 강화
- 노이즈성 고주파 → 억제

Fundus 영상에서 optic disc / optic cup SSDG를 실험.
기존 SSDG 방법들이 "해부학적 위상을 포착하거나 외양과 구조를 분리하지 못한다"고 비판.

**Continuous-ONA와의 비교**

| 항목 | WaveSDG | Continuous-ONA |
|------|---------|----------------|
| 핵심 동기 | 구조-외양 분리가 SSDG에 필수 | 구조 관찰가능성이 augmentation budget을 결정해야 함 |
| 조작 공간 | Feature-space (wavelet sub-band) | Input-space (augmentation strength) |
| 구조 신호 | 주파수 대역 (저/고주파) | 국소 혈관 반경 r (연속값) |
| Intra-class 이질성 | 없음 (disc/cup 전체 균일 처리) | 있음 (thin vs. thick vessel 연속 구분) |
| 태스크/모달리티 | 2D fundus (disc/cup) | 3D TOF-MRA (cerebrovascular) |
| 특수 구조 보호 | 없음 | 얇은 혈관 과증강 보호 |

**결론**: WaveSDG는 개념적 parallel이지만 mechanism 차이가 명확하다.
"WaveSDG가 frequency 공간에서 구조-외양 분리를 하는 반면,
Continuous-ONA는 geometric space에서 vessel observability에 의해 조절된
augmentation budget을 설계한다"로 구분 가능.

---

### 2.2 VESSELTTOK — radius-invariance 지지 근거 (P2)

**제목**: VesselTok: Tokenizing Vessel-like 3D Biomedical Graph Representations for Reconstruction and Generation  
**arXiv**: 2603.18797 | **게재**: Preprint (March 2026)  
**Category**: C (vessel representation)  
**Relevance**: Medium

**내용 요약**

혈관 그래프를 centerline points + pseudo-radius로 인코딩하여 latent token으로 압축.
학습된 token이 lung airways, lung vessels, brain vessels 등 여러 해부학/모달리티에 걸쳐 전이됨.
생성 모델링과 link prediction을 지원.

**Continuous-ONA에 대한 함의**

VesselTok의 핵심 발견: **pseudo-radius 인코딩이 cross-anatomy, cross-modality 전이를 가능하게 한다**.
이는 Continuous-ONA의 핵심 가정인 "local vessel radius는 domain-invariant한 안정적 기하학적 속성이다"를 독립적으로 지지하는 empirical 증거다.
내 방법의 동기 섹션에서 "radius가 domain shift에 불변한 구조적 신호임은 VesselTok [xx]에서도 확인된다"로 인용 가능.

---

### 2.3 VESSELPOSE — 평가 지표 강화 가능성 (P2)

**제목**: VesselPose: Vessel Graph Reconstruction from Learned Voxel-wise Direction Vectors in 3D Vascular Images  
**arXiv**: 2605.00538 | **게재**: Preprint (May 1, 2026)  
**Category**: C (vessel topology)  
**Relevance**: Medium

**내용 요약**

각 voxel에서 혈관의 진행 방향 벡터를 예측, TEASAR 알고리즘의 direction-guided 확장으로 vascular graph를 추출.
**False-split/false-merge topology metric** 도입: Dice보다 tubular structure의 연결성 오류를 직접 측정.
Rat heart micro-CT vasculature 등 3개 benchmark에서 SOTA.

**Continuous-ONA에 대한 함의**

현재 내 실험은 Dice/clDice 중심. VesselPose의 false-split metric을 추가하면
"얇은 혈관의 연결성 보존" 효과를 더 직접적으로 측정 가능.
특히 thin vessel에 conservative augmentation을 적용한 경우의 topology 개선을 정량화하는 데 유용.

---

### 2.4 TUBEMLLLM — 참고용 (P2)

**제목**: TubeMLLM: A Foundation Model for Topology Knowledge Exploration in Vessel-like Anatomy  
**arXiv**: 2603.09217 | **게재**: Preprint (March 13, 2026)  
**Category**: C (tubular/vessel)  
**Relevance**: Medium

**내용 요약**

자연어 prompted topology priors를 통합한 MLLM 기반 vessel segmentation foundation model.
TubeMData 위상 중심 multimodal benchmark 제안.
Zero-shot cross-modality transfer: fundus → X-ray angiography (Dice 67.50%, β₀ error 1.21).

**Continuous-ONA에 대한 함의**

Foundation model 기반 DG 접근법으로, 내 방법(lightweight SSDG aug)과 paradigm이 완전히 다름.
Betti number β₀ error가 vessel topology 평가의 새로운 기준으로 부상 중 — 내 evaluation에 추가 검토 가능.

---

### 2.5 ASFOSDA — 낮은 관련성 (Preprint-Only 목록만)

**제목**: Active Source-free Domain Adaptation in Open-set Medical Image Segmentation via Decomposed Uncertainty and Prototype Discrepancy  
**arXiv**: 2606.08749 | **게재**: Preprint (June 7, 2026)  
**Category**: —  
**Relevance**: Low

Domain *adaptation* (test-time, 소수 target label 필요) 방법으로
Continuous-ONA의 domain *generalization* (target 불필요) 설정과 paradigm 자체가 다름.
Related work에서 "본 방법과 달리 적응 방법은 target data를 요구한다"의 예시로만 언급 가능.

---

## 3. Novelty Gap 현황 (Run #8 기준)

| 키워드 | 상태 |
|--------|------|
| "radius-conditioned augmentation budget" | ❌ 해당 논문 없음 |
| "observability-conditioned augmentation" | ❌ 해당 논문 없음 |
| "vessel thickness conditioned augmentation strength" | ❌ 해당 논문 없음 |
| "intra-class continuous augmentation scheduling" | ❌ 해당 논문 없음 |
| Nearest: WaveSDG (structure-appearance decoupling for SSDG) | ✅ mechanism 완전히 다름 |

**결론**: Continuous-ONA의 핵심 novelty — "단일 전경 클래스 내에서 국소 혈관 반경에 따라 augmentation 강도를 연속적으로 조절" — 은 Run #8에서도 선점된 논문 없음. Gap 유지.

---

## 4. 다음 탐색 우선순위

- [ ] WAVESDG 전문 독해 및 paper_notes 작성
- [ ] VESSELTTOK pseudo-radius 계산 방식 → 내 ONA radius 계산과 비교
- [ ] VESSELPOSE false-split/false-merge metric → 내 evaluation protocol 보완 검토
- [ ] MICCAI 2026 notification 예정일 확인 (보통 6월 말)
- [ ] DG-EBF @ CVPR 2026 proceedings 공개 시 탐색
- [ ] ICLR 2026 / ICML 2026 proceedings 공개 시 DG/augmentation 논문 탐색
