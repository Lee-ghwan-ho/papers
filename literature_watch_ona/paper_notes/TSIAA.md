# TSIAA — Paper Note

**제목:** Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**Venue:** IEEE Transactions on Medical Imaging, Vol.45, pp.764–776, 2026  
**DOI:** 10.1109/TMI.2026.11146907  
**IEEE Xplore:** https://ieeexplore.ieee.org/document/11146907  
**발견 Run:** #8 (2026-06-23)  
**Novelty 충돌 위험도:** ★★★ (ADA와 함께 직접 경쟁 — 구분 필수)

---

## 핵심 아이디어

### 문제의식
기존 adversarial image augmentation 기반 SSDG는 다음 두 가지 한계를 지적:
1. **Simple structure**: augmenter가 단순해 생성 다양성이 제한됨
2. **Image-level**: 이미지 전체에 동일한 변환 적용 → over-augmentation이 쉽게 발생

### 제안 방법: TSIAA
- **Teacher-Student framework**: Teacher가 augmentation strength를 가이드
- **IIAG (Instance-Level Image Augmenter)**:
  - IAM (Instance Augmentation Module) 여러 개를 순차 스택
  - 각 IAM: learnable constrained Bézier 변환 함수 사용
  - per-instance: 각 이미지에 대해 다른 Bézier 파라미터를 학습
- Adversarial training: augmented image가 harder for segmentation → model robustness 향상

---

## 내 Continuous-ONA와의 비교

| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 적용 단위 | 이미지 전체 (per-instance global) | 공간적 location별 (intra-image spatial) |
| Bézier 파라미터 | 이미지마다 다른 global param | vessel radius에 따라 공간적으로 다른 param |
| Thin vessel 보호 | 없음 (이미지 전체에 동일하게) | 핵심 기능 (얇은 혈관 = 약한 augmentation) |
| 훈련 방식 | Adversarial (teacher-student) | 비-adversarial (구조 기반 scheduling) |
| 설정 | SSDG (single source) | SSDG (single source) |
| Target | source aug diversity ↑ | observability-guided aug budget |
| Label-image consistency | 암묵적 보장 | 명시적 보장 (thin vessel=weak aug) |

### 핵심 구분점 (논문 작성 시 강조)
> TSIAA augments each image with a globally consistent Bézier mapping, inevitably applying the same nonlinear appearance distortion to both thick and thin vessel regions within a single image.
> In contrast, ONA conditions the augmentation magnitude on the local vessel observability, spatially protecting fragile thin structures from excessive appearance shifts while allowing stronger variations on well-resolved vessels.

---

## ADA vs TSIAA vs ONA 3-way 비교

| | ADA (MICCAI 2025) | TSIAA (TMI 2026) | ONA (ours) |
|-|-------------------|------------------|------------|
| 적응 기준 | content feature (per-sample) | adversarial gradient (per-instance) | vessel radius (per-location) |
| 공간적 차등 | No | No | Yes |
| Thin vessel 보호 | No | No | Yes |
| Adversarial | No | Yes | No |
| 출발 구조 | Bézier transform | Bézier + teacher-student | Nonlinear monotone |
| Training overhead | 중간 | 높음 | 낮음 |

---

## 실험 데이터셋 확인 필요
- TSIAA의 실험 데이터셋이 prostate / cardiac / fundus인지 확인 → 내 TOF-MRA와 겹침 여부
- 비교 baseline 목록 확인: SLAug, ADA 등과 함께 제시 여부

---

## 논문 활용 방안

1. **Related Work에서 인용**: "Per-sample adaptive augmentation (ADA, TSIAA)과 달리, ONA는 동일 이미지 내 vessel thickness에 따라 공간적으로 augmentation budget을 차등화한다"
2. **Baseline 비교**: TOF-MRA 실험에서 TSIAA를 적용한 비교 실험 고려 (구현 복잡도 높을 수 있음)
3. **동기 지지**: TSIAA도 "uniform augmentation은 harmful"하다는 전제를 공유 → 내 문제의식의 일반성 지지

---

## 미확인 사항
- [ ] 실험 데이터셋 상세 (MICCAI 2024 prostate 6-center? cardiac? fundus?)
- [ ] IAM 내부 Bézier 파라미터 제약 방식 (monotone 보장?)
- [ ] Teacher-student consistency loss 수식
- [ ] TSIAA code 공개 여부
