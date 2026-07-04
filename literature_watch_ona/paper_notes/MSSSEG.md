# MSSSeg: Learning Multi-Scale Structural Complexity for Self-Supervised Segmentation

> **Venue**: arXiv preprint  
> **Status**: Preprint Only  
> **Category**: B — 방법론 유사  
> **Relevance**: **High** — conditioning 신호 설계의 대안 사례  
> **arXiv**: 2512.23997  
> **Added**: Run #8 (2026-07-04)

---

## 요약

Self-supervised segmentation을 위한 구조 복잡도 기반 학습 프레임워크.

**핵심 구성 요소**:

1. **Differentiable Box-Counting 모듈**
   - Multi-scale fractal/structural complexity를 semantic feature와 정렬해 추정
2. **StructAug**
   - 이 구조 복잡도에 따라 pixel-intensity 패턴을 얼마나 강하게 corrupt할지
     학습 가능한(learnable) 방식으로 결정 — appearance가 아닌 structural cue에
     의존하도록 네트워크를 강제
3. **Persistent Homology Loss**
   - 예측 결과의 topological correctness를 supervise

---

## 내 방법(Continuous-ONA)과의 비교

### 유사점 (가장 근접한 mechanism 구조)

이 논문은 지금까지 조사한 논문 중 **"continuous structural signal → continuous
augmentation 강도"**라는 정확히 같은 형태의 매핑 함수를 사용하는 유일한 사례다.

| 항목 | MSSSeg | Continuous-ONA |
|------|--------|-----------------|
| 조건 신호의 연속성 | ✅ Continuous (box-counting complexity) | ✅ Continuous (vessel radius/observability) |
| 신호 계산 방식 | Differentiable, 학습 도중 추정 | Annotation 기반, 사전 계산(고정) |
| 증강 강도의 방향 | 복잡도가 높을수록 강하게 corrupt (shortcut 방지 목적) | Radius가 클수록 강하게 augment (thin vessel 보호 목적) |

### 핵심 차이

| 항목 | MSSSeg | Continuous-ONA |
|------|--------|-----------------|
| **Task setting** | Self-supervised representation learning | Supervised SSDG segmentation |
| **신호의 물리적 의미** | Fractal/box-counting complexity (추상적 통계량) | Vessel radius/vesselness (해부학적으로 해석 가능) |
| **목적** | Shortcut(appearance) 의존 억제 | Label-image consistency 보호 (fragile structure) |
| **Vessel/tubular 특화** | ❌ | ✅ |
| **Loss 변경 여부** | ✅ (Persistent Homology Loss 병행) | ❌ (내 POC는 augmentation만) |

### 대응 전략

1. **Radius와 fractal complexity의 상관관계 검증 필요**: thin vessel이
   box-counting complexity 관점에서 높게 나올지 낮게 나올지는 자명하지 않다.
   가는 혈관은 국소적으로 단순한 형태(직선에 가까운 원통)일 수도 있고,
   partial volume effect로 인해 fractal 경계가 복잡하게 보일 수도 있다.
   이 관계를 명시적으로 논하면 내 신호 선택(radius)의 정당성을 강화할 수 있다.

2. MSSSeg는 "복잡한 구조는 더 강하게 증강"하는 반면, 내 방법은
   "관찰 가능한(굵은) 구조는 더 강하게, 관찰 불가능한(가는) 구조는 약하게"
   증강한다 — 두 방법의 강도 방향이 반드시 일치하지 않을 수 있음을
   Discussion에서 명확히 해야 한다 (복잡도 ≠ 관찰가능성).

3. Self-supervised라는 task 차이를 명확히 해 직접 경쟁이 아님을 밝히되,
   "continuous structure-conditioned augmentation"이라는 mechanism
   class의 선행 사례로 인용.

---

## 미해결 질문

- [ ] Box-counting complexity가 실제로 vessel radius와 어떤 상관관계를 갖는지 실증적으로 확인 필요
- [ ] StructAug의 강도 스케줄이 사전 정의된 함수인지, end-to-end 학습되는지
- [ ] Persistent Homology Loss가 없을 때 StructAug만으로 thin structure에 미치는 영향 (ablation 존재 여부)
