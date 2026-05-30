# REAUG — Domain Generalization by Rejecting Extreme Augmentations

**Venue:** WACV 2024  
**arXiv:** 2310.06670  
**Authors:** Masih Aminbeidokhti, Fidel Alejandro Guerrero Peña, Heitor Medeiros, et al. (École de Technologie Supérieure)  
**DOI:** openaccess.thecvf.com/WACV2024  
**Status:** Accepted Conference  
**발견:** Run #4 (2026-05-30)  
**Category:** B  
**Rel:** High

---

## 핵심 방법

- augmentation transformation의 강도를 높이면 out-of-domain diversity가 증가하지만, 특정 threshold를 넘으면 오히려 학습에 해롭다.
- **Reward function:** augmented sample의 diversity(다른 sample과의 거리)와 consistency(예측 신뢰도)를 비교하여 reward를 계산.
- reward가 낮은(= 너무 극단적인) augmentation transformation은 자동으로 reject.
- 기본 aug policy: uniform sampling on standard transformations (color jitter, blur 등)
- 자연영상 DG benchmark (DomainNet, PACS 등)에서 평가.

---

## 내 Continuous-ONA와의 관계

### 공통점
- "강한 augmentation이 모든 경우에 좋지는 않다"는 핵심 인식 공유.
- augmentation에 어떤 형태의 budget 또는 rejection 메커니즘이 필요하다는 방향.

### 핵심 차이 (내 novelty 보호 논거)

| 항목 | REAUG | 내 Continuous-ONA |
|------|-------|-------------------|
| 처리 단위 | 이미지 전체 (image-level) | 이미지 내 각 혈관 구조 (intra-image, spatial) |
| 결정 방식 | Binary (apply / reject) | Continuous (반지름에 따라 연속적 강도) |
| 거부 기준 | diversity + consistency reward (학습 손실 기반) | vessel radius / observability (구조적 측정값 기반) |
| 구조 개념 | 없음 (모든 픽셀/구조에 동일 결정) | Thin vessel vs thick vessel 명시적 구분 |
| Medical 적용 | 자연영상 DG benchmark | TOF-MRA cerebrovascular segmentation |
| 이론적 근거 | 학습 안정성 (diversity-consistency trade-off) | 구조 관찰 가능성 (label-image consistency) |

### 내 주장에서 REAUG를 언급하는 방식 (권장 서술)

> "Recent work [REAUG] has recognized that extreme augmentation transformations can be harmful to training, proposing to reject augmentations exceeding a diversity-consistency threshold. However, this operates at the image level with a binary decision — a transformation either applies to the entire image or is rejected entirely. In contrast, our method provides **intra-image, continuous, structure-conditioned augmentation budgeting**: within the same image, thin vessels receive conservative perturbations while thick vessels tolerate stronger appearance variations. This spatial differentiation is motivated by the distinct observability properties of vascular structures at different diameters, a concern that image-level rejection schemes cannot address."

---

## 읽을 때 확인할 것

- [ ] reward function의 정확한 수식: diversity와 consistency를 어떻게 수치화하는가?
- [ ] medical image에 적용한 실험이 있는가?
- [ ] transformation family: color jitter, blur, gamma 외에 nonlinear intensity transformation이 있는가?
- [ ] rejection이 per-sample인가, per-batch인가?
