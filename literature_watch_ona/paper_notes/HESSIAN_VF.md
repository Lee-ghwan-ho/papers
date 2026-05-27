# Domain Generalization for Retinal Vessel Segmentation via Hessian-based Vector Field

**KEY**: HESSIAN_VF  
**Category**: C (혈관 특화)  
**Status**: Published Journal Article  
**Venue**: Medical Image Analysis (April 2024)  
**PMC**: PMC11756701  
**ScienceDirect**: https://www.sciencedirect.com/science/article/abs/pii/S1361841524000896  

---

## 핵심 요약

혈관 분할의 domain generalization을 위해 **Hessian 행렬의 secondary eigenvector field**를
domain-invariant feature로 사용.

### 기술적 접근
- Hessian 행렬의 고유벡터: 혈관 방향과 동일한 방향의 벡터 필드 생성
- 혈관 내부의 벡터들이 혈관 방향을 따라 균일하게 정렬됨 (blood flow 모사)
- Scalar vesselness (Frangi filter 기반)가 아닌 **vector field** 표현 → 공간 방향 정보 포함
- Vision Transformer의 self-attention과 결합하여 multi-scale local feature 강조

### 평가
여러 modality (fundus, OCT angiography)의 public dataset에서 SOTA 일반화 성능.

---

## 내 연구와의 관계

### 내 observability 계산의 기술적 기반

내 방법에서 local vessel radius / observability score를 계산하기 위해
Hessian 기반 수치를 사용할 수 있음. 이 논문은 그 **기술적 정당성을 제공**:

1. **Hessian eigenvalues** → vesselness measure (Frangi filter 원리)
2. **Hessian 기반 vessel radius** → local tube radius 추정 가능
3. **Scale-space analysis** → 다양한 크기의 혈관에 대한 vesselness 동시 계산

### 차이점

| 구분 | Hessian VF | Continuous-ONA |
|------|-----------|----------------|
| Hessian 사용 목적 | Domain-invariant feature representation | Augmentation budget 계산 (observability score) |
| 출력 | Vector field (backbone input) | Scalar observability map (aug conditioning) |
| 적용 단계 | Feature extraction | Data augmentation |

### 인용 계획

**방법론 섹션**: "We compute local vessel radius using Hessian-based scale-space analysis [HESSIAN_VF, Frangi et al.], which provides a reliable geometric estimate of vessel structure independent of image appearance."

---

## 관련 논문 연결

- **VesselMorph**: 동일하게 Hessian-based tensor field 사용 (bipolar tensor field)
- **Frangi Filter** (MICCAI 1998): 이 두 논문 모두의 기반. 내 observability 계산의 foundational reference.
