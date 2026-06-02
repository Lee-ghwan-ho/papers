# MIXSTYLEFLOW — Paper Note

**KEY**: MIXSTYLEFLOW  
**제목**: MixStyleFlow: Domain Generalization in Medical Image Segmentation using Normalizing Flows  
**Venue**: MICCAI 2025, Paper 0571-Paper3460  
**URL**: https://papers.miccai.org/miccai-2025/0571-Paper3460.html  
**발견**: Run #6 (2026-06-02)  
**우선순위**: P1

---

## 한 줄 요약

Normalizing flows로 도메인 feature style 분포를 명시적으로 학습하고, 원본 feature statistics와 mix하여 unseen domain style을 체계적으로 생성하는 DG 방법.

---

## 핵심 방법

### 기존 MixStyle의 한계
- MixStyle은 training batch 내 두 instance의 feature statistics (mean, std)를 확률적으로 swap
- 실제 domain 분포 범위를 벗어난 스타일은 생성 불가 → 제한적 다양성

### MixStyleFlow의 혁신
1. **Normalizing Flow 도입**: feature channel별 style (mean, std)의 **분포**를 명시적으로 학습
   - Flow는 invertible transformation으로 복잡한 분포를 tractable 분포로 변환
2. **Style Sampling**: 학습된 flow에서 새로운 스타일 샘플링 → unseen domain style 생성 가능
3. **Mixing**: 샘플링된 스타일과 원본 feature statistics를 feature channel dimension 따라 mix
4. **차이**: MixStyle은 instance pair에서만 sampling, MixStyleFlow는 학습된 flow distribution 전체에서 sampling

### 평가 도메인
- Prostate MRI cross-site (NCI + ISBI 2013 + PROMISE12)
- Fundus optic disc/cup segmentation (RIGA dataset)

---

## 내 Continuous-ONA와의 비교

| 측면 | MixStyleFlow | Continuous-ONA |
|------|--------------|----------------|
| Aug 단위 | feature map 전체 (channel-wise statistics) | pixel-level, structure-conditioned |
| 구조 인식 | 없음 (이미지 전체 동일 style) | vessel observability에 따라 다른 강도 |
| 생성 방식 | Normalizing flow → feature statistics 샘플링 | Bézier/spline nonlinear intensity mapping |
| Thin vessel 보호 | 없음 | 핵심 기여 |
| 도메인 | Prostate MRI + Fundus (2D) | TOF-MRA (3D, SSDG) |

**핵심 구분**: MixStyleFlow는 더 넓은 feature style space를 탐색하지만, 모든 spatial location에 uniform한 style을 적용. Continuous-ONA는 style 종류를 바꾸는 것이 아니라 **구조마다 다른 aug 강도**를 부여해 thin vessel의 evidence를 보존.

---

## 내 논문에서의 활용

- Related work에서 "feature-level style diversification" 계열로 분류 가능
- 내 방법과 orthogonal: MixStyleFlow의 style generation + 내 structure-conditioned strength 조절을 결합하는 future work 방향 제시 가능
- Baseline 후보: TOF-MRA SSDG 설정에 MixStyleFlow 적용 시 성능 비교 (단, prostate/fundus 특화이므로 직접 적용에 한계 있을 수 있음)

---

## 읽어야 할 내용

- [ ] Section 3: Normalizing flow 구체적 구조 (어떤 flow 사용?)
- [ ] Table 1/2: Prostate MRI cross-site 정량 결과
- [ ] Ablation: Flow 사용 여부에 따른 성능 차이
- [ ] arXiv preprint 존재 여부 확인 (현재 MICCAI proceedings only)
