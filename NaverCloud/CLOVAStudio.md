# 네이버 클라우드 CLOVA Studio 이론
- [강의 자료](https://brunch.co.kr/@topasvga/5245)
  - 이전 강의까지는 GPU 서버 사용법·운용에 대해 공부했음
  - 오늘부터는 생성형 AI 활용법
    - CSP(Cloud Service Provider)가 제공하는 생성형 AI 배워보기

## 네이버 클라우드: 사용 서비스 4가지
1. 개인용
   - [CLOVA X](https://clova-x.naver.com)
   - [CUE](https://cue.search.naver.com)
   - 네이버 클로바 앱
2. GPU 서버 제공
   - 네이버 클라우드 [Server 서비스](https://console.ncloud.com/vpc-compute/server) 사용
3. 생성형 AI
   - 네이버 클라우드 내 CLOVA Studio 사용
     - [CLOVA Studio](https://console.ncloud.com/clova-studio/product)     
4. 기타 AI 서비스들
   - 네이버 클라우드 사용
     - Chatbot
     - OCR 등

## 생성형 AI 도구 'CLOVA Studio' 알아보기
### 목차
1. AI / 머신러닝 / 딥러닝 / 자연어 처리
2. 생성형 AI
3. 네이버 클라우드 AI 서비스
4. 네이버 생성형 AI 개발 플랫폼: CLOVA Studio
5. 프롬프트 엔지니어링
6. 파인 튜닝
7. 네이버 익스플로러
8. 검색증강생성(RAG)
9. 랭체인
10. 네이버 스킬 트레이너

### 1. AI / 머신러닝 / 딥러닝 / 자연어 처리
<img width="2048" height="861" alt="image" src="https://github.com/user-attachments/assets/eb4d8a60-4df3-4079-b524-0b2c2603c1bb" />

#### AI, 머신러닝, 딥러닝
- AI(인공지능)
  - 컴퓨터가 인간의 지능적인 행동을 모방하도록 만드는 포괄적인 분야
  - 넓은 의미의 지능적 행동 (학습, 추론, 의사 결정 등)을 모두 포함
- 머신러닝
  - 인공지능의 하위 집합
  - 컴퓨터가 데이터로부터 스스로 학습하고 의사 결정을 내리도록 하는 기술
  - 알고리즘을 통해 학습하고 패턴을 인식
    - 머신러닝 등장 전(Rule-Based System): 데이터에 대한 명시적인 규칙을 프로그래밍(IF-THEN)했었음
  - 딥러닝
    - 머신러닝의 한 분야
    - 인간의 뇌 구조를 모방한 '인공 신경망'을 여러 층으로 깊게 쌓아 복잡한 패턴을 학습하는 기술
    - 매우 복잡한 데이터(이미지, 음성, 텍스트 등)를 처리하는 데 효과적이며, 특징(feature)을 스스로 추출함

>생성형 AI는 딥러닝 안에 있음

#### 자연어 처리(NLP, Natural Language Process)
<img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/8ff9326c-397d-48c8-8d60-3149a1b6a8fa" />

- 머신러닝을 사용하여 텍스트의 구조와 의미를 파악
- 자연어
  - 일상 생활에서 사용하는 언어
- 자연어 처리
  - 자연어의 의미를 분석하여 컴퓨터가 처리할 수 있도록 하는 일
  - 음성 인식, 내용 요약, 번역, 사용자 감성 분석, 텍스트 분류 작업(스팸 메일 분류, 뉴스 기사 카테고리 분류), 질의 응답 시스템, 챗봇과 같은 곳에서 사용되는 분야
  - 주로 기계 학습과 딥러닝 접근법을 활용해 대규모 텍스트 데이터를 학습하여 패턴을 인식하고 예측하는 모델을 구축
  - 순환 신경망, 합성곱 신경망, 생성형 사전훈련 등 다양한 모델 사용

#### 언어 모델(Language Model)
- 인간의 언어를 인공지능이 이해할 수 있도록 하는 훈련 모델
  - 통계를 이용한 방법
  - 인공 신경망을 이용한 방법
- 인공 신경망을 활용해 언어의 패턴이나 구조, 관계 학습
- 문맥을 만드는 데 가장 자연스러운 문맥을 만들어 내는 모델
  - 이전 단어들을 보고 자연스럽게 이어질 수 있는 다음 단어 예측
  
<img width="432" height="494" alt="image" src="https://github.com/user-attachments/assets/fa43a10a-3a95-47b3-8d0b-dff04b30199d" />

- 특정 문장이 등장할 확률을 계산해 주는 모델
- 딥러닝 전이 학습(Transfer Learning)과 결합하여 BERT, GPT-3 공개 이후 인공지능 기술의 중요한 축으로 자리잡음
  - BERT(Bidirectional Encoder Representations from Transformers)
    - 구글에서 개발한 자연어 처리(NLP) 모델
    - 단어 단위의 의미를 문맥 전체에서 앞뒤 방향 모두 고려해 파악
- 매개변수(Parameter) 크기를 늘리는 형태의 초거대 언어모델(LLM, Large Language Models) 구축 경쟁 촉발
  - LLM(대규모 언어 모델)
    - 언어 모델의 한 축인 언어 '생성' 모델을 방대한 데이터, 수백 억 개의 매개변수, 이를 학습하기에 충분한 컴퓨팅 자원을 기반으로 계산력을 크게 키워 다음 단어 예측 정확도를 획기적으로 향상한 모델
    - 방대한 양의 데이터로 사전 학습된 초대형 모델
    - Foundation Model(FM)이라고도 함
  - LMM(대규모 멀티모달 모델 Large Multimodal Model)
    - 대상 데이터를 텍스트에 국한하지 않고 이미지, 동영상, 음성과 같은 멀티모달로 확장
    - 멀티모달: '다수' 또는 '여러 개'를 의미하는 멀티(Multi)와 '양식'을 뜻하는 모달(Modal)의 합성 → 시각, 청각, 촉각, 미각 등 다양한 정보를 이해하고 처리할 수 있는 AI


### 2. 생성형 AI
- 생성형 AI
  - 데이터 학습과 질문 의도 분석을 바탕으로 가장 정확환 답변을 내놓도록 설계되어 있음
  - 기존 데이터를 단순히 가공하거나 분석하는 것이 아니라 새롭고 독창적인 콘텐츠나 데이터 생성
    - 원본과 유사하지만 동일하지는 않음
  - 아주 자연스러운 응답을 제시하지만 생성형 AI의 답이 정확하지 않을 수 있음
    - 분석형(Analytical) AI: 전통적 인공지능 기술이라고도 불림. 주로 데이터 분석이나 분류 → '예측'
    - 생성형(Generative) AI: 이용자의 특정 요구 + 이전에 학습한 패턴 기반으로 새롭고 독창적인 콘텐츠나 데이터 생성 → '예측'+'짐작하여 생성' 


#### 생성형 AI 사용
- LLM(대규모 언어 모델)

|no|작업|난이도|비용|정확성|
|--|--|--|--|--|
|1|프롬프트 엔지니어링|쉬움|저비용|정확도 낮음|
|2|검색 증강 생성(RAG)|보통|중비용|정확도+1|
|3|파인 튜닝|-|-|정확도+2|
|4|지속적 학습(LLM 기업)|어려움|고비용|정확도+3|

- 프롬프트 엔지니어링만으로도 80%의 정확한 데이터를 얻을 수 있다고 함
  - FM이 구축된 상태에서 프롬프트 엔지니어링만으로 대응하는 것이기 때문에 비용이 낮음
  - 나머지 방법에 비해 상대적으로 정확도는 낮은 편
- 지속적 학습의 경우 학습한 데이터의 '날짜'도 중요함


#### ⅰ. 프롬프트 엔지니어링
- 대형 언어 모델(LLM)과 상호 작용하는 방법과 사용자의 목적에 맞게 모델의 반응을 최적화하는 기술

#### ⅱ. 검색 증강 생성(RAG)

<img width="1280" height="687" alt="image" src="https://github.com/user-attachments/assets/413118ec-2c1a-4c1c-8989-36fb39fd9e16" />


- 정보 검색과 생성을 결합
  1. 정보 검색
  2. 관련 문서 반환
  3. 문장 생성
- 정보 저장과 질문 동작
  1. 정보 저장
  2. 프롬프트
  3. 정보 검색
  4. 정보 제공
  5. 질문+문맥

<img width="692" height="260" alt="image" src="https://github.com/user-attachments/assets/e30c3c5f-9a0e-4505-89a2-0dfe9004c1cd" />

- 그냥 질의를 하면 Foundation Model이 옛날 정보나 정확하지 않은 정보를 줄 때가 있음 → RAG로 보완

<img width="800" height="450" alt="image" src="https://github.com/user-attachments/assets/c4ec751b-bb46-4161-900d-e7fb36243a93" />


- 동작 순서
  1. 데이터, 매뉴얼 텍스트를 벡터화한다.
  2. 사용자가 질문한다.
  3. 벡터화된 매뉴얼과 질문을 비교하여 매뉴얼 중 질문한다.
     - 예: 인사정보 → n년 차에 휴가는 몇 개인가요?
  4. 연관성 있는 정보를 획득한다.
  5. 획득한 매뉴얼의 정보를 프롬프트에 삽입해 LLM에 질문한다.
  6. LLM이 답변한다.
     - 예: ChatGPT, HCX(네이버)

#### ⅲ. 파인튜닝

<img width="600" height="274" alt="image" src="https://github.com/user-attachments/assets/aec995fc-8cac-4fdc-b5b1-f49b69c4ff50" />

- 기존의 LLM을 조금 더 훈련시키는 과정
- LLM을 특별한 상황에 맞게 가르치는 것
  - 예: 법률 데이터, 의료 데이터
- 추가 학습된 새로운 LLM이 생김

>전이 학습 vs. 파인튜닝<br>
><br>
><img width="600" height="235" alt="image" src="https://github.com/user-attachments/assets/d4652019-3056-43aa-8569-be07652dff40" />


#### ⅳ. 랭체인(LangChain)
- LLM과 외부 도구를 사슬처럼 결합시켜주는 것

<img width="559" height="280" alt="image" src="https://github.com/user-attachments/assets/96fd645a-70c1-42bd-a9c2-e4f60d7107b5" />

- [스트림릿](https://wikidocs.net/230759)
  - 상호작용하는 데이터 주도형 웹 애플리케이션을 쉽게 만들 수 있는 오픈 소스 Python 라이브러리 
    - 데이터 분석과 시각화를 함께 간편하게 수행할 수 있음
    - 프런트엔드에 대한 지식 없이도 머신러닝 모델을 쉽고 빠르게 시연하고 배포할 수 있음

<img width="1400" height="1197" alt="image" src="https://github.com/user-attachments/assets/59db2c17-2181-4d90-91be-3a2298f4604b" />


### 3. 네이버 클라우드 AI 서비스

<img width="694" height="351" alt="image" src="https://github.com/user-attachments/assets/ba346c34-0c59-4644-87c3-787558b29585" />

- 2025-11 기준 네이버 클라우드 AI 상품
  - AI NAVER API
    - CLOVA Speech Recognition (CSR)
    - CLOVA Voice
    - CAPCHA
    - Maps
    - Search Trend
  - AI Service
    - Papago Translation
    - CLOVA Chatbot
    - CLOVA OCR → 예: 지로용지 인식, 영수증 이벤트 영수증 읽기 등	
    - CLOVA Speech
    - CLOVA Dubbing
    - AITEMS → "추천" 서비스: 사용자별 이력을 분석해 관심사와 취향에 맞는 상품을 추천하는 서비스(커머스, 리테일 산업에서 많이 사용)
    - CLOVA Studio
    - CLOVA NSML → AI, ML 연구/개발할 때 사용
    - CLOVA Greeneye

>문제 자주 나옴: 다음 중 네이버 클라우드 AI 서비스가 아닌 것은?

#### HyperCLOVA X(HCX) 특장점
- [KorNAT](https://arxiv.org/pdf/2402.13605): 한국형 LLM 평가 데이터 → 한국의 사회적 가치와 일반 지식에 맞춘 대형 언어 모델(LLM) 정렬 벤치마크
  - 영어권 문화에서 받아들일 수 있는 대답은 다른 문화(대한민국)에서는 완전히 부적절할 수 있음
  - 미국산 LLM이 영어권 문화에 대한 편견을 나타낸다는 것을 고려할 때 한국어를 평가할 수 있는 별도 지표가 필요
  - KorNAT 지표
    - 한국어 적합도 평가
    - 한국정보통신기술협회(TTA)에서 정성적 및 정량적 표준에 대한 철저한 평가 통과 & 정부 승인
- HCX는 한국어 문장의 맥락을 잘 구별 & 한국 고유 지식에 상당한 이해력을 보여주는 데 탁월
  - 특히 국어와 한국사에서 높은 점수 획득
  - 수학과 과학을 제외한 대부분의 과목에서 다른 모델보다 뛰어난 성능
- 한국어 맥락에 맞게 특별히 훈련된 모델이 한국어 공통 지식을 포착하는 데 특히 효과적이라는 것을 시사함
- 한국어 문장 구조를 가장 잘 이해하는 모델
  - 조사 + 명사/구
    - 조사: 은/는, 이/가, 을/를, -보다, -마저 등
    - 명사: 사람/사물/동물의 이름
  - 어미 + 어간 = 다양한 의미 파생
    - 어미: 로서/로써, 러/려, 으므로/음으로, 대요/데요
    - 어간: 동사, 형용사 등에 어미가 붙어 형태가 변할 때 변하지 않는 부분 → 산뜻하(어간) 다(어미)
  - 토큰화 기법으로 자연스러운 한국어 문장 구사
- 라인업
  - [cue:](https://cue.search.naver.com)
    - 네이버 생성형 AI 검색
  - 네이버 대화형 AI 서비스
    - 네이버 클로바 앱

<img width="1280" height="683" alt="image" src="https://github.com/user-attachments/assets/a6ba80f2-ea7a-4873-961c-b58f83653d57" />


#### HyperCLOVA X 모델의 기능 발전

|구분|2024|2025||
|--|--|--|--|
|플래그십 모델|**HCX-003**|**HCX-005**|**HCX-007**|
|경량모델||HCX-DASH-001|**HCX-DASH-002**|
|오픈소스|||**HCX-SEED**<br>**HCX-THINK**|
|CLOVA Studio|LangChain<br>Compatibility<br>Embedding V2|Router<br>Function Call|Structured Output<br>RAG Reasioning API<br>Re-ranker API|

- HCX-005
  - 국내 최초 Vision Model
    - 이미지 내용을 분석(이미지 분석)
  - 국내 최대 Context Size(128K)
- HCX-007
  - 추론(Reasioning) 모델(128K)
- HCX-DASH-002
  - 32K Context
  - 경량모델은 1/4~1/5 가격이라고 함
- HCX-SEED
  - 초경량 모델 3종
    - HCX-SEED-3B(vision)
    - HCX-SEED-1.5B
    - HCX-SEED-0.5B
- HCX-THINK
  - 경량 추론 모델 1종
    - HCX-SEED-Think


- 플레이그라운드 > [Model](https://guide.ncloud-docs.com/docs/clovastudio-model)

<img width="938" height="778" alt="image" src="https://github.com/user-attachments/assets/38835d61-3a78-4984-8c25-49098496fcf8" />

- 모델 선택

|THINK 모델|SEED 모델|DASH 모델|
|--|--|--|
|추론 능력을 강화한 생성형 AI 모델|상업용 무료 오픈소스 AI 모델|높은 속도의 경량화 AI 모델|
|복잡한 질의를 단계적으로 분석<br>텍스트와 이미지를 함께 이해해 정확한 추론 결과를 생성하는 모델|세 가지 모델<br>추론 능력을 갖춘 SEED Think 모델<br>오픈 소스로 제공|속도와 비용 면에서 탁월한 활용성 제공|

<img width="1559" height="715" alt="image" src="https://github.com/user-attachments/assets/0f2915c2-90cf-4a0a-8ace-f1324a0002f8" />

- 주요 고객 사례
  - 플래그십 모델 사례: HCX-005 채용 AI
    - LLM이 인재 추천, 추천 사유 생성
  - 플래그십 모델 사례: HCX-007 생성형 AI 기반 그룹사 검색 서비스 구축
    - LLM + RAG + Orchestrator
    - 사용자는 챗봇형태로 질의/답변
  - 경량 모델 사례: HCX-DASH-002 (32K Context) 금융 특화 sLLM 구축
    - 금융 규제로 인해 사내에 설치할 수 있는 모델


- 네이버 클라우드: 생성형 AI 도구(기능)

|||
|--|--|
|문장 생성 및 이미지 이해|Chat Completion|
|튜닝|튜닝|
|분류|라우터|
|외부 데이터 연동|Function call|
|외부 API 연동|Skill Trainer|
|문단 나누기|Segmentation|
|요약|Summarize|
|토큰 계산기|Tokenize|
|임베딩|Embedding|
|리랭커|Reranker|
|RAG추론|RAG Reasoning|

- 외부 데이터 연동 예시
  - 실시간 항공권 조회 → Foundation Model만 쓰면 안 됨(해당 모델은 몇 년 전 데이터 학습한 거니까)
  
### 4. 네이버 생성형 AI 개발 플랫폼 CLOVA Studio
- CLOVA Studio 서비스 모델
<img width="1365" height="633" alt="image" src="https://github.com/user-attachments/assets/05b946fa-ece4-468f-8df9-97a1f3a00645" />


- Basic
  - Foundation 모델만 보내면 최근 정보에 대응할 수 없기 때문에 외부 연동을 통해 최근 정보를 가져와서 단점 보완
<img width="1432" height="581" alt="image" src="https://github.com/user-attachments/assets/6d47e838-cf8a-4e8d-87c2-4558bab23748" />

- Exclusive
<img width="1405" height="583" alt="image" src="https://github.com/user-attachments/assets/50662b24-2037-452d-88b1-862675cf488f" />

- Neurocloud of HyperCLOVA X

- 랜딩 페이지
  1. 메뉴
     - CLOVA Studio에서 제공하는 메뉴
     - 플레이그라운드, 튜닝, 익스플로러, 라우터, 스킬 트레이너
  2. 설정
     - 언어 설정
     - 한국어, 영어, 일본어 지원
  3. 사용자명
     - 계쩡 관리, 조직 관리, 이용 현황 등
  4. 기능 소개
     - CLOVA Studio에서 제공하는 기능 소개

- 주요 기능
<img width="1398" height="622" alt="image" src="https://github.com/user-attachments/assets/c33e5ea5-a8bc-4e14-bc5d-6896c95bf5a9" />
<img width="1319" height="562" alt="image" src="https://github.com/user-attachments/assets/97e372a1-7f04-4bf3-88a1-9ab980067fd4" />

#### 플레이그라운드
- 인공지능 관련 지식이나 코딩 지식이 없어도 인공지능 기술을 사용할 수 있도록 설계
- 파라미터를 설정하고 프롬프트를 입력하여 텍스트를 생성할 수 있는 공간
- 주요 구성 요소
  1. 파라미터
     - 텍스트 생성을 위해 설정하는 값
  2. 기능
     - 플레이그라운드에서 제공되는 기능
  3. 제목
  4. **시스템**
     - 시스템 지시문 입력하는 영역
  5. **메시지 입력/실행**
     - 클릭하여 작업 실행

#### 플레이그라운드 파라미터
- Model
  - HCX-005 모델 제공
    - 이전 모델에 비해 주요 영역 전반에 걸쳐 수행 능력이 크게 향상됨
    - 시스템 프롬프트 지시를 더욱 잘 따르고 자연스러운 문장을 생성
    - 정교하고 복잡한 작업이 필요한 경우 활용 가능
  - HCX-DASH-002
    - 보다 간단한 작업 시 선택
    - 향상된 속도
      - HCX 기존 모델 대비 평균 응답 시간 단축 → 쾌적한 서비스 사용 환경 제공 가능
    - 비용 효율성
      - 문장 치환, 분류와 같은 간단한 작업부터 커스텀 챗봇까지 다양한 분야에서 뛰어난 성능 발휘
      - 기존 모델 대비 비용 1/4 - 1/5 수준으로 절감 가능
    - 유연한 모델 선택
      - 비즈니스 유즈케이스에 맞춰 유연하게 적용할 수 있어 선택의 폭이 확장됨 
  - 사용자 데이터를 학습한 튜닝 모델 선택 가능

- Top P, Top K
<img width="1355" height="690" alt="image" src="https://github.com/user-attachments/assets/d1389cb5-1b57-4760-8045-890f126f0c96" />

- Maximum Token(0-4096으로 설정)
  - AI가 생성하는 결과값의 최대 길이
  - 작업의 목적과 형태에 따라 값 조정
    - 카피라이터, 자소서, 소설 창작 등
  - HCX-005 모델은 최대 4096 토큰까지 허용
- Token
  - 문장을 형태에 따라 가장 작은 단위로 쪼갠 단어 조각
  - 주로 한국어 단어는 형태소 단위의 1-2 토큰으로 나뉨
   - 맛있어 → 맛/있어
 - 토큰은 돈 → 영어 토큰보다 한글 토큰이 조금 더 비쌈
- 토큰화
  - 토큰을 만드는 작업
  - 언어 모델이 자연어를 처리하기 위한 필수 과정

<img width="590" height="304" alt="image" src="https://github.com/user-attachments/assets/c52d14ca-f053-427e-b356-e21a3dcb1da9" />

- Temperature(0-1로 설정)
  - 확률 분포에 가중치 변화를 주어 문장의 다양성을 조절하는 값
  - 값을 낮게 설정하면 정형적인 결과값 생성, 후보 토큰 간 확률 값 차이 큼
  - 예측 가능한 결과물이 필요하면 값을 낮게 설정
  - default는 0.5
  - 값을 높게 설정하면 다양한 문장 생성 가능
  - 문장 품질 편차 발생, 토큰 간 확률 값 차이 적음
  - 소설 창작, 마케팅 문구 작성 등 창의적인 결과물이 필요하면 값을 높게 설정
- Pepetition penalty
  - 0~10으로 설정
  - 결과 문구 생성 시 동일 토큰에 감점 요소를 부여하는 값
  - 값이 낮으면 같은 결과를 반복 생성할 확률 높아짐
  - 값이 높으면 같은 결과를 반복 생성할 확률 낮아짐
    - Input값이 같아도 새로운 output 값을 생성함
- 활용 예시
  - 목록화
  - 키워드 추출
  - 어휘 치환


### 5. 프롬프트 엔지니어링(Prompt Engineering)
- 인공지능, 특히 언어 모델에게 정확한 정보나 창의적인 콘텐츠를 생성하도록 지시하는 방법을 연구하는 분야
- 프롬프트
  - 인공지능에게 주는 입력 문장이나 질문
  - 최적 입력 값: 지시문, 예제 등
  - 인공지능이 원하는 대답을 생성하도록 프롬프트를 잘 설계하는 것이 인공지능의 출력 품질을 결정
- In-Context Learning 유형
  - zero-shot
    - 예제를 구성하지 않고 요청
    - 예: 마케팅 문구 작성해줘, 분류해줘, 요약해줘 등
  - one-shot
    - 하나의 예제로 구성하는 프롬프트
    - 예: 마케팅 문구 작성해줘 / 상품: 커피 / 문구: 향기롭고 산미가 가득한 아메리카노
  - few-shot
    - 여러 개의 예제로 구성하는 프롬프트
- 프롬프트 작성 노하우
  1. 지시 사항은 최대한 구체적이고 명확하게 전달(역할, 형태 등)
  2. 프롬프트 예제(형식 등)를 다양하게 구성
  3. 계절감 있는 출력 결과가 나오게 하고 싶으면 예제에 날짜나 시즌 어휘 추가
  4. 대상자를 지정함으로써 대상자를 정확히 인식하면서 문장의 목적을 구체화할 수도 있음
  5. 단계별로 사고하기(CoT, Chain of Thought)
  6. 부정문보다 긍정문 사용
- AI Safety
  - 편견 및 차별
  - 인권침해
  - 사이버 공격
  - 불법 콘텐츠
  - 잘못된 정보
  - 일관성

### 6. 파인튜닝
- Pre-training vs. Fine tuning
  - Pre-training
    - 다양한 분야 기본 지식 습득 
  - Fine tuning: Foundation Model + 미세 조정
    - 특정 분야의 전문 지식을 쌓고 해당 분야의 세부적인 내용 학습
- Foundation Model(파운데이션 모델, 기반 모델)
  - 


### 


### 8. 검색증강생성(RAG)
- Retrieval
  - 요청된 정보를 가져온다
- Argumentation
  - **원래 정보에 덧붙이거나 보탠다**
- Generation
  - 응답을 텍스트로 생성한다

<img width="2560" height="1585" alt="image" src="https://github.com/user-attachments/assets/64231403-6b4d-4aff-ac49-06284adeccac" />

- RAG 도입 이유
  - DB 최신 정보 반영
  - Context 기반 정확도, 연관성 높은 답변 품질 확보
    - Hallucination 예방
  - Personal/Enterprise DB 검색 (활용)
  - Context 유사 케이스 (양적) 검색
  - 복합적인 조건의 DB 검색
  - 변동된 정보에 맞게 DB 비교 및 업데이트가 필요한 영역
- 도메인별 활용 예시
  - 고객 지원
    - 고객센터
    - 고객 대응 답변 품질 재고
    - 복잡한 쿼리 대응
  - 컨텐츠 제작
    - 정확한 최신의 데이터를 기반으로 한 컨텐츠 작업
    - 리서치 등
  - 교육
    - 커스텀 자료 제작
    - 최신 정보를 기반으로 한 교육 자료 계산
  - 의료
    - 최신 연구
    - 희귀 케이스
    - 최신 의료 시술/수술 방법 검색
    - 환자용 교육 자료(커스텀)
  - 법률
    - 유사 케이스 검색
    - 법 개정으로 인한 법 위반 케이스 단속

>결국 외부에 있는 LLM에게 보내는 건 똑같기 때문에 보안 문제 동일하게 있음
다만 컨텐츠 저장소(DB)에 있는 답변을 좀 더 많이 참조해 정확한 답변을 준다는 것
→ SLM을 고객사에 직접 넣어서 그 안에서 서비스하면 대응 가능

- RAG 구조

|순서|주요 내용|
|--|--|
|문서 로드(Load)|문서(PDF, word), RAW DATA, 웹페이지 등 데이터 읽기|
|분할(Split)|불러온 문서를 **chunk 단위**로 분할|
|임베딩(Embedding)|문서를 벡터 표현으로 변환|
|벡터DB(VectorStore)|변환된 벡터를 DB에 저장|
|검색(Retrieval)|유사도 검색(similarity, Multi-Query, Multi-Retriever)|
|프롬프트(Prompt)|검색된 결과를 바탕으로 원하는 결과를 도출하기 위한 프롬프트|
|모델(LLM)|언어 모델 선택|
|결과(Output)|텍스트, JSON, 마크다운|

<img width="818" height="307" alt="image" src="https://github.com/user-attachments/assets/8235a48c-1997-418e-8f77-6a1765bf9f81" />


>벡터DB는 관계형 DB와는 좀 다름
→ 기존의 관계형 DB가 행과 열의 2차원 형식으로 데이터를 표현하는 반면, 벡터 DB는 다차원 공간으로 그 개념을 확장시킨 것
→ 본격적인 고차원 데이터 저장 기술의 시작
[벡터 DB는 기존의 데이터베이스와 어떻게 다를까?](https://brunch.co.kr/@harryban0917/336)

- 비교 
<img width="1316" height="537" alt="image" src="https://github.com/user-attachments/assets/47c81010-2726-4394-a13e-608d97d2ccd2" />
<img width="1359" height="582" alt="image" src="https://github.com/user-attachments/assets/d3c4b86e-6e4e-4d1d-aa46-bf497b1b6d5b" />


- RAG 장점
  - Fine Tuning에 비해 시간과 비용이 적게 소요됨


### 9. LangChain
- 대규모 언어 모델(LLM)을 기반으로 애플리케이션을 구축하기 위한 오픈 소스 프레임워크
- LLM과 애플리케이션을 간소화할 수 있도록 설계된 SDK
<img width="531" height="178" alt="image" src="https://github.com/user-attachments/assets/9ab990b2-258b-42a8-b458-73293b8c988f" />



#### 구성 요소
- Chat Models
- Prompt Template
- Vector Stores
- Tools
- Document Loaders
- Text Splitters
- Output parsers

#### Document Loaders
1. URL Document Loaders
   - WebBaseLoader
   <img width="957" height="428" alt="image" src="https://github.com/user-attachments/assets/873d19d8-9572-49ea-a76a-3e64d245e5cb" />

   - MyPDFLoader
   <img width="834" height="503" alt="image" src="https://github.com/user-attachments/assets/ecb1f35b-fc87-4621-b2e1-2c1cd038661c" />




### 10. 네이버 스킬 트레이너
- 챗봇 vs. 스킬
  - 챗봇의 단점
    - 시나리오를 한땀 한땀 모두 만들어야 함
    - 데이터 양이 너무 많이 필요함
    - 말귀를 전혀 못 알아들음
    - 할 줄 아는 게 별로 없음
  - HyperCLOVA X SkillSet
    - 초거대 모델인 HyperCLOVA X의 한국어 능력을 활용해 최소한의 데이터로 사용자의 자연스러운 발화에 대응 가능
    - DB에 있는 지식을 활용해 답변
    - API를 이해하고 대신 수행
    - 멀티턴 대화 가능
    - 기존 챗봇이 수행하지 못하는 '스킬' 구현 가능
- 스킬 == 외부 API 연결
![](https://velog.velcdn.com/images/be2be2/post/9ad081c1-bb6d-4168-878b-ffd4f3ee2d68/image.png)

- 튜닝 vs 스킬
![](https://velog.velcdn.com/images/be2be2/post/a04f3925-1767-4ad4-aaa5-a8ead1d24ee5/image.png)
  - 스킬은 api를 통해 외부 데이터와 실시간 소통
     - Foundation Model이 기존에 학습한 내용보다 외부 연동 데이터 가져온 내용을 더 우선시 
  - 튜닝은 데이터를 넣어 추가 학습
- 활용 예시
![](https://velog.velcdn.com/images/be2be2/post/3d7137ae-f118-4d28-8d07-2e855ac2488a/image.png)
- 스킬 트레이너 사용 단계
![](https://velog.velcdn.com/images/be2be2/post/2bebec2e-5842-4200-a2bf-6c7f61e72ecd/image.png)
  - 스킬셋은 그냥 이름임
    - 여행
    - 금융
    - 교육 등
  - API Spec
    - 네이버 지역 정보
    - 유튜브 등
  - Manifest?
    - 
- 요약
  - Fine-Tuning
    - 특정 구조, 스타일, 판단 방식을 따라야 할 때
  - RAG
    - 질문에 답하기 위해 특정 정보가 필요할 때
    - 내부 정보를 주로 사용
  - Skill
    - **특정 조건에 맞는 정보 조회**가 필요할 때
    - 외부 api를 주로 사용
    


## CLOVA Studio 실습 1차
https://brunch.co.kr/@topasvga/5246

- 프롬프트 엔지니어링으로 80% 원하는 답변 얻을 수 있음
  - 나머지 20%를 채우기 위해 RAG 등을 이용
  
### 클로바 스튜디오 메뉴 보기
- Services > AI Services > CLOVA Studio
  - 상품 이용 신청해야 이용 가능
- 화면
![](https://velog.velcdn.com/images/be2be2/post/62f715ab-5798-44a5-94b4-f2b23f480609/image.png)

- 플레이그라운드
  - 프롬프트
  - 튜닝
    - 기존 모델에 add해서 새로운 모델 생성 → 모델명이 바뀐다!
  - 익스플로러
    - 다양한 도구 모음
    - 예: 데이터 생성(내 데이터 샘플을 원하는 양만큼 늘려줌)
  - 라우터
    - 분류 & 필터링
  - 스킬 트레이너
    - 외부 API 연결


### 플레이그라운드 사용
1. 목록화 가능
![](https://velog.velcdn.com/images/be2be2/post/346136d5-b301-415b-ac56-7da3879ee8d2/image.png)
   - 토큰 == 돈
   ![](https://velog.velcdn.com/images/be2be2/post/235e3ae0-345b-472e-8100-01e5c8651708/image.png)
2. 키워드 추출 가능
![](https://velog.velcdn.com/images/be2be2/post/bce5afc0-f112-4a3c-9211-8ba9765b3030/image.png)
   - skill로 유튜브 가지고 와서 동영상 요약하게 시킬 수도 있음
3. 어휘 치환 가능
![](https://velog.velcdn.com/images/be2be2/post/95b96050-0ae6-4927-8fe0-2523bb10b863/image.png)
4. 역할, 형태 등 지시 사항을 최대한 최대한 구체적이고 명확하게 전달
   - 시스템 설정 없음
   ![](https://velog.velcdn.com/images/be2be2/post/56823e1f-1619-4053-8e0e-9392640b2a65/image.png)
   - 시스템 설정
     - 친절하게 답변하는 과학 선생님입니다.
     - 설명은 8살 초등학생이 이해하기 쉽게 명료하고 간단하게 합니다. 

![](https://velog.velcdn.com/images/be2be2/post/277b671d-792c-4c01-b6b3-8511f184ed5f/image.png)
5. 프롬프트 예제(형식 등)를 다양하게 구성
![](https://velog.velcdn.com/images/be2be2/post/f7c4692c-5b43-43bf-a6a5-e4b06d2f9f89/image.png)
![](https://velog.velcdn.com/images/be2be2/post/5848efef-2d67-4f64-a888-c002a690d751/image.png)
메모리 기능 때문에 잘 안 됨
초기화하고 다시 하면 잘 됨
![](https://velog.velcdn.com/images/be2be2/post/63ad0e38-6903-4cf7-aae5-17978dc623d3/image.png)
6. 계절감 있는 출력결과가 나오게 하고 싶으면, 예제에 날짜나 시즌 어휘를 추가
![](https://velog.velcdn.com/images/be2be2/post/9c9f1703-17e4-45e6-934f-eb28d4cdfe90/image.png)
![](https://velog.velcdn.com/images/be2be2/post/8cecc7b2-3ce6-45b3-a1e9-b080dde6792a/image.png)
![](https://velog.velcdn.com/images/be2be2/post/b103569b-7d1f-409c-8cdb-d68c42f89690/image.png)
7. 대상자를 지정함으로써 대상자를 정확히 인식하면서 문장의 목적을 구체화할 수도 있음
![](https://velog.velcdn.com/images/be2be2/post/91cf356b-166f-4d6f-92fa-f93549e4fca5/image.png)
![](https://velog.velcdn.com/images/be2be2/post/4699c80c-c113-406d-a07f-17f75d7ccbce/image.png)
8. 단계별로 사고하기 CoT : (Chain of thought)
![](https://velog.velcdn.com/images/be2be2/post/65d9960c-320e-4587-be3b-33d15792715c/image.png)
![](https://velog.velcdn.com/images/be2be2/post/dbf03408-9c09-4f03-8d76-46c3d2768b3d/image.png)
![](https://velog.velcdn.com/images/be2be2/post/594effeb-1109-4a0a-945f-42bb1b7d6a72/image.png)
![](https://velog.velcdn.com/images/be2be2/post/1ec0a177-cb19-47be-9baf-a5bd7556b358/image.png)

### 마케팅 문구 생성하기
https://brunch.co.kr/@topasvga/5268
![](https://velog.velcdn.com/images/be2be2/post/201e91cd-9f8b-4e6a-8768-4d07f61d36eb/image.png)
![](https://velog.velcdn.com/images/be2be2/post/1c56d1d6-f2fb-4231-bb9c-2cf8d0d97d49/image.png)
![](https://velog.velcdn.com/images/be2be2/post/49b83153-eea7-4db5-b12f-2c68a3621cc0/image.png)
![](https://velog.velcdn.com/images/be2be2/post/326a5c41-00e6-4897-8313-2ddc5349f53f/image.png)
![](https://velog.velcdn.com/images/be2be2/post/de68bfa6-b564-446f-aed0-f72c81cf96a4/image.png)
- Temperature, Repetition penalty 조정
  - Repetition penalty
    - 결과 문구 생성 시 동일 토큰에 감점 요소를 부여하는 값
    - 값이 낮으면 같은 결과를 반복 생성할 확률 높아짐


### 여행계획 수립하기
![](https://velog.velcdn.com/images/be2be2/post/d0219a50-a9d6-4f5d-bc49-f4b8453bea56/image.png)

제목

여행계획 수립하기

시스템

- 지역, 기간, 여행 대상을 입력하면 여행 일정을 작성해줘
- 여행 일정은 표 형태로 구조화해서 보여줘
- 여행 계획을 작성할 때 각 일정별 예상 소요시간도 명시해줘
- 마지막 여행 일정은 마지막 여행 일정이라고 명시해줘

사용자

지역: 속초
기간: 2박3일
여행 대상: 3인 가족(엄마, 아빠, 자녀)


**속초로의 행복한 2박 3일 가족 여행**

| **날짜** | **일정** | **내용** | **예상 소요 시간** |
| --- | --- | --- | --- |
| 첫째 날 | 오전 출발 및 이동 | 서울에서 속초로 이동 (차량으로 약 2~3시간 소요) | 2~3시간 |
|         | 점심 식사 | 속초 도착 후 현지 맛집에서 해산물 요리 즐기기 | 1시간 30분 |
|         | 속초 중앙시장 방문 | 다양한 먹거리와 기념품 구경하기 | 1시간 30분 |
|         | 숙소 체크인 및 휴식 | 호텔 또는 리조트에 짐을 풀고 잠시 휴식 취하기 | 1시간 |
|         | 저녁식사 및 야경 감상 | 해변가 근처 식당에서 저녁 식사 후 속초의 아름다운 야경 관람 | 2시간 30분 |
| 둘째 날 | 아침 산책 및 조식 | 영랑호수지 둘레길 걷기 & 호텔에서 조식 즐기기 | 2시간 30분 |
|         | 설악산 국립공원 탐방 | 케이블카 탑승하여 권금성까지 올라가기, 자연 경관 감상하며 트레킹 하기 | 4시간 |
|         | 점심 식사 | 산채비빔밥 등 현지 음식 맛보기 | 1시간 30분 |
|         | 아바이마을 순대골목 탐방 | 드라마 촬영지로 유명한 곳에서 벽화와 예술 작품 감상하고, 순대와 전통주 체험하기 | 2시간 |
|         | 속초해변에서의 여유로운 시간 | 모래사장 산책과 바다 수영, 그리고 가족과 함께 사진 찍기 | 2시간 30분 |
| 셋째 날 | 아침 일출 감상 | 속초해변에서 멋진 일출 보기 | 1시간 |
|         | 체크아웃 및 마지막 쇼핑 | 숙소 체크아웃 후 속초 관광 시장에서 마지막 쇼핑 | 1시간 30분 |
|         | 귀가 | 서울로 돌아오기 (차량으로 약 2~3시간 소요) | 2~3시간 |

마지막 여행 일정입니다. 이 일정이 여러분의 소중한 추억이 되기를 바랍니다! 안전 운전하시고 즐거운 여행 되세요. 🚗🌊🏞️

### 수학 문제

3차원 좌표공간에서 식 $4z^{2} = x^{2} + y^{2} − 1$로 주어진 곡면을 S라 하자. 곡면 S가 점 P(1, 2, 1)을 지나는
직선을 정확히 두 개 포함하고 있음을 보이고, 두 직선이 이루는 예각을 θ라 할 때 cos θ의 값을 구하여라

- HCX-005

>
먼저, 구하고자 하는 두 직선의 방정식을 각각 l과 m이라 하겠습니다.
점 P(1, 2, 1)이 곡면 S 위에 있으므로, 이 점을 지나는 법선 벡터는 다음과 같이 구할 수 있습니다.
\[ \nabla f(1, 2, 1) = (2x, 2y, -8z) |_{(1,2,1)} = (2, 4, -8) \]
따라서, 두 직선은 다음과 같은 매개변수 방정식으로 나타낼 수 있습니다.
l: \( x=1+2t, \quad y=2+4t, \quad z=1-4t \)
m: \( x=1-2s, \quad y=2-4s, \quad z=1+4s \)
이제 두 직선 사이의 각도 θ를 구하기 위해 다음 공식을 사용합니다.
\[
\cos{\theta} = \frac{|a \cdot b|}{||a|| \cdot ||b||}
\]
여기서 a와 b는 두 직선의 방향벡터입니다.
두 직선의 방향벡터는 다음과 같습니다.
\( a=(2, 4, -4) \), \( b=(-2, -4, 4) \)
따라서,
\[
\cos{\theta} = \frac{|2(-2)+4(-4)-8(4)|}{\sqrt{2^2+4^2+(-8)^2}\sqrt{(-2)^2+(-4)^2+4^2}}
\]
이를 계산하면,
\[
\cos{\theta} = \frac{96}{28 \times 6} = \frac{16}{7}
\]
따라서 정답은 $\frac{16}{7}$ 입니다.

- HCX-007

주어진 곡면 \( S: 4z^2 = x^2 + y^2 - 1 \)이 점 \( P(1, 2, 1) \)에서 **두 개의 접선**을 가짐을 확인하고, 이들 접선이 이루는 예각 \(\theta\)의 \(\cos\theta\) 값을 구합니다.

---

### 단계별 풀이:
1. **점 \( P \)가 곡면 위에 있음 확인**:  
   \( 4(1)^2 = 1^2 + 2^2 - 1 \Rightarrow 4 = 4 \). 성립하므로 \( P \in S \).

2. **접선의 방향 벡터 도출**:  
   곡면의 편미분으로부터 **법선 벡터** \( \mathbf{n} = (2x, 2y, -8z) \)를 얻습니다.  
   \( P(1, 2, 1) \)에서 법선 벡터는 \( \mathbf{n}_P = (2, 4, -8) \).  

   접선은 법선 벡터와 수직이어야 하므로, 접선의 방향 벡터 \( \mathbf{v} = (a, b, c) \)는  
   \( 2a + 4b - 8c = 0 \)을 만족합니다.  

3. **방향 벡터의 일반형**:  
   \( \mathbf{v} \)는 스칼라 배수를 제외하면 \( (1, 2, k) \) 형태로 가정 가능합니다.  
   \( 2(1) + 4(2) - 8k = 0 \Rightarrow k = \frac{10}{8} = \frac{5}{4} \).  
   따라서 두 접선의 방향 벡터는  
   \( \mathbf{u} = (1, 2, \frac{5}{4}) \), \( \mathbf{w} = (-1, -2, -\frac{5}{4}) \) (스칼라 배수 고려).

4. **코사인 계산**:  
   \[
   \cos\theta = \frac{|\mathbf{u} \cdot \mathbf{w}|}{\|\mathbf{u}\| \|\mathbf{w}\|}
   \]  
   계산 결과:  
   \[
   \cos\theta = \frac{|-\frac{25}{4}|}{\sqrt{1^2 + 2^2 + (\frac{5}{4})^2} \cdot \sqrt{1^2 + 2^2 + (\frac{5}{4})^2}} = \frac{25/4}{(9/4 + 25/16)} = \frac{25}{21}.
   \]


**최종 답**: \(\boxed{\dfrac{25}{21}}\)

실제 풀이
![](https://velog.velcdn.com/images/be2be2/post/bb369498-3067-4645-9a42-8252b0d12359/image.png)
