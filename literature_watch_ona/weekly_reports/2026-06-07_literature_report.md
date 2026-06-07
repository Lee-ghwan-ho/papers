# Literature Watch Report — Run #8
**날짜**: 2026-06-07  
**모델**: claude-sonnet-4-6  
**신규 논문**: 12편 (Accepted Conference 3 + Published Journal 4 + Preprint Only 5)  
**누적 인덱스**: 108편

---

## 핵심 요약

이번 Run #8에서는 직접 경쟁 논문 **UniFreqSDG (ACM MM 2024)** 와 topology augmentation 논문 **CoLeTra (arXiv 2503.05541)** 가 가장 중요한 신규 발견이다. 전자는 SSDG에서 frequency perturbation을 학습적으로 수행하는 접근으로 직접 비교 대상이며, 후자는 내 thin vessel observability 보호 동기를 label-appearance inconsistency 관점에서 지지하는 방법론적 근거를 제공한다. Novelty gap ("radius/observability-conditioned augmentation budget")은 Run #8에서도 유지됨.

---

## 신규 논문 목록

### Category A — 직접 경쟁

| KEY | 제목 | Venue | Status | 관련성 |
|-----|------|-------|--------|--------|
| UNIFREQ | Universal Frequency Domain Perturbation for SSDG | ACM MM 2024 | Accepted Conference | **High** |
| WAVESDG | Decoupling Wavelet Sub-bands for SSDG in Fundus Image Segmentation | arXiv 2603.28463 | Preprint Only | Medium |
| CQI | Color-Quality Invariance for Robust Medical Image Segmentation | arXiv 2502.07200 | Preprint Only | Low |
| DGTTA | DG-TTA: Out-of-Domain Medical Seg via Augmentation + TTA | Sensors 2025 | Published Journal | Low |

### Category B — 방법론 유사

| KEY | 제목 | Venue | Status | 관련성 |
|-----|------|-------|--------|--------|
| COLORMAP | Colormap Augmentation for Cross-Modality DG | IJCARS Dec 2025 | Published Journal | Low |
| SCSD | Exploring Semantic Consistency and Style Diversity for DGSS | AAAI 2025 | Accepted Conference | Medium |

### Category C — 구조 및 혈관 특화

| KEY | 제목 | Venue | Status | 관련성 |
|-----|------|-------|--------|--------|
| VESSELSDF | VesselSDF: Distance Field Priors for Vascular Reconstruction | MICCAI 2025 | Accepted Conference | Medium |
| URVSM | Universal Vessel Segmentation for Multi-Modality Retinal Images | IEEE TIP 2025 | Published Journal | Medium |
| TOPOFAST | Topology Optimization in Medical Image Seg with Fast Euler Characteristic | IEEE TMI 2025 | Published Journal | Medium |
| DEFORMCL | Learning Deformable Centerline Representation for 3D Vessel | arXiv 2506.05820 | Preprint Only | Low |
| COLETRA | Disconnect to Connect: Topology Augmentation for Thin Structures | arXiv 2503.05541 | Preprint Only | **High** |
| TUBEMLLM | TubeMLLM: Foundation Model for Vessel Topology | arXiv 2603.09217 | Preprint Only | Low |

---

## 핵심 논문 상세 분석

### 1. UNIFREQ — ACM MM 2024 ⚠️ 직접 경쟁

**방법**: Learnable Spectral Perturbation Module + Active Domain-variance Inducement Loss.
- 학습 가능한 spectral perturbation module이 sample의 frequency distribution range를 adaptive하게 결정
- Low-frequency perturbation으로 stylistically diverse sample 생성 (anatomy 보존)
- Content Preservation Reconstruction: freq perturbation 전/후 feature 결합으로 discriminative content 손실 방지
- Active Domain-variance Inducement Loss: domain style feature를 명시적으로 분리/억제

**실험**: Fundus (+7.47% Dice), Prostate (+4.99% Dice) vs. SLAug, RASS 등 SOTA.

**내 방법과의 차이**:
- UniFreqSDG = **전체 feature map에 균일한 frequency perturbation** (모든 픽셀 동일).
- 나 = **intra-image vessel radius별 augmentation budget 연속 조절** (thin vessel은 conservative, thick는 aggressive).
- UniFreqSDG에는 "얇은 혈관을 강한 변형으로부터 보호"하는 개념 자체가 없음.
- 내 방법이 UniFreqSDG보다 더 세밀한 intra-image structure-specific 조절을 제공.

**논문 위치**: DOI 10.1145/3664647.3681536, ACM MM 2024 Melbourne.

---

### 2. COLETRA — arXiv 2503.05541 ⚠️ 내 동기와 직접 연결

