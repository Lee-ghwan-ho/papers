# Keep the Core: Adversarial Priors for Significance-Preserving Brain MRI Segmentation

> **Venue**: arXiv preprint  
> **Status**: Preprint Only  
> **Category**: B — 방법론 유사  
> **Relevance**: **High** — "얇은/취약한 구조 보호" 동기의 대칭적 선행 사례  
> **arXiv**: 2512.15811  
> **Added**: Run #8 (2026-07-04)

---

## 요약

Brain MRI segmentation을 위한 fragility-aware augmentation 억제 프레임워크.

**핵심 구성 요소**:

1. **SAGE (Sparse Adversarial Gated Estimator)**
   - Offline 모듈. Micro-perturbation을 가했을 때 segmentation 경계를
     뒤집는(flip) 최소한의 token 집합을 adversarial 최적화(ℓ1 sparsity
     penalty on Token Importance Map)로 탐색
2. **KEEP (Key-region Enhancement & Preservation)**
   - SAGE가 찾은 high-importance token에는 augmentation transform을
     "형식적으로는" 적용하되, 원본 픽셀 값을 강제로 복원 — 즉 그 영역에서는
     augmentation 효과를 사실상 무효화(억제)

---

## 내 방법(Continuous-ONA)과의 비교

### 유사점 — 동기의 거울상

| 항목 | KEEPSAGE | Continuous-ONA |
|------|----------|-----------------|
| 핵심 발상 | "Fragile(importance-sensitive)한 영역은 augmentation을 억제해야 한다" | "Fragile(가늘고 관찰이 어려운) vessel은 augmentation을 억제해야 한다" |
| 보호 메커니즘 | 원본 픽셀 값 강제 복원 (강도 = 0에 가깝게) | Radius가 작을수록 강도를 낮춤 (연속적 감쇠) |
| Fragility의 공간적 국소성 | ✅ Token(영역) 단위 | ✅ Voxel/vessel 단위 |

이 논문은 "관찰/구조적으로 취약한 영역은 강한 변형으로부터 보호되어야 한다"는
내 두 번째 핵심 주장(label-image inconsistency 방지)과 정확히 대칭적인 논리를
공유하는 첫 사례다.

### 핵심 차이

| 항목 | KEEPSAGE | Continuous-ONA |
|------|----------|-----------------|
| **Fragility 정의** | Adversarial sensitivity (모델이 예측을 얼마나 쉽게 바꾸는지, model-centric) | Geometric radius/observability (annotation에서 계산, data-centric) |
| **신호 계산 비용** | Adversarial search 필요 (offline이지만 최적화 기반) | Distance transform/skeleton 기반, 계산 저렴 |
| **강도 분포 형태** | 이진에 가까움 (highlighted token은 억제, 나머지는 정상 augmentation) | 연속적 (radius에 비례해 매끄럽게 변화) |
| **Task/도메인** | Brain MRI 일반 segmentation | TOF-MRA vessel-specific SSDG |
| **목적 프레이밍** | Significance-preserving (모델 결정 경계 보호) | Label-image consistency (annotation과 영상 간 정합성 보호) |

### 대응 전략

1. **Model-centric vs. Data-centric 구분**을 FIESTA 대응 논리와 동일한
   패턴으로 적용할 수 있다: KEEPSAGE의 fragility는 "현재 모델이 얼마나
   민감하게 반응하는가"에 의존하므로 학습이 진행됨에 따라 달라지고
   backbone/seed에 따라 재현성이 떨어질 수 있다. 내 radius 기반 신호는
   annotation만으로 결정되므로 모델·학습 단계와 무관하게 고정적이고 재현 가능하다.

2. KEEPSAGE는 이진에 가까운 on/off 방식(강조된 token은 augmentation
   무효화, 나머지는 정상)인 반면, 내 방법은 연속적 스케일링이라는 점을
 명확히 대비 — "단순히 보호할지 말지"가 아니라 "얼마나 보호할지"를
   연속적으로 결정하는 것이 내 기여의 핵심.

3. 이 논문의 존재는 "구조적으로 취약한 영역에서 augmentation을 억제해야
   한다"는 내 문제의식이 임의적이지 않고 다른 연구 그룹에서도 독립적으로
   도달한 결론임을 보여주는 지지 근거로 인용 가능 (Related Work 또는
   Motivation 섹션).

---

## 미해결 질문

- [ ] Adversarial sensitivity map과 vessel radius map이 실제로 얼마나 상관관계를 갖는지(같은 영역을 가리키는지) 검증 필요 — 만약 강한 상관관계가 있다면 내 방법이 "더 저렴한 근사"라는 논거가 가능
- [ ] SAGE의 offline 계산 비용 및 데이터셋 규모에 따른 확장성
- [ ] KEEP이 vessel/tubular 구조를 포함하는 실험에서 어떻게 동작하는지 (해당 없을 가능성 높음 — brain MRI 일반 구조 대상으로 추정)
