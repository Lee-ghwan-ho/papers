# MRFFD — Paper Note

**Full title**: Multi-receptive Field Feature Disentanglement with Distance-Aware Gaussian Brightness Augmentation for Single-Source Domain Generalization in Medical Image Segmentation  
**Venue**: Neurocomputing, 2025  
**DOI**: 10.1016/j.neucom.2025.130120  
**Status**: Published Journal Article  
**Category**: A (직접경쟁) + B (방법론유사)  
**Relevance**: **High**  
**Added**: Run #8 (2026-06-12)

---

## 방법 요약

### MRFFD (Multi-Receptive Field Feature Disentanglement)
- Multi-scale feature extraction: 크기가 다른 convolutional kernel로 fine-grained detail + global context 동시 포착
- Channel-level feature disentanglement: 채널 단위로 style feature와 structural feature를 분리
- Domain-specific style variation에 대한 robustness 향상이 목적

### DAGBA (Distance-Aware Gaussian Brightness Augmentation)
- **핵심 아이디어**: pixel의 foreground 구조 및 이미지 경계까지의 거리(distance)를 기반으로 brightness augmentation을 동적으로 조절
- Gaussian 함수 형태로 brightness perturbation 강도를 spatial하게 조절
- "uneven brightness distribution in medical images"를 보정하고, 복잡한 brightness variation을 시뮬레이션

---

## 실험

- Prostate MRI (multi-center SSDG)
- Fundus image segmentation (multi-domain)
- SOTA 대비 유의미한 성능 향상 보고

---

## 내 방법(Continuous-ONA)과의 관계

### 공통점
- "spatial location에 따라 augmentation 강도를 조절"이라는 space-conditioned augmentation 아이디어 공유
- "distance to structure"를 aug budget의 conditioning variable로 사용한다는 점에서 surface level 유사성 존재

### 핵심 차이 (Novelty 보호 논거)

| 비교 항목 | MRFFD/DAGBA | Continuous-ONA |
|-----------|-------------|----------------|
| Distance 측정 대상 | pixel ↔ foreground 경계 (background-side spatial distance) | vessel foreground 내 pixel의 local vessel radius |
| Conditioning 목적 | 이미지 전체의 uneven brightness 보정 | thin vessel의 weak evidence 보호 + thick vessel의 augmentation 극대화 |
| Augmentation 종류 | brightness only (단일 변환) | nonlinear appearance family (full intensity curve) |
| Intra-class 이질성 | 없음: foreground 내 구조 차이 무시 | 핵심: foreground 내 vessel radius로 연속적 구분 |
| 동기 | appearance bias 보정 (general purpose) | fragile structure 보호 + shortcut 억제 (vessel-specific) |
| 논문 문제의식 | "medical images have uneven brightness distribution" | "thin vessel loses evidence under strong appearance augmentation" |
| Vessel thickness 개념 | 없음 | 있음 (local radius = observability proxy) |

### 결론

DAGBA = "이미지 경계 / foreground 경계로부터의 거리 기반 brightness 조절"  
내 ONA = "vessel 내 local radius / observability 기반 nonlinear appearance augmentation 강도 조절"

두 방법은 "distance-conditioned augmentation"이라는 표면적 공통점을 가지나,  
conditioning variable, augmentation type, problem motivation이 모두 다르다.  
특히 DAGBA는 thin vessel 보호 개념이 전혀 없고,  
내 핵심 claim인 "intra-class vessel radius → augmentation budget"을 다루지 않는다.

**Novelty 위협도**: Medium — paper review 시 "your method is similar to DAGBA"라는 reviewer comment가 나올 수 있음.  
**대응 전략**: DAGBA를 related work로 언급하되, conditioning variable의 차이 + vessel observability 보호 동기의 차이를 명확히 구분.

---

## Related Work 포함 여부 추천

- Related Work / Background에서 "structure-aware brightness augmentation"의 선행 사례로 MRFFD를 언급하고,  
  DAGBA = boundary-distance conditioning vs. 내 방법 = intra-vessel-radius conditioning의 차이를 한 문장으로 구분.

---

## 미확인 사항

- [ ] DAGBA의 정확한 distance 계산 공식: skeleton distance transform 사용 여부 확인 필요
- [ ] DAGBA가 vessel segmentation에서도 실험했는지 확인 (prostate + fundus라면 내 tubular domain과 차이 더 명확)
- [ ] Multi-receptive field 구조의 구체적 kernel size 조합 (내 scale-space 관련성 검토)