**방법**: Disconnect-to-Connect (CoLeTra) augmentation.
- Image inpainting으로 tubular structure의 일부를 가려서 시각적으로 **disconnected**되게 보이게 만듦
- Ground truth label은 원본 (connected) 그대로 유지
- 모델이 "겉보기에 끊어진 구조도 사실은 연결됨"을 학습 → topology accuracy 향상
- Dice, clDice, Hausdorff distance 동시 개선

**내 동기와의 관계**:
| 항목 | CoLeTra | Continuous-ONA |
|------|---------|----------------|
| 핵심 문제 | Thin vessel이 appearance에서 끊어져 보이는 label-appearance discrepancy | Thin vessel에 강한 augmentation 적용 시 label-image inconsistency |
| 대응 전략 | Deliberately 끊어진 appearance를 만들어 robustness 학습 | 끊어질 수 있는 augmentation을 thin vessel에 미적용 |
| 방향 | Discrepancy를 학습에 활용 (resilience) | Discrepancy를 애초에 방지 (protection) |

두 방법은 opposite하지만 같은 계열의 문제 인식. CoLeTra의 존재 자체가 "thin vessel의 appearance fragility가 label-image inconsistency를 만든다"는 내 동기를 independent하게 지지하는 evidence가 됨.

**논문 상태**: OpenReview Dec 2025 → ICLR 2026 또는 CVPR 2026 심사 중 추정.

---

### 3. SCSD — AAAI 2025, arXiv 2412.12050

**방법**: Semantic Consistency + Style Diversity for Domain Generalized Semantic Segmentation (자연영상).
- **Semantic Query Booster**: semantic awareness와 discrimination capacity 향상
- **Text-Driven Style Transform**: domain-difference text embeddings로 style transformation guidance

**내 연구 관련성**:
- SCSD의 핵심 tension = "semantic content는 유지하되 style은 다양하게" = 내 Continuous-ONA의 핵심 trade-off와 동일
- 내 방법: thin vessel = "semantic content"가 fragile (label-image inconsistency 위험) → conservative augmentation budget
- thick vessel = "semantic content"가 robust → aggressive style diversity 허용
- SCSD는 natural image DG에서 이 tension을 해결하는 방법론을 제시.

---

### 4. TOPOFAST — IEEE TMI 2025, arXiv 2507.23763

**방법**: Fast χ (Euler Characteristic) 기반 topology optimization.
- 2D/3D 모두에서 fast χ computation 공식 유도
- χ error = topology evaluation metric (connected components, holes, voids)
- Topological violation map: χ error가 있는 공간 위치를 highlighting

**내 연구 관련성**:
- Thin vessel 분할의 topology correctness 평가에 χ metric 활용 가능
- Topological violation map이 내 observability map과 결합될 경우: "topology-violated + low-observability region" = 내 방법이 가장 조심해야 하는 영역

---

### 5. VESSELSDF — MICCAI 2025, arXiv 2506.16556

**방법**: Vessel segmentation을 binary voxel classification → continuous SDF regression으로 재정의.
- SDF loss 개선 + distance-weighted regularization으로 geometric prior (vessel continuity) 인코딩
- CT vessel reconstruction에서 generalization 향상

**내 연구 관련성**:
- VesselSDF의 distance transform이 내 observability score 계산(skeleton-based distance transform)과 동일한 geometric prior 활용
- "거리 변환 기반 geometry representation이 일반화에 도움"이 VesselSDF로 independently 증명됨 → 내 radius/observability 계산의 정당성 지지

---

## Novelty Gap 재확인

| 키워드 | Run #8 결과 |
|--------|------------|
| "vessel radius conditioned augmentation" | 명시 논문 없음 ✓ |
| "thickness-conditioned augmentation budget" | 명시 논문 없음 ✓ |
| "observability-conditioned augmentation" | 명시 논문 없음 ✓ |
| "intra-class structure-specific augmentation strength" | 명시 논문 없음 ✓ |
| "thin vessel protection in augmentation" | CoLeTra가 관련 방향 탐색하지만 opposite approach ✓ |

**결론**: Continuous-ONA의 핵심 novelty ("radius/observability에 따른 연속적 augmentation budget 조절, intra-vessel structure 단위")는 Run #8에서도 직접 competition 없음.

---

## 다음 실행 탐색 우선순위

1. UniFreqSDG full text 독해: SLAug/RASS와의 직접 비교 실험 결과 확인
2. CoLeTra full text 독해: inpainting 방식 상세 (morphological erosion vs. GAN-based vs. diffusion)
3. TOPOFAST full text: fast χ 공식 구현 → 내 thin vessel topology evaluation에 적용
4. ICLR 2026 accepted list 공개 후 DG/augmentation 재탐색
5. MICCAI 2026 accepted list (예상 2026-07 공개) 즉시 탐색
