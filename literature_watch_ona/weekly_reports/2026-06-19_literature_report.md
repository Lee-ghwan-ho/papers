# Literature Watch Report — Run #8

> 날짜: 2026-06-19  
> 모델: claude-sonnet-4-6  
> 신규 논문: **3편** (Published Journal 1편 + Workshop Paper 1편 + Preprint 1편)

---

## 요약

이번 Run에서 확인한 가장 중요한 신규 논문은 **TSIAA (IEEE TMI 2026)**이다. Instance-level Bézier transformation + teacher-student framework를 SSDG에 적용한 논문으로, "augmentation 강도를 content에 따라 adaptive하게 결정"이라는 방향에서 내 ONA와 일부 겹친다. 그러나 TSIAA = per-image-instance 단위 adversarial adaptation인 반면, 내 ONA = intra-image vessel radius별 연속 조절로 차이가 명확히 분리된다.

추가로 WaveSDG(wavelet 기반 SSDG fundus) preprint와 DomainFlow(coronary vessel SSDG workshop paper)를 확인했다. "vessel observability conditioned augmentation" 핵심 gap은 이번 Run에서도 유지된다.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ 최우선 주의

**논문**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging, Vol 45, pp 764–776  
**Year**: 2026  
**Status**: Published Journal Article  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**arXiv**: 미확인

#### 방법 요약

TSIAA는 기존 adversarial augmentation이 image-level에서 단순한 augmenter를 사용하는 한계를 지적하고, **instance-level**에서 adversarial augmentation을 수행하는 teacher-student 프레임워크를 제안한다.

**IIAG (Instance-level Image Augmenter)**:
- 여러 IAM(Instance-level Augmentation Module)으로 구성
- 각 IAM은 **learnable constrained Bézier transformation** 기반 (ADA와 동일한 변환 함수 계열)
- 이미지의 각 instance (region/patch) 단위로 독립적인 Bézier 파라미터 결정
- 전체 이미지 단위(image-level)보다 훨씬 세밀한 diversity 생성

**Teacher-Student Framework**:
- Teacher: 원본 또는 약하게 augmented 이미지로 안정적인 표현 학습
- Student: adversarially augmented 이미지로 어려운 조건에서의 robust 표현 학습
- Teacher-student consistency loss로 over-augmentation에 의한 label inconsistency 방지

**Adversarial Training**:
- Augmenter가 segmentation 네트워크를 어렵게 만드는 방향으로 함께 최적화
- 기존 adversarial aug의 붕괴 문제를 teacher-student 안정화로 해결

#### 내 방법과의 관계

**공통점**:
- SSDG setting
- Learnable Bézier transformation 활용 (ADA와 동일 변환 계열, 내 방법도 nonlinear 변환)
- Over-augmentation에 대한 explicit protection 메커니즘 존재

**핵심 차이**:
- **TSIAA** = instance(patch/region) 단위로 서로 다른 augmentation 적용 → 여전히 이미지 영역 기반으로, 혈관 두께 정보는 없음
- **내 ONA** = 동일 이미지 내 local vessel radius에 기반하여 연속적으로 augmentation budget 조절
- TSIAA의 "instance"는 이미지 공간적 patch이지 anatomical structure 단위가 아님
- TSIAA에는 **thin vessel의 약한 signal이 강한 augmentation으로 인해 label-image inconsistency가 발생한다**는 문제의식이 없음

**Novelty 위협도**: **Medium-High** — ADA, TSIAA, 내 ONA 세 방법 모두 "더 세밀한 단위에서 Bézier-based adaptive augmentation"을 사용하지만 각각의 "단위"가 다름.

> **구분 논거**: "ADA = per-sample (전체 이미지), TSIAA = per-instance (spatial patch), ONA = per-vessel-observability (anatomical structure의 관찰 가능성). 기존 방법들은 공통적으로 동일 foreground class 내에서의 구조 이질성을 무시한다."

---

### CORONARYDG — STACOM 2024 Workshop

