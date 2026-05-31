# L2CP — Paper Note

**제목**: Test-Time Training with Local Contrast-Preserving Copy-Pasted Image for Domain Generalization in Retinal Vessel Segmentation  
**Venue**: MICCAI 2025 (Paper 3277)  
**SpringerLink**: https://link.springer.com/chapter/10.1007/978-3-032-04981-0_58  
**Authors**: Gu, Y.; Sun, Z.; Liu, Z.; Xu, Y.  
**Status**: Accepted Conference Paper

---

## 핵심 방법 요약

### 문제의식
- 망막 혈관 분할 모델의 domain shift 대응을 위한 test-time training (TTT) 접근
- 기존 copy-paste augmentation은 배경(background)의 도메인 스타일을 무시하고 혈관을 그대로 붙여넣음
- Target domain의 배경 스타일 + source domain의 혈관 구조를 결합한 synthetic 이미지 생성 필요

### L2CP 방법

1. **Morphological Closing으로 thin vessel 제거**
   - Test 이미지에 morphological closing operation 적용
   - **Thin vessel 구조를 배경으로 채워** vessel-free 이미지 생성
   - 이 vessel-free 이미지는 target domain의 intensity/style 분포는 유지
   
2. **Local Contrast Map 추출**
   - Source 이미지에서 혈관의 local contrast map 계산
   - Local contrast = 혈관 픽셀과 주변 배경 픽셀 간의 intensity 차이
   
3. **Copy-Paste**
   - Vessel-free target background + source vessel local contrast → synthetic training image
   - 이 이미지로 test-time training 수행

### 핵심 관찰
- **"thin nature of retinal vessel structures"를 명시적으로 활용**
- Morphological closing으로 thin vessel이 자동으로 inpainted됨
- Thick vessel은 closing으로 완전히 제거되지 않을 수 있음
  → thin vessel이 더 완전히 제거됨 (혈관 두께 구분이 내재적)

---

## 내 연구(Continuous-ONA)와의 관계

### 공통점
- **Thin vessel의 특수성을 명시적으로 DG 방법 설계에 활용**
- 혈관 구조의 관찰 가능성(observability)이 방법론적 결정에 영향을 줌
- 논리적 전제: "얇은 혈관은 굵은 혈관과 다르게 취급되어야 한다"

### 핵심 차이점

| 차원 | L2CP | Continuous-ONA |
|------|------|----------------|
| **Paradigm** | Test-time training (target data 필요) | Single-source DG (training-time only) |
| **목적** | Target style을 배경에 반영한 synthetic 이미지 생성 | Source training 시 thin vessel에 보수적인 aug 적용 |
| **Thin vessel 처리** | Morphological closing으로 제거 (target image) | Radius-conditioned aug budget 감소 (source image) |
| **Label safety** | 직접 다루지 않음 | Label-image inconsistency 명시적 방지 |
| **Conditioned signal** | Morphological closing scale (binary) | Local radius / observability score (continuous) |
| **Domain info** | Target image 사용 | Source annotation만 사용 |

### Novelty 충돌 평가

**충돌 위험: 낮음**

L2CP는 test-time adaptation 계열로, training-time SSDG인 내 방법과 설정이 근본적으로 다름.
내 방법은 test-time 정보 없이 source training 시 thin vessel 보호를 구현하는 유일한 접근.

다만, L2CP의 "thin vessel 구조를 배경으로 채워 domain-invariant background를 얻는다"는 아이디어는,
thin vessel이 강한 appearance augmentation에 취약하다는 내 동기와 논리적으로 동일한 전제를 공유.

---

## 논문 작성 시 활용 방안

### Related Work에서의 위치
- "Test-time adaptation 계열" 별도 단락에서 L2CP를 언급
- L2CP와의 공통 전제를 강조: "thin vessel의 특수성을 explicit하게 다룬다"
- 핵심 차이: L2CP = test-time (target 필요), 내 방법 = training-time SSDG (target 불필요)

### 동기 강화 근거로 활용
- "thin vessel 구조의 특수성은 domain generalization에서 중요하다"는 주장을 L2CP가 독립적으로 지지
- L2CP가 morphological closing으로 thin vessel을 분리하는 것 → 우리 논문에서 "thin vessel은 domain appearance에 fragile하다"는 동기의 supporting evidence로 인용 가능

---

## 실험적 관련성

- 데이터셋: 망막 혈관 (DRIVE, STARE, CHASE 등 fundus 계열)
- 내 데이터: TOF-MRA cerebrovascular (3D)
- 직접 비교 필요성: 낮음 (설정 다름)
- 단, "thin vessel 보호" 계열 논문으로 함께 묶어 related work 그룹화 가능

---

## 인용 가능성

- MICCAI 2025 Accepted → 출판 확정
- Related Work 인용: ✅ 권장
- Comparison Baseline: ❌ (설정 다름, test-time 방법)
