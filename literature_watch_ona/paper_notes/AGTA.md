# AGTA — Paper Note

**제목**: Improving Single-Source Domain Generalization via Anatomy-Guided Texture Augmentation for Cervical Tumor Segmentation  
**Venue**: MICCAI 2024 Workshop (CMMCA — Computational Mathematics Modeling in Cancer Analysis)  
**DOI**: 10.1007/978-3-031-73360-4_8  
**Status**: Workshop Paper  
**발견**: Run #3 (2026-05-29)  
**Novelty 충돌 가능성**: ⚠️ 중간 (개념 방향 유사, granularity 다름)

---

## 핵심 주장

기존 SSDG augmentation 방법들(SLAug 등)은 texture를 무차별적으로 파괴하여 domain robustness를 얻으려 하지만, **tumor boundary 같은 중요한 texture cue를 파괴하면 오히려 segmentation 성능이 저하**된다고 주장한다.

따라서, anatomy segmentation map을 이용하여 **anatomy-guided texture augmentation**을 설계한다:
- Anatomical region에 따라 texture augmentation 방식을 다르게 적용
- Tumor 관련 region은 texture를 보존하고, 배경/다른 조직은 더 강하게 변형

---

## 기술적 접근

1. **Anatomy segmentation 활용**: 사전 학습된 또는 함께 학습되는 anatomy segmentation으로 region mask 생성
2. **Region-specific texture augmentation**: region mask에 따라 augmentation 강도 또는 방식을 조절
3. 평가: cervical tumor segmentation에서 SSDG 성능 향상

---

## 내 방법과의 비교

| 차원 | AGTA | Continuous-ONA (제안) |
|------|------|----------------------|
| **Aug 단위** | Anatomical class (tumor vs. 주변 조직) — class-level | 단일 foreground class 내 local radius — **continuous, intra-class** |
| **Aug 조건** | Binary (어느 anatomical region인가) | Continuous (local vessel radius/observability) |
| **Structure signal** | Anatomy segmentation map | Local radius 또는 vesselness score from source annotation |
| **Label safety** | Tumor texture cue 보존 | Thin vessel label-image inconsistency 방지 |
| **적용 구조** | Tumor (2D, closed surface) | Tubular vessel (3D, connected, thin) |
| **논문 수준** | MICCAI Workshop | — |

---

## 차별화 포인트

AGTA와 내 방법은 "texture/appearance augmentation은 모든 구조에 동일하게 적용되어서는 안 된다"는 동일한 메시지를 가지지만, 다음 세 가지 점에서 명확히 다르다:

1. **Granularity**: AGTA는 class 간 binary 구분, 나는 single class 내 continuous spectrum
2. **조건 신호**: AGTA는 anatomy segmentation (external tool 필요 가능), 나는 source annotation 자체에서 직접 계산되는 local radius (추가 tool 불필요)
3. **문제 정의**: AGTA는 "texture cue가 important하므로 보존", 나는 "thin structure는 강한 aug 후 label이 있지만 이미지에서 안 보이는 inconsistency 발생"을 명시적으로 정의

---

## Related Work에서의 위치

내 논문 related work에서 AGTA를 다음 위치에 인용:
> "Anatomy-guided texture augmentation [AGTA] makes a similar observation that texture disruption should be anatomy-aware. However, their approach applies different augmentation policies at the *class level* (tumor vs. non-tumor), while our method operates *within a single class*, conditioning augmentation strength on continuous structural observability of individual vessels."

---

## 추가 확인 필요 사항

- [ ] Full text 접근 (Springer Link DOI 10.1007/978-3-031-73360-4_8)
- [ ] 구체적인 texture aug 방식 (gamma, histogram, Bezier 중 어느 것인지)
- [ ] Anatomy segmentation이 별도 모델인지, joint 학습인지
- [ ] Quantitative 결과: 어떤 baseline 대비 얼마나 향상?
