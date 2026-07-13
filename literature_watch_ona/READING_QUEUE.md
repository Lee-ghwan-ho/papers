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
| ★★★ | **COSTA** | **Run #5 신규** 8-center TOF-MRA multi-vendor dataset + CESAR network (style self-consistency loss). 내 실험 환경과 정확히 동일한 TOF-MRA 도메인 이질성 문제를 다룬 최신 IEEE TMI 2024 논문. 내 방법 평가에 COSTA 데이터셋 활용 가능성 확인 필요. |
| ★★★ | **AADG** | **Run #6 신규** Automatic Augmentation for DG on Retinal Image Segmentation (IEEE TMI 2022). Adversarial training + RL로 augmentation policy를 자동 탐색, Sinkhorn distance 기반 domain diversity proxy. 내 방법과 "augmentation 강도를 자동 조절"이라는 방향 유사 — 차이는 AADG = 전체 이미지 단위 policy search, 나 = intra-image vessel structure 단위 연속 조절. novelty 구분 필수. |
| ★★★ | **DCON** | **Run #7 신규** Hybrid Dual-Augmentation Constraint Framework for SSDG (Pattern Recognition 2025). Dual-view asymmetric augmentation: image-level(global-local stylized aug) + feature-level perturbation을 결합, bilevel contrastive learning으로 domain-invariant representation 학습. 내 방법과 "dual-level augmentation for SSDG"라는 방향이 일부 겹침. 핵심 차이: DCON = class-level feature/style diversity, 나 = intra-class vessel radius별 augmentation budget 연속 조절. 구분 논거 파악 필수. |
| ★★★ | **AG-TAL** | **Run #7 신규** Anatomically-Guided Topology-Aware Loss for CoW segmentation (arXiv 2604.27357, April 2026). **radius-aware Dice loss**: GT vascular radius를 localized weighting으로 활용하여 소혈관 집중. breakage-aware clDice (group convolution으로 효율적 topology 보존). 핵심: 내 ONA의 "vessel radius/observability 기반 차별 처리"와 동일한 radius 개념을 loss 설계에 적용한 논문. 내 augmentation 정당화에 활용 가능. 단, 목적은 loss weighting (not augmentation). |
| ★★★ | **TSIAA** | **Run #8 신규** Teacher–Student Instance-Level Adversarial Augmentation for SSDG (IEEE TMI 2026). Learnable Bezier 기반 Instance-level Augmentation Module로 **이미지 내 구조마다 다른 augmentation**을 적용한다는 상위 프레이밍이 내 핵심 주장("uniform augmentation budget is wrong")과 정면으로 겹침. Instance-level(discrete) + adversarially learned vs. 내 continuous radius-conditioned. 즉시 full text 확인 필수 (IEEE Xplore doc 11146907). paper_notes/TSIAA.md 작성 완료. |
| ★★★ | **AMAP** | **Run #8 신규** Anatomically-guided MAE with Domain-Adaptive Prompting for Cerebral Aneurysm Detection/Segmentation (npj Digital Medicine 2026). TOF-MRA-adjacent cerebrovascular 영역에서 augmentation이 아닌 foundation-model prompting으로 generalization을 달성하는 경쟁 패러다임. Related Work에서 "augmentation-based vs. prompting-based generalization" 대비축으로 활용 가능. |

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
| ★★ | **MIXSTYLEFLOW** | **Run #6 신규** MixStyleFlow: Domain Generalization using Normalizing Flows (MICCAI 2025). Normalizing flows로 feature style distribution 명시적 모델링 후 MixStyle과 결합. Prostate MRI + fundus. 내 방법과 직접 경쟁. 차이: feature-level uniform style mix vs. 내 pixel-level structure-conditioned appearance aug. |
| ★★ | **DAGMRI** | **Run #6 신규** Data-Agnostic Augmentations for Unknown Variations (MIDL 2025, arXiv 2505.10223). MixUp + Auxiliary Fourier Augmentation in nnU-Net for OOD MRI. 내 baseline 구성 참고 (MixUp aug 효과 평가). |
| ★★ | **ARFU** | **Run #7 신규** Anatomically-Robust and Feature-Unbiased DG for Medical Segmentation (Expert Systems with Applications 2025). SRG(shape regularization-guided aug) + APG(anatomical prior-guided aug) 조합, low-frequency 구조를 appearance transform의 regularizer로 사용. CT-MRI abdominal + cardiac MRI 실험. 내 방법과 유사점: low-freq 구조 보존 + augmentation controllability. 차이: ARFU = organ-level shape bias 방지, 나 = intra-vessel radius별 augmentation budget. |
| ★★ | **A3POINT** | **Run #8 신규** Adaptive Augmentation-Aware Latent Learning for Robust LiDAR Semantic Segmentation (ICLR 2026). Semantic confusion(무해) vs. semantic shift(유해)를 지역별로 구분해 다른 augmentation 처리를 적용. 자연영상/LiDAR이지만 "이미지 내 위치마다 augmentation 안전성이 다르다"는 이론적 근거로 인용 가치 높음. |
| ★★ | **SRA** | **Run #8 신규** Sample-Aware RandAugment: Search-Free Automatic Data Augmentation (IJCV 2025). Per-sample augmentation 강도를 sample complexity 기반으로 동적 조절. "augmentation 강도를 continuous하게 조절"하는 general-vision 선례로 인용 가치 높음. 단, whole-image 단위(per-sample)이며 intra-image 구조 단위 아님. |
| ★★ | **LOCALGAMMA** | **Run #8 신규** Local Gamma Augmentation for Ischemic Stroke Lesion Segmentation (NLDL 2024). Binary lesion mask로 제한된 영역에만 gamma augmentation 적용 — "전체 이미지 균일 증강은 문제"라는 정성적 선례. Continuous 아님, radius 기반 아님. Related Work 필수 인용 후보. |
| ★★ | **MORVESS** | **Run #8 신규** MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network (arXiv 2606.24214). Vessel mask + distance map + **thickness map**을 joint 예측하는 auxiliary supervision. 내 radius/observability 계산과 가장 근접한 사례지만, thickness는 augmentation이 아닌 loss/supervision target으로만 사용됨. |
| ★★ | **AC2RUNET** | **Run #8 신규** Anatomically Conditioned Recurrent Refinement for Topology-Aware CoW Segmentation (arXiv 2606.12319). Static/Dynamic Stream 분리 + coarse-to-topology curriculum. Training-time curriculum이 내 augmentation strength conditioning과 결이 유사(단, spatial이 아닌 temporal 축). |

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
| ★ | **OVS_NET** | **Run #5 신규** Dual-branch for small vessel enhancement + morphology-aware correction module (topology/connectivity). IEEE TIP 2025. "segmentation algorithms optimized for overlap scores overlook small/fragile structures"라는 정확히 내 동기와 맞닿는 진술 포함. arXiv 2411.15251. |
| ★ | **DOMAIN_GAME** | **Run #5 신규** Geometric transformation sensitivity로 anatomical vs domain-specific feature 분리. MICCAI 2024 Workshop (CMMCA). 내 방법과 feature space 분리 방향이 다르지만 AGTA와 같은 workshop volume에 실린 경쟁 논문. arXiv 2406.02125. |
| ★ | **VESSELSIM** | **Run #6 신규** VesselSim: 3D blood vessel segmentation without expert annotations (arXiv 2605.26277, May 2026). Stochastic geometry-driven vascular simulation + domain-randomized intensity synthesis. 16,500 synthetic 3D volumes. vesselFM와 경쟁. 합성 데이터 기반 DG의 최신 사례 — domain randomization scheme 상세 확인 필요. |
| ★ | **DOMAINFLOW** | **Run #8 신규** DomainFlow: SSDG for Coronary Vessels Segmentation in X-Ray Angiography (STACOM 2024 workshop). Binary mask 대신 connectivity/distance-style mask 예측으로 topology 보존. AngioDG의 전신 격 논문. |
| ★ | **WAVESDG** | **Run #8 신규** Decoupling Wavelet Sub-bands for SSDG in Fundus Image Segmentation (arXiv 2603.28463). WISER module로 저주파(구조)/고주파(도메인 외형) 분리. Fundus vessel SSDG 직접 경쟁. |
| ★ | **VESSELSDF** | **Run #8 신규** VesselSDF: Distance Field Priors for Vascular Network Reconstruction (MICCAI 2025). Binary voxel classification 대신 continuous SDF regression. Topology 표현의 대안적 접근. |
| ★ | **PASCNET** | **Run #8 신규** PASC-Net: Shape Self-learning Convolutions + Hierarchical Topology Constraints (Biomedical Signal Processing and Control 2025). Tubular shape에 적응된 strip convolution + 계층적 topology 제약. |
| ★ | **TOPOSCULPT** | **Run #8 신규** TopoSculpt: Betti-Steered Topological Sculpting of 3D Tubular Shapes (arXiv 2509.03938). Airway/CoW/coronary 3개 tubular domain 교차 검증. |
| ★ | **SEMIR** | **Run #8 신규** Topology-Preserving Graph Minors for Thin-Structure Segmentation (ECCV 2026). Pixel lattice 대신 graph-minor 표현으로 full-resolution thin structure inference. |
| ★ | **CONTEXTLOSS** | **Run #8 신규** ContextLoss: Context Information for Topology-Preserving Segmentation (ICIP 2025). 주변 context를 포함한 topology error 계산으로 connectivity 복구율 향상. |
| ★ | **GRAPHMORPH** | **Run #8 신규** GraphMorph: Tubular Structure Extraction by Morphing Predicted Graphs (NeurIPS 2024). Branch-level graph decoder + SkeletonDijkstra. |
| ★ | **DEFORMCL** | **Run #8 신규** DeformCL: Learning Deformable Centerline Representation for Vessel Extraction (CVPR 2025). Voxel mask 대신 continuous deformable centerline 표현. |
| ★ | **COROTOPO3STAGE** | **Run #8 신규** Topology-Preserving Three-Stage Framework for Coronary Artery Extraction (Medical Image Analysis 2025). Thin distal vessel reconnection에 초점. |
| ★ | **FLATMIN_AUG** | **Run #8 신규** A Flat Minima Perspective on Understanding Augmentations and Model Robustness (AAAI 2026). Label-preserving augmentation이 robustness를 개선하는 이유를 flatness/PAC bound로 이론화. 강한 augmentation이 약한 evidence 구조에서 역효과를 낼 수 있다는 내 동기의 이론적 뒷받침으로 활용 가능. |
| ★ | **PDAF** | **Run #8 신규** Exploring Probabilistic Modeling Beyond DG for Semantic Segmentation (ICCV 2025). Diffusion 기반 latent domain prior로 test-time feature alignment. Input-level augmentation과 상호보완적 대안. |
| ★ | **PAPT_SDG** | **Run #8 신규** Adversarial Domain Prompt Tuning and Generation for Single Domain Generalization (CVPR 2025). T2I diffusion 기반 학습된 augmentation policy(invariant-content vs. style prompt 분리). |

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
| AMAP | **Run #8 신규** Cerebral aneurysm MAE + domain-adaptive prompting. npj Digital Medicine 2026. Prompting 기반 경쟁 패러다임, discussion 참고용. |
| ROBUSTWT | **Run #8 신규** Whitening + curriculum loss weighting for fundus optic-disc DG. arXiv 2606.03069. Augmentation은 uniform, 간접 참고. |
| ROBUSTSURG | **Run #8 신규** Surgical scene OOD DG. arXiv 2512.02188. Style/content 분리, 도메인 거리 있음. |
| SHORTCUTKD | **Run #8 신규** Intermediate layer KD로 shortcut 억제. arXiv 2511.17421. Augmentation 대안 mechanism, 간접 비교 참고. |
| BEZDIFF | **Run #8 신규** Bézier + diffusion UDA style transfer. arXiv 2509.22476. Target data 필요(UDA), SSDG 아님. |
| CSWINUNETR | **Run #8 신규** Thin anatomical structure backbone (cross-shaped stripe attention). arXiv 2606.19824. Architecture-only, DG 아님. |
| TOPOLORASAM | **Run #8 신규** SAM LoRA 기반 thin-structure + cross-domain adaptation. arXiv 2601.02273. |
| COWCENTERLINEGRAPH | **Run #8 신규** CoW centerline graph 데이터셋 (segment radius/length morphometrics 포함). arXiv 2510.13720. Radius 기반 evaluation resource로 활용 가능. |
| EBIL_HADS | **Run #8 신규** Evidential Bi-Level Hardest Domain Scheduler. NeurIPS 2024. Training-time domain difficulty scheduling, 참고용. |
