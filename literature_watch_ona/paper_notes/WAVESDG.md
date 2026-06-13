# WAVESDG — Paper Note

**제목**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**arXiv**: 2603.28463  
**제출일**: March 31, 2026  
**상태**: Preprint Only  
**Category**: A (SSDG) / B (structure-conditioned)  
**Relevance**: High  
**발견**: Run #8 (2026-06-13)

---

## 핵심 내용

### 제안 방법: WISER (Wavelet-based Invariant Structure Extraction and Refinement)

SSDG를 위해 인코더 feature를 wavelet sub-band로 분해하고,
각 sub-band에 명시적으로 다른 semantic 역할을 할당:

| Sub-band | 역할 | 처리 방식 |
|----------|------|-----------|
| 저주파 (LL) | 해부학 구조 앵커 | 보존 (domain-invariant signal) |
| 고주파 방향성 (LH, HL, HH) | 엣지/경계 디테일 | 선택적 강화 |
| 노이즈성 고주파 | 도메인 노이즈 | 억제 |

### 핵심 주장
"기존 SSDG 방법들은 해부학적 위상을 포착하거나 appearance와 structure를 분리하지 못한다."

### 실험
- Task: Fundus optic disc / optic cup segmentation
- Source 1 → Target 5 (unseen fundus datasets)
- 기존 SSDG 방법(SLAug, RASS 등) 대비 성능 향상 보고

---

## Continuous-ONA와의 비교 분석

### 공통점 (충돌 위험 요소)
1. **동일한 핵심 동기**: "구조와 외양을 분리해야 SSDG가 개선된다"
2. **SSDG 설정**: 동일 (source only, no target during training)
3. **의료영상 분할**: 동일 카테고리

### 결정적 차이 (novelty 구분 논거)

| 항목 | WaveSDG | Continuous-ONA |
|------|---------|----------------|
| 조작 공간 | Feature-space (wavelet) | Input-space (pixel-level augmentation) |
| 구조 신호 | 주파수 대역 (이산적, 2-3 레벨) | 국소 혈관 반경 r (연속값) |
| Intra-class 이질성 처리 | **없음** — disc/cup 전체 동일 처리 | **있음** — thin/thick vessel을 연속적으로 구분 |
| 보호 대상 | 없음 (증강 강도 조절 없음) | 얇은 혈관(fragile structure)을 과증강으로부터 보호 |
| 태스크 | 2D fundus disc/cup | 3D TOF-MRA 전뇌 혈관망 |
| 모달리티 | 자연광 카메라 + 형광 | MR angiography |
| Augmentation 메커니즘 | 없음 (feature 재구성만) | Nonlinear appearance augmentation strength 조절 |

### 핵심 구분 문장 (논문에 사용 가능)

> WaveSDG [xx] decouples structure from appearance in feature space via wavelet sub-band decomposition.
> In contrast, Continuous-ONA operates in input space and modulates the *strength* of nonlinear appearance
> augmentation according to the continuous local vessel radius, explicitly protecting fragile thin vessels
> from label-image inconsistency caused by over-augmentation.

---

## Related Work 활용 방안

### Continuous-ONA 논문에서의 포지셔닝

WaveSDG를 "구조-외양 분리 접근의 대표 사례"로 인용하되,
다음 세 가지 차이를 명확히 강조:

1. **Intra-class heterogeneity**: WaveSDG는 같은 클래스 내 구조 이질성을 다루지 않음
2. **Augmentation budget**: WaveSDG는 augmentation strength를 조절하지 않음
3. **3D tubular structure**: WaveSDG는 2D disc/cup, 내 방법은 3D 혈관망

### 관련 Related Work 그룹

```
"Structure-aware SSDG" 그룹:
- ICRN (foreground/background binary aug)
- AGTA (anatomy-guided texture aug for tumor)
- WaveSDG (wavelet frequency-space structure-appearance decoupling) ← 신규
- Continuous-ONA (radius-conditioned continuous augmentation budget) ← 본 논문
```

---

## Action Items

- [ ] WaveSDG 전문 다운로드 및 실험 결과 상세 확인
- [ ] WISER 구현 상세: wavelet 계층 수, sub-band 처리 방식 (learnable vs. fixed)
- [ ] WaveSDG 코드 공개 여부 확인
- [ ] Fundus 실험 baseline 구성 (SLAug, RASS 등) 확인 → 내 baseline 구성과 비교
