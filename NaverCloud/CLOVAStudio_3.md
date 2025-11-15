# CLOVA Studio 실습 3차
- 목표
  - 메일 작성 챗봇 UI 구성
  - RAG 구성

>내년부터는 RAG보다 Agent AI가 대세가 되지 않을까?<br>
>- RAG: 외부 데이터를 검색하여 정확한 답변을 생성하는 기술<br>
>- AI 에이전트: RAG 기술을 활용하여 더 나아가 실제 작업을 수행하는 시스템<br>
>- RAG가 정보 검색 및 생성에 초점을 맞춘다면, AI 에이전트는 추론, 계획, 의사 결정, 행동 수행까지 아우르는 더 광범위한 개념!<br>

- 목차
  1. 실습 환경 구성: 주피터 노트북
     - 우분투 서버 생성
  2. 메일 작성 챗봇 UI 구성
  3. 랭체인을 활용한 PDF 파일 기반의 RAG 구성
     - 주피터 노트북
   
## 1. 실습 환경 구성
- VPC 생성
  - Services > Networking > VPC > VPC 생성
    - VPC 이름: ai-vpc (보통 개발용일 경우 이름에 dev 붙여서 ai-dev-vpc라고 명확하게 쓴다고 함)
    - IP 주소 범위: `172.16.0.0/20` (서브넷 pub 1개만 쓸 거라 `172.16.0.0/16` 써도 됨)
    - 유형: NORMAL
- Subnet 생성
  - VPC > Subnet Management > Subnet 생성
    - Subnet 이름: ai-pub-subnet
    - IP 주소 범위: `172.16.3.0/24`
    - 용도: 일반
- ACG 수정
  - Server > ACG > ai-vpc-default-acg 수정
  - ai-vpc-default-acg 선택 > ACG 설정
  - ACG 규칙 설정: Inbound
    - 프로토콜: TCP
    - 접근 소스: myIp 또는 0.0.0.0/0 (전체) → myIp 버튼 누르는 걸 추천
    - 허용 포트: 1-65535 (전체)
