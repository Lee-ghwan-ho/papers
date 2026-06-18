# VASOMIM — Paper Note

**KEY**: VASOMIM  
**논문**: VasoMIM: Vascular Anatomy-Aware Masked Image Modeling for Vessel Segmentation  
**저자**: De-Xing Huang 외 (Institute of Automation, Chinese Academy of Sciences)  
**arXiv**: 2508.10794 (August 2025)  
**Status**: Preprint Only  
**Cat**: C (혈관·구조 특화)  
**Rel**: Medium

---

## 핵심 방법

VasoMIM은 X-ray angiogram vessel seg를 위한 anatomy-guided self-supervised pretraining framework이다.

### 두 가지 핵심 구성:

**1. Anatomy-guided masking strategy**:
```
Standard MIM: 무작위 patch 마스킹
VasoMIM: vessel-containing patch를 우선적으로 마스킹

이유: 
- Class imbalance (vessel vs. background)
- Uniform masking은 주로 background patch를 마스킹 → vessel 학습 signal 약화
- Vessel-containing patch 우선 마스킹 → 모델이 vessel reconstruction에 집중
```

**2. Anatomical consistency loss**:
```
Original image와 reconstructed image 간 vascular semantic 일치성 강화
→ vascular connectivity, branching pattern 보존
```

### 실험
- Dataset: XCAD 등 3개 X-ray angiogram dataset
- 결과: Standard MAE 대비 consistent improvement, 특히 limited-label setting

---

## 내 Continuous-ONA와의 관계

### 방향 인접성 (주목)

> **VasoMIM의 직관**: "vessel-rich region(=well-resolved, thick vessel)을 우선 마스킹 → 더 큰 reconstruction training signal"
> **내 ONA의 직관**: "resolved vessel(=thick, observable) → larger augmentation budget 허용"

두 방법 모두 **"vessel의 관찰 가능성/크기에 따라 training effort를 차별 배분"**한다는 공통 전제를 가진다.

### 핵심 차이

| 항목 | VasoMIM | Continuous-ONA |
|------|---------|----------------|
| **Training phase** | Self-supervised pretraining | Supervised training-time augmentation |
| **조절 대상** | 어떤 위치를 마스킹할지 (masking strategy) | 어떤 vessel에 얼마나 강한 변형을 허용할지 (aug strength) |
| **DG 다루는가** | 아니오 (pretraining only) | 예 (SSDG) |
| **Task** | X-ray angiogram vessel seg | TOF-MRA cerebrovascular SSDG |
| **Vessel 정의** | vessel-containing patch (coarse) | local vessel radius (continuous, fine-grained) |

---

## 내 연구에서의 활용 방향

### Related Work에서의 언급 (선택적)

> "Recent work on vessel-aware pretraining [VasoMIM] demonstrates that allocating greater training signal to well-resolved vessel regions improves segmentation of vascular structures. Our Continuous-ONA extends this intuition to the augmentation domain: instead of prioritizing well-resolved vessels for reconstruction, we allow them to undergo stronger appearance perturbations during augmentation, while protecting fragile thin vessels from destructive transformations."

이렇게 활용하면:
- VASOMIM을 내 동기를 지지하는 선행 근거로 사용할 수 있음
- "pretraining level" vs. "augmentation level"의 상보성을 주장할 수 있음

### 동기 강화 논거

1. VASOMIM이 "vessel의 크기/관찰가능성에 따른 차별 training"이 효과적임을 pretraining 측면에서 보여줌
2. 따라서 augmentation 측면에서도 동일한 차별화가 의미 있을 것으로 기대 가능
3. AG-TAL: loss weighting에서 radius-aware 차별화 효과 확인
4. VASOMIM: pretraining masking에서 vessel-aware 차별화 효과 확인
5. 내 ONA: augmentation budget에서 radius-conditioned 차별화 → 세 가지 방법이 동일 직관을 서로 다른 training component에서 독립적으로 검증

---

## 미확인 사항

- [ ] anatomy-guided masking에서 "vessel-containing"의 정확한 기준 (vesselness threshold? annotation-based?)
- [ ] Thin vessel과 thick vessel에 대한 masking 비율 차이가 있는지
- [ ] MIM pretraining 후 DG 실험 여부
- [ ] 제출 venue 확인 필요
