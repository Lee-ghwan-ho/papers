# COLETRA — Disconnect to Connect: Topology Augmentation for Thin Structures

**Full Title**: Disconnect to Connect: A Data Augmentation Method for Improving Topology Accuracy in Image Segmentation  
**arXiv**: 2503.05541  
**OpenReview**: openreview.net/forum?id=EZlzkRjnmK (December 2025)  
**Status**: Preprint Only (likely under review: ICLR 2026 or CVPR 2026 추정)  
**Category**: C — 구조 및 혈관 특화  
**Relevance**: High (동기 지지 / 대비 관계)  
**Authors**: Juan Miguel Valverde et al. (DTU Compute, Aarhus University, Virtanen Institute)

---

## 핵심 아이디어

**문제**: Deep learning models for thin tubular structures (vessels, neurites, roads) often produce segmentations with **topological errors** — small misclassifications that break thin connections.

**해결**: CoLeTra augmentation.
1. 원본 이미지에서 thin structure 영역을 **image inpainting**으로 가려 시각적으로 끊어지게 만듦
2. Ground truth label은 **원본 (connected) 그대로 유지**
3. Model은 "appearance가 끊어져 보여도 연결된 label을 예측"하도록 학습
4. → topology 오류에 robustness 획득

**구현**:
- Morphological dilation으로 vessel centerline 근방 inpainting mask 생성
- Inpainting: simple texture copying 또는 GAN-based 방법
- 코드: github.com/jmlipman/CoLeTra

**결과**:
- Dice + Hausdorff distance + clDice 동시 개선
- Retinal vessel, neurite, road crack 등 다양한 thin structure에서 효과

---

## 내 연구와의 관계

### 공통 문제 인식
둘 다 "thin tubular structure의 appearance 특성이 label과 misalign될 수 있다"는 문제를 출발점으로 삼음.

| 항목 | CoLeTra | Continuous-ONA |
|------|---------|----------------|
| 핵심 관찰 | Thin vessel은 appearance에서 끊어져 보일 수 있다 | Thin vessel은 appearance aug 후 invisible할 수 있다 |
| 문제 유형 | Label-appearance discrepancy가 이미 발생한 상황 | Label-image inconsistency를 augmentation이 유발하는 상황 |
| 대응 전략 | Discrepancy를 deliberately 강화 → resilience 학습 | Discrepancy를 유발하는 strong aug를 thin vessel에서 방지 |
| 철학 | "끊어져 보여도 연결됨을 학습" | "끊어질 수 있는 aug를 적용하지 않음" |

### 역방향 전략 (Complementary Evidence)
- CoLeTra: disconnected appearance → connected label 학습 (discrepancy 허용 → robustness)
- Continuous-ONA: thin vessel에 strong aug 금지 (discrepancy 자체를 방지)

두 방법이 opposite direction이라는 사실 자체가 "label-appearance consistency가 thin vessel에서 중요하다"는 공통 가정을 independently 지지함.

### 논문에서의 활용
1. **동기 지지**: "CoLeTra (arXiv 2503.05541)는 thin tubular structure에서 label-appearance discrepancy가 segmentation quality에 미치는 영향을 augmentation 관점에서 독립적으로 다루었다. 우리의 Continuous-ONA는 이 문제를 예방적 관점에서 접근한다."
2. **차이 명확화**: CoLeTra는 test-domain generalization이 아닌 topology accuracy improvement가 목적. 내 방법은 SSDG에서 domain shift robustness + thin vessel label-image consistency 보호.

---

## 추가 독해 포인트

- [ ] Inpainting 방식 상세: morphological erosion 규모 vs. 혈관 두께
- [ ] Thin structure 정의 기준: centerline 근방 N pixels? 반경 기준?
- [ ] OpenReview 제출 conference 확인: ICLR 2026 vs. CVPR 2026?
- [ ] Thin/thick vessel 구분 없이 uniform하게 적용하는지 확인 (내 방법과의 차별점)
