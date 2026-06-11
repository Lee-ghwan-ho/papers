# TOPOVST — Paper Note

**제목:** TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking  
**arXiv:** 2603.14909 (March 2026)  
**저자:** Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu  
**Status:** Preprint Only  
**Category:** C (구조·혈관 특화)  
**Rel:** Medium (vessel radius 추정 개념 → ONA 연결)

---

## 핵심 방법

- **Multi-scale sphere graph** 구성: 입력 영상에서 다중 스케일 sphere 샘플링으로 그래프 노드 생성
- **GNN**: 추적 방향 + **vessel radius 동시 추정**
- **Wave-propagation skeleton tracking**: space-occupancy filtering으로 spurious skeleton 억제
- topological faithfulness 평가: overlapping + topological metrics 동시 최적화

---

## ONA 연구와의 관련성

### 왜 중요한가

TopoVST는 **vessel radius를 GNN으로 명시적으로 추정**한다. 이는 내 Continuous-ONA에서 사용하는 "local vessel radius / observability score" 개념과 직접 연결된다.

- ONA: GT annotation에서 distance transform으로 local vessel radius 계산 → augmentation budget 조절
- TopoVST: 영상에서 GNN으로 vessel radius 추정 → skeleton tracking에 활용

두 방법은 radius를 **다른 목적**으로 사용하지만, "vessel radius를 명시적으로 측정해야 한다"는 전제를 공유한다.

### 활용 가능성

1. **ONA 논문 introduction에서 동기 강화**: "vessel radius를 명시적으로 모델링하는 것이 topology tracking(TopoVST)에서도 핵심임이 입증됨"
2. **radius 추정 방법 참고**: TopoVST의 GNN 기반 radius 추정 → 내 방법에서 annotation-free radius 추정이 필요한 경우 참고

### 차이점

- TopoVST: **inference-time** tracking, radius를 구조 탐색에 활용
- ONA: **training-time** augmentation, radius를 appearance perturbation budget에 활용
- 설정: TopoVST = 단일 도메인 segmentation, ONA = SSDG (cross-domain)

---

## 결론

ONA novelty와 직접 충돌하지 않음. 오히려 "radius가 vessel 분석에서 핵심 물리량임"을 뒷받침하는 근거 논문으로 활용 가능.
