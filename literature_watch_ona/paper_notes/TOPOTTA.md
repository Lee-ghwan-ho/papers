# TopoTTA — Topology-Enhanced Test-Time Adaptation for Tubular Structure Segmentation

**KEY**: TOPOTTA  
**Category**: C (구조·혈관 특화) / D (Top-tier Vision)  
**Status**: Accepted Conference Paper  
**Venue**: ICCV 2025  
**arXiv**: 2508.00442  
**ICCV 2025 Open Access**: https://openaccess.thecvf.com/content/ICCV2025/html/Zhou_TopoTTA_...  

---

## 핵심 요약

Tubular structure segmentation (TSS)에서 domain shift가 topological continuity를 파괴하는 문제를 
**Test-Time Adaptation (TTA)**으로 해결한 최초의 프레임워크.

### 두 단계 구성

**Stage 1 — Topological Meta Difference Convolutions (TopoMDCs)**:
- Pre-trained 파라미터를 변경하지 않으면서 topological representation을 강화
- 학습 없이 plug-and-play로 TSS 모델에 적용

**Stage 2 — Topology Hard sample Generation (TopoHG)**:
- Pseudo-break region 생성 → hard topological sample 생성
- 이 pseudo-hard sample에 대한 pseudo-label alignment로 topological continuity 회복

### 결과
4개 시나리오, 10개 데이터셋에서 평균 31.81% clDice 향상.
CNN 기반 TSS 모델들에 모두 적용 가능한 plug-and-play TTA.

---

## 내 연구와의 관계

### 직접 경쟁 관계는 아님 (TTA vs Training-time DG)

내 방법(Continuous-ONA): Training-time augmentation으로 domain shift에 견고한 모델 학습  
TopoTTA: Test-time에서 이미 domain shift된 상황을 topological adaptation으로 복구

### 상보적 관계 (Complementary)

"Continuous-ONA + TopoTTA" 조합 가능:
- 내 방법으로 훈련한 모델이 기본 topological robustness를 가짐
- 실제 배포 시 TopoTTA로 추가 topological 정확도 회복

### 개념적 연결

TopoTTA의 "topological hard sample" ↔ 내 "thin vessel = topologically critical fragile structure":
둘 다 topological continuity가 깨지기 쉬운 구조를 식별하고 특별 처리.

---

## 인용 계획

- Discussion/Future Work에서: "TopoTTA [ICCV 2025] demonstrates that topological distribution shifts in tubular structures can be addressed at test time. Our method is complementary, operating at training time to build robustness against appearance shifts that disproportionately affect thin, fragile structures."
