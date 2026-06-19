# TSIAA — Paper Note

**제목**: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging  
**Year**: 2026  
**Volume/Pages**: Vol 45, pp 764–776  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Status**: Published Journal Article  
**Category**: A (직접 경쟁)  
**Relevance**: High — novelty 위협 존재, 차이 명확히 파악 필요

---

## 방법 요약

TSIAA(Teacher-Student Instance-level Adversarial Augmentation)는 SDG(Single Domain Generalization) 의료영상 분할을 위한 방법으로, 기존 adversarial augmentation의 한계를 해결한다.

### 핵심 문제의식
- 기존 adversarial 기반 SSDG 방법들은 단순한 image-level augmenter를 사용
- Image-level augmentation은 diversity가 제한적 → out-of-source distribution 탐색 불충분

### 핵심 구성요소

1. **IIAG (Instance-level Image Augmenter)**:
   - 여러 IAM(Instance-level Augmentation Module)으로 구성
   - 각 IAM은 **learnable constrained Bézier transformation function** 기반
   - Bézier 파라미터가 입력 이미지의 instance (patch 또는 region) 단위로 독립적으로 결정
   - 전체 이미지가 아닌 per-instance로 서로 다른 augmentation 적용

2. **Teacher-Student Framework**:
   - Teacher 네트워크: augmentation이 약한 또는 원본 이미지로 학습
   - Student 네트워크: adversarially augmented 이미지로 학습
   - Teacher-student 간 consistency loss로 over-augmentation 방지
   - 기존 adversarial aug에서 발생하는 label inconsistency 완화

3. **Adversarial Training**:
   - Augmenter가 segmentation 네트워크를 어렵게 만드는 방향으로 최적화
   - Segmentation 네트워크가 더 강한 augmentation에도 robust하도록 훈련

---

## 내 방법과의 관계 분석

### 공통점
- SSDG setting에서 augmentation strength를 content에 따라 조절
- Bézier transformation을 augmentation에 활용 (ADA와도 공유)
- Over-augmentation 방지 메커니즘 존재 (teacher-student vs. radius-based threshold)
- 학습 가능한(learnable) 파라미터로 augmentation 결정

### 핵심 차이 ← 논문 작성 시 필수 구분 논거

| 구분 | TSIAA | 내 Continuous-ONA |
|------|-------|-------------------|
| 조절 단위 | **Per-instance (이미지 패치/영역 단위)** | **Per-vessel-radius (혈관 내 반경별 연속 조절)** |
| 조절 기준 | 학습된 adversarial objective (어떤 aug가 어렵게 만드나) | **GT annotation에서 직접 계산한 local vessel radius/observability** |
| 인트라-클래스 구분 | ❌ 혈관 class 내 thin/thick 구분 없음 | ✅ 동일 foreground 내 관찰 가능성에 따라 다른 budget |
| 타겟 문제 | 전반적 image diversity 확장 (domain coverage) | 얇은 혈관의 label-image inconsistency 방지 |
| 방향 | 어렵게 → 잘 분류하도록 (adversarial) | 관찰 가능성에 비례하여 변형 허용 (protective) |
| 이론적 근거 | Adversarial augmentation theory | Observability-budget consistency |

### 내 novelty claim 보호 수준: **Medium**
- "augmentation strength를 image content에 따라 적응적으로 결정"이라는 큰 방향은 겹침
- 그러나 TSIAA = image-level instance diversity, 나 = **intra-vessel radius conditioning**
- TSIAA는 thin vessel 보호 개념이 없음 → 내 핵심 contribution과 분리 가능

---

## 실험 정보 (확인 필요)
- 데이터셋: 추정 prostate T2-MRI, cardiac, fundus (Vol 45, pp 764-776 기준으로 추정)
- 정확한 실험 결과는 IEEE Xplore 전문 접근 후 확인 필요

---

## 내 논문에서 활용 방안
- Related Work에서 "per-instance adaptive Bézier augmentation" 범주로 TSIAA를 ADA와 함께 묶어 기술
- **구분 논거**: "기존 방법들(ADA, TSIAA)은 이미지 또는 인스턴스 단위로 augmentation을 조절하지만, 동일 이미지 내 혈관의 관찰 가능성 차이를 무시한다"
- 비교 baseline으로 포함 검토 (IEEE TMI 2026이므로 중요도 높음)

---

## 탐색 필요 항목
- [ ] IEEE Xplore 전문 접근: 실험 데이터셋, Dice 수치, ablation 결과 확인
- [ ] arXiv preprint 버전 존재 여부 확인 (IEEE TMI 최종 게재 전 preprint가 있을 수 있음)
- [ ] Bézier 파라미터가 어떤 단위로 독립적으로 결정되는지 상세 메커니즘 확인
