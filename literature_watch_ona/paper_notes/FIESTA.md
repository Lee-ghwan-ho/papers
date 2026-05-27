# FIESTA — Fourier-Based Semantic Augmentation with Uncertainty Guidance

**KEY**: FIESTA  
**Category**: A (직접 경쟁)  
**Status**: Preprint Only  
**arXiv**: 2406.14308 (2024년 6월)  
**Authors**: Kwanseok Oh, Eunjin Jeon, Da-Woon Heo, Yooseung Shin, Heung-Il Suk  

---

## 핵심 요약

Fourier 공간에서 semantic-aware 방식으로 amplitude와 phase를 조절하여 SSDG.
Epistemic uncertainty를 사용해 aug 강도를 fine-tune — 불확실한 영역에 더 집중.

### 구성 요소
1. **Fourier Augmentative Transformer (FAT)**: semantic 정보 기반으로 Fourier 주파수 amplitude/phase 조절. Angular points (meaningful frequency 위치)에 기반한 amplitude modulation.
2. **Uncertainty guidance**: Epistemic uncertainty (prediction entropy 기반)로 모델이 어려워하는 영역에 더 강한 aug 배분.

세 가지 cross-domain segmentation 시나리오에서 SOTA.

---

## 내 방법과의 비교

| 구분 | FIESTA | Continuous-ONA |
|------|--------|----------------|
| Aug 공간 | Fourier (frequency) | Image intensity (spatial) |
| Aug 강도 조절 기준 | Epistemic uncertainty (model 예측) | Local radius / observability (annotation geometry) |
| Conditioning 시점 | 학습 중 동적 (batch마다 변화) | 사전 계산 (annotation에서 고정) |
| 구조 인식 방식 | Frequency 공간에서 semantic-aware | Spatial 공간에서 structural property |
| Thin structure 명시 보호 | ❌ (예측 불확실성 기반이라 간접) | ✅ (observability 낮은 구조를 직접 보호) |
| Target domain 정보 | ❌ | ❌ |

---

## 핵심 차이 설명

**FIESTA의 uncertainty**: 모델이 어느 픽셀에서 예측을 못 하는지 → 더 강한 aug로 어려운 학습 유도.
→ 모델 관점(model-centric). 학습 중 변화하는 값.

**내 방법의 observability**: annotation에서 혈관이 얼마나 얇은지/희미한지 → 약한 구조에 보수적인 aug.
→ 데이터 관점(data-centric). 사전 계산된 고정값.

**방향이 반대다**: FIESTA는 불확실한 곳에 더 강한 aug. 내 방법은 구조적으로 fragile한 곳에 더 약한 aug.

---

## 인용 계획

- Related Work: "FIESTA uses model prediction uncertainty to guide augmentation strength in frequency domain, applying stronger perturbations where the model is uncertain. In contrast, our method uses annotation-derived structural observability — a data-centric property — to apply conservative perturbations on thin, fragile vessels where label-image consistency is at risk."
- 이 대비는 내 방법의 고유성을 더 명확히 드러낸다.
