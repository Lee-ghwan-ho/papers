# DOMAINFLOW — Single-Source Domain Generalization for Coronary Vessels Segmentation in X-Ray Angiography via Connectivity Mask Prediction

**KEY**: DOMAINFLOW
**Category**: C (혈관 특화, 방법론적으로는 A와도 겹침)
**Status**: Accepted Conference Paper (Springer LNCS chapter, 정확한 venue명 미확정 — 검증 필요)
**Year**: ~2025
**Authors**: Atwany et al. (검증 필요)

---

## 핵심 요약

Coronary vessel SSDG (X-ray angiography)를 위한 방법. 모델이 conventional binary
segmentation mask 대신 **connectivity mask**를 예측하도록 학습 target을 재정의.
Connectivity 기반 표현이 raw appearance 변화에 덜 민감한 domain-invariant
structural/topological 관계를 포착한다는 가설.

---

## 내 방법과의 비교

| 구분 | DOMAINFLOW | Continuous-ONA |
|------|-----------|----------------|
| 문제 설정 | Coronary vessel SSDG (X-ray angiography) | Cerebrovascular SSDG (TOF-MRA) |
| 해법 레버 | Target representation (connectivity mask) | Input augmentation strength (continuous, radius-conditioned) |
| Augmentation 사용 여부 | 불명확 (target 재정의가 핵심) | 핵심 메커니즘 |
| Thin vessel 보호 개념 | 간접적 (connectivity가 topology 보존을 유도) | 직접적 (관찰 가능성 낮은 혈관에 약한 aug) |
| Loss/label 변경 | ✅ (connectivity mask로 label 자체를 재정의) | ❌ (내 POC는 augmentation만 비교, loss/label 불변) |

---

## 차별화 논리

DOMAINFLOW는 "무엇을 예측할 것인가(target representation)"를 바꿔서 도메인 불변성을
얻으려는 접근이고, Continuous-ONA는 "어떻게 학습 데이터를 증강할 것인가"를 바꾸는
접근이다. 두 방법은 **orthogonal**하며 원칙적으로 결합 가능하다 (connectivity mask
prediction + continuous observability-conditioned augmentation을 동시에 적용).

Related work에서 "vessel SSDG를 해결하는 대안적 레버(target representation)"로
인용하되, 직접 경쟁 baseline으로 재현하기는 domain(X-ray angiography vs TOF-MRA)과
label 재정의 방식의 차이로 인해 어려울 수 있음.

---

## 인용 계획

- Related Work: "Beyond appearance augmentation, some SSDG methods for vessel
  segmentation instead redefine the prediction target itself to be more
  domain-invariant [DOMAINFLOW]."

## 추가 확인 필요

- [ ] 정확한 venue/DOI 확인 (Springer LNCS chapter — MICCAI workshop 추정되나 미확정)
- [ ] Augmentation을 병행 사용하는지 여부 확인 (현재는 스니펫 기반 요약)
