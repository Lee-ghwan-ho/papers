# HarmonySeg — Tubular Structure Segmentation with Deep-Shallow Feature Fusion and Growth-Suppression Balanced Loss

**KEY**: HARMONYSEG  
**Category**: C (구조·혈관 특화) / D (Top-tier Vision)  
**Status**: Accepted Conference Paper  
**Venue**: ICCV 2025  
**arXiv**: 2504.07827  
**Authors**: Yi Huang, Ke Zhang, Wei Liu, Yuanyuan Wang, Vishal M. Patel, Le Lu, Xu Han, Dakai Jin, Ke Yan  

---

## 핵심 요약

의료 영상에서 vessel/airway 등 tubular structure segmentation을 위한 프레임워크.
2D/3D 모두 지원.

### 세 가지 핵심 기술

1. **Deep-to-Shallow Decoder**: 다양한 크기의 receptive field를 가진 convolutional block으로 다양한 스케일의 tubular structure 처리

2. **Shallow-and-Deep Fusion Module**: Vesselness map을 보조 입력으로 사용. 해부학적 관심 영역 강조 + unreasonable candidate 제거 → small tubular structure recall 향상

3. **Growth-Suppression Balanced Loss**: Contextual/shape prior를 활용한 topology-preserving loss. Low-quality/incomplete annotation 처리 가능.

---

## 내 연구와의 관계

### Vesselness Map as Auxiliary Input (★★★)

HarmonySeg는 **vesselness map을 보조 입력**으로 사용하여 small vessel의 recall을 높인다.

이는 내 방법에서 **vesselness/local radius map을 사전 계산하여 augmentation conditioning에 사용**하는 것과 개념적으로 유사한 방향. 즉, vesselness/local radius라는 geometric signal을 학습에 통합하는 방식이 서로 다를 뿐.

### Growth-Suppression Loss와 내 방법의 관계

HarmonySeg의 loss는 tubular structure의 "growth (false positive)"와 "suppression (false negative)"을 균형 있게 다룸.

내 방법은 augmentation level에서 thin vessel의 false negative를 줄이는 방향으로 보수적인 aug를 적용. 이 두 관점은 **상보적**.

### 인용 계획

- "HarmonySeg demonstrates that vesselness maps, which encode structural observability, can directly improve small vessel segmentation when incorporated as auxiliary supervision [HarmonySeg]. We adopt a complementary approach, using similar geometric signals to modulate augmentation strength at training time."
