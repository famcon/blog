---
title: "Heroku and GCP 배포"
category: "TIL"
date: "2022-06-01"
desc: "NodoeJS로 구현한 서버 배포하기"
thumbnail: "../images/heroku-gcp.png"
alt: "alt"
---

![](https://images.velog.io/images/c-on/post/b39bd412-458c-4f6b-a860-0c8b37889220/image.png)

# 🔨 Heroku

헤로쿠는 aws, gcp와 같은 클라우드 플랫폼이다.
한 계정당 최대 5개까지의 앱이 무료라서 백수인 내가 사용하기 좋은 것 같다.

결제 정보를 등록하면 MySQL DB도 제공해준다.

**다만, 서버가 사용되지 않을 때는 sleep에 빠진다.**
30분정도 사용하지 않다가 서버에 접속하면 처음에는 응답을 받을 수 없기에 서버를 계속 깨워주는 프로그램이 있는 것 같다.

> 나는 프론트엔드 서버가 GCP에 배포되어있어서 처음 사이트에 접속할 때 의미없는 요청을 헤로쿠 서버에 날려서 잠을 미리 깨우는 방법을 사용했다.

따라서 발표나 포트폴리오 용으로 사용하기에 적절하며 실제 서비스용으로 사용하려면 돈을 내고 업그레이드를 하면 되지만 비용을 지불해야 한다면 aws나 gcp를 사용하는게 더 이득이지 싶다.

## 배포

### 1. 가입

먼저 [헤로쿠 사이트](https://id.heroku.com/login)로 이동하여 가입을 진행한다.

---

### 2. 앱 생성

![](https://images.velog.io/images/c-on/post/8de25c99-5e81-4536-903f-97d74d1ce69b/image.png)

가입 후 로그인을 하면 개인페이지에 접속된다.
우측에 new 버튼에서 Create new app 를 클릭해준다.

![](https://images.velog.io/images/c-on/post/259ec891-d2cb-4382-8e7e-3b9a10d8c05e/image.png)

앱 이름은 프로젝트를 나타내는 주는 이름으로 지어주고 지역은 미국과 유럽중에 고르는 것이라서 크게 차이없으므로 아무거나 선택해서 Create app을 눌러주면 앱 생성 성공!

---

### 3. CLI다운

![](https://images.velog.io/images/c-on/post/b042179b-c551-4626-881b-9a880dd73b9b/image.png)

생성한 앱의 deploy 메뉴로 이동하면 배포 방법의 안내가 보인다.

그 중 우리는 헤로쿠 CLI를 다운하여 cmd로 헤로쿠 배포를 진행할 것이다.
[다운로드 주소](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

또한 heroku CLI는 GIT이 필수이기 때문에 설치되어 있지 않다면 [이곳](https://git-scm.com/downloads)에서 함께 설치해준다.

터미널에 `heroku --version`을 입력했을 때 `heroku/숫자 어쩌고 저쩌고`가 나오면 설치 성공!

---

### 4. CLI 로그인

터미널을 띄워서 `heroku login`을 입력한다.

![](https://images.velog.io/images/c-on/post/95332d49-823e-44d7-a5a5-1cd8383e86b1/image.png)

이렇게 아무 키를 눌러주라고 뜨면 정말 아무키나 눌러준다.
그러면 브라우저가 열리고 로그인 입력 창이 나온다. 로그인을 성공하면

![](https://images.velog.io/images/c-on/post/dbb9fd27-f322-4f19-bd49-b7402f4eec7e/image.png)

이렇게 CLI에서 로그인이 이뤄진다.

---

### 5. CLI명령으로 배포 마무리

이제 몇가지 명령만으로 쉽게 배포를 할 수 있다.

![](https://images.velog.io/images/c-on/post/884d98eb-2a7e-4e69-b413-e9fb90df0da8/image.png)

먼저 내가 생성한 앱 이름은 `caarcamping`이라서 명령에 포함된 `caarcamping`은 자신이 생성한 앱 이름으로 변경해서 사용한다.

```
heroku git:clone -a caarcamping
```

나의 헤로쿠 가상서버에 프로젝트 소스코드를 클론한다.

```
$ git add .
$ git commit -am "make it better"
$ git push heroku master
```

명령어를 차례로 입력하여 헤로쿠 원격 저장소에 소스코드를 업로드 해준다.

![](https://images.velog.io/images/c-on/post/9bb3fa87-3503-42b6-9825-c2044a644b22/image.png)

이런 명령줄이 촤라락 뜨면

배포 끝.

수정하게 되면

```
$ git add .
$ git commit -am "make it better"
$ git push heroku master
```

이 명령어 다시 입력해주면 된다.

---

### tip. 브랜치

팁으로 배포하는 브랜치는 나누는 게 좋으니까

```
git checkout -b heroku-deploy
```

로 브랜치를 생성하고

```
git push heroku heroku-deploy:master
```

를 명령해서 배포할 수도 있다.

> GIT 개념이 없으면 브랜치 변경하거나 push, pull 꼬일 수 있으니 브랜치를 생성할거면 GIT 공부를 먼저 해야함.

---

<br>

# 🔨 GCP

GCP는 google cloud platform이다.

GCP에는 app engine이라는 플랫폼이 있다. 비교적 쉬운 배포 서비스를 제공한다.
구글 웹사이트에 사용되는 것과 동일한 기술이 사용되어 앱엔진을 통해 배포를 하면 코드만 제공해주면 인프라는 알아서 관리해준다.

AWS에서 배포를 하게되면 EC2+로드밸런서+route53 ... 등 설정할 것이 많은 반면 앱엔진은 많은 설정들을 자동으로 혹은 간단한 옵션으로 관리해주며 무중단 서비스도 알아서 해준다. 👍

## 문서보기

[GCP문서](https://cloud.google.com/appengine/docs/flexible/nodejs?hl=ko)는 정말 잘 정리되어있다. 보기 깔끔하며 번역도 잘되어있다. 단점은 필요한 내용이 어디에 있는지 못찾을 경우 구글링을 해야하는데 한글로 된 정보가 많지 않다는 것.

## 배포

### 1. [GCP에 프로젝트 생성](https://console.cloud.google.com/home)

[구글클라우드플랫폼](cloud.google.com)으로 이동해서 가입하고 로그인한다.

카드 계정 등록을 해야 사용할 수 있다.
조금 귀찮지만 카드랑 민증 찍어서 인증해야 구글에서 확인 후에 등록을 시켜줬던 걸로 기억한다.

여튼 계정이 생성되면

![](https://images.velog.io/images/c-on/post/fe1f282b-988c-4f23-9d18-7ba8a26429a2/image.png)

왼쪽 상단 Google Cloud Platform 오른쪽에 프로젝트 어쩌고있을텐데(사진에선 프로젝트가 존재해서 `caarcamping`이라는 프로젝트명으로 나와있음) 그것을 클릭.

`프로젝트 선택` 다이얼로그가 나오면 오른쪽 상단의 `새 프로젝트`를 클릭해서 나의 프로젝트를 대표해주는 이름으로 하나 생성해준다.

---

### 2. [App Engine](https://console.cloud.google.com/appengine)

프로젝트 생성을 마치면 왼쪽 메뉴를 클릭해서 `App Engine` 을 찾아서 이동하거나 `제품 및 리소스 검색`이라는 검색창에 입력해서 이동한다.

나와있는 안내에 따라 잘 따라가주면 된다.

Region선택은 우리나라랑 가까운거 해주고 Langauge는 nodeJS로 선택해준다

---

### 3. [Cloud SDK](https://cloud.google.com/sdk/docs/install?hl=ko)

[다운로드문서](https://cloud.google.com/sdk/docs/install?hl=ko#installation_instructions)로 이동하여 SDK를 설치해준다.

이미 설치한 경우 넘어가기

---

### 4. [app.yaml로 앱 구성](https://cloud.google.com/appengine/docs/flexible/nodejs/configuring-your-app-with-app-yaml?hl=ko#intro)

버전과 URL이 포함된 Node.js 앱의 런타임 구성을 app.yaml 파일에 지정할 수 있다. 이 파일은 특정 서비스 버전의 배포 설명자 역할을 한다.

서버를 구동하는 파일과 동일 경로에 `app.yaml` 파일을 생성한다.

```
runtime: nodejs
env: flex

manual_scaling:
  instances: 1
resources:
  cpu: 1
  memory_gb: 0.5
  disk_size_gb: 10
```

app.yaml에 다음 코드를 작성해준다.
최대한 과금이 안되도록 하드웨어를 저저저용량으로 셋팅했다.

레진코믹스가 앱엔진으로 서버를 관리한다는데 이렇게 실제 서비스를 할 때는

```
runtime: nodejs
env: flex
```

이 코드만 적어주면 자동으로 구글이 트래픽에 맞춰 사양을 조절해준다.

> app.yaml파일에 포트를 지정해주지 않으면 디폴트로 8080포트를 사용한다. 포트를 변경하려면 [이 문서](https://cloud.google.com/appengine/docs/flexible/nodejs/reference/app-yaml?hl=ko#network_settings)를 참고한다.

---

### 4. [Node.js용 Google Cloud 클라이언트 라이브러리 설치](https://cloud.google.com/appengine/docs/flexible/nodejs/using-nodejs-libraries?hl=ko#installing_google_cloud_client_libraries_for_nodejs)

```
npm install --save @google-cloud/storage
```

Node.js용 Google Cloud 클라이언트 라이브러리는 Node.js 개발자가 Cloud Datastore, Cloud Storage 등의 Google Cloud Platform 서비스와 통합하는 데 일반적으로 사용하는 방법이다.

위 명령어로 라이브러리를 설치한다.

---

### 5. [애플리케이션 배포](https://cloud.google.com/appengine/docs/flexible/nodejs/testing-and-deploying-your-app?hl=ko#deploying_your_application)

gcloud에 로그인 한 적이 없다면 `gcloud init`을 CLI에 입력하여 로그인을 진행한다.

로그인이 된 상태라면 cli의 경로를 프로젝트 폴더로 이동한 뒤 `gcloud app deploy`를 입력한다.

잠깐 기다리면 계속할 거냐고 물어본다. `y`를 입력해준다.

이제 좀 걸린다.

3~5분 뒤에 빌드에 성공했다고 뜬다.

이제 `gcloud app browse`를 입력하면 브라우저에 배포된 앱이 나타난다!
