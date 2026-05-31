# READING QUEUE

> 우선순위 순으로 정렬. 아직 읽지 않은 논문 목록.  
> 읽은 뒤에는 paper_notes/PAPER_KEY.md 파일로 이동.

---

## P0 — 즉시 읽어야 할 논문 (내 novelty claim과 직접 충돌 가능)

| 우선순위 | KEY | 이유 |
|---------|-----|------|
| ★★★ | **ADA** | **Run #2 신규** Learnable Bezier Remap으로 per-sample adaptive aug. "aug 강도를 content에 따라 조절"이라는 아이디어 공유. 즉시 full text 확인 필수. MICCAI 2025 Paper 0315. |
| ★★★ | **MBFCV** | **Run #2 신규** Multi-Branch Feature Extractor로 혈관 두께 명시적 구분. 내 intra-class thickness conditioning과 방향 유사. MICCAI 2025 Paper 0782. |
| ★★★ | **SRCSM** | Semantic-aware RC는 label 단위로 다른 augmentation 적용. 내 방법이 "같은 class 내 다른 강도"임을 명확히 구분하기 위해 즉시 독해 필요. arXiv:2512.01510 |
| ★★★ | **CONSTYX** | Over-augmented feature suppression 개념. 내 방법의 "thin vessel 보호"와 개념적으로 유사할 수 있음. MICCAI 2025. |
| ★★★ | **SEMDIR** | Feature-space semantic direction augmentation. 내 방법과 동일한 문제의식(domain-specific vs anatomical feature 분리)을 다름. arXiv:2507.23326 |
| ★★★ | **FIESTA** | Uncertainty-guided augmentation strength 조절. 내 방법과 가장 개념 유사. 차이 명확히 파악 필요. arXiv:2406.14308 |
| ★★★ | **DGSSA** | Structural + Stylistic aug for retinal vessel. 혈관 구조 특성을 aug에 사용한 논문. 직접 유사 가능성. Neural Networks 2025. |
| ★★★ | **AGTA** | **Run #3 신규** Anatomy-Guided Texture Augmentation for cervical tumor SSDG. "texture를 무조건 파괴하면 안 된다"는 논지 → 내 "fragile structure 보호" 주장과 방향 유사. 구분점: AGTA는 tumor texture 보존 목적, 나는 thin vessel visibility 보호. MICCAI 2024 Workshop (CMMCA). |
| ★★★ | **ICRN** | **Run #3 신규** Invariant Content Representation + local style augmentation으로 foreground/background style을 분리 증강. 내 "class 내 구조 단위 분리 증강"과 level이 다르지만 "annotation 기반 region-specific aug"라는 개념 유사. IEEE TMI 2024. |

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
| ★★ | **GLCP** | **Run #2 신규** Global-to-Local Connectivity Preservation for Tubular. Local discontinuity 탐지. MICCAI 2025. |
| ★★ | **GFSA** | **Run #2 신규** Generative Feature Style Aug for DG in Medical Seg. VAE 기반 feature style generation. Pattern Recognition 2025. |
| ★★ | **SPOCKMIP** | **Run #2 신규** TOF-MRA vessel segmentation with MIP loss. 내 exact 데이터셋 도메인에서의 thin vessel continuity 문제 다룸. arXiv 2407.08655. |
| ★★ | **CRISP** | **Run #2 신규** Rank-Guided Iterative Squeezing for Robust Medical Seg. Model-agnostic DG, lung vessel CT 실험 포함. arXiv 2604.05409. |
| ★★ | **RANDDG** | **Run #3 신규** frequency-aware domain randomization for SSDG. GIN 기반 input-space aug + ULoFT feature-space perturbation. 내 baseline과 직접 비교 후보. Medical Physics 2025. |
| ★★ | **FREESDG** | **Run #3 신규** FreeSDG: frequency-mixed SSDG for medical seg. MICCAI 2023 foundational freq aug. ADA/ConStyX 등이 이를 baseline으로 사용하는지 확인 필요. MICCAI 2023. |
| ★★ | **ADAL** | **Run #3 신규** Adversarial SDG via Lyapunov Exponent-Guided Optimization. ICCV 2025. 자연영상이지만 single-domain DG에서 adversarial aug 강도 조절의 이론적 근거 제공. |
| ★★ | **LANGDAUG** | **Run #4 신규** LangDAug: Langevin Data Augmentation for Multi-Source DG. ICML 2025. Energy-Based Model + Langevin dynamics로 source domain 간 intermediate 샘플 생성. 의료영상 DG aug의 이론적 분석 참고 (Rademacher complexity 상한). |
| ★★ | **L2CP** | **Run #4 신규** Test-Time Training with Local Contrast-Preserving Copy-Pasted Image for Retinal Vessel DG. MICCAI 2025. **"thin vessel 구조를 morphological closing으로 제거"라는 아이디어를 DG에 명시적으로 사용**. 내 thin vessel 보호 동기와 직접 연결. 설정은 test-time (target 필요), 나는 training-time SSDG. |
| ★★ | **VESSELFM** | **Run #4 신규** vesselFM: Foundation Model for Universal 3D Blood Vessel Segmentation. CVPR 2025. Domain randomization + flow matching generative model로 zero-shot DG. TOF-MRA 포함 4가지 modality 실험. 3D vessel DG의 최신 CVPR 기준 논문. |

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
| ★ | **SPATIAL_TOPO** | **Run #2 신규** Spatial-Aware Persistent Feature Matching for topology. ICCV 2025 Workshop. |
| ★ | **CF_SEG** | **Run #2 신규** Counterfactual image generation for segmentation. MICCAI 2025. |
| ★ | **FMS2** | **Run #2 신규** Flow Matching for thin structure segmentation and synthesis. Cross-domain generalization. arXiv 2026. |
| ★ | **XDOMAINMIX** | **Run #3 신규** Cross-domain feature augmentation: class-specific vs domain-specific component swap. IJCAI 2024. feature-space DG aug의 이론적 decomposition 참고. |
| ★ | **UNIDDG** | **Run #3 신규** One Image as One Domain (OIOD). Content-style cross-batch recombination + EMA boundary attention. arXiv 2501.04741. "각 이미지를 독립 도메인으로"는 내 thin vessel 개념과 level이 다름 — 참고용. |
| ★ | **WFEX** | **Run #3 신규** Wavelet Frequency Exchange + Parametric Spline layers for label scarcity + domain shift. MICCAI 2025. 반지름 기반이 아닌 frequency 기반이지만 spline activation 참고 가능. |
| ★ | **DSUSNAKE** | **Run #3 신규** Dynamic Snake Upsampling + Boundary-Skeleton Weighted Loss. plug-and-play for tubular DG. arXiv 2505.08525. |
| ★ | **TTDG_MGM** | **Run #4 신규** Test-Time DG via Universe Learning + Multi-Graph Matching for medical seg. CVPR 2025. Morphological prior를 graph matching에 통합. Retinal fundus + polyp benchmark. |
| ★ | **GRAPHSEG** | **Run #4 신규** Generalizable Retina Vessel Segmentation with Deformable Graph Priors. NeurIPS 2025. Variational Bayesian + retinal atlas deformable graph prior + structure-preserved/degraded decomposition. CHASE/DRIVE/HRF. |

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
| BOUNDLESS | Adaptive Feature Blending + Dual Cross-Attention Regularization. arXiv 2024. |
| FDGP | Frequency federated DG for polyp. ICASSP 2025. |
| FPGM | Frequency Prior Guided Matching for polyp. arXiv 2025. |
| DRIPS | Domain randomisation for perivascular spaces. medRxiv 2025. |
| DCAM | Domain-invariant CAM for weakly-supervised DG. MICCAI 2025. weakly-supervised 설정, 간접 참고. |
| DPMFORMER | Language-driven DG (CLIP prompts + texture perturbation). ICCV 2025. 자연영상, 간접 참고. |
| SDFTOPONET | SDF pre-training + topology fine-tuning for tubular. arXiv 2503. 낮은 계산 비용 topology loss 참고. |
| BISDG | Bi-Level Optimization for SDG: inner task + outer generalization. arXiv 2604. 일반 DG이론 참고. |
| CLFA | Causality-inspired latent feature aug for SDG. arXiv 2406. feature-space causal intervention 참고. |
| DROPGEN | **Run #4 신규** Foundation model representation + source intensities for biomedical DG. arXiv 2604.02564. Architecture-agnostic, 3D biomedical seg. |
| VESSHAPE | **Run #4 신규** VessShape: shape bias via synthetic vessel dataset. arXiv 2510.27646. Few/zero-shot vessel DG. Shape-bias vs texture-bias 관련 참고. |
| SDAIRM | **Run #4 신규** Semantic Aug + Invariant Risk Minimization for medical DG. arXiv 2502.05593. Multi-source, classification 위주. 간접 참고. |
