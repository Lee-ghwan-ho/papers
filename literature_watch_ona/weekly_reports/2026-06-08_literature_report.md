# Literature Watch Report — Run #8

> 날짜: 2026-06-08  
> 모델: claude-sonnet-4-6  
> 신규 논문: **3편** (Published Journal 1편 + Preprint 2편)

---

## 요약

이번 Run에서는 세 가지 신규 논문을 발견했다.

1. **TSIAA (IEEE TMI 2026)**: Instance-level adversarial augmentation for SDGMIS — "이미지 내 구조별 augmentation uniformity를 깨야 한다"는 동기를 명시적으로 선언한 **IEEE TMI 2026** 논문. 내 Continuous-ONA와 가장 유사한 동기를 가진 최신 경쟁 논문.

2. **WaveSDG (arXiv 2603.28463)**: Wavelet sub-band decomposition으로 anatomy와 appearance를 분리하는 fundus SSDG 방법.

3. **TopoVST (arXiv 2603.14909)**: Multi-scale sphere graph + GNN으로 vessel skeleton tracking + radius 동시 추정. 내 observability score 계산 파이프라인과 연결 가능성 있음.

**핵심**: TSIAA가 "uniformity 파괴"라는 동기를 공유하나, thin vessel protection과 radius-based conditioning은 없음. Continuous-ONA의 novelty gap 유지.

---

## Category A — 신규 직접경쟁 논문

### TSIAA — IEEE TMI 2026 ⚠️ P0 최고 주의

**논문**: Teacher-Student Instance-Level Adversarial Augmentation for Single Domain Generalized Medical Image Segmentation  
**저자**: Zhengshan Wang, Long Chen et al.  
**Venue**: IEEE Transactions on Medical Imaging  
**Year**: 2026  
**Status**: Published Journal Article  
**IEEE Xplore**: https://ieeexplore.ieee.org/document/11146907/  
**Code**: https://github.com/Wangzts0228/TSIAA

#### 방법 요약

**핵심 동기**: 기존 adversarial augmentation 방법은 image-level로 작동하여 이미지 내 다양한 구조(instance)에 균일한 augmentation rule을 적용한다. 이는 (1) over-augmentation 문제와 (2) 다양성 부족을 동시에 유발한다.

**IIAG (Instance-level Image Augmenter)**:
- 여러 IAM (Instance-level Augmentation Modules)로 구성
- 각 IAM = **learnable constrained Bézier transformation** function
- Instance (annotation region)별 독립적인 파라미터 학습
- 이미지 내 구조별 augmentation rule의 uniformity를 명시적으로 파괴

**Teacher-Student 프레임워크**:
- **Augmentation phase**: IIAG가 adversarial하게 out-of-source data 탐색
- **Generalization phase**: Student가 original + augmented에서 consistent representation 학습
- Teacher EMA → stable training

**실험**: Prostate T2-MRI SSDG (6 centers), Cardiac MRI cross-domain

#### 내 방법과의 관계

**공통 동기**: "이미지 내 구조별 augmentation uniformity를 깨야 한다"는 주장이 동일  
**방향 차이**: TSIAA = diversity 극대화 (adversarial), Continuous-ONA = thin vessel 보호 (conservative)  

| 항목 | TSIAA | Continuous-ONA |
|------|-------|----------------|
| conditioning signal | annotation mask region (instance) | vessel radius / observability (연속값) |
| 강도 조절 | 최대 다양성 탐색 | 관찰 가능성 낮을수록 보수적 |
| intra-class 세분화 | class 내 동일 강도 | thin/thick 연속 구분 |
| target | prostate/cardiac (blob organ) | cerebrovascular (tubular) |
| thin vessel 보호 | 없음 | 핵심 기여 |

**Novelty 위협도**: Medium-High  
**핵심 구분**: TSIAA는 "같은 이미지 내 다른 instance(foreground/background mask region)에 다른 augmentation을 적용"한다. 반면 Continuous-ONA는 "같은 foreground class 내에서도 vessel radius에 따라 연속적으로 다른 강도를 적용"하며, 이는 TSIAA가 다루지 않는 intra-class 이질성(thin vs. thick vessel)에 초점을 맞춘다.

---

## Category A — 신규 Preprint (방법론 유사)

### WaveSDG — arXiv 2603.28463 (March 2026)

**논문**: Decoupling Wavelet Sub-bands for Single Source Domain Generalization in Fundus Image Segmentation  
**저자**: Shramana Dey, Abhirup Banerjee, Varun Ajith, Sushmita Mitra  
**소속**: Indian Statistical Institute Kolkata + University of Oxford  
**Venue**: arXiv preprint  
**Year**: 2026  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2603.28463

#### 방법 요약

