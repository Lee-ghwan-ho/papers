# PCSDG — Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement

> **KEY**: PCSDG  
> **Category**: B (방법론 유사 — Structure-conditioned Augmentation)  
> **Venue**: Biomedical Signal Processing and Control, Vol 99, 2025  
> **Status**: Published Journal Article  
> **DOI**: 10.1016/j.bspc.2024.106801  
> **Code**: https://github.com/HopkinsKwong/PCSDG  
> **Novelty 위협도**: ⚠️ Medium

---

## 논문 기본 정보

**제목**: Structure-Aware Single-Source Generalization with Pixel-Level Disentanglement for Joint Optic Disc and Cup Segmentation  
**저자**: Jia-Xuan Jiang, Yuee Li, Zhong Wang  
**출판**: Biomedical Signal Processing and Control, Vol 99, 2025 (DOI 10.1016/j.bspc.2024.106801)

---

## 핵심 기여

### 문제의식

- 기존 SSDG 방법들이 "단순 스타일 변환" 수준 — 구조 정보와 무관하게 일괄 적용
- Optic disc/cup segmentation에서 structure-unaware augmentation은 anatomy cue를 파괴
- "첫 번째 픽셀 수준 대조 분리 + attention mechanism을 SSDG에 적용"이라고 주장

### 제안 방법

**PCSDG 프레임워크** = 두 가지 주요 구성요소:

#### 1. Pixel-level Contrastive SSDG (PCSDG)
- Shallow feature extraction → style/structure contrastive disentanglement
- Pixel-level attention map으로 saliency 기반 content vs. style 분리
- Segmentation은 structure representation만 사용 (style 무관)

#### 2. SABA (Structure-Aware Brightness Augmentation) — 핵심
- **Curve initialization**: pixel grayscale 값 기반 brightness curve 생성
- **Randomization**: brightness factor를 truncated Gaussian distribution으로 랜덤화
- → 밝은/어두운 픽셀 영역에 서로 다른 brightness perturbation 적용
- → "구조 정보에 따른 brightness augmentation 차별화" (structure-aware)
- Domain shift의 brightness variation을 시뮬레이션

---

## 실험 결과

- **데이터셋**: Optic disc/cup segmentation — fundus images, SSDG 설정
- **도메인**: 단일 source → 복수 unseen target fundus domains
- PCSDG + SABA 조합으로 baseline 대비 개선

---

## 내 Continuous-ONA와의 비교

### 공통점

| 공통 요소 | PCSDG/SABA | ONA |
|-----------|-----------|-----|
| "Structure-aware augmentation" 개념 | ✓ | ✓ |
| SSDG 설정 | ✓ | ✓ |
| Annotation 기반 구조 정보 활용 | ✓ (indirect) | ✓ (direct — vessel mask) |

### 핵심 차이

| 차이 차원 | PCSDG/SABA | Continuous-ONA |
|-----------|-----------|----------------|
| **구조 신호** | pixel grayscale intensity (명도값) | vessel radius / observability score (geometry) |
| **대상 구조** | optic disc/cup (원형 organ) | cerebrovascular network (tubular, 연속) |
| **Aug 종류** | brightness perturbation only | nonlinear appearance (intensity + contrast mapping) |
| **Conditioning** | pixel intensity value (밝기) | local vessel radius (두께 기하학) |
| **연속성** | 연속적이나 geometry 무관 | vessel radius에 직접 연동 |
| **보호 개념** | 없음 (brightness만 조절) | thin vessel label-image inconsistency 방지 |
| **Task** | optic disc/cup (2D fundus) | TOF-MRA cerebrovascular (3D) |

### 결정적 구분

**SABA의 conditioning**: pixel grayscale value (intensity)  
→ 밝은 픽셀 vs. 어두운 픽셀 구분 (intensity-based)  
→ vessel thickness와 무관: 두꺼운 혈관과 얇은 혈관이 같은 intensity를 가지면 같은 aug 적용  
→ TOF-MRA에서 vessel intensity는 vessel radius와 직접 비례하지 않음 (partial volume, SNR 차이 등)

**내 ONA의 conditioning**: local vessel radius (geometry 기반)  
→ vessel mask에서 distance transform으로 계산된 반지름 — intensity와 독립  
→ 얇은 혈관 = 낮은 반지름 = 낮은 observability = 작은 aug strength  
→ intensity가 동일해도 radius가 다르면 다른 aug 적용 가능

> SABA conditions brightness augmentation on pixel intensity, not on structural geometry. Our method conditions on locally-estimated vessel radius derived from the ground-truth annotation — a signal that reflects the geometric observability of tubular structures independently of their intensity values, which SABA's grayscale-based conditioning cannot distinguish.

---

## Related Work에서 활용 방안

1. **"structure-aware augmentation" 계보**: AGTA (MICCAI 2024W) → PCSDG (BSPC 2025) → ONA. 세 방법 모두 "구조 정보에 따른 aug 차별화"를 목표로 하나 conditioning signal이 점점 정교해짐.
   - AGTA: class-level binary (tumor vs. background)
   - PCSDG: pixel intensity-based (brightness)
   - ONA: vessel radius-based (geometric observability, continuous)
   
2. **Venue 고려**: PCSDG는 Biomedical Signal Processing and Control (하위 tier). 내 논문에서 직접 비교 대상이 아닐 수 있으나 Related Work에서 언급 가치 있음.

---

## TODO

- [ ] PCSDG full text 독해: SABA brightness curve 수식 정확히 확인
- [ ] "structure-aware" 용어 사용 문맥 정확히 파악 → 내 논문에서 same term 사용 시 구분 필요
- [ ] 실험 결과 상세 확인: 어느 baseline 대비 얼마나 개선?
- [ ] 혈관 intensity와 radius의 상관관계 분석: TOF-MRA에서 intensity-based와 radius-based conditioning의 실질적 차이 정량화 가능성
