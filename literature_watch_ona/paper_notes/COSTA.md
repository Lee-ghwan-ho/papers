# COSTA — Paper Note

**"COSTA: A Multi-Center TOF-MRA Dataset and a Style Self-Consistency Network for Cerebrovascular Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | COSTA |
| Venue | IEEE Transactions on Medical Imaging |
| Year | 2024 |
| Vol/Pages | 43(12), 4442–4456 |
| DOI | 10.1109/tmi.2024.3424976 |
| PubMed | 39012728 |
| GitHub | https://github.com/iMED-Lab/COSTA |
| Category | A |
| Relevance | **High** |

---

## 논문 주제

Time-of-flight MRA (TOF-MRA) 기반 cerebrovascular segmentation에서 multi-center, multi-vendor inter-site heterogeneity 문제를 해결하기 위해:
1. 8개 기관 데이터셋 **COSTA** 구축
2. Style self-consistency 네트워크 **CESAR** 제안

---

## 데이터셋 COSTA

- 8개 imaging center 데이터 통합
- Multi-vendor (다양한 MRI 장비)
- 전체 volume manually annotated
- 6개 subset 공개 (GitHub)

---

## CESAR Network 핵심 구성

### 1. Coarse-to-Fine Architecture
- Iterative refinement: coarse prediction → fine segmentation 반복

### 2. Automatic Feature Selection Module
- Global long-range dependency (transformer-like) + local contextual feature (conv) 선택적 융합
- 뇌혈관 구조의 복잡한 위상을 처리하기 위한 모듈

### 3. Style Self-Consistency Loss (핵심)
- 서로 다른 center style의 TOF-MRA를 **단일 표준 style**로 정렬
- Style normalization을 loss level에서 강제
- Cross-center domain gap을 style alignment로 해결

---

## 내 연구와의 관계

### 데이터셋 관련
- COSTA dataset은 내 실험 환경 (TOF-MRA cerebrovascular segmentation, multi-center)과 정확히 일치
- 내 방법 평가에 COSTA 데이터셋 활용 가능 → GitHub에서 접근 가능한 6개 subset 확인 필요
- MULTIDOMAIN_BRAIN (MELBA 2025) 논문이 이미 TOF-MRA 포함 multi-domain DG를 다루므로 비교 대상 체계에서 COSTA 위치 정리 필요

### 방법론 관련
- COSTA의 방법은 **feature alignment (style self-consistency)** — 나의 **structure-conditioned augmentation**과 원리 수준에서 다름
- Novelty 충돌 없음

### 비교 baseline 관련
- COSA는 내 paper에서 반드시 cite해야 할 논문
- CESAR가 내 비교 baseline 중 하나가 되어야 할 가능성 있음
- 특히 "TOF-MRA specific DG" 관련 related work에 포함 필수

---

## 핵심 인용 가능 표현

> "inter-site and inter-vendor heterogeneity in TOF-MRA making accurate and robust cerebrovascular segmentation challenging"  
> → 내 paper introduction에서 동기 서술 시 이 논문 인용 가능

> "COSA dataset: 8 individual imaging centers, fully manually annotated"  
> → 내 방법 평가 데이터셋으로 가장 적합한 후보

---

## 읽어야 할 내용

- [ ] Style self-consistency loss 수식: 어떤 방식으로 style을 정의하고 alignment를 강제하는가?
- [ ] COSTA dataset의 6개 공개 subset 구성: 어느 기관 데이터가 공개되어 있는가?
- [ ] CESAR와 기존 방법들의 정량적 비교 결과: Dice, HD95
- [ ] RASS, MoreStyle 등 기존 SSDG 방법들이 COSTA와 비교되었는가?
