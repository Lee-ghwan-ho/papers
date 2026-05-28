# A Multi-Branch Framework for Cross-Domain Vessel Segmentation via the Few-Shot Paradigm

> **Venue**: MICCAI 2025 (Paper 0782)  
> **Status**: Accepted Conference Paper  
> **Category**: A + C — 직접 경쟁 (혈관 두께 조건부 표현 + 혈관 특화 DG)  
> **Relevance**: **High** — 혈관 두께를 명시적으로 모델링  
> **URL**: https://papers.miccai.org/miccai-2025/0019-Paper0782.html  
> **Added**: Run #2 (2026-05-28)

---

## 요약

Few-shot segmentation(FSS) paradigm을 활용한 cross-domain vessel segmentation framework.
혈관 domain shift의 핵심 문제인 "texture와 thickness 차이"를 명시적으로 다룬다.

**핵심 구성 요소:**

1. **Multi-Branch Feature Extractor (MBFE)**  
   - 두께가 다른 혈관 구조(thick / thin)를 구분하는 multi-branch  
   - **혈관 두께에 따라 서로 다른 feature 경로** 사용
   
2. **High-Frequency Auxiliary Modality**  
   - 혈관의 고주파 구조 정보를 보조 modality로 도입  
   - 세부 혈관 구조에 집중하도록 유도

3. **Dual-Modality Feature Extraction and Fusion (DM-FEF)**  
   - 원본 이미지와 고주파 이미지 정보를 fusion

**설정**: Few-shot cross-domain (support set 필요)

---

## 내 방법(Continuous-ONA)과의 비교

### 유사점 (위험 요소)

| 항목 | MBFCV | Continuous-ONA |
|------|-------|----------------|
| 혈관 두께 명시적 모델링 | ✅ MBFE | ✅ local radius/observability |
| 혈관 구조 내 이질성 인식 | ✅ thick vs thin | ✅ continuous observability |
| Domain shift 대응 | ✅ | ✅ |

### 핵심 차이

| 항목 | MBFCV | Continuous-ONA |
|------|-------|----------------|
| **접근 방식** | Feature representation 분리 (at inference) | **Augmentation budget 조절 (at training)** |
| **Domain 설정** | Few-shot (test-time support 필요) | **Single-source DG (test-time 정보 없음)** |
| **두께 조건 적용** | Feature branch 선택 (이진적) | **Augmentation strength (연속적)** |
| **label-image consistency** | ❌ | ✅ |
| **SSDG 조건** | ❌ (few-shot에 의존) | ✅ |

### 대응 전략

> "MBFCV distinguishes vessel structures of varying thickness at the feature representation level
> and requires test-time support images. In contrast, ONA encodes structural observability
> into the augmentation process at training time, requiring no target-domain information."

---

## 미해결 질문

- MBFE에서 thin/thick 분류가 binary인지, 연속 스펙트럼인지 full text 확인 필요
- few-shot 실험의 target domain 설정 (modality? center?)
- MBFE의 thickness 구분 기준: 학습된 것인가, 사전 정의된 것인가