- AI를 위한 VM 생성
  - 2분 소요
  - Service > Compute > Server
  - 서버 생성 > **NCP 서버 이미지** > Ubuntu > KVM > **ubuntu-22.04-base**
    - 서버 이름: ai-001
    - Network Interface `+추가` 버튼 누르기
    - 공인 IP: 새로운 공인 IP 할당
    - 스토리지 크기(GB): 50 GB로 변경
    - 새로운 인증키 생성: `ncp-ai-001`(또는 `ncp-{생성일}`
  - 공인 IP 확인 후 Putty를 사용해 공인 IP로 접속 → 패스워드 필요 
- 패스워드 조회
  - 서버 리스트에서 ai-001 서버 선택
  - `서버 관리 및 설정 변경`에서 `관리자 비밀번호 확인` 선택
  - ROOT 비밀번호 확인
    - 비밀번호를 복사해서 사용
    - 공란이 들어가는 경우가 있으므로 노트패드에 복사해서 사용하기
- 공인 IP 확인
  - 서버 리스트에서 ai-001 서버 공인 IP 확인
  - Putty를 사용해 공인 IP로 접속
- AI 서버 접속: Putty
  - 공인 ip 입력
  - SAVE
  - OPEN
- 패스워드 입력
  - 터미널 `login as:`에 ROOT 입력
  - 확인한 ROOT 비밀번호 붙여넣기
- 주피터 노트북 설치
  - 우분투 22.04 버전만 동작함
    - ★`apt-get update` 꼭 해주기!★

```
apt-get update
apt install python3-pip
pip3 install jupyter
```

▼

중간에 Y 누르기
보라색 창 뜨면 enter 누르기

▼

```
jupyter notebook --generate-config
jupyter notebook password
```

▼

Enter password:
Verify password:

▼

`vi .jupyter/jupyter_notebook_config.py`

▼

/c.ServerApp.ip 입력 후 엔터
`#`에 커서 놓고 x 두 번
o 눌러서 insert 모드 진입

▼

`c.ServerApp.ip = 'localhost'` → `c.ServerApp.ip = '*'`로 수정
esc 눌러서 insert 모드 나오기
`:wq!` 입력 후 enter

▼

`jupyter notebook --allow-root` 명령어로 jupyter notebook 실행(데몬)

▼

`공인 ip:8888`로 접속
데몬 떠 있는 상태 그대로 유지한 채로 접속해야 함!

▼

앞에서 설정한 password 입력

▼

<img width="630" height="547" alt="image" src="https://github.com/user-attachments/assets/107991bb-aacc-4a86-83c6-1d56e52ce4a8" />

▼

파일 업로드는 서버 터미널에서 `wget`으로 바로 올려도 되고 주피터 노트북 화면에서 upload 눌러서 해도 됨

- [AI 예제코드 모음](https://brunch.co.kr/@topasvga/5268)

## 2. 메일 작성 챗봇 UI 구성
- 작업 순서

|No.|작업 위치|작업 내용|
|--|--|--|
|1|CLOVA Studio|시스템, 메시지 작성<br>저장|
|2|CLOVA Studio|api 발급|
|3|ai-001 서버|주피터 노트북 실행|
|4|주피터 노트북|실습 파일 업로드|
|5|ai-001 서버|App.py 실행|
|6|웹 접속|챗봇 UI에 메일 초안 작성 요청 후 결과 확인|

1. CLOVA Studio
   - 파라미터 설정
     - title : 이메일 초안 작성 
     - 모델 : HCX-005 
     - Maximum tokens : 300 
     - Temperature : 0.2 
  - 우측 프롬프트 화면에서 아래 내용 설정

```
// 시스템

- 키워드를 포함하여 이메일 내용을 생성합니다.
- 메일 제목과 본문 내용을 출력합니다.
- 아래 예제와 유사한 포맷으로 작성합니다.
- 예제  
키워드:  
* 주제: 업무 협조  
* 요구사항: 요청하신 내용으로 작업했습니다. 확인해주세요.  
* 발신자: 임소희  
* 수신인: 최주희  
제목: 작업 내용 확인  
메일: 안녕하세요 최주희님, 저희 회사에서 진행하고 있는 프로젝트에 대해 말씀드리려고 합니다.  
현재 개발중인 웹사이트가 있는데 혹시 괜찮으시다면 디자인 시안을 한번 봐주실 수 있을까요?  
가능하시다면 오늘 오후 3시쯤 뵙고 싶습니다.  
감사합니다.  
임소희 드림


// 사용자 

키워드: 
* 주제: 미팅 파일 공유 요청 
* 요구사항: 미팅때 보여주셨던 파일을 보내주세요. 
* 발신자: ○○○ 
* 수신인: □□□ 


// 사용자 : 

키워드:  
* 주제: AI 테크밋업 준비를 위한 미팅 요청  
* 요구사항: AI 테크밋업 세미나 때 진행 시기와 내용 관련 미팅 요청, 미팅 일시는 4월25일 3~6시 중 
1시간  
* 발신자: ○○○ 
* 수신인: □□□ 
```

→ 결과 확인 후 저장


- 내 작업 > 코드 보기 > python > 복사


2. CLOVA Studio > API 키 > 테스트 API 키 발급
   - api 키 노트 패드에 복사해두기!


3. ai-001 서버: 주피터 노트북 실행
   - 서버 터미널에 `jupyter notebook --allow-root` 입력
   - 데몬 화면 떠 있는 상태에서 웹 브라우저로 접속
     - 검색창에 `공인 ip:8888` 입력


4. 주피터 노트북: 실습 파일 업로드
   - 주피터 노트북 환경(python3)에서 실습 파일 업로드
     - 서버 터미널에서 `wget https://kr.object.ncloudstorage.com/material/email-test.ipynb` 입력해 바로 받기
     - email-test.ipynb 다운로드 받아서 주피터 노트북 화면의 `upload` 눌러서 직접 업로드하기

>UI는 Streamlit 사용

- `email-test.ipynb` 내용

```python
# 1
# streamlit 설치 
!pip install streamlit 
# <Alt> + <Enter>로 셀 실행

# 2
!pip install streamlit_chat

# 3
# 설치 후 버전 확인
!pip list | grep streamlit 

# 4
# -*- coding: utf-8 -*-
import requests
import json

class CompletionExecutor:
    def __init__(self, host, api_key, request_id):
        self._host = host
        self._api_key = api_key
        self._request_id = request_id

    def execute(self, completion_request):
        headers = {
            'Authorization': self._api_key,
            'X-NCP-CLOVASTUDIO-REQUEST-ID': self._request_id,
            'Content-Type': 'application/json; charset=utf-8'
        }

        with requests.post(self._host + '/testapp/v1/chat-completions/HCX-003',
                           headers=headers, json=completion_request, stream=True) as r:    
            for line in r.iter_lines():
                if line:
                    result = json.loads(line.decode("utf-8"))
                    print(result['result']['message']['content'])

if __name__ == '__main__':
    completion_executor = CompletionExecutor(
        host='https://clovastudio.stream.ntruss.com',
        api_key='Bearer <각자의 API Key>',
        request_id='1ecd7eaadf06431e92f647ff4d47a0f6'
    )

    preset_text = [{"role":"system","content":"- 키워드를 포함한 이메일 초안을 작성합니다.\n- 메일 제목과 본문 내용을 출력합니다."},{"role":"user","content":"키워드:\r\n* 주제: 업무 협조\r\n* 요구사항: 요청하신 내용으로 작업했습니다. 확인해주세요.\r\n* 발신자: 임소희\r\n* 수신인: 최주희\r\n제목: 작업 내용 확인\r\n메일:\r\n안녕하세요 최주희님,\r\n저희 회사에서 진행하고 있는 프로젝트에 대해 말씀드리려고 합니다.\n\n현재 개발중인 웹사이트가 있는데 혹시 괜찮으시다면 디자인 시안을 한번 봐주실 수 있을까요?\r\n가능하시다면 오늘 오후 3시쯤 뵙고 싶습니다.\r\n감사합니다.\r\n임소희 드림\r\n###\r\n키워드:\r\n* 주제: 미팅 파일 공유\r\n* 요구사항: 미팅때 보여주셨던 파일을 보내주세요.\r\n* 발신자: 김사랑\r\n* 수신인: 유하나"}]

    request_data = {
        'messages': preset_text,
        'topP': 0.8,
        'topK': 0,
        'maxTokens': 300,
        'temperature': 0.2,
        'repeatPenalty': 5.0,
        'stopBefore': [],
        'includeAiFilters': True,
        'seed': 0
    }

    # print(preset_text)
    completion_executor.execute(request_data)
```

>`!pip list | grep streamlit`<br>
>- `!` 기호: 주피터 노트북 또는 IPython 환경에서는 셀 시작 부분에 `!`를 붙여서 해당 줄의 명령을 파이썬 코드가 아닌 운영 체제의 셸(Bash, Windows Command Prompt 등) 명령으로 실행하도록 지시합니다.<br>
>- 파이프(`|`): 셸 명령이므로 파이프(`|`)를 사용하여 pip list의 출력을 grep 명령의 입력으로 전달할 수 있습니다. grep은 전달된 텍스트에서 "streamlit" 문자열을 포함하는 줄만 필터링합니다.<br>
>- 환경 일치: 이 명령은 주피터 노트북이 실행되고 있는 동일한 파이썬 환경에 설치된 패키지 목록을 보여줍니다. 다른 가상 환경에 설치된 패키지는 표시되지 않을 수 있습니다.<br>
>- 대체 방법: grep 명령을 사용할 수 없는 환경(예: 일부 기본 Windows 환경)에서는 파이썬 코드를 사용하여 동일한 작업을 수행할 수도 있습니다<br>

```python
import pip
for package in pip.get_installed_distributions():
    if 'streamlit' in str(package.project_name).lower():
        print(package.project_name, package.version)
# 또는
import pkg_resources
for d in pkg_resources.working_set:
    if 'streamlit' in d.key:
        print(f"{d.key} {d.version}")
```


5. ai-001 서버 접속: app.py 실행
   - ssh로 ai-001 서버 로그인
   - putty - ssh 로 ai-001 서버 접속 후 실습 파일 다운로드
   - 아래 내용 터미널에 입력

```
cd ~ 
wget https://kr.object.ncloudstorage.com/material/app.py 
vi app.py 
```

▼

`api_key='Bearer <각자의 api키값>',` 부분 수정 → 발급받은 api 값 넣기 

▼

ai-001 서버에서 Streamlit 실행하기 
```
streamlit run app.py
```

6. 웹 접속
   - `ai-001 서버 공인 ip: 포트`로 접속하기
     - 포트 번호는 streamlit 실행하면 뜸
   - 챗봇 UI에 메일 초안 작성 요청 후 결과 확인

```
키워드:  
* 주제: AI 테크밋업 준비를 위한 미팅 요청  
* 요구사항: AI 테크밋업 세미나 때 진행 시기와 내용 관련 미팅 요청, 미팅 일시는 4월25일 3~6시 중 1시
간  
* 발신자: ○○○ 
* 수신인: □□□ 
```

<img width="815" height="760" alt="스크린샷 2025-11-13 170458" src="https://github.com/user-attachments/assets/b5cc63d1-df24-488d-bbdc-f1aa8f53e217" />


### 3. 랭체인을 활용한 PDF 파일 기반의 RAG 구성
- 랭체인(LangChain)이란?
  - 대규모 언어 모델(LLM)을 기반으로 애플리케이션을 구축하기 위한 오픈 소스 프레임워크
  - LLM과 애플리케이션의 통합을 간소화할 수 있도록 설계된 SDK
  - 데이터 소스, 임베딩, 벡터 데이터베이스 등을 랭체인을 이용해 LLM과 '연결'하는 게 핵심

<img width="1609" height="1050" alt="image" src="https://github.com/user-attachments/assets/982c1080-5958-488c-a553-e0e40f1b0c18" />


- 구성 요소
  - Chat Models
    - 메시지를 입력으로 사용하고 메시지를 출력으로 반환하는 언어 모델
  - Prompt Template
    - 사용자의 input값을 언어 모델에 전달하는 포맷
  - Vector Stores
    - 벡터값들은 벡터DB에 저장됨
    - 사용자가 쿼리와 가장 유사한 벡터값을 찾아서 리턴하는 역할을 수행하게 됨 
  - Tools(도구)
    - 에이전트, 체인 또는 LLM이 외부 세계와 상호작용하기 위한 인터페이스
  - Document Loaders
    - 외부의 다양한 소스로부터 문서 데이터를 끌어올 수 있음
    - 현재 100여 개 넘는 Document loader가 제공 중
    - HTML, PDF, code 형태의 문서를 로드할 수 있도록 지원
  - Text Splitters
    - 문서를 작은 청크 단위로 나누는 것
    - 검색의 효율성을 높이기 위해 중요한 과정
    - 현재 langchain의 text splitting에서는 recursive, HTML, Markdown 등 다양한 Splitter가 제공 중
  - Output parsers
    - 모델의 출력을 처리하고, 그 결과를 원하는 형식으로 변환하는 역할
    - 모델에서 반환된 원시 텍스트를 분석하고 특정 정보를 추출하거나 출력을 특정 형식으로 재구성하는 데 사용
- RAG(Retrieval Augmented Generation; 검색 증강 생성) 
  - Retrieval
    - 요청된 정보를 가져옴
  - Augmentation
    - 원래 정보에 덧붙이거나 보탬
  - Generation
    - 응답을 텍스트로 생성
  - 최신 정보 등을 잘 가져오게 만듦
- RAG 도입 이유
  - **DB 최신 정보 반영**
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
    - 최신 정보를 기반으로 한 교육 자료 제작
  - 의료
    - 최신 연구
    - 희귀 케이스
    - 최신 의료 시술/수술 방법 검색
    - 환자용 교육 자료(커스텀)
  - 법률
    - 유사 케이스 검색
    - 법 개정으로 인한 법 위반 케이스 단속
- RAG 구조

|순서|주요 내용|
|--|--|
|문서 로드(Load)|문서(pdf, word), RAW DATA, 웹페이지 등 데이터 읽기|
|분할(Split)|불러온 문서를 chunk 단위로 분할|
|임베딩(Embedding)|문서를 벡터 표현으로 변환|
|백터DB(VectorStore)|변환된 벡터를 DB에 저장|
|검색(Retrieval)|유사도 검색(similarity, Multi-Query, Multi-Retriever)|
|프롬프트(Prompt)|검색된 결과를 바탕으로 원하는 결과를 도출하기 위한 프롬프트|
|모델(LLM)|언어 모델 선택|
|결과(Output)|텍스트, JSON, 마크다운|


#### 실습
- 작업 순서

|No.|작업 위치|작업 내용|
|--|--|--|
|1|ai-001 서버 로그인|실습 파일 다운로드|
|2|주피터 노트북|실습 파일 업로드<br>새로운 주피터 환경(python3) 실행<br>네이버-랭체인 및 랭체인 커뮤니티 설치|
|3|클로바 스튜디오|익스플로러 > 임베딩|
|4|주피터 노트북|임베딩 값을 저장할 ChromaDB 설치<br>랭체인 Document loader 설정<br>실습에 사용할 PDF 파일 업로드<br>파일 내 텍스트를 textsplitter를 이용하여 chunk 단위로 나누고 확인<br>결과값을 벡터DB에 저장<br>쿼리에 대한 결과값을 잘 반환하는지 확인|

1. ai-001 서버 로그인: 실습 파일 다운로드
   - ai-001 서버 로그인 후 `wget`으로 다운로드
     - `wget https://kr.object.ncloudstorage.com/material/langchain.ipynb`

<img width="1199" height="564" alt="스크린샷 2025-11-13 171344" src="https://github.com/user-attachments/assets/3654cccf-9eb9-4d69-b985-91008073c6e4" />


2. 주피터 노트북 실행
   - File > Open > langchain.ipynb
   - 네이버-랭체인 및 랭체인 커뮤니티 설치
     - pip 오류 시 서버 터미널에서 `apt install pip`

```python
# 1
# 네이버-랭체인 및 랭체인 커뮤니티 설치
# 실행은  <CTRL> + <Enter> 
!pip install -U langchain 
!pip install -U langchain-naver 
!pip install langchain-community

# 2
# 발급받은 API키 값 입력
import getpass 
import os 
 
os.environ["CLOVASTUDIO_API_KEY"] = getpass.getpass( 
        "CLOVA Studio API Key 입력: " 
    )

# 클로바 스튜디오에서  API키 발급 후 입력 후 <엔터>

# 3
# Hyper CLOVA X 모델 이용: Chatmodel로 클로바 스튜디오 import 및 테스트
from langchain_naver import ChatClovaX 
chat= ChatClovaX( 
    model="HCX-005", 
    max_tokens=1000, 
    temperature=0.5, 
)


messages = [ 
    ( 
        "system", 
        "너는 영어를 한글로 번역하는 번역가야. 사용자가 입력한 문장에 대해서 정확히 번역해
줘.", 
    ), 
    ("human", "I love using NAVER AI."), 
] 
 
ai_msg = chat.invoke(messages) 
ai_msg 

# 4
# 임베딩 도구 이용: 텍스트 임베딩을 위한 CLOVA Embedding API version2 설정 
# 클로바 스튜디오 > 익스플로러 > 임베딩 클릭
# 임베딩 v2 선택 후 코드보기 클릭 
os.environ["NCP_CLOVASTUDIO_APP_ID"] = input("Enter NCP CLOVA Studio App ID: ")

# Embedding API 파이썬 코드 내 ‘request ID’를 Emedding App ID값으로 입력 후 <엔터>

# 5
from langchain_naver import ClovaXEmbeddings
 embeddings=ClovaXEmbeddings(
 model="bge-m3", #모델명(기본값:clir-emb-dolphin)
 ) 

# 6
# 임베딩 값을 저장할 ChromoaDB 설치 
!pip install -qU "langchain-chroma>=0.1.2" 

# 7
import chromadb 
from langchain_chroma import Chroma 

# 8
# 랭체인 Document loader설정 
!pip install -q pypdf 
!pip3 install pymupdf 
!pip3 install tiktoken 

# 9
import json 
from langchain_community.document_loaders import PyMuPDFLoader 


# 10
# 실습에 사용할 PDF 파일 업로드
# 터미널에서 wget https://kr.object.ncloudstorage.com/material/LangChain.pdf
# 주피터 노트북에서 !pip install wget 해도 되긴 함

# 해당 파일을 주피터 노트북 환경에 업로드: 파일 경로 지정
FILE_PATH = "./LangChain.pdf"

# 11
# 파일 내 텍스트 읽어오기
loader = PyMuPDFLoader(FILE_PATH) 
data = loader.load() 

# 12
# 파일 내 텍스트를 textsplitter를 이용하여 chunk 단위로 나누고 확인
from langchain_text_splitters import RecursiveCharacterTextSplitter 
text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder( 
    chunk_size=1000, 
    chunk_overlap=200 
) 
documents=text_splitter.split_documents(data)

print(len(documents)
print(len(documents[2].page_content)

# 13
# 결과값을 벡터DB에 저장
vectordb = Chroma.from_documents( 
        documents=documents, 
        embedding=embeddings 
  )

# 쿼리에 대한 결과값을 잘 반환하는지 확인 
results = vectordb.similarity_search_by_vector( 
    embedding=embeddings.embed_query("CLOVA Studio에 연동하여 LangChain을 사용하려면 필
요한 파이썬 최소버전은?"), k=1 
) 
for doc in results: 
    print(f"* {doc.page_content}]")

# 14
from langchain_naver import ChatClovaX 
 
llm = ChatClovaX( 
    model="HCX-005", 
    max_tokens=1800, 
)

# 15
from langchain_core.prompts import PromptTemplate 
 
prompt1 = PromptTemplate.from_template( 
    """당신은 질문-답변(Question-Answering)을 수행하는 친절한 AI 어시스턴트입니다. 당신의 임무
는 원래 가지고있는 지식은 모두 배제하고, 주어진 문맥(context) 에서 주어진 질문(question) 에 답
하는 것입니다. 
검색된 다음 문맥(context) 을 사용하여 질문(question) 에 답하세요. 만약, 주어진 문맥(context) 에
서 답을 찾을 수 없다면, 답을 모른다면 `주어진 정보에서 질문에 대한 정보를 찾을 수 없습니다` 
라고 답하세요. 
 
#Question: 


{question} 
 
#Context: 
{context} 
 
#Answer:""" 
) 
 
retriever = vectordb.as_retriever( 
    search_type="mmr", search_kwargs={"k": 1, "fetch_k": 5} 
)


# 16
from langchain_core.runnables import RunnablePassthrough 
from langchain_core.output_parsers import StrOutputParser 
 
# 체인을 생성합니다. 
rag_chain = ( 
    {"context": retriever, "question": RunnablePassthrough()} 
    | prompt1 
    | llm 
    | StrOutputParser() 
)

# 17
rag_chain.invoke("CLOVA Studio에 연동하여 LangChain을 사용하려면 필요한 파이썬 최소버전은?") 


```

<img width="1235" height="369" alt="스크린샷 2025-11-13 171459" src="https://github.com/user-attachments/assets/1195f8de-2781-4f8e-ae8f-fab9ecdfa5a7" />

- HyperCLOVA X 언어 모델 기반의 RAG 구성 
  - PromptTemplate은 단일 문자열의 서식을 지정하는 데 사용되며, 일반적으로 간단한 입력에 사용되는 템플릿
  - PromptTemplate은 Python의 문자열 포맷팅을 사용하여 동적으로 특정한 위치에 입력 값을 포함시킬 수 있어, 다양한 상황에 맞게 프롬프트를 구성 가능
    - 아래 예제처럼 `{question}`, `{context}`와 같은 사용자 입력 및 매개 변수를 언어 모델에 전달하게 할 수 있음
  - retriever를 통해 관련 문서를 context로 불러오고, context와 question이 프롬프트 템플릿으로 전달되고, llm 모델을 통해 프롬프트가 실행되고, StrOutputParser를 통해 답변만 파싱해 문자열로 출력 
