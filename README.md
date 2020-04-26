# 플러터 온라인 스터디

본 프로젝트는 플러터 온라인 스터디를 위한 것입니다.

목표는 간단한 영어 단어장을 만드는 것입니다.

## 해야 할 일

* 업무 분담 참고
* 앱이 시작 될 때 마다 서버에 단어장 업데이트가 있는지 확힌
  * 업데이트가 있으면 다운 받아
  * Hive 에 저장
* 앱에서 리스트로 단어를 표시
  * 단어를 클릭하면 상세 페이지 표시
* 단어장 목록
  * 아는 단어
  * 모르는 단어
  * 전체 목록
* 인공지능 단어장 자동 분류
  * 단어마다 CEFR_LEVEL 이 필요함. 이 데이터가 빠져 있음.

## 프로젝트 참여 방법

1. Git Repo 에 개발자로 등록합니다. 단톡방에 요청해주세요.
2. 개발은 반드시 자신의 'branch' 에서 해 주세요. 'master' 는 가능한 건드리지 마세요.
3. 프로젝트 관리자로 부터 역활을 분담 받으시고 작업을 진행해주세요.

## 업무 분담

### 영자(송재호)

* 프로젝트 관리

### 김동후

* 메뉴 추가
  * 아는단어/모르는단어/전체/자동 분류
  * 단어장 목록 페이지 하나에 `아는단어/모르는단어/전체` 세가지 기능을 사용
  * 자동 분류 페이지 따로 작성


### 영통호림

* 단어 클릭시 단어 상세 설명 표시

### 윤태일

* Hive 를 통한 데이터 관리
* 앱 시작시 서버에서 업데이트가 있는 경우에만, 단어장 데이터 다운로드
* 아는 단어를 밀면 모르는 단어로 이동
* 모르는 단어를 밀면 아는 단어로 이동


## 코딩 가이드라인

### 순서도

1. 앱이 처음 시작 될 때, 서버에서 데이터가 업데이트되었는지 확인
   1. Dio 를 통해서 HTTP 호출을 함. (Dio 가 더 편하고, 기능이 좋아서)
   2. Hive 에 단어 목록을 저장. (SQLite 로 하면 매우 느림. Hive 가 딱 맞음.)
2. Hive 에서 단어를 리스트 뷰로 표시.
   1. 기본적으로 모르는 단어에 모두 표시
   2. 아는 단어로 이동 가능
   3. 아는 단어/모르는 단어 테스트 후 자동 분류
3. 단어 클릭시 단어 상세 페이지 표시

### 모델

* 앱의 규모가 크지 않아 `app.model.dart` 하나로 할 예정입니다.
  * `globals.dart` 에 `AppModel` 객체를 등록해 놓고 편하게 사용합니다.
* 모델이 하나에 많은 이벤트가 사용 될 수 있으므로 가능한 Consumer 대신 Selector 를 사용합니다.

### 플러그인 설명

#### Dio

* 원래 http 를 쓰는데, 갑자기 프로젝트 시작하려니 Flutter 의 기본 http 이 위젯이 번거롭게 느껴집니다. 특히, http 클래스명, 함수명 등에서 매뉴얼을 뒤지려고 하니 머리가 아픕니다. 번뜩이는 생각으로 간단하고 강력한 Dio 를 쓰기로 했습니다.

#### Hive

* 단어장은 이전에도 여러번 만든 경험이 있습니다. 특히, 단어장을 ListView에서 스크롤 할 때, 보여지는 단어를 SQLite 에서 가져왔는데, 데이터 로딩 시간이 걸리고, 단어장 목록에서 이리 저리 이동 할 때에도 로딩 시간이 걸려, 로딩 애니메이션을 보여줬었습니다.
* 물론 용납되지 않는 코드죠. 연습용이라면 모르겠지만 배포용 앱에서는 그런 코드로는 성공할 수 없어서, 해결 책을 찾다가 Hive 라는 좋은 플러그인을 발견했습니다.

### 기능별 코딩가이드

#### 단어장 목록

##### 모르는 단어

* 본인이 모르는 단어(공부해야 하는 단어)를 목록
* 단어 수는 굉장히 많은데, 매번 단어장을 펼칠 때 마다 모르는 단어만 골라서 공부하기 어렵습니다. 그래서 모르는 단어만 따로 표시하는 것이빈다.

##### 아는 단어

* 아는 단어 본인이 아는 단어 또는 공부를 다 한 단어를 목록
* 모르는 단어 목록에서 공부를 다 했으면 아는 단어 목록으로 이동 할 수 있습니다.


##### 전체 단어

* 전체 단어를 목록


##### 자동 분류

* 단어 수는 굉장히 많습니다. 그런데 아는 단어와 모르는 단어를 일일히 하나씩 분류하면 시간이 많이 걸리지 않겠어요? 그래서 자동으로 분류합니다.
* 어떻게요? 개인마다 영어 단어 지식이 틀리고 단어 수도 몇 천개나 되는데 ...
* 단어 테스트를 통해서 합니다. 참고로 단어마다 레벨이 필요합니다.



## 백엔드

* 단어장 서버에서 RestfulApi 로 단어 목록을 가져옵니다.

### 단어장 가져오기

* 단어장 Api: https://api.english-fun.com/wordpress-api-v2/res/englishfun/tmp/words.json
* 단어장 전체 사이즈: https://api.english-fun.com/wordpress-api-v2/res/englishfun/tmp/words-length.php

단어장이 업데이트 되면 전체 사이즈가 변경된다. 따라서 기존 사이즈를 보관해 놓고, 업데이트가 있는지 없는지 확인하면 된다.


## 레퍼런스

### 왕초보를 위한 팁

아래 순서대로 공부하세요.

1. www.github.com 사용법을 배웁니다.
   1. 참고: [깃허브 공부 자료](https://sunnykwak.tistory.com/97?fbclid=IwAR09R6DMdhXdjVfNWv8bdCWYTmnYOaMCIZTtlOD34aTfw2yuUxQM-TQERWc)
2. Flutter 기초를 배웁니다.
   1. [오준석의 생존코딩 - 입문편](https://www.youtube.com/watch?v=lRbZsBvG9Ig&list=PLxTmPHxRH3VUueVvEnrP8qxHAP5x9XAPv)
   2. [오준석의 생존코딩 - 중급편](https://www.youtube.com/watch?v=ei8TX-uqP6E&list=PLxTmPHxRH3VWLY-eyQuV1C_IbIQlCXEhe)
   3. [코딩셰프의 Flutter 동영상 강좌](https://www.youtube.com/channel/UC_2ge45JCuJH1z6VYt4iCgQ)
   4. [더코딩파파의 FLutter 동영상 강좌](https://www.youtube.com/channel/UCUH2DSbsNUz2sW3kBNn4ibw)

### Flutter 기본 자료

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

