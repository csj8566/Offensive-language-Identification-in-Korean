# 📝 **Offensive Language Identification in Korean**

> # ⚠️ **주의:**  
> - **본 프로젝트에서 사용된 **KODOLI 데이터셋**에는 욕설, 외설적 표현, 혐오 표현이 다수 포함되어 있습니다.**  
> - **학습 코드 ipynb 파일에는 이와 같은 내용이 드러나 있으므로 불쾌감을 유발할 수 있습니다.**  
> - **이를 원치 않는 분께서는 README 및 보고서 위주로 본 문서를 열람해 주시기 바랍니다.**



## ✅ **프로젝트 개요**
- **프로젝트명:** Offensive Language Identification in Korean  
- **목적:**  
   - 욕설 포함 여부뿐만 아니라, 욕설이 없어도 공격적 의도를 내포한 문장 식별  
   - 다양한 사회·문화적 맥락을 고려한 부적절한 표현 감지
   - **욕설이 있지만 공격적 의도가 아닌 문장, 욕설이 없지만 긍정적인 문장 등을 구분**  
- **예시:**  
   - "기다려라 찾아간다 ㅋㅋ 평생 누워있게 해줄게" → 직접 욕설 없음, 그러나 공격적 의도 포함  

## ✅ **모델링 전략**
![모델링전략](images/Offensive-language-detection_modeling.png)
---

## ✅ **관련 연구 동향**
### **1. KOAS (Korean Text Offensiveness Analysis System)**  
- **주요 작업:**  
   - **욕설 감지 (Abusive Language Detection)**: 욕설 포함 여부 판단  
   - **감정 분석 (Sentiment Analysis)**: 긍정, 중립, 부정 분류  
- **모델 구조:**  
   - Shared Convolution Layer (공통 레이어)  
   - Task-specific Layers (작업별 특화 레이어)  
- **평가 데이터:**  
   - 유튜브, 네이버 영화 리뷰, DC인사이드  
   - 약 4만 5천 개 문장 학습  

### **2. KODOLI (Korean Dataset for Offensive Language Identification)**  
- **데이터셋 구성:**  
   - 총 38,525개 댓글  
   - 주요 소스: DC인사이드, 네이버 뉴스 댓글, 기타 쇼핑 및 게임 플랫폼  
- **주요 작업:**  
   - Offensive Language Detection (OFFEN, LIKELY, NOT)  
   - Abusive Language Detection (ABS, NON)  
   - Sentiment Analysis (POS, NEU, NEG)  
- **사용 모델:**  
   - BiLSTM, CNN, KoBERT, KoELECTRA  

---

## ✅ **모델 선정 및 설명**
### **선정 모델:**  
- **KoBERT:** 한국어 특화된 BERT 모델  
- **KoELECTRA:** 한국어 특화된 ELECTRA 모델  

### **모델 특징:**  
- **KoBERT:** Transformer 기반 양방향 문맥 이해  
- **KoELECTRA:** Replaced Token Detection 기법 활용  

### **토크나이저 비교:**  
- **Mecab_ko:** 형태소 분석 최적화  
- **SentencePiece:** 일반적인 토크나이저  

---

