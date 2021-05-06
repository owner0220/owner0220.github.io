---
title: 구글 클라우드 스피치 API 사용법 정리
date: 2019-02-17 23:29:53
categories:
 - GOOGLE CLOUD
tags:
 - Google_Studygem
---

> 19.02.17 기준 구글 클라우드 스피치 API 사용법 정리
>
> Google Cloud Speech API: Qwik Start
>
> 환경 : bash

### API 사용을 위한 사전 준비물

- 크롬
- 구글 개인 GCP 계정
- 계정에서 생성한 GCP Project ID
- 구글 API key

### API 사용 과정

- 프로젝트 아이디 세팅 및 확인
- 활성 계정 확인
- 프로젝트 아이디 확인

- Speech API request 생성
- Speech API request 요청



#### 준비하기

- Google Cloud Platform 에서 우리가 사용할 명령 입력 창 Cloud shell 을 열어준다.

- 아래 같은 창이 뜨는데 **START CLOUD SHELL** 클릭

- **활성 계정 확인하기** 위해 아래와 같은 코드를 입력하면 아래와 같이 확인이 가능한다.

  ```bash
  gcloud auth list
  ```

  ```bash
  Credentialed accounts:
   - <myaccount>@<mydomain>.com (active)
  ```

  ```bash
  Credentialed accounts:
   - google1623327_student@qwiklabs.net
  ```




- **프로젝트 계정 확인하기** 위해 아래와 같은 코드를 입력해 아래와 같이 확인

  ```bash
  gcloud config list project
  ```

  ```bash
  [core]
  project = <project_ID>
  ```

  ```bash
  [core]
  project = qwiklabs-gcp-44776a13dea667a6
  ```




#### 1. API Key 생성하기

- Google Cloude 에서는 curl을 사용해서 api 요청을 받는데 먼저 API Key가 필요하다.

  - **Navigation menu  >  APIs & services > Credentials**

  - 키를 발급 받기 위한 메뉴 **Create credentials** 찾아 클릭

  - **API Key** 를 찾아서 클릭

  - 여기서 만들어진 API Key를 복사해 curl 환경에서 환경변수로 사용할 수 있도록 다음 코드를 입력하고 = 옆에 붙여넣는다.

    ```bash
  export API_KEY =
    ```



#### 2. API 요청 전 작업

- ##### 요청에 사용할 json 파일 만들기

```json
{
  "config": {
      "encoding":"FLAC",
      "sample_rate": 16000,
      "language_code": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```

요청에 사용되는 json 파일의 구성은 크게

config 와 audio 두부분으로 나뉘는데

**config**

- encoding : 보낼 오디오파일 확장자 (FLAC, LINEAR16 권장)  (※ 필수)

  > 세부 설명 참고 : https://cloud.google.com/speech/reference/rest/v1/RecognitionConfig

- sample_rate : 보내는 오디오 Hertz (※ 필수)

- language_code : 오디오에서 나오는 언어 (※ 선택)

**audio**

- url : 오디오 파일 위치 url 주소



#### 3. API 요청 하기

- API Key와 함께 json 파일을 같이 보내 요청한다.

  ```bash
  curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
  "https://speech.googleapis.com/v1beta1/speech:syncrecognize?key=${API_KEY}"
  ```



**요청 내용**

- POST 요청

- 헤더

  Content-Type : application/json

  data : request.json

- 요청 주소

  https://speech.googleapis.com/v1beta1/speech:syncrecognize?key=${API_KEY}





- 요청이 성공하면 Json 응답이 다음과 같이 나온다.

  ``` bash
  {
    "results": [
      {
        "alternatives": [
          {
            "transcript": "how old is the Brooklyn Bridge",
            "confidence": 0.98267895
          }
        ]
      }
    ]
  }
  ```

  

결과에서 보여주는 값은

**transcript**  : audio에서 나오는 언어를 인식한 결과물

**confidence** : 결과의 정확도가 의 수치로 제공된다.