# AG-TAL: Anatomically-Guided Topology-Aware Loss

> **KEY**: AG-TAL  
> **Venue**: arXiv 2604.27357 (April 30, 2026) — Preprint Only  
> **Status**: Preprint Only  
> **Category**: C (구조·혈관특화)  
> **Relevance**: High  
> **Novelty 충돌**: Low — 목적/mechanism/setting 분리 명확

---

## 논문 기본 정보

- **제목**: AG-TAL: Anatomically-Guided Topology-Aware Loss for Multiclass Segmentation of the Circle of Willis Using Large-Scale Multi-Center Datasets
- **저자**: Jialu Liu et al.
- **문제**: Circle of Willis (CoW) multiclass arterial segmentation — 복잡한 혈관 토폴로지, 소혈관 class imbalance, 인접 동맥 간 misclassification
- **Task**: Supervised closed-set segmentation (No DG setting)
- **Data**: Large-scale multi-center CoW dataset (unified annotation) + 6 independent test datasets

---

## 핵심 방법: AG-TAL (3-component loss)

### 1. Radius-Aware Dice Loss (RAD)

```
L_RAD = 1 - (2 * Σ r(x) · p(x) · g(x)) / (Σ r(x) · p(x) + Σ r(x) · g(x))
```

- **r(x)**: voxel x에서의 GT vascular radius (distance transform on GT skeleton으로 계산)
- **p(x)**: predicted probability
- **g(x)**: GT binary mask
- **효과**: 소혈관(작은 r)에서는 분모/분자 기여도 감소 → 표준 Dice의 소혈관 무시 문제를 radius-weighting으로 보정
- **성능**: Small artery Dice +1.05~3.09% over standard Dice/clDice SOTA

### 2. Breakage-Aware clDice (BAC)

- 기존 clDice: 3D multiclass 환경에서 voxelwise skeleton extraction이 매우 비쌈 (prohibitive)
- **Group Convolution 기반 효율화**: 각 arterial class를 독립 group으로 병렬 처리
- Local connectivity를 효율적으로 보존하면서 breakage penalty 부과

### 3. Adjacency-Aware Co-occurrence Loss (AAC)

- CoW 해부학: 특정 artery 쌍은 반드시 인접 → anatomical prior로 인코딩
- 인접 동맥 경계에서 inter-class confusion matrix를 penalize
- Class confusion 패턴이 해부학 지식과 일치하도록 강제

---

## 실험 결과

| Dataset | All CoW Dice | Small Artery Dice |
|---------|-------------|-------------------|
| Cross-val (ours) | 80.85% | SoTA +1.05~3.09% |
| 6 independent sets | 74.46~81.17% | +2.20~9.98% over SoTA |

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점 (주의 필요)

| 항목 | AG-TAL | Continuous-ONA |
|------|--------|----------------|
| Radius 활용 목적 | loss weighting | augmentation budget conditioning |
| 근거 | 소혈관 Dice가 반영 부족 | 얇은 혈관은 label-image inconsistency 위험 |
| Radius 계산 | GT skeleton distance transform | (동일한 방식 사용 가능) |

### 결정적 차이

| 항목 | AG-TAL | Continuous-ONA |
|------|--------|----------------|
| **Mechanism** | Loss re-weighting (training signal) | Augmentation budget (input-space) |
| **Task** | Single-domain closed-set CoW seg | Single-source DG for TOF-MRA seg |
| **DG setting** | ❌ 없음 (동일 distribution) | ✅ SSDG |
| **Input** | 정해진 GT로 weight 계산 | augmentation 전/후 intensity map |

### 내 연구에서의 활용

1. **Related Work 인용 근거**: "vessel radius를 training에 활용한 선행 연구 — loss에서는 AG-TAL, augmentation에서는 ONA"로 구분
2. **Radius 계산 방법**: AG-TAL의 skeleton distance transform → ONA의 observability score 계산에 직접 차용 가능
3. **소혈관 집중의 필요성**: AG-TAL도 radius-based weighting이 소혈관 성능을 크게 향상시킨다는 empirical evidence 제공 → ONA의 동기 지지 근거로 활용 가능

---

## 인용 전략

AG-TAL을 Related Work에서 다음과 같이 포지셔닝:

> "While prior work has recognized the importance of vessel radius in training (AG-TAL uses radius-weighted Dice loss for CoW segmentation), the impact of vessel observability on *augmentation*-based DG has not been studied. Our ONA framework addresses this gap by conditioning augmentation strength on local vessel radius..."

---

## 메모

- CoW segmentation focus이므로 직접 경쟁 논문은 아님
- arXiv preprint — venue 확정 시 status 업데이트 필요
- Radius 계산식이 논문에 명시되어 있음 → ONA 구현 시 참조 가능
