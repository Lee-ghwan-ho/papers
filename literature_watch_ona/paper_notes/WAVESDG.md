# WAVESDG — Paper Note

**KEY**: WAVESDG  
**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**저자**: Shramana Dey 외 (Indian Statistical Institute)  
**arXiv**: 2603.28463 (v2: April 27, 2026)  
**Status**: Preprint Only  
**Cat**: A (직접 경쟁 — SSDG)  
**Rel**: High

---

## 핵심 방법

**WaveSDG** + **WISER module** (Wavelet-based Invariant Structure Extraction and Refinement):

```
Input image
  → Wavelet decomposition
     ├── LL (Approximation sub-band): 전역 구조 맥락 (global structural context)
     │     → Anatomical layout anchor (보존 대상)
     ├── LH (Horizontal detail): 수평 방향 에지
     ├── HL (Vertical detail): 수직 방향 에지  
     │     → Directional edges (selective enhancement)
     └── HH (Diagonal detail): 대각 방향 / sensor noise + acquisition artifacts
           → Noise suppression (억제 대상)
  → WISER module: sub-band별 feature 정제
  → Segmentation decoder
```

핵심 아이디어:
- **LL sub-band** = 해부학적 형태 정보 (domain-invariant) → 보존
- **HH sub-band** = 기기/도메인 특이 잡음 → 억제
- LH/HL = 구조 경계 에지 → selective enhancement

---

## 실험 설정

- Task: Fundus image segmentation (optic disc + optic cup)
- Source: 1개 dataset
- Target: 5개 unseen fundus datasets
- Baseline 비교: SLAug, RandConv, IBN, MoreStyle, RASS, FreeSDG 등
- Result: 7개 SOTA 능가, best balanced Dice + cross-domain stability

---

## 내 Continuous-ONA와의 비교

| 항목 | WAVESDG | Continuous-ONA (내 방법) |
|------|---------|------------------------|
| **분석 단위** | Image-level frequency sub-band | Intra-image vessel-level (pixel) |
| **보호 대상** | LL 주파수 성분 (전역 구조) | Thin/fragile vessel (local structure) |
| **조절 방식** | Sub-band별 feature weighting (이진적) | Vessel radius → augmentation strength (연속) |
| **augmentation 적용** | Sub-band 분리 후 재합성 | Strength를 radius에 따라 연속 조절 |
| **intra-class heterogeneity** | 없음 (전체 이미지 uniform) | 명시적 모델링 (thin vs. thick vessel) |
| **Task** | Fundus (optic disc/cup, 2D) | Cerebrovascular TOF-MRA (vessel binary, 3D) |
| **Domain gap** | Fundus device/center | MRA scanner/protocol/center |

---

## 구분 논거

WAVESDG는 "어떤 주파수 성분을 보존/억제할지"를 전체 이미지 수준에서 결정한다.
내 방법은 "특정 vessel에 얼마나 강한 appearance change를 허용할지"를 vessel 단위로 연속적으로 결정한다.

핵심 차별점:
1. **Granularity**: global (image-level frequency) vs. local (vessel-level structure)
2. **Heterogeneity modeling**: WAVESDG는 foreground class 내 thin/thick vessel을 동일하게 처리함 → 내 방법의 핵심 gap을 여전히 다루지 않음
3. **Motivation**: WAVESDG = "low-freq=structure, high-freq=noise"라는 주파수 분리 가정. 나 = "얇은 혈관은 appearance 변화에 취약하다"는 structural observability 개념

---

## Related Work에서의 활용

내 논문의 Related Work에서:
> "Frequency-based methods such as WaveSDG [cite] separate anatomical structure from domain-specific appearance at the image level via wavelet sub-band decomposition. However, these methods apply identical augmentation strength to all vessel structures regardless of their observability. Our Continuous-ONA addresses the overlooked intra-class heterogeneity by assigning larger appearance perturbations to well-resolved vessels while conservatively augmenting fragile thin structures."

---

## 미확인 사항

- [ ] WISER module의 구체적 구현 상세 (channel attention? adaptive weighting?)
- [ ] SLAug 대비 정확한 수치 개선폭
- [ ] 3D 데이터(TOF-MRA) 적용 가능성 여부
- [ ] 제출 venue (MICCAI 2026? 또는 기타?)
