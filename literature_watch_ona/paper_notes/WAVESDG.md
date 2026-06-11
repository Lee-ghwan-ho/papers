# WAVESDG — Paper Note

**제목:** Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv:** 2603.28463 (March 2026)  
**저자:** Shramana Dey, Abhirup Banerjee, Varun Ajith, Sushmita Mitra (Indian Statistical Institute)  
**Status:** Preprint Only  
**Category:** A (직접 경쟁 — SSDG)  
**Rel:** Medium

---

## 핵심 방법

**WaveSDG**: Wavelet-guided segmentation network

- **WISER 모듈** (Wavelet-based Invariant Structure Extraction and Refinement):
  - **저주파 sub-band**: global anatomy 정보를 anchor — 도메인 불변 구조 보존
  - **고주파 sub-band**: 방향성 edge 선택적 강화 + noise 억제
  - encoder feature를 wavelet sub-band로 처리
- 평가: optic cup/disc segmentation × 1 source + 5 unseen target datasets

---

## ONA 연구와의 관련성

### 공통점

- Single-source DG 설정 (source 1개 → unseen target 여러 개)
- "structure-relevant frequency를 augmentation/feature processing에서 별도 처리"라는 방향

### 차이점

| 구분 | WaveSDG | Continuous-ONA |
|------|---------|----------------|
| 접근 방식 | Feature-space wavelet sub-band separation | Pixel-space nonlinear appearance augmentation |
| 조절 granularity | 전체 이미지 단위 (whole-image wavelet) | 혈관별 intra-class (radius별 연속 조절) |
| 대상 modality | Fundus (2D RGB) | TOF-MRA (3D grayscale) |
| thin/thick 구분 | 없음 (wavelet은 spatial frequency, 혈관 두께 무관) | 있음 (observability별 budget 차별화) |
| 목적 | 도메인 불변 feature 학습 | label-image consistency 보호 + shortcut 억제 |

### Novelty 관계

WaveSDG는 주파수 domain에서 feature level 조절이고, 나는 pixel level에서 structure-specific appearance 조절. 직접 충돌 없음. 같은 SSDG fundus segmentation에서 wavelet 방법이 나왔으므로 관련 baseline으로 인지해야 함.

---

## 내 방법 포지셔닝에서의 시사점

- WaveSDG 등 "주파수 분리 기반 feature 접근"과 대비하여, ONA는 **구조 물리량(radius/observability)에 기반한 augmentation budget 연속 조절**이라는 다른 방향임을 명확히 할 필요 있음
- 혈관 두께 의존적 domain shift (얇은 혈관 = fragile = 다른 증강 필요) → WaveSDG가 전혀 다루지 않는 문제 → ONA의 gap 유지