**핵심**: wavelet sub-band decomposition으로 encoder feature에서 anatomy structure와 domain-specific appearance를 분리

**WISER (Wavelet-based Invariant Structure Extraction and Refinement) 모듈**:
- Low-frequency sub-band → global anatomy 앵커 (invariant component)
- High-frequency sub-band → directional edge 강화 + noise 억제
- Encoder feature에 wavelet transform 적용 후 selective refinement

**실험**: Optic disc and cup SSDG (1 source + 5 unseen target datasets)

#### 내 방법과의 관계

- **approach**: frequency domain structure preservation (WaveSDG) vs. spatial radius-conditioned appearance perturbation (Continuous-ONA)
- **task**: fundus optic disc/cup vs. TOF-MRA cerebrovascular
- 겹침 없음 — 방법론적으로 orthogonal

---

## Category C — 신규 혈관 특화 논문

### TopoVST — arXiv 2603.14909 (March 2026)

**논문**: TopoVST: Toward Topology-fidelitous Vessel Skeleton Tracking  
**저자**: Yaoyu Liu, Minghui Zhang, Junjun He, Yun Gu  
**소속**: EndoluminalSurgicalVision-IMR (SJTU)  
**Venue**: arXiv preprint  
**Year**: 2026  
**Status**: Preprint Only  
**arXiv**: https://arxiv.org/abs/2603.14909  
**Code**: https://github.com/EndoluminalSurgicalVision-IMR/TopoVST

#### 방법 요약

**핵심 문제**: 혈관 skeleton의 topology-faithful delineation — frequent discontinuity 및 spurious segment 방지

**방법**:
- **Multi-scale sphere graphs**: 입력 이미지를 multi-scale로 샘플링
- **Graph Neural Networks**: tracking direction + **vessel radius를 동시 추정**
- **Gating-based feature fusion**: multi-scale representation 통합
- **Geometry-aware weighting scheme**: directional loss에서 class imbalance 처리
- **Wave-propagation-based skeleton tracking**: space-occupancy filtering으로 spurious skeleton 억제

#### 내 방법과의 관계

**직접 연결 가능성**: TopoVST의 vessel radius estimation pipeline을 내 observability score 계산의 preprocessing으로 활용 가능  
- TopoVST: radius estimation을 local geometric cue (sphere graph)에서 직접 추정
- Continuous-ONA: local radius를 skeleton distance transform으로 계산 (현재 구현)
- TopoVST의 방법이 더 정확한 radius map을 제공할 경우 내 augmentation conditioning 정확도 향상 가능

---

## Novelty Gap 재확인

이번 Run에서도 다음 키워드로 명시적으로 다룬 논문은 발견되지 않았다:

- "vessel observability conditioned augmentation"
- "radius-conditioned augmentation budget"
- "thin vessel appearance protection during augmentation"
- "intra-class structure-specific augmentation strength"

**TSIAA가 "uniformity 파괴" 동기를 명시한 최초의 IEEE TMI 논문**이라는 점은, 역설적으로 내 방법의 방향성이 옳음을 확인해준다. 단, TSIAA는 adversarial diversity 극대화에 집중하고, thin vessel 보호나 continuous radius conditioning은 없다.

---

## 미수록 검토 논문

| 논문 | arXiv | 미수록 이유 |
|------|-------|-------------|
| IELDG: Inverse Evolution Layers for DGSS | 2508.19604 | 자연영상 (city scene) DG, 의료영상 아님 |
| DG-TTA: SSC + GIN + TTA | 2312.06275 (Sensors 2025) | Low-tier venue, 낮은 방법론적 novelty |
| PCL: Pixel-level Counterfactual Contrastive | 2603.17110 | DG 설정 아님, domain robustness 초점 |
| PPAR: Prototypical Progressive Alignment | 2507.11955 | 자연영상 semantic DG, CLIP 기반 |
| CTL Survey: Causal Transfer Medical Imaging | 2603.24388 | Survey 논문, 직접 방법 없음 |

---

## 다음 Run 우선 탐색 항목

- [ ] TSIAA 전문 독해: IAM 내부 구현 (몇 개? foreground instance 정의? ablation?)
- [ ] TSIAA code 확인: https://github.com/Wangzts0228/TSIAA — IAM 적용 방식 상세
- [ ] CVPR 2026 accepted list 공개 시 즉시 탐색 (domain generalization, vessel, tubular)
- [ ] "instance-level" + "vessel" + "domain generalization" 재탐색 (TSIAA 이후 유사 논문)
- [ ] MICCAI 2026 early accept 발표 시 탐색 (예상: 2026-07-08 이후)
- [ ] WaveSDG venue 확정 여부 추적 (submitted 여부 확인)
- [ ] TopoVST venue 확정 여부 추적 (MICCAI/TMI submission 가능성)
