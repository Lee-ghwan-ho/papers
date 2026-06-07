# UNIFREQ — Universal Frequency Domain Perturbation for SSDG

**Full Title**: Universal Frequency Domain Perturbation for Single-Source Domain Generalization  
**Venue**: ACM International Conference on Multimedia 2024 (ACM MM 2024)  
**DOI**: 10.1145/3664647.3681536  
**arXiv**: OpenReview only (no arXiv preprint found)  
**Status**: Accepted Conference Paper  
**Category**: A — 직접 경쟁  
**Relevance**: High

---

## 핵심 아이디어

기존 SSDG frequency augmentation (FreeSDG, RASS, MoreStyle 등)의 한계: **manually 설계된 frequency perturbation**은 sample-specific diversity를 포착하지 못함.

UniFreqSDG는 **학습 가능한** spectral perturbation module로 이를 해결:

1. **Learnable Spectral Perturbation Module**
   - 각 sample의 frequency distribution range를 adaptive하게 학습
   - Low-frequency를 정밀하게 perturb → stylistically diverse sample 생성
   - Anatomy (high-freq structure) 보존

2. **Content Preservation Reconstruction**
   - Frequency perturbation 전/후 feature를 결합해 discriminative content 손실 방지

3. **Active Domain-variance Inducement Loss (ADIL)**
   - Feature level에서 domain-invariant vs. domain-style 명시적 분리/억제
   - Hierarchical feature-level perturbation으로 다양한 OOD style 커버

---

## 실험 결과

- **Fundus 데이터셋**: Dice +7.47% (77.98% → 85.45%) vs. SOTA (SLAug, RASS, MoreStyle)
- **Prostate 데이터셋**: Dice +4.99% (71.42% → 76.73%) vs. SOTA

---

## 내 방법과의 비교

| 항목 | UniFreqSDG | Continuous-ONA |
|------|-----------|----------------|
| Perturbation 단위 | 전체 feature map (모든 픽셀 균일) | intra-image vessel 별 (radius 연속적 조절) |
| Perturbation 공간 | Frequency domain (feature level) | Spatial domain (image level, nonlinear intensity) |
| Thin vessel 처리 | 별도 처리 없음 | Conservative augmentation (낮은 budget) |
| Thick vessel 처리 | 별도 처리 없음 | Aggressive augmentation (높은 budget) |
| Adaptivity | Per-sample (전체 이미지 단위) | Per-pixel / per-structure (intra-image) |
| Label-image consistency | 고려 없음 | Thin vessel visibility 보호 목적 |

**핵심 구분 논거**:
> UniFreqSDG applies a uniform frequency perturbation to the entire feature map, treating all structures identically regardless of their observability or structural complexity. Continuous-ONA addresses the intra-image heterogeneity of tubular structures: within a single image, thin and fragile vessels require conservative appearance perturbations to prevent label-image inconsistency, while thicker resolved vessels can tolerate stronger perturbations to suppress domain-specific shortcuts.

---

## 관련 선행 연구와의 관계

- FreeSDG, RASS, MoreStyle을 직접 능가하는 2024 방법
- SLAug와의 비교에서도 개선 보고
- ACM MM 2024 (CORE A tier, MICCAI와 동급)

---

## 내 논문에서의 활용

1. **Related work**: SSDG에서 frequency-domain perturbation을 learnable하게 만든 최신 baseline.
2. **Baseline 비교**: UniFreqSDG를 baseline에 포함하되, "uniform frequency perturbation" 카테고리로 분류.
3. **Novelty 구분**: "our method conditions the perturbation strength on local vessel observability, whereas UniFreqSDG applies uniform frequency perturbation irrespective of intra-image structural heterogeneity."
