# TSIAA — Paper Note

**제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue**: IEEE Transactions on Medical Imaging (IEEE TMI)  
**Year**: 2026  
**Volume/Pages**: Vol. 45, pp. 764–776  
**Status**: Published Journal Article  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Category**: A (직접 경쟁 — SSDG)  
**Relevance**: **High**  
**Added**: Run #8 (2026-06-15)

---

## 핵심 문제의식

Single-source domain generalization (SDG)에서 adversarial augmentation은 강력하나, 기존 방법은 **image-level** 구조 단순 augmenter를 사용 → **over-augmentation** 문제 발생.  
기존 adversarial SSDG aug는 전체 이미지 단위로 동일한 변환 적용 → 구조/세부 특징이 다른 영역을 동일하게 왜곡.

---

## 방법

### Instance-level Image Augmenter (IIAG)
- IAMs (Instance-level Augmentation Modules): **learnable constrained Bézier transformation** 기반
- Input을 per-sample 단위로 독립적으로 변환 (이미지별 서로 다른 aug 파라미터)
- Teacher-Student 구조:
  - **Teacher**: IIAG를 이용해 hard augmented views 생성 (adversarial)
  - **Student**: teacher가 생성한 어려운 augmented 이미지에서 domain-invariant feature 학습

### Key Difference from Prior Work (ADA, SLAug)
- **SLAug**: global + local (class별) Bézier remap. Class 단위로 서로 다른 변환
- **ADA (MICCAI 2025)**: per-sample adaptive Bézier remap (내용 기반 조절)
- **TSIAA**: adversarial teacher-student로 per-sample **hard** example 탐색. 여전히 이미지 전체에 단일 강도 적용

---

## 실험 설정

| 벤치마크 | 설정 |
|---------|------|
| Prostate MRI | T2-weighted, 6 sources → leave-one-out SSDG |
| Retinal Fundus | 4 sources → leave-one-out SSDG |

---

## 내 ONA와의 비교 분석

### 공통점
- Bézier transformation 계열 사용 (TSIAA: learnable Bézier, 나: nonlinear 변환 계열)
- SSDG 설정 (단일 소스 도메인)
- augmentation diversity 확대를 통한 generalization

### 핵심 차이점 (내 novelty 방어 근거)

| 구분 | TSIAA | Continuous-ONA (나) |
|------|-------|---------------------|
| Augmentation unit | 이미지 전체 (image-level uniform) | Intra-image pixel/region 단위 |
| Structural conditioning | 없음 (adversarial search only) | vessel radius/observability score |
| 공간적 변화 | 없음 (단일 aug strength per image) | 연속적 공간 변화 (thick→강함, thin→약함) |
| Training mechanism | Teacher-student adversarial | Deterministic radius-based scheduling |
| Fragile structure 보호 | ❌ 고려 없음 | ✅ thin vessel protection |
| Label-image inconsistency 문제 | ❌ 고려 없음 | ✅ 핵심 동기 |

### 내 주장에 대한 TSIAA의 함의
TSIAA도 over-augmentation 문제를 인식하지만, 해결 방식은 **per-sample adversarial difficulty 조절** (이미지 단위).  
내 ONA는 더 세밀한 **intra-image spatial resolution**에서 thin vs. thick vessel의 observability 차이를 반영한 augmentation budget 할당.  
→ "TSIAA가 image-level per-sample 문제를 해결했지만, intra-image structural heterogeneity는 여전히 미해결"이라는 논거로 활용 가능.

---

## 관련 논문 비교 위치

- ADA (MICCAI 2025) vs. TSIAA (IEEE TMI 2026): 둘 다 Bézier + SSDG, 차이는 adversarial(TSIAA) vs. non-adversarial(ADA)
- SLAug (AAAI 2023): class-level Bézier. TSIAA = 이를 adversarial instance-level로 발전
- **내 방법**: 이 계보와 orthogonal한 spatial 차원의 조절 추가

---

## 읽을 때 확인사항

- [ ] TSIAA와 ADA의 실험 결과 수치 비교 (같은 벤치마크에서)
- [ ] IIAG의 Bézier parameter optimization 방식 (adversarial loss 정확히 무엇?)
- [ ] Per-sample uniform aug 주장의 ablation 결과
- [ ] TOF-MRA cerebrovascular 도메인 실험 없음 확인 (내 차별화 강화)
