# TSIAA: Teacher–Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation

> **Venue**: IEEE Transactions on Medical Imaging, Vol. 45, Issue 2, pp. 764–776  
> **Status**: Published Journal Article  
> **Category**: A — 직접 경쟁  
> **Relevance**: **High** — Novelty 충돌 가능성 최우선 경계 대상  
> **URL**: https://ieeexplore.ieee.org/document/11146907/  
> **Code**: https://github.com/Wangzs0228/TSIAA (검증 필요)  
> **Added**: Run #8 (2026-07-04)

---

## 요약

TSIAA는 SSDG(Single-Source Domain Generalization) medical image segmentation을 위한
teacher-student adversarial augmentation framework다.

**핵심 주장 (논문 동기 부여 문장, 재구성)**:
> Image-level uniform adversarial augmentation은 최적이 아니다.
> Instance-level adversarial augmentation은 이미지 내 서로 다른 구조 간
> 증강 규칙의 균일성을 깨뜨림으로써(breaks the uniformity of augmentation
> rules across different structures within an image) 더 큰 다양성을 제공한다.

**핵심 구성 요소**:

1. **Instance-level Image Augmenter (IIAG)**
   - 여러 개의 Instance-level Augmentation Module(IAM)로 구성
   - 각 IAM은 learnable constrained Bézier transformation function
   - 이미지 내 서로 다른 instance(구조 단위 추정, 전문 확인 필요)마다
     서로 다른 Bézier 파라미터를 적용

2. **Teacher-Student Adversarial Loop**
   - Augmentation search 단계: source distribution 밖의 plausible한 appearance를 탐색(adversarial)
   - Representation learning 단계: teacher-student consistency로 domain-invariant 표현 학습

**실험**: 4개 SSDG segmentation 벤치마크 (fundus/prostate/polyp 계열로 추정, vessel 특화 여부 미확인)

---

## 내 방법(Continuous-ONA)과의 비교 — ⚠️ 최우선 위험 논문

### 유사점 (위험 요소)

| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| 핵심 주장 | "구조마다 다른 augmentation rule을 적용해야 한다" | "구조마다 다른 augmentation budget을 적용해야 한다" |
| Augmentation type | Bézier transformation (nonlinear appearance) | Bézier/spline 기반 nonlinear appearance transformation |
| 증강 단위 | **Intra-image, instance-level** | **Intra-image, structure(vessel)-level** |
| SSDG 목적 | ✅ | ✅ |

이 두 논문은 **"전체 이미지 또는 semantic class 단위로 균일하게 증강해서는 안 된다"**는
동일한 상위 명제를 공유한다. 이는 지금까지 조사한 논문 중 내 novelty claim과
문장 수준에서 가장 근접한 사례다.

### 핵심 차이 (Novelty 방어 포인트)

| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| **조건 신호의 성격** | Adversarial하게 탐색/학습되는 opaque parameter | **Annotation에서 직접 계산되는 explicit, interpretable local vessel radius/observability** |
| **강도 분포 형태** | Instance별 이산적 값 (adversarial search 결과) | **연속적(continuous) 함수 — radius가 클수록 강도가 매끄럽게 증가** |
| **Instance의 정의** | Object/lesion 단위로 추정 (전문 확인 필요) | Single foreground class(vessel) 내부의 continuous thickness spectrum |
| **구조 특이성** | Tubular/vessel 구조에 특화되지 않음 | Vessel radius/vesselness 기반 설계 |
| **얇은 구조 보호** | ❌ 명시적 언급 없음 | ✅ fragile structure 보호가 핵심 동기 |
| **Label-image consistency** | ❌ 문제로 정의하지 않음 | ✅ 핵심 문제의식 |
| **조건화 메커니즘의 목적** | Adversarial diversity 극대화 (강도가 클수록 좋다는 방향) | **강도를 제한하는 것 자체가 목적** (얇은 구조는 약하게) |

### 대응 전략

1. **우선권 인정 + 차별화**: TSIAA가 "구조별 비균일 증강"이라는 아이디어를
   먼저 저널에 출판했다는 사실을 Related Work에서 명시적으로 인정한다.
   그 위에서 "무엇을 조건 신호로 쓰는가"와 "왜 이 신호를 써야 하는가"로
   기여를 재정의해야 한다.

2. Adversarial search 기반 강도 결정은 **모델이 무엇을 어렵다고 느끼는지**를
   따라간다(model-centric, 극단적 diversity를 지향). 반면 내 방법은
   **annotation이 무엇을 관찰 가능하다고 말하는지**를 따라간다(data-centric,
   보수적 보호를 지향). 이 방향성 자체가 정반대다: TSIAA는 "더 다양하게",
   나는 "선택적으로 보수적으로".

3. TSIAA가 vessel/tubular 구조에 대한 실험을 포함하는지, "instance"가
   실제로 thin/thick vessel 같은 continuous property로 구분되는지
   전문에서 반드시 확인해야 한다. 만약 instance가 semantic object
   (e.g., 개별 polyp, 개별 병변) 단위라면 SLAug의 "class-level" 구분과
   본질적으로 유사한 확장이며, 내 "단일 class 내부의 연속적 구분"과는
   granularity 자체가 다르다는 논거가 성립한다.

4. Related Work 문안 초안:
   > "TSIAA breaks the uniformity of adversarial augmentation across
   > instances via learnable, adversarially-optimized Bézier parameters.
   > While this shares our premise that a single global augmentation
   > policy is suboptimal, TSIAA's per-instance strength is an opaque,
   > model-driven quantity intended to maximize diversity. In contrast,
   > Continuous-ONA derives augmentation strength from an explicit,
   > annotation-based continuous measure of local vessel observability,
   > and uses this measure to conservatively constrain — rather than
   > maximize — perturbation strength on fragile structures."

---

## 미해결 질문 (즉시 확인 필요)

- [ ] "Instance"의 정확한 정의: connected component 단위인지, semantic object 단위인지, 아니면 patch/region 단위인지
- [ ] IAM 개수와 이미지 분할 방식 (자동 분할인지, annotation 기반인지)
- [ ] 실험 데이터셋에 vessel/tubular segmentation이 포함되는지 여부
- [ ] Adversarial search의 강도 범위(bound)가 존재하는지 — 만약 강도를 제한하는 constraint가 있다면 내 "보수적 보호" 개념과 더 가까워질 수 있음
- [ ] GitHub 코드(Wangzs0228/TSIAA)에서 Bézier 파라미터가 실제로 어떤 신호에 conditioned 되는지 확인
