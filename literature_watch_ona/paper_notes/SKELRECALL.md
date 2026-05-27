# Skeleton Recall Loss for Connectivity Conserving Segmentation of Thin Tubular Structures

**KEY**: SKELRECALL  
**Category**: C (구조·혈관 특화) / D (Top-tier Vision)  
**Status**: Accepted Conference Paper  
**Venue**: ECCV 2024  
**arXiv**: 2404.03010  
**GitHub**: https://github.com/MIC-DKFZ/Skeleton-Recall  
**Authors**: Cai et al. (MIC-DKFZ)

---

## 핵심 요약

얇은 tubular structure 분할에서 표준 Dice/CE loss가 volumetric overlap에 집중하여
thin structure의 connectivity를 희생한다는 문제를 해결.

**Skeleton Recall Loss**: 예측 segmentation의 skeleton이 ground truth skeleton과
얼마나 겹치는지를 recall 형태로 직접 최적화.

**핵심 장점**: GPU 기반 differentiable skeletonization 불필요. CPU 연산으로 효율적.
3D 대용량 및 multi-class segmentation에서도 적용 가능.

---

## 내 연구와의 관계

### 동기 논거로서의 가치 (★★★★★)

이 논문의 핵심 관찰:
> "Standard segmentation losses focus on volumetric overlap, at the expense of preserving structural connectivity of thin tubular structures."

이를 내 augmentation 동기에 연결:
> "If standard volumetric losses already fail to protect thin vessel connectivity, 
> then applying uniform strong augmentation — which further reduces the visible signal 
> of thin vessels — compounds the problem. Our method addresses this at the 
> augmentation level: thin, fragile vessels receive conservative perturbations 
> that preserve their already-weak structural evidence."

### 인용 전략

**도입부에서**: "Thin tubular structures receive insufficient supervisory signal from volumetric losses [SKELRECALL], and we show that uniform appearance augmentation further exacerbates this problem by creating label-image inconsistency in fragile structures."

**방법론 섹션에서**: Skeleton-based observability 계산 시 SKELRECALL의 skeleton 개념 인용 가능.

---

## 추가 활용

- 내 thin vessel 정의 및 observability score 계산에서 skeleton 기반 접근 채택 시 직접 인용.
- Evaluation metric으로 skeleton recall / clDice를 사용할 때 이 논문 참조.
