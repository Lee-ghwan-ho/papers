# SRCSM — Semantic-aware Random Convolution and Source Matching

**KEY**: SRCSM  
**Category**: A (직접 경쟁)  
**Status**: Preprint Only  
**arXiv**: 2512.01510 (2025년 12월)  
**Authors**: Franz Thaler, Martin Urschler, Mateusz Kozinski, Matthias AF Gsell, Gernot Plank, Darko Stern  

---

## 핵심 요약

DG 학습 시 이미지의 semantic label에 따라 영역별로 다른 Random Convolution(RC)을 적용.
테스트 시 target domain의 intensity를 source distribution에 가깝게 source matching.

- **학습**: Semantic-aware Random Convolution — foreground와 background에 독립적으로 random convolution 적용
- **테스트**: Source Matching — target image의 intensity를 source 통계에 맞게 변환

abdominal, whole-heart, prostate segmentation에서 기존 DG 방법들을 대부분 능가.
일부 설정에서 in-domain baseline과 동등한 성능.

---

## 내 방법과의 비교

| 구분 | SRCSM | Continuous-ONA |
|------|-------|----------------|
| Aug 대상 | FG class vs BG class (inter-class) | vessel class 내부 (intra-class) |
| Conditioning 단위 | semantic label mask | local radius / observability score |
| Conditioning 방식 | Binary (FG/BG) | Continuous |
| Thin structure 보호 | ❌ (foreground 전체 동일) | ✅ thin vessel은 약하게 |
| Label-image inconsistency 고려 | ❌ | ✅ |
| Augmentation type | Random Convolution (nonlinear) | Nonlinear intensity transform (Bézier/spline) |
| Test-time component | ✅ Source Matching | ❌ (training-time only) |

---

## 차별화 논리 (내 논문에서 쓸 문장)

> "SRCSM applies different augmentation to foreground and background regions,
> achieving inter-class differentiation. Our method addresses a complementary but
> distinct problem: within a single foreground class (vessel), structural observability
> varies continuously. Thin, fragile vessels that are already difficult to observe
> in source images require conservative perturbation, while well-resolved vessels
> can tolerate larger appearance variation. This intra-class continuous conditioning
> is not addressed by SRCSM or any prior work."

---

## 인용 계획

- Related Work: "Most prior SSDG methods apply augmentation uniformly across the foreground class or differentiate only at the class level [SRCSM, SLAug]."
- Baseline: SRCSM을 비교 baseline에 포함 고려 (구현 복잡도 확인 필요)
