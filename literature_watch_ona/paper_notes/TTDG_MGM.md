# TTDG-MGM — Paper Note

**"Test-Time Domain Generalization via Universe Learning: A Multi-Graph Matching Approach for Medical Image Segmentation"**

| 항목 | 내용 |
|------|------|
| KEY | TTDG_MGM |
| Venue | CVPR 2025 (main track) |
| arXiv | 2503.13012 |
| GitHub | https://github.com/Yore0/TTDG-MGM |
| Category | A |
| Relevance | High |

---

## 논문 주제

Medical image segmentation에서 test-time domain generalization:
- Training 시 multi-source domain의 morphological prior를 universe embeddings로 통합
- Test time에 multi-graph matching으로 target domain에 적응

---

## 핵심 방법

### Universe Learning (Training Phase)
- 여러 source domain 데이터에서 morphological prior를 추출하는 **learnable universe embeddings** 학습
- Domain-specific + domain-invariant morphological 정보를 모두 수용하는 공통 embedding space

### Multi-Graph Matching (Test-Time Phase)
- Target domain 이미지를 universe embeddings와 graph matching
- Cycle-consistency 보장: 매칭 → 역방향 매칭 일관성
- Unsupervised: test-time label 불필요

### 지원 설정
- Multi-source DG
- Single-source DG (SSDG)
- 두 벤치마크에서 state-of-the-art

---

## 내 연구와의 관계

### Motivation 공유
- "Morphological priors matter for domain generalization" — 나와 공유하는 핵심 관찰
- 혈관 구조의 형태적 특성이 domain shift를 넘어 보존되어야 한다는 아이디어

### 방법론적 차이 (Novelty 충돌 없음)
| 관점 | TTDG-MGM | Continuous-ONA (나) |
|------|---------|---------------------|
| 적용 시점 | Test-time adaptation | Training-time augmentation |
| 접근 방식 | Graph matching으로 morphological prior 전이 | Structure observability 기반 aug strength 조절 |
| 전제 | Test 시 target 이미지 접근 필요 | Test 시 target 불필요 (순수 generalization) |
| 혈관 특화 | 일반 medical seg | Tubular/vessel 특화 |

→ **Novelty 충돌 없음.** 두 방법은 complementary.

### 활용 방안
- TTDG-MGM을 내 방법의 test-time 후처리로 stacking 가능성 탐색
- 내 논문에서 "orthogonal approach" 로 언급 가능

---

## 실험 벤치마크

- 논문에서 구체적 벤치마크 dataset 이름 확인 필요 (arXiv 읽어야 함)
- Prostate segmentation, cardiac segmentation 등 일반 medical seg 추정

---

## 읽어야 할 내용

- [ ] Universe embedding의 morphological prior 정의: 어떤 vessel/structure 특성을 encoding?
- [ ] SSDG 실험 설정: 내 TOF-MRA 설정과 비교 가능한가?
- [ ] Multi-graph matching의 계산 복잡도: test-time efficiency
- [ ] 비교 baseline: SLAug, RASS, ADA 등과 직접 비교하는가?
