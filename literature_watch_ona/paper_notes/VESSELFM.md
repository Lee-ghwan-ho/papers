# VESSELFM — Paper Note

**제목**: vesselFM: A Foundation Model for Universal 3D Blood Vessel Segmentation  
**Venue**: CVPR 2025 (Poster)  
**arXiv**: 2411.17386  
**Authors**: Wittmann, Bastian; Wattenberg, Yannick; Amiranashvili, Tamaz; Shit, Suprosanna; Menze, Bjoern  
**GitHub**: https://github.com/bwittmann/vesselFM  
**Status**: Accepted Conference Paper

---

## 핵심 방법 요약

### 문제의식
- 3D 혈관 분할에서 imaging modality-specific variation (artifacts, vascular pattern, SNR, background) + domain gap으로 인한 일반화 문제
- 기존 supervised learning 방법은 unseen domain에 취약

### 3-소스 학습 전략

1. **실제 주석 데이터**: 17개 annotated blood vessel segmentation dataset 큐레이션
2. **Domain Randomization**: 3D 혈관 분할에 특화된 정교한 domain randomization scheme
3. **Flow Matching 생성 모델**: Mask- and class-conditioned flow matching으로 3D medical image 생성

### 핵심 성능
- 4가지 (pre-)clinically relevant imaging modality에서 zero-shot, one-shot, few-shot DG
- SOTA medical image segmentation foundation model 대비 우수

### 실험 데이터
- Brain MRA (TOF-MRA 포함), coronary CT angiography, retinal vessel, liver vessel 등 다양
- Zero-shot 조건에서도 강한 성능

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점
- 혈관(vessel) segmentation의 domain generalization을 다룸
- TOF-MRA cerebrovascular 관련 데이터 포함 실험

### 핵심 차이점

| 차원 | vesselFM | Continuous-ONA |
|------|----------|----------------|
| **Paradigm** | Foundation model (대규모 다중 데이터셋) | Single-source DG (단일 source dataset) |
| **Aug 조건** | Domain randomization (전체 영상 단위) | Local vessel radius 기반 continuous conditioning |
| **Thin vessel 처리** | 명시적 thin vessel 보호 없음 | Continuous aug budget으로 thin vessel 보호 |
| **데이터 요구사항** | 17개 dataset 필요 | Single source domain만 필요 |
| **Target domain 정보** | 불필요 (zero-shot) | 불필요 (SSDG) |

### Novelty 충돌 평가

**충돌 없음**

vesselFM은 대규모 multi-dataset training + flow matching generative model 기반 foundation model.
내 방법은 단일 source dataset에서 local vessel structure를 conditioner로 사용하는 augmentation.
방향과 데이터 요구사항이 근본적으로 다름.

vesselFM이 domain randomization을 사용하더라도, 이는 global/whole-image 단위 randomization이고
내 방법은 intra-image local vessel radius 기반 조건부 augmentation.

---

## 논문 작성 시 활용 방안

### Related Work에서의 위치
- "Foundation model / Large-scale pretraining for vessel DG" 계열에서 언급
- "SSDG(단일 source) 설정과 foundation model 설정의 차이"를 명확히 구분
- 내 방법은 clinical 현장에서 대규모 데이터가 없을 때도 작동하는 lightweight SSDG 접근

### 비교 관계
- vesselFM은 내 방법의 "upper bound" baseline으로 참고 가능
- 직접 비교하기에는 데이터 요구사항이 너무 다름

---

## 인용 가능성

- CVPR 2025 Accepted → 출판 확정, high-impact
- Related Work 인용: ✅ 권장 (vessel DG 최신 CVPR 논문)
- Comparison Baseline: △ (설정이 달라 직접 비교는 어렵지만 reference point로 언급 가능)
