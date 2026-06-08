# TSIAA — Paper Note

> KEY: TSIAA  
> 날짜: 2026-06-08  
> Novelty 위협도: **Medium-High** ⚠️

---

## 기본 정보

- **제목**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
- **저자**: Zhengshan Wang, Long Chen et al.  
- **Venue**: IEEE Transactions on Medical Imaging (IEEE TMI)  
- **Year**: 2026  
- **Status**: Published Journal Article  
- **IEEE Xplore DOI**: 10.1109/TMI.11146907 (Article 11146907)  
- **arXiv**: 미확인 (IEEE Xplore에만 공개된 것으로 보임)  
- **Code**: https://github.com/Wangzts0228/TSIAA

---

## 핵심 주장 요약

### 문제 정의
기존 adversarial augmentation 기반 SDGMIS 방법의 한계:
1. **Image-level augmentation**: 이미지 전체에 동일한 augmentation rule 적용 → uniformity 문제
2. **Over-augmentation problem**: 증강 강도가 너무 강해 오히려 representation 학습을 방해
3. 단순 구조의 augmenter만 사용 → augmented image의 다양성 부족

### 제안 방법: TSIAA

#### 1. Instance-level Image Augmenter (IIAG)
- 여러 **Instance-level Augmentation Modules (IAMs)**로 구성
- 각 IAM = **learnable constrained Bézier transformation function** 기반
- "instance-level 연산이 이미지 내 구조별 augmentation rule의 **uniformity를 깨서** 더 큰 diversity 제공"
- 기존 image-level augmentation의 균일성을 극복

#### 2. Teacher-Student Adversarial Learning
- **Augmentation phase (adversarial)**: IIAG가 out-of-source data를 탐색하는 adversarial augmentation
- **Generalization phase**: Student가 원본 + 증강 이미지에서 consistent representation 학습
- Teacher EMA로 업데이트 → stable training
- Alternating optimization으로 두 phase 번갈아 학습

#### 3. Over-augmentation 방지
- Instance-level으로 증강 → 전체 이미지 레벨보다 제어 가능
- Constrained Bézier: 변환이 monotonic하게 유지 (label-preserving)

### 실험
- Prostate T2-MRI SSDG (6 centers): SOTA
- Cardiac MRI segmentation cross-domain

---

## 내 방법 (Continuous-ONA)과의 비교

### 공통 동기
| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 핵심 비판 | image-level augmentation의 uniformity 파괴 | uniform augmentation budget 파괴 |
| 방법론적 방향 | adversarial training + instance-level aug | structure-conditioned aug budget |
| Bezier 활용 | learnable per-instance Bezier parameters | nonlinear appearance transformation family |

### 핵심 차이점

| 차이 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| **목적** | augmentation diversity 극대화 (adversarial) | thin vessel appearance 보호 (conservative) |
| **conditioning signal** | annotation mask 단위 (foreground instance 전체) | vessel radius / observability (연속값) |
| **강도 조절 방향** | 최대 강도 탐색 (hard augmentation) | observability ↓ → 강도 ↓ (protective) |
| **intra-class 세분화** | class 내에서는 동일 강도 가능 | class 내에서 thin/thick 구분 (핵심 gap) |
| **target structure** | prostate, cardiac (blob-shape organs) | cerebrovascular (tubular, thin-to-thick gradient) |
| **setting** | single source DG + adversarial | single source DG, loss-free (POC) |
| **thin vessel 보호** | 없음 | 핵심 기여 |

### 내 novelty 구분 논거

> TSIAA는 instance-level로 uniformity를 깨지만, 각 instance 내에서는 여전히 동일한 강도를 적용한다.  
> 반면 Continuous-ONA는 동일한 혈관 class 내에서도 radius/observability에 따라 연속적으로 다른 강도를 적용한다.  
> 더 중요하게, TSIAA는 "어떤 diversity를 추가할 것인가?"를 adversarial하게 탐색하는 반면,  
> Continuous-ONA는 "fragile structure는 왜 강한 augmentation으로부터 보호되어야 하는가?"를  
> morphological observability에서 도출한다.  
> TSIAA에는 thin vessel protection의 개념이 없으며, tubular structure의 이질성(observability gradient)을 다루지 않는다.

---

## Related Work에서의 활용

- TSIAA를 "instance-level augmentation의 선행 연구"로 인용 가능
- 내 방법이 TSIAA와 동기는 유사하나 **방향이 반대**: TSIAA는 다양성 극대화, 나는 fragile structure 보호
- "TSIAA와 달리 우리는 augmentation strength를 vessel morphology로 regulate한다"는 서술로 차별화

---

## 즉시 확인 필요 사항

- [ ] IEEE Xplore 전문 접근: IAM 수 (몇 개?) 및 이미지 내 어떻게 분할하는지
- [ ] ablation: instance-level vs. image-level 성능 차이 확인
- [ ] vessel segmentation 실험 여부 확인 (prostate/cardiac 외)
- [ ] code 확인: IAM이 서로 다른 annotation region에 독립적으로 적용되는지
