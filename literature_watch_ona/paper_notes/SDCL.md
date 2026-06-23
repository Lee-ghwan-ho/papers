# SDCL — Paper Note

**제목:** Causal Inference via Style Bias Deconfounding for Domain Generalization  
**Venue:** IEEE Transactions on Pattern Analysis and Machine Intelligence (Early Access, 2025)  
**arXiv:** 2503.16852  
**IEEE Xplore:** https://ieeexplore.ieee.org/iel8/34/11474534/11344809.pdf  
**발견 Run:** #8 (2026-06-23)  
**Novelty 충돌 위험도:** ★★ (같은 SSDG 경쟁이지만 mechanism 완전히 다름)

---

## 핵심 아이디어

### 문제의식
기존 DG 방법이 training set 내 style frequency(빈도)의 편향(bias)을 무시함.
→ 모델이 spurious style-label correlation을 학습 → domain shift에 취약

### 제안 방법: SDCL
**Structural Causal Model (SCM) 구성:**
```
     Style
      ↓ ↓
Content ← Domain → Style
  ↓
Label
```
- Style이 Domain과 Content에 모두 영향 → confounding variable

**Backdoor Adjustment (do-calculus):**
- P(Label|do(Content)) = Σ_s P(Label|Content, Style=s) · P(Style=s)
- Style을 stratify하여 각 stratum에서 effect를 추정 후 average

**두 모듈:**
1. **SGEM (Style-Guided Expert Module)**: domain label 없이 style clustering → expert 할당
   - 각 이미지를 style vector로 표현 (instance normalization statistics)
   - K개의 expert: 각 style cluster 처리
2. **BDCL (Backdoor Causal Learning Module)**: style-balanced batch 구성
   - 다양한 style group에서 balanced sampling으로 backdoor adjustment 구현

---

## 실험 결과
- Medical image segmentation: SLAug (TPAMI 2023) 대비 성능 향상
- Semantic segmentation: TLDR baseline 대비 개선
- 정확한 데이터셋과 수치는 full text 확인 필요

---

## 내 Continuous-ONA와의 비교

| 항목 | SDCL | Continuous-ONA |
|------|------|----------------|
| 접근 방식 | Causal style de-confounding | Observability-conditioned aug strength |
| 작동 레벨 | Image-level style statistics | Pixel-level spatial augmentation |
| Thin vessel 보호 | 없음 | 핵심 기능 |
| 이론 기반 | SCM + do-calculus | Vessel morphology (radius/observability) |
| 적용 대상 | Style confounding 제거 | Augmentation budget spatial 조절 |
| 학습 방식 | Style-balanced training | Structure-conditioned aug |

### 핵심 구분점 (논문 작성 시 강조)
> SDCL addresses the style-content confounding at the image level through causal intervention, removing domain-spurious style effects from the learned representation.
> ONA instead directly controls where and how much appearance perturbation is applied within a single image, allocating augmentation budget as a function of local vessel observability to prevent label-image inconsistency in fragile thin structures.

---

## 논문 활용 방안

1. **Related Work에서 인용**: "Causal approaches (SDCL) remove style confounding at the image level. ONA is orthogonal: it controls the spatial distribution of augmentation intensity within a single image."
2. **동기 지지**: SDCL이 SLAug를 능가한다는 것은 단순 augmentation diversity 증가보다 더 정교한 DG 전략이 필요함을 보여줌 → 내 구조 기반 접근의 필요성 지지
3. **IEEE TPAMI 권위**: 내 논문에서 최신 SSDG baseline으로 인용 (ADA, TSIAA, SDCL의 3대 방향 대비 ONA)

---

## 미확인 사항
- [ ] SGEM expert 수 K 파라미터 및 ablation
- [ ] Medical segmentation 실험: 정확한 데이터셋 (prostate? fundus? cardiac?)
- [ ] SDCL과 SLAug의 정량적 비교 수치
- [ ] Code 공개 여부 (GitHub)
- [ ] SDCL이 vessel segmentation에 적용 가능한지 (TOF-MRA 도메인 적합성)