**논문**: Single-Source Domain Generalization for Coronary Vessels Segmentation in X-Ray Angiography  
**저자**: Atwany et al.  
**Venue**: STACOM 2024 (15th Workshop on Statistical Atlases and Computational Models of the Heart, MICCAI 2024 Workshop)  
**DOI**: 10.1007/978-3-031-87756-8_1  
**Year**: 2024  
**Status**: Workshop Paper

#### 방법 요약 (DomainFlow)

- Gaussian posterior at latent space: 모델이 latent space에서 Gaussian 분포를 학습
- Supervised prior으로 posterior 정제: label 정보를 활용해 prior를 감독
- 더 넓은 범위의 data variation에 모델을 노출 → SSDG 강화
- **connectivity mask** 예측: 기존 binary mask 대신 connectivity mask를 예측 target으로 설정하여 도메인 불변 공간 구조 관계 포착

#### 결과
- Out-of-distribution: 0.85–3.85% Dice 개선
- In-domain: 2.86–5.09% Dice 개선

#### 내 방법과의 관계
- Coronary vessel SSDG라는 직접 경쟁 영역
- Paradigm이 다름: generative latent space vs. augmentation-based
- 두께 이질성에 대한 인식 없음
- Workshop 논문 (2024) → 낮은 tier, AngioDG(arXiv 2025)와 같은 문제를 다루나 방법이 다름

---

## Preprint 후보 — WaveSDG

### WaveSDG — arXiv 2603.28463

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**저자**: Shramana Dey, Varun Ajith, Abhirup Banerjee, Sushmita Mitra  
**arXiv**: 2603.28463 (submitted April 27, 2026)  
**Status**: Preprint Only (possibly MICCAI 2026 — 확인 필요)

#### 방법 요약

- **WISER module (Wavelet-based Invariant Structure Extraction and Refinement)**:
  - Encoder feature를 wavelet sub-band로 분해
  - 저주파 성분(LL): 전역 해부학적 구조 anchor
  - 고주파 성분(LH, HL, HH): 방향성 edge 강화 + noise 억제
  - 각 sub-band의 의미적 역할을 활용하여 domain-invariant structure와 domain-specific style 분리
- 평가: optic cup/disc SSDG, 1 source + 5 unseen target
- 7개 SOTA 대비 일관된 성능 향상

#### 내 방법과의 관계
- Cat A (SSDG) 직접 경쟁
- Fundus 도메인, optic disc/cup → 나의 TOF-MRA cerebrovascular와 modality 다름
- Sub-band 분해를 통한 domain-specific / anatomy-specific 분리 아이디어
- 내 방법과 겹치지 않음 (wavelet frequency decomposition vs. vessel radius-conditioned aug)

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드를 직접 명시한 논문은 발견되지 않았다:
- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "intra-class structure-specific augmentation strength"
- "thin vessel appearance protection during augmentation"

TSIAA가 "instance-level adaptive Bézier augmentation"으로 기존 이미지 단위 방법보다 한 단계 세밀해진 것은 내 방법 방향의 연장선이나, **intra-class anatomical observability 기반 augmentation budget**은 TSIAA에도 없다.

**내 ONA의 핵심 gap 유지됨**:
> ADA(per-image) → TSIAA(per-instance patch) → **내 ONA(per-vessel observability)** 순서로 점점 더 세밀한 adaptive augmentation으로 진화하는 방향의 최전선에 내 방법이 있음. 이를 motivation으로 활용 가능.

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문 접근 (IEEE Xplore): 실험 데이터셋, Dice 결과, ablation 확인. arXiv preprint 존재 여부 확인.
- [ ] WaveSDG MICCAI 2026 공식 acceptance 여부 확인 (abstract/arxiv에 "accepted to MICCAI" 표기 탐색)
- [ ] MICCAI 2026 final accepted list (2026년 7-8월 공개 예상) 공개 시 즉시 탐색
- [ ] AG-TAL, DCON 전문 독해 계속 (Run #7 미탐색 항목)
- [ ] ICLR 2026 proceedings (openreview.net 직접 탐색): DG/augmentation 관련 신규 논문
- [ ] "ADA → TSIAA → ONA 계층적 progression" 논거를 Related Work 섹션 draft에 통합
