# 모니터링 설정
**Quickstarts > 8. 모니터링 설정**

이번 학습 모듈에서는 NHN Cloud 콘솔에서 제공하는 Monitoring 서비스에 대해 자세히 알아보고 직접 실습해 봅니다. NHN Cloud의 Cloud Monitoring 서비스는 클라우드 환경에서 운영되는 인프라와 애플리케이션의 상태를 실시간으로 모니터링하고 이상 징후를 신속하게 감지할 수 있도록 지원합니다.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20%EC%84%A4%EC%A0%95.png)
## 학습 목표

이번 학습 모듈에서 배울 내용은 다음과 같습니다.

* **NHN Cloud 모니터링 개념**
    * NHN Cloud에서 제공하는 모니터링 서비스 개요
    * 시스템 성능, 네트워크 트래픽, 애플리케이션 로그 모니터링의 중요성
    * 실시간 상태 추적과 장기적인 성능 분석의 차이점
* **NHN Cloud 모니터링 툴 소개**
    * **Cloud Monitoring** 개요 및 기능
    * 모니터링 대시보드 구축
        * NHN Cloud 기본 대시보드 활용
        * 템플릿을 이용한 대시보드 설정
    * 경고 및 알림 설정
        * NHN Cloud 알림 시스템을 통한 이상 상태 감지
        * 이메일, SMS 등 조건별 알림 설정
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/diagram/%EB%AA%A8%EB%93%88%208.%20%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20%EC%84%A4%EC%A0%95.png)

<p style="text-align: center; color: black;">최종 구성도</p>

## 시작하기 전에

이번 학습 모듈을 실습하기 전에 다음을 수행하는 것이 좋습니다.

* **표준 인터넷 브라우저**
    * Google Chrome, Microsoft Edge, Firefox, Safari 등의 최신 버전 브라우저가 설치되어 있어야 합니다.
    * 브라우저 설정에서 JavaScript 및 쿠키가 활성화되어 있어야 합니다.
* **인터넷 연결 환경**
    * 안정적인 인터넷 연결이 필요하며, 권장 대역폭은 최소 5Mbps 이상입니다.
    * HTTPS를 통한 안전한 통신이 가능해야 합니다.
