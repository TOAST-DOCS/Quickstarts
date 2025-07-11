# 조직과 프로젝트 생성
**Quickstarts > 2. 조직과 프로젝트 생성**

이번 학습 모듈에서는 NHN Cloud 콘솔의 주요 기능을 이해하고 활용하는 데 필요한 기본 개념과 설정 방법을 안내합니다. NHN Cloud 콘솔은 다양한 클라우드 리소스를 효율적으로 관리하고 설정할 수 있는 통합 관리 도구입니다. 사용자 친화적인 인터페이스를 통해 서비스 생성, 모니터링, 설정 변경 등의 작업을 손쉽게 수행할 수 있으며, 실시간 리소스 상태와 비용 관리 기능도 제공합니다. 
콘솔의 프로젝트 대시보드를 통해 사용 중인 클라우드 서비스와 리소스 정보를 한눈에 확인할 수 있으며, 세부 설정 및 관리 옵션은 직관적인 메뉴를 통해 쉽게 접근할 수 있습니다.

![module_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EC%A1%B0%EC%A7%81%EA%B3%BC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%83%9D%EC%84%B1.png)
## 학습 목표

이번 학습 모듈에서 배울 내용은 다음과 같습니다.

* **NHN Cloud 콘솔 접속**
    * 클라우드 관리를 위한 콘솔 접속
* **조직, 프로젝트 및 리전 설정**
    * 클라우드 리소스 관리를 위한 기본 설정 구성

<br></br>

![mod2_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/diagram/%EB%AA%A8%EB%93%88%202.%20%EC%A1%B0%EC%A7%81%EA%B3%BC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%83%9D%EC%84%B1.png)

<p style="text-align: center; color: black;">최종 구성도</p>

> 리전, 조직, 프로젝트, 가용성 영역은 이후 학습 모듈에서 동일하게 사용되므로 이후 학습 모듈의 최종 구성도에서는 생략됩니다.

## 시작하기 전에

이번 학습 모듈을 시작하기 전에 필요한 사항은 다음과 같습니다.

* **표준 인터넷 브라우저**
    * Google Chrome, Microsoft Edge, Firefox, Safari 등의 최신 버전 브라우저가 설치되어 있어야 합니다.
    * 브라우저 설정에서 JavaScript 및 쿠키가 활성화되어 있어야 합니다.
* **인터넷 연결 환경**
    * 안정적인 인터넷 연결이 필요하며, 권장 대역폭은 최소 5Mbps 이상입니다.
    * HTTPS를 통한 안전한 통신이 가능해야 합니다.
* **회원 계정**
    * 결제수단을 등록한 NHN Cloud 계정이 있어야 합니다.
    * NHN Cloud 홈페이지에 로그인해야 합니다.

    **본 가이드는 [1. 계정 생성과 로그인](https://docs.nhncloud.com/ko/quickstarts/ko/create-account/) 이후 단계부터 시작합니다.**

## NHN Cloud 콘솔 사용을 위한 준비

### 단계 1. NHN Cloud 콘솔 접속하기

1. NHN Cloud 홈페이지([https://www.nhncloud.com](https://www.nhncloud.com/))에 로그인합니다.
2. 상단 메뉴에서 **CONSOLE**을 클릭합니다.
3. 새로운 브라우저 창 또는 탭에서 **NHN Cloud 콘솔 페이지**를 확인합니다.

### 단계 2. 조직 생성하기

1. NHN Cloud 콘솔 상단에 위치한 **조직을 생성해 주세요.** 옆의 **+** 를 클릭합니다.
2. 조직 생성 창에서 아래 정보를 입력 후 **확인**을 클릭합니다.
    * 조직 이름: `MyORG`
3. 알림 창에서 **확인**을 클릭합니다.
4. 생성한 조직의 대시보드와 콘솔 화면을 확인합니다.

!!! tip "알아두기"
    * 이미 생성된 조직이 있을 경우
        * 이미 생성한 조직이 있으면 해당 조직 목록 하단에 **+ 조직 만들기**를 클릭하여 아래 작업을 진행할 수 있습니다.

### 단계 3. 프로젝트 생성하기

1. NHN Cloud 콘솔 상단에 위치한 **조직 탭**에서 `MyORG`를 클릭합니다. 조직이 한 개인 경우 자동으로 선택되어 있습니다.
2. 선택한 조직 탭 우측에 위치한 **새 프로젝트 생성** 옆의 **+**을 클릭합니다.
3. **프로젝트 생성** 창에서 아래 정보를 입력 후 **확인**을 클릭합니다.
    * 프로젝트 이름: `MyPRJ`
4. 알림 창에서 **확인**을 클릭합니다.
5. 생성한 프로젝트의 대시보드와 콘솔 화면을 확인합니다.

### 단계 4. 리전 선택하기

1. NHN Cloud 콘솔 상단 오른쪽에 위치한 **한국(판교) 리전** 으로 마우스 커서를 이동합니다.
2. 리전 목록에서 `한국(평촌) 리전`을 클릭합니다.

## 참고 자료

* [콘솔 정책 가이드](https://docs.nhncloud.com/ko/nhncloud/ko/console-guide/)
* [리소스 제공 정책](https://docs.nhncloud.com/ko/nhncloud/ko/resource-policy/)

## 이전 단계

* [1. 계정 생성과 로그인](https://docs.nhncloud.com/ko/quickstarts/ko/create-account/)

## 다음 단계

* [3. IAM 계정과 거버넌스 설정](https://docs.nhncloud.com/ko/quickstarts/ko/iam-accounts/)
