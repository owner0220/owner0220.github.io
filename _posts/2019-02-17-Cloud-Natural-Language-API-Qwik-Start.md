---
title: 구글 클라우드 자연어 처리 API 사용법 정리
date: 2019-02-17 00:00:00
categories:
 - PRODUCT
tags:
 - Google Cloud
---

> 19.02.17 기준 구글 클라우드 자연어 처리 API 사용법 정리
>
> Cloud Natural Language API: Qwik Start
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

- Natural API request 생성
- Natural API request 요청



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




#### 1. API Key 생성 및 서비스 Credential 생성, 로그인

- ##### API Key 생성하기

  - **GOOGLE_CLOUD_PROJECT** 환경 변수 설정하기

    ```bash
    export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
    ```

  - Natural Language API 사용을 위한 **Credential** 및 **사용 어카운트** 생성

    ```bash
    gcloud iam service-accounts create my-natlang-sa \
      --display-name "my natural language service account"
    ```

  - **어카운트 로그인을 위한 key.json 파일 생성**

    ```bash
    gcloud iam service-accounts keys create ~/key.json \
      --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
    ```

  - **GOOGLE_APPLICATION_CREDENTIALS** 환경 변수 설정

    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
    ```

    

#### 2. 분석 요청 보내기

- 예제

  `Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'`

  문장을 분석하기 위해 아래와 같은 코드로 분석 요청

  ```bash
  gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'."
  ```



- 결과

  - json 파일

  ```json
  {
    "entities": [
      {
        "name": "Michelangelo Caravaggio",
        "type": "PERSON",
        "metadata": {
          "wikipedia_url": "http://en.wikipedia.org/wiki/Caravaggio",
          "mid": "/m/020bg"
        },
        "salience": 0.83047235,
        "mentions": [
          {
            "text": {
              "content": "Michelangelo Caravaggio",
              "beginOffset": 0
            },
            "type": "PROPER"
          },
          {
            "text": {
              "content": "painter",
              "beginOffset": 33
            },
            "type": "COMMON"
          }
        ]
      },
      {
        "name": "Italian",
        "type": "LOCATION",
        "metadata": {
          "mid": "/m/03rjj",
          "wikipedia_url": "http://en.wikipedia.org/wiki/Italy"
        },
        "salience": 0.13870546,
        "mentions": [
          {
            "text": {
              "content": "Italian",
              "beginOffset": 25
            },
            "type": "PROPER"
          }
        ]
      },
      {
        "name": "The Calling of Saint Matthew",
        "type": "EVENT",
        "metadata": {
          "mid": "/m/085_p7",
          "wikipedia_url": "http://en.wikipedia.org/wiki/The_Calling_of_St_Matthew_(Caravaggio)"
        },
        "salience": 0.030822212,
        "mentions": [
          {
            "text": {
              "content": "The Calling of Saint Matthew",
              "beginOffset": 69
            },
            "type": "PROPER"
          }
        ]
      }
    ],
    "language": "en"
  }
  ```

- The entity `name` and `type`, a person, location, event, etc.

- `metadata`, an associated Wikipedia URL if there is one

- `salience`, and the indices of where this entity appeared in the text. Salience is a number in the [0,1] range that refers to the centrality of the entity to the text as a whole.

- `mentions`, which is the same entity mentioned in different ways.