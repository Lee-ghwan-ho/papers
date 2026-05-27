# READING QUEUE

> 우선순위 순으로 정렬. 아직 읽지 않은 논문 목록.  
> 읽은 뒤에는 paper_notes/PAPER_KEY.md 파일로 이동.

---

## P0 — 즉시 읽어야 할 논문 (내 novelty claim과 직접 충돌 가능)

| 우선순위 | KEY | 이유 |
|---------|-----|------|
| ★★★ | **SRCSM** | Semantic-aware RC는 label 단위로 다른 augmentation 적용. 내 방법이 "같은 class 내 다른 강도"임을 명확히 구분하기 위해 즉시 독해 필요. arXiv:2512.01510 |
| ★★★ | **CONSTYX** | Over-augmented feature suppression 개념. 내 방법의 "thin vessel 보호"와 개념적으로 유사할 수 있음. MICCAI 2025. |
| ★★★ | **SEMDIR** | Feature-space semantic direction augmentation. 내 방법과 동일한 문제의식(domain-specific vs anatomical feature 분리)을 다름. arXiv:2507.23326 |
| ★★★ | **FIESTA** | Uncertainty-guided augmentation strength 조절. 내 방법과 가장 개념 유사. 차이 명확히 파악 필요. arXiv:2406.14308 |
| ★★★ | **DGSSA** | Structural + Stylistic aug for retinal vessel. 혈관 구조 특성을 aug에 사용한 논문. 직접 유사 가능성. Neural Networks 2025. |

---

## P1 — 높은 우선순위 (기준선 및 배경 이해)

| 우선순위 | KEY | 이유 |
|---------|-----|------|
| ★★ | **ANGIODG** | Vessel segmentation SSDG 직접 경쟁. Channel-informed feature reweighting. arXiv:2511.17724 |
| ★★ | **STYCONA** | Content+Style decomposition aug. 유사 구조 포함. arXiv:2502.20619 |
| ★★ | **HESSIAN_VF** | Hessian-based vessel DG. 내 observability 계산 근거로 사용 가능. MedIA 2024. |
| ★★ | **SKELRECALL** | Thin tubular connectivity recall. 내 thin vessel 보호 동기와 직접 연결. ECCV 2024. |
| ★★ | **CBDICE** | Centerline + Boundary Dice. clDice의 large vessel 편향 해결이 내 동기와 연결. MICCAI 2024. |
| ★★ | **MULTIDOMAIN_BRAIN** | TOF-MRA 포함 brain vessel multi-domain DG. 직접적인 경쟁 데이터셋. MELBA 2025. |
| ★★ | **TOPOTTA** | TTA for tubular topology. 내 방법 이후 적용 가능성. ICCV 2025. |
| ★★ | **HARMONYSEG** | Growth-suppression balanced loss for tubular. ICCV 2025. |

---

## P2 — 중간 우선순위 (방법 구현 참고)

| 우선순위 | KEY | 이유 |
|---------|-----|------|
| ★ | **RASS** | Frequency-based SSDG augmentation. 비교 baseline 후보. MICCAI 2024. |
| ★ | **MORESTYLE** | MoreStyle plug-and-play Fourier aug. 비교 baseline 후보. MICCAI 2024. |
| ★ | **RAFFESDG** | Random freq filtering SSDG. 비교 baseline 후보. arXiv 2024. |
| ★ | **WAVERNETV** | Wavelet multi-source vessel DG. arXiv 2026. |
| ★ | **FRACTAL_FFM** | Fractal feature maps for tubular. ECCV 2024. |
| ★ | **DEEPCLOSING** | Topology connectivity for tubular. IEEE TMI 2024. |
| ★ | **SI2CRL** | Spectrum intervention causal SSDG. MedIA 2025. |
| ★ | **GENORDETECT** | Generalize or Detect NeurIPS 2024. 증강 기반 OOD+DG. |
| ★ | **FLOWAXIS** | Continuous vessel parameterization. npj Digital Medicine 2026. |

---

## P3 — 낮은 우선순위 / 참고용

| KEY | 이유 |
|-----|------|
| DET2PROB | Probabilistic modeling for DG. 간접 참고. |
| MCDRL | Multimodal causal DG. 간접 참고. |
| INVCAUSAL | Cross-modality causal mechanisms. |
| SAM_SDG | SAM 기반 SSDG. |
| SDG_REALWORLD | 실제 배포 경험 사례. |
| SHAPEBIAS | Shape bias evaluation. |
| SHAPEBIAS_CNN | Frequency 기반 shape bias. |
| TOPUNCERT | DMT 기반 topology uncertainty. |
| DYNSNAKE | Dynamic snake convolution ICCV 2023. |
| HALLUDG | Hallucinated DG network. |
| DYNSDG | Dynamic DG. |
| BIRF_SDG | Band importance freq filter. |
| ISAC | Vascular mask completion cross-domain. |
| CLCE | Centerline Cross-Entropy loss. |
| VESSELMORPH | Shape-aware vessel DG. (이미 알고 있는 기준 논문) |
| CLDICE | clDice topology loss. (이미 알고 있는 기준 논문) |
| DAPSAM | Domain-adaptive SAM prompt. |
| INTRA_STYLE | Intra-source style aug. |
