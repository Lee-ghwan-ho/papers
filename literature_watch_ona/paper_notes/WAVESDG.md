# WAVESDG — Paper Note

**제목:** Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv:** 2603.28463  
**날짜:** March 2026  
**Status:** Preprint Only  
**Cat:** A (SSDG 직접 경쟁)  
**Rel:** High  

---

## 핵심 기여

### 문제 설정
- Single-Source Domain Generalization(SSDG) for fundus image segmentation (optic disc/cup)
- 단일 source domain + 5개 unseen target domain에서의 일반화

### 제안 방법: WISER Module
**Wavelet-based Invariant Structure Extraction and Refinement**

1. **저주파 sub-band 처리:**
   - global anatomy를 anchor로 활용
   - domain-invariant low-frequency structure를 보존/정규화

2. **고주파 sub-band 처리:**
   - directional edge(혈관 경계, 구조물 경계) 강화
   - noise (domain-specific artifact) 억제
   - "texture = domain, edge = content"라는 암묵적 분리

### 실험 결과
- 7개 SOTA 방법 대비 최고 balanced Dice score
- 최저 95th percentile Hausdorff distance
- 감소된 분산 → 개선된 cross-domain 안정성

---

## 내 방법(Continuous-ONA)과의 비교

### 공통점
- SSDG 설정 (단일 source, zero target during training)
- 추가 annotation 불필요 (source label만 사용)
- appearance augmentation 계열 (이미지 레벨 변형)

### 핵심 차이점

| 항목 | WAVESDG | Continuous-ONA |
|------|---------|----------------|
| 변형 축 | 주파수 도메인 (wavelet sub-band) | 공간 도메인 (nonlinear intensity mapping) |
| 적용 단위 | 전체 이미지 균일 | 혈관 local radius에 따라 연속적으로 차별 |
| 구조 이질성 고려 | 없음 (thin/thick 혈관 동일 취급) | 핵심 (thin vessel 보호, thick vessel 강화) |
| augmentation budget | 동일 (전체 이미지) | radius-conditioned (intra-class 연속 조절) |
| domain diversity | 주파수 특성 다양화 | nonlinear intensity curve 다양화 |
| 보호 대상 | edge 일반 (directional) | 관찰 가능성 낮은 fragile vessel |

### 충돌 가능성 평가
**낮음.** 두 방법은 orthogonal한 축에서 동작:
- WAVESDG: frequency domain separation (WHERE to perturb in frequency space)
- ONA: spatial structure-conditioned amplitude (HOW MUCH to perturb for each structure)

두 방법을 결합하면 상호 보완적일 수 있음.

---

## 인용 전략

### Related Work에서 위치
"Frequency-domain SSDG methods" 섹션:
> "Recent methods have explored wavelet-based [WAVESDG] or Fourier-based [RASS, MoreStyle, FreeSDG] decomposition to separate domain-specific appearance from anatomical content. However, these approaches apply uniform perturbation across all structures, treating thin and thick vessel segments equally..."

### Baseline 비교 필요성
- WAVESDG가 preprint이므로 공식 비교에서 제외 가능하지만, 동일 fundus 도메인에서의 비교 가치 있음
- 단, 내 방법이 TOF-MRA cerebrovascular에 집중하므로 직접 비교 데이터셋이 다를 수 있음

---

## TODO
- [ ] 전문 독해: WISER 모듈의 고주파 억제 메커니즘이 내 thin vessel 보호와 비교 가능한지
- [ ] Prostate/cardiac 등 다른 task에도 적용됐는지 확인 (fundus-specific인지)
- [ ] 학회 제출 여부 (MICCAI 2026? ICCV 2026?) 추적
