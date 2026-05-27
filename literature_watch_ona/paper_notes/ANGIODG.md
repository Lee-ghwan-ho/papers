# AngioDG — Interpretable Channel-informed Feature-modulated SSDG for Coronary Vessel Segmentation

**KEY**: ANGIODG  
**Category**: A (직접 경쟁) / C (혈관 특화)  
**Status**: Preprint Only  
**arXiv**: 2511.17724 (2025년 11월)  

---

## 핵심 요약

X-ray Coronary Angiography (XCA)에서의 coronary vessel segmentation을 위한 SSDG 방법.
6개 XCA dataset에서 평가.

### 핵심 아이디어: Channel Regularization
- Feature channel별 기여도를 task-specific metric으로 평가
- Domain-invariant 채널은 amplify, domain-specific 채널은 attenuate
- Feature channel reweighting → 해석 가능성(interpretable)

### 구체적 설정
- Single-source domain generalization (SDG)
- X-ray angiography modality 특화
- Channel-wise feature engineering (image-space aug 아님)

---

## 내 방법과의 비교

| 구분 | AngioDG | Continuous-ONA |
|------|---------|----------------|
| 적용 공간 | Feature space (channel) | Image space (intensity) |
| 조건 기반 | Channel 기여도 (task metric) | Local radius / observability (annotation geometry) |
| 혈관 modality | X-ray Angiography | TOF-MRA |
| 구조 인식 | ❌ (channel 단위) | ✅ (vessel thickness 단위) |
| Thin vessel 명시 보호 | ❌ | ✅ |

---

## 관계 및 인용 계획

- Related Work: "AngioDG employs channel-wise feature modulation to suppress domain-specific information in vessel segmentation, operating in feature space. Our method operates in image space, conditioning augmentation strength on structural observability."
- 직접 비교 baseline으로는 포함하지 않는 것이 적합 (modality가 다름).
- 참고: AngioDG의 coronary artery DG 실험 설계를 내 TOF-MRA 실험 설계 참조로 사용 가능.