* **회원 계정**
    * 결제수단을 등록한 NHN Cloud 계정이 있어야 합니다.
    * NHN Cloud 홈페이지에 로그인 해야 합니다.

    **본 가이드는 [7. 스토리지 생성 및 설정](https://docs.nhncloud.com/ko/quickstarts/ko/create-storage/) 이후 단계부터 시작됩니다.**

## Cloud Monitoring 서비스를 통한 클라우드 리소스 모니터링

### 단계 1. Cloud Monitoring으로 인스턴스 상세 지표 대시보드 만들기

1. NHN Cloud 콘솔 상단 메뉴에서 실습에 사용할 조직(`MyORG`), 프로젝트(`MyPRJ`), 그리고 `한국(평촌) 리전`을 선택합니다.
2. 콘솔 창 왼쪽 메뉴 중 **Monitoring - Cloud Monitoring**을 클릭합니다.
3. **+ 대시보드 생성**을 클릭합니다.
4. **대시보드 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 대시보드 이름: `MyDashboard`
5. `MyDashboard` 탭 오른쪽 끝에 **+ 위젯 추가**를 클릭합니다.
6. **위젯 추가** 화면에서 아래 정보를 설정 후 **추가**를 클릭합니다.
    * 기본 설정
        * 위젯 이름: `Instance-CPU-Basic`
        * 그래프 유형: `Line`
        * 서비스: `Instance`       
    > [참고] 서비스 변경 알람
    >
    > * 위젯의 서비스가 변경되면 설정한 내용이 삭제될 수 있습니다. 변경 시 알림으로 확인 가능하니 기존 작업한 내용을 미리 추가한 후에 변경하여 작업할 수 있도록 합니다.
    * 지표 설정
        * 리소스 유형: `CPU`
        * 지표 항목: `CPU 사용률, CPU 평균 부하(5m)`
7. **+ 위젯 추가**를 클릭해 위젯을 추가로 생성합니다.
8. **위젯 추가** 화면에서 아래 정보를 설정 후 **추가**를 클릭합니다.
    * 기본 설정
        * 위젯 이름: `Instance-Disk-Basic`
        * 그래프 유형: `Stacked Area`
        * 서비스: `Instance`
    * 지표 설정
        * 리소스 유형: `Disk`
        * 지표 항목: `디스크 사용률, 마운트별 디스크 사용률, 장치별 디스크 읽기, 장치별 디스크 쓰기`
9. **+ 위젯 추가**를 클릭해 위젯을 추가로 생성합니다.
10. **위젯 추가** 화면에서 아래 정보를 설정 후 **추가**를 클릭합니다.
    * 기본 설정
        * 위젯 이름: `Instance-Network-Basic`
        * 그래프 유형: `Column`
        * 서비스: `Instance`
    * 지표 설정
        * 리소스 유형: `Network`
        * 지표 항목: `장치별 네트워크 데이터 송신, 장치별 네트워크 데이터 수신`
11. `MyDashboard`에 추가된 위젯이 정상적으로 보이는지 확인합니다.

### 단계 2. 프로젝트 커스텀 대시보드 확인하기

1. 콘솔 창 상단에 `MyProject` 이름의 프로젝트 탭을 클릭합니다.
2. `MyProject` 메인 화면에서 `커스텀 대시보드` 탭을 클릭합니다.
3. 단계 1에서 추가한 `MyDashboard`의 위젯이 정상적으로 보이는지 확인합니다.

### 단계 3. 인스턴스에 CPU 과부하 발생 시 이메일로 알림 설정하기

1. **Cloud Monitoring** 서비스 화면에서 **알림 관리** 탭을 클릭합니다.
2. **+ 알림 설정**을 클릭합니다.
3. **알림 생성** 화면에서 아래 정보를 설정 후 **저장**을 클릭합니다.
    * 기본 정보
        * 이름: `MyAlarm`
        * 서비스: `Instance`
    * 알림 설정
        * 리소스 유형: `CPU`
        * 지표: `CPU 상세(user)`
        * 필터
            * **+ 추가**를 클릭 후 아래 설정을 입력합니다.
            * 레이블: `인스턴스`, 연산자: `=`, 조건: `linux-server-basic`
            * 편집: **저장** 클릭
        * 조건
            * 비교 방법: `>`, 임계치: `50`, 지속 시간: `1` 분
                * 임계치가 50을 초과하면 알림이 1분 동안 지속됩니다.
            * 편집: **저장** 클릭
    * 알림 수신 대상
        * 알림 수신 그룹명이 `기본 알림 수신 그룹`인 대상 오른쪽 **추가** 체크박스를 **선택**

    > [참고] 알림 수신 그룹 선택
    >
    > * 해당 알림 수신 그룹명의 오른쪽 추가 체크박스를 선택하면 바로 위에 선택한 알림 수신 그룹이 추가되는 것을 확인할 수 있습니다.

4. `MyAlarm`이 생성된 것을 확인하고 **알림 사용 여부** 토글 버튼이 활성화된 상태인 것을 확인합니다.

!!! tip "알아두기"
    * 토글 버튼 상태
        * 토글 버튼 활성화 상태는 타원 내에 흰색 원이 오른쪽으로 이동한 상태입니다. 토글이 활성화되면 색상이 파란색으로 표시됩니다.
        * 토글 버튼 비활성화 상태는 타원 내에 흰색 원이 왼쪽으로 이동한 상태입니다. 토글이 비활성화되면 색상이 회색으로 표시됩니다.


### 단계 4. 인스턴스의 CPU 과부하 이벤트 발생 이력 확인하기     

1. 콘솔창 왼쪽 메뉴 중 **Network - Floating IP** 를 클릭합니다.
2. 플로팅 IP 리소스 목록 중 연결된 장치가 `linux-server-basic`인 IP 주소를 **복사** 후 **기록**합니다.
3. 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-basic 플로팅 IP 주소`를 입력하여 웹 페이지에 접속합니다.
4. 웹 페이지 본문에 있는 **Start Stress Test**를 클릭 후 2분 동안 대기합니다. 해당 작업은 `linux-server-basic` 인스턴스 CPU 사용량(CPU 상세(user))에 임의로 과부하를 발생시킵니다.
5. 잠시 뒤 문자 및 이메일로 알람 발생 결과가 수신되는지 확인합니다.
6. **Cloud Monitoring** 서비스 화면에서 **알림 관리** 탭을 클릭합니다.
7. **알림 관리 화면**에서 **알림 발생 이력** 탭을 클릭합니다.
8. 본문 내에 **검색**을 클릭하여 알림 발생 이력을 확인합니다.

## 참고 자료

* [Metric](https://en.wikipedia.org/wiki/Metric_system)
* [Monitoring](https://en.wikipedia.org/wiki/System_monitor)
* [Metric Dictionary](https://docs.nhncloud.com/ko/Monitoring/Cloud%20Monitoring/ko/metric-dictionary/)
* [Cloud Monitoring](https://docs.nhncloud.com/ko/Monitoring/Cloud%20Monitoring/ko/overview/)
* [CloudTrail](https://docs.nhncloud.com/ko/Governance%20&%20Audit/CloudTrail/ko/overview/)
* [Stress testing](https://en.wikipedia.org/wiki/Stress_testing_(computing))

## 이전 단계

* [7. 스토리지 생성 및 설정](https://docs.nhncloud.com/ko/quickstarts/ko/create-storage/)

## 다음 단계

* [9. 백업 및 복구](https://docs.nhncloud.com/ko/quickstarts/ko/backup-restore/)
