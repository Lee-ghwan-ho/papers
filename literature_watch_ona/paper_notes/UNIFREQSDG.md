# UNIFREQSDG — Paper Note

**제목:** Universal Frequency Domain Perturbation for Single-Source Domain Generalization  
**Venue:** ACM International Conference on Multimedia 2024 (ACM MM '24)  
**DOI:** 10.1145/3664647.3681536  
**Status:** Accepted Conference Paper  
**Category:** A — 직접 경쟁  
**Relevance:** High  
**발견:** Run #8 (2026-06-06)

---

## 핵심 요약

UniFreqSDG는 단일 source domain에서 학습 가능한 frequency perturbation strength를 통해 out-of-distribution style을 시뮬레이션하는 SSDG framework다. **"aug strength를 자동 조절"이라는 방향에서 내 Continuous-ONA와 부분적으로 겹치므로 반드시 구분 논거를 확보해야 한다.**

---

## 방법론 상세

### 구성 요소

1. **LSP (Learnable Spectral Perturbation)**
   - 주파수 공간에서 LF 영역의 radius를 learnable parameter로 정의
   - Source image의 LF 구성요소를 해당 radius 내에서 Gaussian perturbation
   - **핵심**: radius는 per-image로 학습 → 이미지 전체에 동일한 frequency 변환 적용

2. **CPR (Content-Preserving Recombination)**
   - 증강 전 feature (content) + 증강 후 feature (augmented) decoupling
   - Decoupled content를 recombine하여 style은 바꾸되 content 구조는 보존
   - Contrastive learning 방식으로 domain-invariant representation 강화

3. **ADI (Active Domain-variance Inducement) Loss**
   - 주파수 공간에서 domain-style feature와 domain-invariant feature의 분리를 명시적으로 강제
   - Frequency-domain perturbation이 실제로 domain variance를 유발하도록 학습

### 실험 결과
- Fundus (optic disc/cup): average Dice 77.98% → 85.45% (+7.47%)
- Prostate (multi-center MRI): 71.42% → 76.73% (+4.99%)
- SLAug, RASS, ConStyX 등과 직접 비교

---

## 내 방법(Continuous-ONA)과의 비교

| 축 | UniFreqSDG | Continuous-ONA |
|---|-----------|---------------|
| 증강 공간 | Frequency domain (FFT) | Spatial/appearance domain (intensity) |
| 조절 단위 | Per-image (전체 이미지 단위 1개 radius) | Intra-image per-voxel (vessel structure별 continuous strength) |
| 조절 기준 | Learnable parameter (data-driven) | Local vessel radius / observability (anatomy-driven) |
| 구조 이질성 인식 | 없음 (전체 픽셀에 동일 frequency 변환) | 있음 (thin vessel ≠ thick vessel) |
| Thin vessel 보호 | 없음 | 있음 (약한 구조는 conservative perturbation) |
| 설정 | SSDG (fundus, prostate) | SSDG (TOF-MRA cerebrovascular) |

### 논문 내 구분 논거 문장 (draft)

> "UniFreqSDG learns a per-image scalar radius in frequency space to expand the source distribution uniformly. In contrast, our method conditions augmentation strength on the local observability of each vascular segment, providing spatially differential treatment within the same image — an intra-image structural awareness that is absent from existing frequency-based or per-sample approaches."

---

## 관련 논문 연결

- **ADA (MICCAI 2025)**: per-sample adaptive aug (Bezier remap) → image-level adaptive
- **UniFreqSDG**: per-image frequency perturbation radius → image-level adaptive
- **Continuous-ONA**: per-voxel vessel radius → intra-image structural adaptive
- **AG-TAL (arXiv 2604.27357)**: radius를 loss weighting에 활용 → 다른 mechanism

---

## 읽기 우선순위

**P1** — novelty 구분 논거 확보를 위해 LSP의 LF radius 학습 알고리즘 상세 확인 필요.  
특히: (1) radius는 어떻게 초기화되는가, (2) gradient를 통한 업데이트 방식, (3) per-sample adaptive인지 global인지.
