---
title:  "Secret Management - Infisical"
description: "Infisical을 활용한 효율적인 비밀 관리 방법. API 키, 비밀번호 등 민감한 데이터를 안전하게 관리하고 팀과 공유하는 모범 사례."
categories: tech
tags:
  - secret manager
  - infisical
  - 비밀관리
  - security
  - DevOps
  - API-keys
  - 보안
  - configuration
  - environment-variables
toc: false
comments: true
---

이 Post에서는 Secret Management에 대해 간략히 알아보고 Secret Manager 중
하나인 Infisical의 간단한 활용에 대해 알아보고자 한다.

## Secret Management란?

개발 및 운영을 하다보면 credentials, private keys, token, password들과 같이
허락되지 않은 사용자들에게 남용되는 것을 방지해야 하는 민감한 정보를 다룰
수 밖에 없다. 이런 정보들 즉 Secret들을 안전하게 저장하고 접근을
엄격하게 관리하는 것을 Secret Management라고 한다.

## 기존의 Secret Management 방법들

일단 프로젝트가 매우 작고 관여된 개발자가 2-3명인 경우를 가정해보자.
이런 경우는 관여된 인원들이 적으므로 Secret들이 바뀔 때마다 어떤식으로든
공유하면 그만이다.

하지만 프로젝트의 규모가 커졌고 관여된 개발자들이
늘었을 때는 Secret들이 변경되었을 때 관여된 모든 사람들에게 알려주고
그들이 인지했다는 사실을 아는 것이 쉽지 않다.

이를 해결하기 위해 초창기에 프로그래머들이 주로 했던 방식은 Secret들을
소스코드에 하드코딩하는 것이다. 하지만 하드코딩을 하면 소스가
노출되는 곳들에 Secret들도 같이 노출된다는 단점이 있다.

소스코드와 Secret들을 분리하기 위해 고안해낸 방법이
바로 configuration 또는 .env 파일에 Secret들을 하드코딩하는 것이다.
하지만 이와 같은 방법도 아래와 같은 문제점을 안고 있다.
  
+ 단순히 configuration 또는 .env파일에 하드코딩했을 경우
  + 소스코드에 하드코딩하는 방식과 마찬가지로 configuation 또는.env 파일이
    소스 컨트롤 시스템과 엮여있어  repository가 노출되는 곳에 같이
    노출된다는 단점이 있다. Private Repository라고 하더라도 접근하는
    모든 구성원들에게 노출되는 것도 안전하다고 할 수 없다.

    실제로 github public repo에 AWS Acess Key가 노출되어 막대한
    요금이 부과됐다는 사례들을 많이 찾아볼수 있으며, 실제로 필자도 cloud가
    익숙하지 않을 때 실수로 public repo에 AWS Access Key를 노출하여
    AWS로부터 구제(?)를 받은 적이 있다.
    {: .notice--info}

+ 소스 컨트롤 시스템을 벗어나기 위해 .gitignore에 해당 configuration,
  .env파일을 추가한 경우
  + 이런 경우 가장 큰 문제는 해당 파일들에 대한 공유이다. A팀의 개발자가
    새로운 환경변수를 추가하고 B팀에 알려주는 것을 **잊는 것**은 매우 흔한
    일이다.

## 그렇다면 어떻게 위와 같은 문제점들을 해결할 수 있을까?

해결책 중 하나는 Secret Management Platform을 이용하는 것이다. 셋업만
잘한다면 로컬 개발환경부터 CI/CD 환경 및 프로덕션 환경까지 잘 사용할 수
있다. Secret Manager의 종류로는  Infisical, Vault, AWS Parameter Store,
Google Secret Manager등이 있으며 이 포스트에서는 Cloud Platform
종속적이지 않고 Open Source이며 이런
[장점]( https://infisical.com/infisical-vs-hashicorp-vault)이 있는
Infisical를 설치해보고 간단히 테스트해보고자 한다.

## Infisical

### install

----

[Github Page](https://github.com/Infisical/infisical)에서 명시한데로
설치해보자. 참고로 필자는 MacBook(M2) 환경이며 Github 페이지에서 언급한
것처럼 이 방식으로 설치하려면 Git과 Docker가 깔려있어야 한다.

```sh
git clone https://github.com/Infisical/infisical && cd "$(basename $_ .git)" && cp .env.example .env && docker-compose -f docker-compose.yml up -d
```

```sh
docker-compose ps
```

위 명령어들을 차례로 실행하면 아래의 이미지와 같이 infisical redis, nginx,
backend, mongo, frontend 컨테이너들이 떠 있음을 알 수 있다.

![docker-compose ps](https://www.dropbox.com/scl/fi/w2urtkhbicjlu7g4w79n3/docker-compose_ps.png?rlkey=r9oiqyzpzqmtk7f1x6033fu2p&raw=1)

확인 했으면 [http://localhost:80](http://localhost:80) 접속하고
**Create an account**를 클릭하여 **Continue with Email**를 통해
필요 정보를 입력하고 로그인 하면 아래와 같은 대시보드가 나온다.

![dashboard](https://www.dropbox.com/scl/fi/v9s9ggwsvesudeg4nyyvz/dashboard-intro.png?rlkey=66axwy1ejiv4qmozodtwptz69&raw=1)

### 프로그램내에서 Secret 불러오기

----

위에 확인한 대시보드에서는 CLI로 secret을 생성하고 조작하는 것에 대한
설명이 있으나 이 포스트에서는 실제 프로그램 내에서 Secret을 불러오는
방법을 아주 심플한 Python 스크립트를 통해 알아보고자 한다.  이를 위해서는
먼저 Service Token을 발급받아야 한다.  대시보드에서 Example Project의
Explore를 클릭하고 왼쪽에서 Project Setting -> Service Token 탭 ->
Create token 버튼을 눌러 Development 환경에서 Service Token을 발급받고 잘
메모해두도록 한다. 

필자는 Python Package Manager로 poetry를 사용하고 있기 때문에 아래와 같이 
poetry add infisical을 했지만 pip를 쓰시는 독자들은
pip install infisical하여 Python Library를 install 한다.

```sh
mkdir -p infisical-test && cd infisical-test
poetry init
poetry add infisical
```

아래 소스안의 token 위치에 위에서 메모해놓은
Service Token을 넣어주자.

test-infisical.py

```python
from infisical import InfisicalClient

client = InfisicalClient(
    token="Service Token You got from Dashboard",
    site_url="http://localhost:80",
)

url = client.get_secret("DATABASE_URL", environment="dev", path="/")
db_user = client.get_secret("DB_USERNAME", environment="dev", path="/")
db_password = client.get_secret("DB_PASSWORD", environment="dev", path="/")

print(f"DB_URL: {url.secret_value}\n")
print(f"DB_USERNAME: {db_user.secret_value}\n")
print(f"DB_PASSWORD: {db_password.secret_value}\n")
```

아래와 같이 스크립트를 실행하면

```sh
poetry run python test-infisical.py
```

아래와 같이 Secret들을 가져온 것을 볼 수 있다.

![Result](https://www.dropbox.com/scl/fi/yrz41h0po3kwh2htwwkyg/result.png?rlkey=9jmbogwkewdk3pmuh8e0t8n7n&raw=1)