# Split-and-Combine: Enhancing Style Augmentation for Single Domain Generalization

> **Venue**: ICCV 2025  
> **Status**: Accepted Conference Paper  
> **Category**: D — Top-tier Vision (아이디어 전이)  
> **Relevance**: **High** — 방법론적으로 가장 가까운 top-tier vision 논문  
> **URL**: https://openaccess.thecvf.com/content/ICCV2025/papers/Zhang_Split-and-Combine_Enhancing_Style_Augmentation_for_Single_Domain_Generalization_ICCV_2025_paper.pdf  
> **Added**: Run #8 (2026-07-04)

---

## 요약

자연영상 single-source domain generalization을 위한 patch 기반 style augmentation 방법.

**핵심 구성 요소**:

1. **Split-and-Combine (SAC)**
   - 이미지를 patch로 분할
   - 각 patch에 **독립적인** style augmentation 적용 (전체 이미지 단일 증강 아님)
   - Patch를 다시 결합한 뒤 adaptive random convolution(deformable conv,
     random/Gaussian filter)으로 texture를 다양화하면서 object structure는 보존

2. **Entropy 기반 반복적 강도 조절**
   - Entropy 기반 risk assessment criterion으로 각 샘플에 대해
     "추가 증강을 더 적용할지, 얼마나 강하게 적용할지"를 반복(iterative) 루프에서 적응적으로 결정

3. **Energy 기반 OOD-discrepancy 제어**
   - Energy 기반 distribution-discrepancy score로 증강된 샘플이
     원본 분포에서 얼마나 멀리 밀려나는지(out-of-distribution 정도)를 제어

---

## 내 방법(Continuous-ONA)과의 비교

### 유사점

| 항목 | Split-and-Combine | Continuous-ONA |
|------|--------------------|-----------------|
| 공간적 국소화 | ✅ Patch 단위 독립 증강 | ✅ Vessel 단위(local radius) 독립 증강 |
| 강도의 적응적 조절 | ✅ Entropy 기반 iterative | ✅ Radius 기반 continuous |
| 강도를 제한하는 메커니즘 존재 | ✅ Energy 기반 OOD score로 상한 제어 | ✅ 얇은 혈관에 상한(보수적 강도) |

### 핵심 차이

| 항목 | Split-and-Combine | Continuous-ONA |
|------|--------------------|-----------------|
| **공간 분할 단위** | 고정 격자 patch (semantic/구조 경계 무관) | Vessel 형태를 따라가는 연속 신호 (annotation 기반) |
| **조건 신호** | Model 예측 entropy (implicit, 학습 도중 변화) | Local vessel radius/observability (explicit, source annotation에서 고정 계산) |
| **강도 결정 방식** | Iterative search (entropy가 낮아질 때까지 반복) | 사전에 계산된 continuous mapping (radius → strength) |
| **도메인** | 자연영상 일반 classification/segmentation | Vessel/tubular structure 특화 |
| **얇은 구조 보호 동기** | ❌ 없음 (entropy가 patch 단위 uncertainty를 반영할 뿐, 구조 두께와 무관) | ✅ 핵심 동기 |

### 대응 전략 / 전이 가능한 아이디어

1. **Patch 경계 vs. Vessel 경계**: SAC의 patch는 고정 격자이므로 하나의
   patch 안에 thin vessel과 thick vessel이 섞여 들어갈 수 있다.
   내 방법은 patch가 아니라 vessel의 실제 형태(centerline/radius map)를
   따라가므로 이 문제가 원천적으로 없다는 점을 대비 논거로 사용 가능.

2. **Iterative entropy-gated stopping rule 차용 가능성**: SAC의 "entropy가
   임계값을 넘으면 증강을 멈춘다"는 메커니즘을, 내 방법에서는
   "local radius 기반 risk threshold에 도달하면 nonlinear 강도 증가를
   멈춘다"는 형태로 변형해 도입할 수 있다 — 향후 ablation/확장 아이디어로 기록.

3. **Energy 기반 OOD-discrepancy score**를 thin vessel 영역에 대해
   추가 검증 지표로 활용 가능: "내 방법이 thin vessel 영역에서 원본
   분포로부터 실제로 덜 벗어나는지"를 정량적으로 보이는 데 참고.

---

## 미해결 질문

- [ ] Patch 크기 선택 기준 및 patch 경계 아티팩트 처리 방식
- [ ] Entropy 계산이 pretrained model 기반인지, augmentation 도중 학습되는 모델 기반인지
- [ ] Object structure 보존을 위한 constraint의 구체적 구현 (deformable conv의 displacement 제한 범위)
- [ ] Segmentation task(특히 thin structure)에 대한 실험이 포함되는지 여부