## ✅ **실험 및 결과**
### **Multi-task Learning (MTL) 적용:**  
- **Shared Layers:** 모든 작업에 공통 적용  
- **Task-specific Layers:** 작업별 특화 레이어  
   ## 1️⃣ **Classifier modeling**

   ### KoBERT – Classifier Modeling

   | Metric | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | Unified Classifier | **91.47%** | 75.91% | **81.38%** |
   | Specific Classifier for each Task | 90.62% | **79.68%** | 78.12% |

   ### KoELECTRA – Classifier Modeling

   | Metric | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | Unified Classifier | **88.86%** | **74.24%** | **80.49%** |
   | Specific Classifier for each Task | 65.##% | 70.##% | 78.##% |

   ---

   ## 2️⃣ **Freezing strategy**

   ### KoBERT – Freezing (Unified)

   | Model | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | **BERT no freezing** | **91.47%** | **75.91%** | **81.38%** |
   | BERT 12th, 11th, 10th layer train | 88.99% | 72.24% | 78.61% |
   | BERT 12th, 11th layer train | 87.95% | 70.58% | 77.28% |
   | BERT 12th layer train | 84.83% | 67.53% | 74.71% |
   | BERT no train | 67.12% | 40.59% | 64.39% |

   ### KoBERT – Freezing (Specific)

   | Model | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | BERT no freezing | 64.06% | 35.93% | 81.38% |
   | BERT 12th, 11th, 10th layer train | 89.06% | 65.62% | 70.31% |
   | **BERT 12th, 11th layer train** | **90.62%** | **79.68%** | **78.12%** |
   | BERT 12th layer train | 81.25% | 70.31% | 70.31% |
   | BERT no train | 87.50% | 60.93% | 81.25% |

   ### KoELECTRA – Freezing (Unified)

   | Model | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | ELECTRA no freezing | 65.##% | 35.##% | 65.##% |
   | **ELECTRA 10th, 11th layer train** | **88.86%** | **74.24%** | **80.49%** |
   | ELECTRA 11th layer train | 88.58% | 73.58% | 79.09% |
   | ELECTRA no train | 81.##% | 57.##% | 71.##% |

   ### KoELECTRA – Freezing (Specific)

   | Model | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | ELECTRA no freezing | 65.##% | 35.##% | 65.##% |
   | ELECTRA 10th, 11th layer train | 65.##% | 70.##% | 78.##% |
   | **ELECTRA 11th layer train** | **65.##%** | **71.##%** | **78.##%** |
   | ELECTRA no train | 65.##% | 65.##% | 74.##% |

   ## 3️⃣ **Task combination**

   **KoBERT**

   | Metric | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | ALD + SA + OLI | **91.47%** | 75.91% | **81.38%** |
   | SA + OLI | Non | **76.91%** | 80.95% |
   | ALD + OLI | 91.33% | Non | 80.98% |
   | OLI | Non | Non | 80.23% |

   | Metric | Pr - not | R - not | F1 - not | Pr - likely | R - likely | F1 - likely |
   | --- | --- | --- | --- | --- | --- | --- |
   | ALD + SA + OLI | 89.67 | **92.71** | 91.17 | **41.89** | 36.08 | 38.77 |
   | SA + OLI | **90.56** | 92.00 | **91.27** | 40.77 | 36.24 | 38.37 |
   | ALD + OLI | 90.20 | 92.21 | 91.19 | 40.82 | 37.51 | 39.09 |
   | OLI | 90.36 | 91.35 | 90.85 | 39.27 | **42.11** | **40.64** |

   | Metric | Pr - off | R - off | F1 - off | Pr - avg | R - avg | F1 - avg |
   | --- | --- | --- | --- | --- | --- | --- |
   | ALD + SA + OLI | 75.01 | 74.41 | 74.71 | 68.85 | 67.73 | 68.21 |
   | SA + OLI | 74.03 | **76.12** | 75.06 | 68.45 | **68.12** | 68.23 |
   | ALD + OLI | 75.80 | 74.57 | **75.18** | 68.94 | 68.09 | **68.48** |
   | OLI | **77.28** | 70.19 | 73.56 | **68.97** | 67.88 | 68.35 |

   **KoElectra**

   | Metric | ALD | SA | OLI |
   | --- | --- | --- | --- |
   | ALD + SA + OLI | **88.86%** | **74.24%** | **80.49%** |
   | SA + OLI | Non | 72.92% | 79.55% |
   | ALD + OLI | 88.19% | Non | 80.21% |
   | OLI | Non | Non | 77.19% |

   | Metric | Pr - not | R - not | F1 - not | Pr - likely | R - likely | F1 - likely |
   | --- | --- | --- | --- | --- | --- | --- |
   | ALD + SA + OLI | **89.45** | 91.40 | **90.41** | **37.92** | **28.38** | **32.46** |
   | SA + OLI | 87.15 | 92.23 | 89.62 | 31.94 | 15.79 | 21.13 |
   | ALD + OLI | 86.38 | 93.80 | 89.94 | 37.13 | 17.16 | 23.47 |
   | OLI | 82.51 | 93.55 | 87.68 | 23.91 | 5.03 | 8.32 |

   | Metric | Pr - off | R - off | F1 - off | Pr - avg | R - avg | F1 - avg |
   | --- | --- | --- | --- | --- | --- | --- |
   | ALD + SA + OLI | 70.62 | **75.80** | **73.12** | **66.00** | **65.19** | **65.33** |
   | SA + OLI | 68.84 | 75.93 | 72.22 | 62.64 | 61.31 | 61.00 |
   | ALD + OLI | **70.90** | 73.36 | 72.11 | 64.80 | 61.44 | 61.84 |
   | OLI | 65.04 | 66.80 | 65.90 | 57.15 | 55.13 | 53.97 |

   → **단독 OLI 사용시보다, Task Combination 수행했을 때 성능이 높게 나오는 경향을 확인할 수 있었습니다.**

## ✅ **참고 문헌**
- KOAS: Korean Text Offensiveness Analysis System  
- KODOLI: Korean Dataset for Offensive Language Identification  

---
