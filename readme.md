## 📝 **Offensive Language Identification in Korean**

### 📚 **프로젝트 개요**
- **프로젝트명:** Offensive Language Identification in Korean  
- **목적:**  
   - 욕설 포함 여부뿐만 아니라, 욕설이 없어도 공격적 의도를 내포한 문장 식별  
   - 다양한 사회·문화적 맥락을 고려한 부적절한 표현 감지  
- **예시:**  
   - "기다려라 찾아간다 ㅋㅋ 평생 누워있게 해줄게" → 직접 욕설 없음, 그러나 공격적 의도 포함  

---

### 🔍 **관련 연구 동향**
#### **1. KOAS (Korean Text Offensiveness Analysis System)**  
- **주요 작업:**  
   - **욕설 감지 (Abusive Language Detection)**: 욕설 포함 여부 판단  
   - **감정 분석 (Sentiment Analysis)**: 긍정, 중립, 부정 분류  
- **모델 구조:**  
   - Shared Convolution Layer (공통 레이어)  
   - Task-specific Layers (작업별 특화 레이어)  
- **평가 데이터:**  
   - 유튜브, 네이버 영화 리뷰, DC인사이드  
   - 약 4만 5천 개 문장 학습  

#### **2. KODOLI (Korean Dataset for Offensive Language Identification)**  
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

### 💻 **모델 선정 및 설명**
#### **선정 모델:**  
- **KoBERT:** 한국어 특화된 BERT 모델  
- **KoELECTRA:** 한국어 특화된 ELECTRA 모델  

#### **모델 특징:**  
- **KoBERT:** Transformer 기반 양방향 문맥 이해  
- **KoELECTRA:** Replaced Token Detection 기법 활용  

#### **토크나이저 비교:**  
- **Mecab_ko:** 형태소 분석 최적화  
- **SentencePiece:** 일반적인 토크나이저  

---

### ⚙️ **실험 및 결과**
#### **Multi-task Learning (MTL) 적용:**  
- **Shared Layers:** 모든 작업에 공통 적용  
- **Task-specific Layers:** 작업별 특화 레이어  

#### **Classifier 성능 비교:**  
- **KoBERT Unified Classifier:** ALD (91.47%), SA (75.91%), OLI (81.38%)  
- **KoELECTRA Unified Classifier:** ALD (88.86%), SA (74.24%), OLI (80.49%)  

#### **Freezing Strategy:**  
- 상위 레이어만 학습 시, 성능 저하 관찰  
- 전체 재학습 시 더 나은 성능 확인  

---

### 📊 **하이퍼파라미터 튜닝**
- **Optimizer:** AdamW  
- **Scheduler:** Cosine  
- **Epochs:** 5, 10  
- **Tokenizer:** Mecab_ko  
- **Classifier Activation Function:** ReLU, Sigmoid, Tanh  

---

### 🚀 **서비스 방향 및 사회적 영향**
- **커뮤니티 플랫폼 댓글 필터링 (예: 고파스, 에브리타임)**  
- **저연령층을 위한 부적절한 언어 필터링**  
- **LLM 출력 검증 및 정렬 (Model Alignment)**  

#### **기대 효과:**  
- 온라인 욕설 및 혐오 표현 노출 감소  
- 온라인 소통에서의 정신적 스트레스 감소  
- 저연령층의 올바른 언어 사용 능력 향상  

---

### 🛠️ **향후 개선 방향**
- 하이퍼파라미터 튜닝 최적화  
- 충분한 학습 시간 확보  
- Optuna와 같은 자동화 도구 활용  

---

### 📑 **기여자**
- **이름:** 고수현  
- **학번:** 2018131425  

---

### 📥 **참고 문헌**
- KOAS: Korean Text Offensiveness Analysis System  
- KODOLI: Korean Dataset for Offensive Language Identification  

---
