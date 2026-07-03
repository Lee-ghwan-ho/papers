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
| ★★★ | **TSIAA** | **Run #8 신규** Teacher–Student Instance-Level Adversarial Augmentation for SDG Medical Segmentation (IEEE TMI 2026, Published Journal Article). Instance-level Image Augmenter가 **이미지 내 서로 다른 구조마다 다른 learnable Bézier augmentation parameter**를 adversarial teacher-student loop로 학습 — "breaks the uniformity of augmentation rules across different structures within an image"라는 문구가 내 핵심 문제의식과 거의 동일. 현재까지 발견된 논문 중 가장 강한 novelty 충돌 후보. 즉시 전문 확인 필수: (1) instance-level 강도가 anatomical/geometric signal(radius 등)로 conditioning되는지, 아니면 순수 adversarial/discrete instance 단위인지 확인. |
| ★★★ | **IPFRDA** | **Run #8 신규** IPF-RDA: Information-Preserving Framework for Robust Data Augmentation (arXiv 2509.16678, TPAMI 심사 중, Preprint Only). 일반 vision(classification/re-ID) 대상이지만, **local region의 class-discriminative "importance score"를 계산해 augmentation 강도를 연속적으로 modulation**하는 mechanism이 Continuous-ONA와 구조적으로 가장 유사한 general-vision 논문. 차이 정리 필수: (1) segmentation이 아닌 classification/re-ID, (2) importance signal이 vessel radius 같은 anatomical proxy가 아니라 learned discriminativeness. |
| ★★★ | **DASA** | **Run #8 신규** Difficulty-Aware Sample Allocation for Adaptive Data Augmentation in Semantic Segmentation (Research Square, 2026, Preprint Only, 동료평가 전). Segmentation에서 **연속적인 difficulty score(prediction ambiguity + loss + class rarity + boundary complexity) → 연속적인 augmentation 강도**를 매핑하는 mechanism이 "continuous conditioning of augmentation strength in segmentation"이라는 점에서 가장 직접적인 메커니즘 유사 후보. 다만 sample-level(전체 이미지 단위) 조건화이고 anatomical radius 신호 없음. 미검증 preprint이므로 최상위 추천에는 포함하지 않되, 정확한 차별화 논거 확보를 위해 전문 확인 필요. |

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
| ★★ | **MORVESS** | **Run #8 신규** MorVess: Morphology-Aware Pulmonary Vessel Segmentation Network (arXiv 2606.24214, June 2026, Preprint Only). Vessel mask + distance map + **thickness map**을 jointly 예측하여 vascular boundary/centerline consistency/smooth diameter transition을 명시적으로 supervise. DG 논문은 아니지만 "local vessel thickness를 first-class training signal로 사용"하는 최신 사례 — 내 ONA의 "radius를 augmentation 신호로 사용"과 대조되는 "radius를 supervision 신호로 사용" 사례로 related work에서 명시적으로 대비 서술 가능. |
| ★★ | **SEMIR** | **Run #8 신규** SEMIR: Topology-Preserving Graph Minors for Thin-Structure Segmentation (ECCV 2026, Accepted Conference Paper). Pixel lattice 대신 thin-structure connectivity를 보존하는 parameterized graph minor 표현 사용, power line/crack/lane marking 등 cross-domain thin-structure에서 단일 파이프라인 검증. Tubular/thin-structure의 cross-domain generalization을 다루는 최신 top-tier venue 논문. Loss/representation 관점의 topology 보존이며 augmentation 강도 조절과는 무관 — Category C 참고문헌으로 우선순위. |
| ★★ | **SENSAUG** | **Run #8 신규** Adaptive Sensitivity Analysis for Robust Augmentation against Natural Corruptions (ICML 2025, Accepted Conference Paper, arXiv 2406.01425). Warmup 후 model sensitivity를 빠르게 측정해 corruption severity별 학습 가중치를 결정하는 model-free augmentation-severity policy. "모델 민감도가 augmentation 강도를 결정한다"는 메타 원칙이 Continuous-ONA와 같은 방향이나, 조건화 신호가 anatomical structure가 아닌 global per-corruption-type sensitivity. Ablation 설계 시 참고: radius-conditioning이 실제로 thin-vessel sensitivity와 상관관계가 있는지 SensAug 스타일 sensitivity probe로 검증하는 방법론 차용 가능. |
| ★★ | **FSDADG** | **Run #8 신규** FSDA-DG: Cross-Domain Generalizability with Few Source Domain Annotations (Medical Image Analysis 2025, arXiv 2311.02583). 이미지를 "global broad region"과 "semantics-guided local region"으로 나누어 서로 다른 augmentation을 적용하는 binary 2-tier 방식. 내 continuous radius conditioning과 대조되는 "binary region-split augmentation" 사례로 즉시 구분 논거 정리 필요 — ICRN, AGTA와 함께 "binary FG/BG 또는 2-tier augmentation" 계열로 묶어서 비교. |
| ★★ | **EISEG** | **Run #8 신규** Explicable Intensity-Aware 3D Cerebrovascular Segmentation with Planar Representation (Medical Image Analysis 2026). TOF-MRA cerebrovascular segmentation에서 MIP 기반 tri-plane 표현으로 효율적 3D segmentation. DG 논문은 아니지만 내 실험 도메인(TOF-MRA cerebrovascular)과 정확히 일치하는 최신 MedIA 논문 — 데이터셋/평가 프로토콜 참고용으로 확인 필요. |
| ★★ | **LIVASNET** | **Run #8 신규** LIVAS-Net: Parameter-Efficient 3D Architecture for Intracranial Artery Segmentation in TOF-MRA (Electronics/MDPI 2026). 동일 modality(TOF-MRA) + 동일 해부 구조(intracranial artery) 아키텍처 논문. DG 관련성은 낮으나 baseline/평가 프로토콜 비교 후보로 확인. |
| ★★ | **MAMBASEA** | **Run #8 신규** Mamba-Sea: Global-to-Local Sequence Augmentation for Generalizable Medical Segmentation (IEEE TMI, early access 2025/2026, arXiv 2504.17515). Global appearance-variation augmentation + local sequence-wise(Mamba token) style transformation의 2-tier 구조. 내 continuous conditioning과 달리 coarse 2-tier(global/local) 구분 — "granularity of augmentation" 논의에서 대조군으로 인용 가능. |

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
| ★ | **WISER** | **Run #8 신규** Decoupling Wavelet Sub-bands for SSDG Fundus Segmentation (arXiv 2603.28463, 2026). Wavelet 분해로 anatomical structure(LL)와 domain-specific appearance(LH/HL/HH) 분리하는 SSDG vessel-adjacent 방법. Frequency-domain 계열 baseline 후보. |
| ★ | **SDCL** | **Run #8 신규** Causal Inference via Style Bias Deconfounding (arXiv 2503.16852, 2025). Style-guided expert 모듈 + back-door causal intervention. Causality_SDG/SI2CRL 계열 후속 연구로 확인 필요. |
| ★ | **ADDGCL** | **Run #8 신규** Adaptive Disentangled DG Collaborative Learning (Neurocomputing 2025). Small organ에 대해 pixel frequency 기반 adaptive region-specific **loss weighting**(augmentation 아님). "작은 구조는 다른 취급이 필요하다"는 동기를 loss 관점에서 다룬 사례 — AG-TAL과 함께 "radius/size-aware loss weighting" 계열로 묶어 인용. |
| ★ | **RLAD** | **Run #8 신규** Layout-Aware Generative Modelling for Retinal Vessel DG (arXiv 2503.01190, 2025). Diffusion 기반 vessel/lesion/disc layout-conditioned image generation. 생성 기반 vessel DG 계열 baseline 후보. |
| ★ | **MARVEL** | **Run #8 신규** Murray's Law-informed Vessel Tree Segmentation and Topology Estimation (arXiv 2605.25363, 2026). Branch-point radius 관계를 biophysical topology prior로 사용. 내 continuous augmentation과 다른 mechanism(topology 제약)이나 "vessel radius를 명시적 신호로 사용"하는 최신 사례로 related work 열거에 포함. |
| ★ | **VESSELTOK** | **Run #8 신규** VesselTok: Tokenizing Vessel-like 3D Biomedical Graphs (arXiv 2603.18797, 2026). Centerline + pseudo-radius를 latent token으로 인코딩하는 generative 표현. Radius-aware representation 사례. |
| ★ | **TOPOGUARANTEE** | **Run #8 신규** Topology-Guaranteed Segmentation: Connectivity, Genus, Width Constraints (SIAM J. Imaging Sciences 2026). Local width/thickness를 hard constraint로 segmentation에 통합하는 변분법적 프레임워크. |
| ★ | **COROBENCH** | **Run #8 신규** Clinically-Informed Benchmark for Topology-Aware Coronary Artery Segmentation (MICCAI 2026). Branch-vessel topology 평가를 위한 새 벤치마크 — 평가 프로토콜 설계 참고용. |
| ★ | **CORO3STAGE** | **Run #8 신규** Topology-Preserving Three-Stage Framework for Coronary Artery Extraction (Medical Image Analysis 2025, arXiv 2504.01597). Centerline-enhanced loss → reconnection → missing-vessel reconstruction. Thin distal vessel 문제를 다루는 최신 MedIA 논문. |
| ★ | **FGOSNET** | **Run #8 신규** Frequency-Aware Anisotropic Serialization for Thin-Structure SSMs (arXiv 2603.28503, 2026). Mamba류 SSM의 isotropic scan이 thin structure를 단절시키는 문제를 주파수-기하 분해로 해결. Retinal vessel + road 교차 도메인 검증. |
| ★ | **SRANDAUG** | **Run #8 신규** Sample-aware RandAugment (IJCV 2025, arXiv 2508.08004). Search-free per-sample augmentation policy — sample complexity score로 augmentation 강도를 조절하는 general-vision 사례. Whole-image classification 수준, segmentation/intra-class 아님. |
| ★ | **FLATMIN** | **Run #8 신규** A Flat Minima Perspective on Understanding Augmentations and Model Robustness (AAAI 2026, arXiv 2505.24592). Label-preserving augmentation이 왜 distribution shift robustness를 개선하는지에 대한 이론적 프레임워크(loss landscape flattening). Continuous-ONA의 "왜 thin vessel에 강한 aug를 주면 안 되는가"에 대한 이론적 근거로 활용 가능 (motivation/theory 섹션 인용 후보). |
| ★ | **GEOINVLEARN** | **Run #8 신규** The Geometry of Invariant Learning (arXiv 2602.14423, 2026). Augmentation의 generalization gap을 divergence/stability/sensitivity term으로 분해하는 정보이론적 프레임워크. Theory 섹션 인용 후보. |

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
| CQINV | **Run #8 신규** Color-Quality Invariance for Robust Medical Image Segmentation. arXiv 2502.07200. Global color/quality normalization, structure-conditioning 없음. |
| GENEVAL | **Run #8 신규** Human Knowledge Integrated Multi-modal Learning for SDG (WACV 2026). Classification 위주(당뇨망막병증 grading), segmentation 아님. |
| NEUROVASCU | **Run #8 신규** NeuroVascU-Net: Cross-Domain Brain Vessel Segmentation (T1CE MRI, arXiv 2511.18422). Architecture 기반 cross-domain, augmentation 방법 아님. |
| PMDG | **Run #8 신규** Pseudo Multi-Source Domain Generalization. arXiv 2505.23173. Single source에서 style transfer로 pseudo-domain 생성 후 multi-source DG 기법 재사용. |
| FGMLDG | **Run #8 신규** FGML-DG: Feynman-Inspired Cognitive Science Paradigm for Cross-Domain Medical Segmentation. arXiv 2604.10524. Meta-learning style-memory 프레임워크. |
| CONDISR | **Run #8 신규** ConDiSR: Contrastive Disentanglement and Style Regularization for SDG (WACV 2025). Medical image **classification**, segmentation 아님. |
| PULMTREE | **Run #8 신규** Topology-Aware Implicit Field for Pulmonary Tree Modeling with Incomplete Supervision. arXiv 2602.02186. Implicit representation, radius conditioning 아님. |
| AIRWAYMULTI | **Run #8 신규** Multiscope Topology Learning with Conditional Updating for Airway Segmentation (Pattern Analysis and Applications 2025). Multi-task topology learning, augmentation과 무관. |
| SADA | **Run #8 신규** On-the-Fly Augmentation via Gradient-Guided Sample-Aware Influence Estimation. arXiv 2510.00434. Training-dynamics 기반 sample-level augmentation 강도 조절 — anatomical signal 없음. |
| A3MDA | **Run #8 신규** Adaptive Hardness-driven Augmentation for Multi-Source Domain Adaptation. arXiv 2501.01142. UDA 설정(target 필요), sample-level hardness. |
