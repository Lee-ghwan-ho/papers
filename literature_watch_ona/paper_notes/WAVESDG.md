# WAVESDG — Paper Note

**제목**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463  
**날짜**: March 31, 2026 (Preprint Only)  
**저자**: Shramana Dey, Abhirup Banerjee, Varun Ajith, Sushmita Mitra (Indian Statistical Institute)  
**Cat**: A (SSDG 직접 경쟁)  
**Rel**: High  

---

## 핵심 방법

### WISER (Wavelet-based Invariant Structure Extraction and Refinement) 모듈

1. **입력**: Encoder feature map (intermediate layer)
2. **Wavelet 분해**: feature를 저주파 / 고주파 서브밴드로 분리
   - 저주파 서브밴드 (LL): 글로벌 해부학 구조 → refinement로 anatomy anchor 역할
   - 고주파 서브밴드 (LH, HL, HH): 방향성 엣지 정보 → 선택적 강화 + 도메인 노이즈 억제
3. **출력**: 도메인 불변 해부학 구조 강조 + 도메인 특화 외관 억제된 feature

### 실험 설정
- Task: Optic Disc (OD) / Optic Cup (OC) segmentation
- Source: 1개 fundus dataset
- Target: 5개 unseen fundus dataset (multi-vendor, multi-center)
- 비교: 7개 SOTA SSDG 방법
- Metric: balanced Dice score, 95th percentile HD

### 결과
- 7개 SOTA 대비 best balanced Dice + 최소 HD
- 저주파 anatomy 고정이 optic disc 전역 형태 보존에 효과적

---

## 내 방법(Continuous-ONA)과의 비교

| 항목 | WaveSDG | Continuous-ONA |
|------|---------|----------------|
| 분리 단위 | 글로벌 이미지 주파수 서브밴드 | 로컬 혈관 구조 (pixel-level radius) |
| anatomy 정의 | 저주파 = 전역 구조 | 혈관 반경/관찰가능성 (local scale) |
| 조절 대상 | feature map 서브밴드 | appearance augmentation strength |
| 조절 방식 | 고정된 wavelet 기저 + 학습된 refinement | 연속 함수 f(observability) |
| 적용 시점 | 학습 중 feature 수준 | 학습 전 data augmentation |
| 대상 해부 구조 | Optic disc/cup (단일, 크고 균일) | 뇌혈관 (다층 두께, 이질적) |
| Domain shift 원인 | Camera vendor, image quality | MRI scanner, protocol |

### 핵심 차이점 요약

WaveSDG가 주장하는 것:
> "저주파 서브밴드가 해부학 구조를 포착하므로, 이를 고정하고 고주파만 변환하면 도메인 불변 표현을 얻는다."

내가 주장하는 것:
> "같은 foreground class 내에서도 구조의 관찰 가능성이 다르므로, 관찰 가능성이 낮은 구조(thin vessel)에는 강한 appearance augmentation을 적용하면 안 된다. 관찰 가능성에 따라 augmentation budget을 연속적으로 조절해야 한다."

→ **접근의 level, 목적, 타겟 해부 구조가 모두 다름. Novelty 충돌 없음.**

---

## 논문에서 인용할 수 있는 포인트

1. "frequency domain approach: global anatomy ↔ appearance 분리 아이디어의 최신 사례"
2. "however, this approach treats all foreground structures uniformly at the frequency level, without accounting for intra-class structural observability variation — particularly critical for tubular structures with varying thickness"
3. WaveSDG와의 대조: global frequency split (whole image) vs. local scale-conditioned budget (per-structure)

---

## 추가 독해 포인트

- [ ] WISER 모듈의 정확한 wavelet 기저 선택 (Haar? Daubechies?)
- [ ] 고주파 서브밴드 "선택적 강화"의 학습 방식 (어텐션? 마스크?)
- [ ] ablation study: 저주파 고정만의 기여 vs. 고주파 처리의 기여
- [ ] 실험: 다른 SSDG 벤치마크(prostate, cardiac)에도 적용 가능한지 확인
