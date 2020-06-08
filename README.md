:bookmark_tabs:TagDocx:bookmark_tabs:
=================================
<p align="center"><img src="https://user-images.githubusercontent.com/29905149/83727296-cbe41000-a67f-11ea-954d-1382433f29a3.png" height="40%" width="40%"></p>



:family: 0. 팀 소개
-------------------
**Team GODOCX**
|Member|Github-link|
|---|---|
|정유진|[Yoojin99](https://github.com/Yoojin99)|
|장소정|[sojung127](https://github.com/sojung127)|
|조예은|[yjo5252](https://github.com/yjo5252)|
|박유진|[jinee525](https://github.com/jinee525)|


:bookmark: 1. TAGDOCX 소개 :bookmark:
---------------------------------------------------
**인공지능을 활용하여 추출한 문서의 형식, 내용 태그를 활용하여 
편리한 검색 & 분류 를 제공하는 프로그램**

### 문제 인식
* 문서 검색 시 생기는 불편함
  - 문서의 제목과 저장된 위치를 정확히 알지 못할 경우 문서를 찾을 수 없음
  - 문서의 내용 및 키워드로 문서를 검색 할 수 없음
  - 찾은 문서의 내용 확인을 위해 문서를 열어야 함
  
* 문서 관리(분류)시 생기는 불편함 
  - 문서 분류 시 문서의 내용을 일일이 확인 후 분류 작업 해야 함
  - 하위 폴더 및 문서의 수가 많은 경우 정리와 탐색 시간이 오래 걸림
  
### 개발 목적
자연어 처리 기술과 인공지능을 활용하여 기존 문서 관리 프로그램의 한계점을 개선하며 문서를 분석해 할당한 태그를 이용하여 문서 관리 시간을 단축한다

:page_facing_up: 2. TAGDOCX 설명 :page_facing_up:
--------------------------------------------------
**TAGDOCX는 문서를 분석 후 형식, 내용 태그를 할당하고 태그를 이용한 문서 관리 기능을 지원한다.**

문서를 분석한 후 딥러닝(SGDClassifier, LDA)을 이용하여 학습한 모델로 문서의 내용, 형식 태그를 추출한다.
  
### 주요 기능
<img src="https://user-images.githubusercontent.com/29905149/83729001-02bb2580-a682-11ea-9ad2-3ee75515a206.png" height="70%" width="70%"> 
    
    
## 3. 구현

  ### 1) 플랫폼
- 데스크톱 프로그램
- 후에 웹 어플리케이션, 모바일 앱으로 제작할 것을 고려

  ### 2) 개발환경
  <img src="https://user-images.githubusercontent.com/29905149/83729137-339b5a80-a682-11ea-960c-ca48abd75dd8.png" height="50%" width="50%">

  ### 3) 기능
  
  #### 형식 태그 할당
  > 지도학습 방법을 이용하여 문서의 형식 태그를 추출하는 모델을 학습시켰다. 태그를 추출
  하기 위한 알고리즘은 확률론적 경사 하강법(SGD)을 이용하였다. 모델의 입력값으로는 각 문서를 토큰 리
  스트로 변환하여 각 문서에서 토큰의 출현 빈도를 센 후 문서를 BOW 인코딩 벡터로 변환하는 작업을 수행
  하는 CountVectorizer를 이용하여 문서들을 벡터화한 값을 사용하였다. 인터넷에서 수집한 공고, 기사, 논
  문, 지원서 형식의 문서들을 각 200개씩 수집하여 벡터화한 후 모델의 학습 데이터로 사용하였고 이를 지
  도학습 방법으로 SGD Classifier 모델을 학습시켰다. 이 학습된 모델에 문서를 분석한 벡터를 입력값으로
  넣으면 SGDclassifier 클래스를 이용하여 모델이 문서의 벡터값을 보고 문서가 어떤 형식에 속하는지 판단
  하여 결과를 도출한다. 이 도출된 형식이 형식 태그로 설정된다.
  분류할 수 있는 형식으로는 논문, 기사, 공고문, 지원서로 총 4개의 항목을 두었다. 형식별로 학습 데이터를
  200개씩 수집하였고 테스트 데이터로 다른 데이터를 형식별로 100개씩 수집하였다. 문서를 분석하여 생성
  된 벡터값을 입력값으로 하고, 출력 값으로는 가장 유력한 문서의 형식이 될 수 있도록 하였다.
  <img src="https://user-images.githubusercontent.com/29905149/83731598-c4276a00-a685-11ea-863b-5eb5d2e71bb5.png" height="50%" width="50%">
  
  #### 내용 태그 할당
  > 내용 태그 추출을 위해 문서를 분석하기에 앞서, 문서의 제목이 가장 핵심적인 내용을 담는다고 생각했기
때문에 제목에서 나온 단어를 내용 태그로 우선 할당하였다. 문서를 분석하였을 때 확인할 수 있는 글자의
크기 및 위치를 이용하여 글자 크기가 가장 크면서 문서의 윗부분에 있는 글자들을 제목으로 추출한 뒤 제
목에서 명사만을 추출하여 내용 태그로 우선 할당하였다. 그 후 제목에서 추출된 명사들이 내용 태그의 최
대 개수인 10개를 넘지 않으면 남은 개수를 LDA 알고리즘을 활용하여 뽑아낸 단어들로 채워 넣었다.
제목 추출을 통해 내용 태그 10개를 채우지 못할 경우, 추가로 문서 전체를 분석하여 내용 태그를 추출, 할
당하였다. 이때 문서 분석에는 LDA 알고리즘을 사용하였는데 이는 Latent Dirichlet Allocation의 준말로 이
루어진 문서에 대하여 각 문서에 어떤 주제들이 존재하는지를 서술하는 확률적 토픽 모델 기법의 하나다.
문서를 분석하여 단어들을 토큰화시키고, 자연어 처리를 하는 Kkomoran 파이썬 라이브러리를 사용하여
문서에서 명사만을 추출하였다. 추출된 명사 리스트들을 LDA 모델의 입력 값으로 설정하였다. LDA 모델
의 출력 값으로는 한 토픽 안에 5개씩 단어를 포함한 5개의 토픽을 설정하였다. (총 25개의 단어 추출) LDA
에서 제공하는 토픽별, 단어별 대표성을 나타낼 수 있는 가중치를 이용하여 토픽마다 가장 대표성이 높다
고 설정된 단어 한 개씩 총 다섯 단어와, 토픽 중 가장 대표성이 높은 토픽의 다섯 단어를 포함하여 총 중복
처리를 한 후 추출하였다.
<img src="https://user-images.githubusercontent.com/29905149/83731601-c5589700-a685-11ea-8052-43179efe0033.png" height="70%" width="70%">
<img src="https://user-images.githubusercontent.com/29905149/83731617-cdb0d200-a685-11ea-9260-4786d4447e1d.png" height="70%" width="70%">


  ### 4) 프로그램 기능 흐름도
 <img src="https://user-images.githubusercontent.com/29905149/83735513-731a7480-a68b-11ea-8998-3a0718097daf.png" height="50%" width="50%">



## 4. 인터페이스

|Member|Github-link|
|---|---|
|메인화면<br><img src="https://user-images.githubusercontent.com/29905149/83731115-ea98d580-a684-11ea-9b00-5ea0652c1276.jpg" height="300" width="450">|주기적 태깅 대상 폴더 설정<br><img src="https://user-images.githubusercontent.com/29905149/83731043-d9e85f80-a684-11ea-81d9-4946709ba406.JPG" height="300" width="450">|
|분류할 폴더 선택<br><img src="https://user-images.githubusercontent.com/29905149/83731040-d94fc900-a684-11ea-8fe5-674b4c13a491.JPG" height="300" width="450">|분류 묶음 설정<br><img src="https://user-images.githubusercontent.com/29905149/83731038-d94fc900-a684-11ea-95cf-884d07605bd3.JPG" height="300" width="450">|
|분류 결과<br><img src="https://user-images.githubusercontent.com/29905149/83731036-d8b73280-a684-11ea-93e0-11081b03dada.JPG" height="300" width="450">|문서 검색<br><img src="https://user-images.githubusercontent.com/29905149/83731029-d6ed6f00-a684-11ea-91e3-ddb4413565cf.JPG" height="300" width="450">|
|검색 후 문서 작업<br><img src="https://user-images.githubusercontent.com/29905149/83731030-d81e9c00-a684-11ea-920b-676e0d26f155.JPG" height="300" width="450">||
  
## 5. 구현 영상


