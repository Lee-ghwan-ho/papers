# MRFD_DAGBA — Paper Note

**제목**: Multi-Receptive Field Feature Disentanglement with Distance-Aware Gaussian Brightness Augmentation for Single-Source Domain Generalization in Medical Image Segmentation  
**Venue**: Neurocomputing, 2025  
**DOI**: 10.1016/j.neucom.2025.130120  
**Status**: Published Journal Article  
**Category**: B (방법론 유사)  
**Run**: #8 (2026-06-14)  
**Priority**: P0 — novelty 충돌 최우선 확인

---

## 왜 P0인가

"Distance-Aware Gaussian Brightness Augmentation"이라는 명칭이 내 Continuous-ONA의 핵심 개념인 "vessel radius/observability 기반 augmentation strength 조절"과 **개념적 근접성**을 가진다.

- 내 방법: vessel centerline distance (= local radius)를 observability proxy로 사용 → augmentation strength를 continuous하게 조절
- DAGBA: 어떤 "distance"를 기반으로 Gaussian brightness augmentation을 적용

"Distance"의 정의가 vessel centerline 기반인지 image-level spatial 기반인지에 따라 충돌 여부가 결정된다.

---

## 현재 파악된 내용 (full text 미확보)

### 방법 개요

**MRFFD (Multi-Receptive Field Feature Disentanglement):**
- 다양한 크기의 convolutional kernel로 fine-grained detail + global context 동시 캡처
- Channel-level에서 style feature와 structure feature를 분리
- domain-specific style variation에 대한 robustness 강화

**DAGBA (Distance-Aware Gaussian Brightness Augmentation):**
- 의료영상에서 흔히 발생하는 복잡한 brightness 불균일성을 시뮬레이션
- "Distance"를 활용해 Gaussian brightness augmentation의 공간적 분포를 결정
- 구체적인 "distance" 정의: **현재 미확인** (full text 필요)

### 실험 데이터셋
- 현재 미확인 (Neurocomputing paywall)
- 일반 의료영상 SSDG 논문이므로 prostate, cardiac, fundus 중 하나일 가능성 높음

---

## 예상 시나리오 (full text 확보 전)

### 시나리오 1: Image-level spatial distance (충돌 낮음)
- DAGBA = 이미지 내 pixel 좌표 기반 distance (e.g., distance from image center, or random Gaussian center)
- 이미지 전체에 공간적 brightness gradient를 생성해 illumination 불균일성 시뮬레이션
- 내 방법과 mechanism이 완전히 다름: 이미지 공간 vs. vessel radius 공간

### 시나리오 2: Tissue/structure boundary distance (충돌 중간)
- DAGBA = segmentation mask에서 각 pixel까지의 distance transform 기반
- Structure 내부와 외부의 brightness pattern을 다르게 적용
- 내 방법과 유사한 방향: annotation 기반 spatially-varying augmentation
- 그러나 DAGBA는 structure 내부를 uniform하게 처리할 가능성이 높음 (intra-class vessel size 차이 없음)

### 시나리오 3: Vessel centerline distance (충돌 높음)
- DAGBA = 혈관 centerline에서의 거리 (= local vessel radius)를 기반으로 brightness 적용 강도 조절
- 이 경우 내 방법과 동일한 principle
- 그러나 DAGBA는 brightness augmentation만, 나는 nonlinear appearance (full tone curve) 조절
- 또한 DAGBA가 SSDG 맥락에서 "thin vessel protection" 목적으로 설계되었는지 확인 필요

---

## 내 방법과의 차별점 (전제)

시나리오 2 또는 3에서도 다음 차별점을 주장할 수 있다:

1. **조절 대상**: DAGBA = brightness만, Continuous-ONA = monotonic nonlinear tone curve (더 포괄적인 appearance 변환)
2. **Conditioned 변수**: DAGBA = 공간적 거리(구체적 대상 불명), Continuous-ONA = vessel 관찰 가능성 (thin→약하게, thick→강하게)
3. **Motivation**: DAGBA = brightness 불균일성 시뮬레이션, Continuous-ONA = label-image consistency 보장 (얇은 혈관에서의 label inconsistency 방지)
4. **Target task**: 현재 DAGBA가 TOF-MRA cerebrovascular 실험을 포함하는지 불명확

---

## 즉시 수행 필요 액션

- [ ] Neurocomputing 논문 full text 확보 (기관 구독 또는 저자 이메일)
- [ ] DAGBA의 "distance" 정의 확인 (수식, pseudocode)
- [ ] 실험 데이터셋 확인 (혈관 포함 여부)
- [ ] 본 논문이 intra-class vessel thickness를 명시적으로 다루는지 확인

---

## Related Notes

- AG-TAL (MRFD_DAGBA.md, arXiv 2604.27357): 같은 "vessel radius" 개념을 loss weighting에 사용 → 다른 mechanism
- PCSDG: class-level structure-aware brightness aug → binary (disc/cup vs. background)
- ICRN (IEEE TMI 2024): foreground/background binary brightness aug (DAGBA보다 덜 세분화)
- AGTA (MICCAI 2024W): anatomy-guided texture aug → class-level protection

**결론**: MRFD-DAGBA의 full text 없이는 novelty 충돌 여부 결론 불가. 즉시 full text 확보 필요.
