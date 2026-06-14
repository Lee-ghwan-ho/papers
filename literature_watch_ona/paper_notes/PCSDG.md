# PCSDG — Paper Note

**제목**: Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation  
**Venue**: Biomedical Signal Processing and Control, 2025  
**DOI**: 10.1016/j.bspc.2024.106801  
**GitHub**: https://github.com/HopkinsKwong/PCSDG  
**저자**: Jia-Xuan Jiang, Yuee Li, Zhong Wang  
**Status**: Published Journal Article  
**Category**: B (방법론 유사)  
**Run**: #8 (2026-06-14)  
**Priority**: P1

---

## 방법 개요

PCSDG는 안저 영상에서 optic disc (OD)와 optic cup (OC)를 joint segmentation하는 SSDG 방법이다.

### 핵심 모듈

**Pixel-Level Contrastive SDG:**
- annotation-derived saliency map으로 content와 style representation을 pixel-level에서 분리
- disentanglement module이 content map과 style map을 생성
- pixel-wise multiplication으로 original image에 적용해 structure representation과 style representation 분리
- contrastive learning으로 domain-invariant content feature 학습

**SABA (Structure-Aware Brightness Augmentation):**
- optic disc/cup annotation에서 saliency-based attention map 생성
- OD/OC 구조 영역과 background 영역에 **다른 brightness transformation** 적용
- 구조 영역 (disc/cup)의 brightness cue는 조심스럽게 변환 → 과도한 변환으로 구조 정보 소실 방지
- background 영역에는 더 강한 brightness 변환 허용

### 실험
- Dataset: RIGA+ (multi-center fundus images for OD/OC)
- SSDG 설정: 한 center → 나머지 centers 일반화

---

## 내 방법과의 관계

### 공통점
1. **annotation을 활용한 region-specific brightness augmentation**: SABA도, 내 Continuous-ONA도 segmentation annotation을 augmentation guidance로 활용한다
2. **"strong uniform augmentation is harmful to certain structures"**: PCSDG의 동기가 내 방법의 동기와 방향 유사 — "texture를 무조건 파괴하면 안된다"는 AGTA와 유사한 논리

### 핵심 차이점 (차별화 근거)

| 차원 | PCSDG (SABA) | Continuous-ONA |
|------|-------------|----------------|
| 보호 단위 | class-level binary (disc/cup vs. background) | intra-class continuous (vessel radius에 따른 연속 조절) |
| 조절 방식 | 두 영역 간 다른 transformation family 사용 | 같은 nonlinear transformation에서 strength를 연속적으로 조절 |
| 조건 변수 | 구조 존재 여부 (binary mask) | local vessel radius / observability (continuous scalar) |
| Task | Optic disc/cup in fundus (2D, 원형) | Brain vessel in TOF-MRA (3D, tubular) |
| Problem | disc vs. background 분리 | 얇은 혈관(fragile) vs. 굵은 혈관(resolved) 차별 |
| Motivation | 구조 texture 보호 | label-image inconsistency 방지 (얇은 혈관에서 강한 aug → label은 있지만 image에서 안 보임) |

### 내 논문에서의 활용

- Related work에서 "annotation-guided region-specific augmentation 선행 연구"로 언급 가능
- 차별점: "PCSDG는 class-level binary region을 구분하지만, 우리는 동일 foreground class 내에서 vessel radius에 따른 연속적(continuous) augmentation budget 할당을 제안한다. 이는 tubular 구조의 intra-class 이질성(thin/thick vessel의 관찰 가능성 차이)을 처음으로 SSDG 증강 설계에 반영한 것이다."

---

## Related Notes

- AGTA: anatomy-guided texture aug → class-level protection (tumor texture), 같은 방향
- ICRN: foreground/background 분리 LSA → binary class-level, SSDG 맥락
- MRFD_DAGBA: distance-aware aug → 더 세밀한 spatial conditioning 가능성
- Continuous-ONA: single foreground class 내 continuous radius → augmentation strength
