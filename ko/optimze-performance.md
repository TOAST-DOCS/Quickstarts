# 확장성과 성능 최적화
**Quickstarts > 10. 확장성과 성능 최적화**

이번 학습 모듈에서는 NHN Cloud 환경에서 애플리케이션의 확장성과 성능 최적화를 위한 아키텍처 구성 방법을 학습합니다. NHN Cloud RDS를 활용해 오토 스케일링, 로드 밸런싱과 안정적인 데이터 관리를 위한 효율적이고 유연하며 확장 가능한 시스템을 설계할 수 있습니다.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%ED%99%95%EC%9E%A5%EC%84%B1%EA%B3%BC%20%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94.png)
## 학습 목표

이번 학습 모듈에서 배울 내용은 아래와 같습니다.

* **로드밸런서 생성**
    * 트래픽 분산을 위해 L4 라우팅 모드의 로드밸런서를 생성하고 플로팅 IP를 연결해 웹 서비스의 접근성을 확보
* **오토 스케일 그룹 구성**
    * 트래픽 변화에 따라 자동으로 인스턴스를 추가하거나 삭제하는 오토 스케일 그룹 설정
    * CPU 사용량 기반 증설 및 감축 정책을 적용하여 최소 1개, 최대 3개의 인스턴스 유지
* **웹 서버 접근 설정**
    * 로드밸런서에 연결된 플로팅 IP를 통해 생성된 웹 서버에 접근 가능하도록 설정

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/diagram/%EB%AA%A8%EB%93%88%2010.%20%ED%99%95%EC%9E%A5%EC%84%B1%EA%B3%BC%20%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94.png)

<p style="text-align: center; color: black;">최종 구성도</p>

## 시작하기 전에

이번 학습 모듈을 시작하기 전에 필요한 사항은 다음과 같습니다.

* **표준 인터넷 브라우저**
    * Google Chrome, Microsoft Edge, Firefox, Safari 등의 최신 버전 브라우저가 설치되어 있어야 합니다.
    * 브라우저 설정에서 JavaScript 및 쿠키가 활성화되어 있어야 합니다.
* **인터넷 연결 환경**
    * 안정적인 인터넷 연결이 필요하며, 권장 대역폭은 최소 5Mbps 이상입니다.
    * HTTPS를 통한 안전한 통신이 가능해야 합니다.
* **회원 계정**
    * 결제수단을 등록한 NHN Cloud 계정이 있어야 합니다.
    * NHN Cloud 포털에 로그인 해야 합니다.

**본 가이드는 [9. 백업 및 복구](https://docs.nhncloud.com/ko/quickstarts/ko/backup-restore/) 이후 단계부터 시작됩니다.**

## 스케일링 그룹과 로드밸런서를 통한 트래픽 분산

### 단계 1. 로드밸런서 생성하기

> Load Balancer 서비스를 사용해 `MyLB` 로드밸런서를 생성한 뒤 플로팅 IP를 생성해 연결해 봅니다.

1. NHN Cloud 콘솔 상단 메뉴에서 실습에 사용할 조직(`MyORG`), 프로젝트(`MyPRJ`), 그리고 `한국(평촌) 리전`을 선택합니다.
1. 콘솔 창 왼쪽 메뉴 중 **Network - Load Balancer**를 클릭합니다.
2. **+ 로드 밸런서 생성**을 클릭합니다.
3. **로드 밸런서 생성 모드 선택** 화면에서 `L4 라우팅 모드`를 선택 후 **확인**을 클릭합니다.
4. **로드 밸런서 생성** 화면에서 아래 정보를 설정 후 **로드 밸랜서 생성**을 클릭합니다.
    * 설정
        * 이름: `MyLB`
        * 타입: `일반`
        * VPC: `MyVPC(192.168.0.0/16)`
        * 서브넷: `MySubnet(192.168.0.0/24) 자동 할당`
        * 정적 라우트: `적용 안 함`
    * 리스너
        * 이름: `http-listener`
        * 연결 제한: `60000`
        * Keep-Alive 타임아웃: `300`
        * 프로토콜: `HTTP`
        * 유효하지 않은 요청 차단: `사용`
        * 로드 밸런서 포트: `80`
        * 멤버 그룹
            * 이름: `linux-server-member`
            * 상태 확인 프로토콜: `HTTP`
            * 상태 확인 포트: `80`
            * 프로토콜: `HTTP`
            * HTTP 메서드: `GET`
            * 포트: `80`
            * HTTP 상태 코드: `200`
            * 로드 밸런싱 방식: `ROUND_ROBIN`
            * URL: `/`
            * 세션 지속성: `세션 지속성 없음`
            * 상태 확인 주기: `30`
            * 최대 응답 대기 시간: `5`
            * 최대 재시도 횟수: `2`
            * 호스트 헤더 : `빈 칸`
    * IP 접근제어 그룹: `없음(기본값)`
    * 삭제 보호: `사용 안함`
5. 성공 창에서 **확인**을 클릭합니다.
6. 잠시 후 생성이 완료되면 `MyLB`를 선택한 뒤 상단에 **플로팅 IP 관리**를 클릭합니다.
7. **플로팅 IP 관리** 창에서 플로팅 IP 생성 항목에서 **+ 생성**을 클릭합니다.
8. **플로팅 IP 생성** 창에서 **생성**을 클릭합니다.
9. **성공** 창에서 **플로팅 IP 주소**를 **복사** 후 **기록**합니다.
10. **확인**을 클릭합니다.
11. **플로팅 IP 관리** 창에서 **플로팅 IP 연결/해제** 항목에서 아래와 같이 설정 후 **연결**을 클릭합니다.
    * 플로팅 IP 선택: `위에 생성 후 확인한 플로팅 IP 주소`
    * 네트워크 인터페이스 선택: `MyLB`
12. **성공** 창에서 **확인**을 클릭합니다.
13. **닫기**를 클릭합니다.

### 단계 2. 오토 스케일링 그룹 생성하기 

> Auto Scale 서비스를 활용해 스케일링 그룹을 생성한 뒤 가용할 수 있는 인스턴스 2개를 생성합니다.

1. 콘솔창 왼쪽 메뉴 중 **Compute - Auto Scale**을 클릭합니다.
2. **Auto Scale** 화면에서 **+ 스케일링 그룹 생성**을 클릭합니다.
3. **스케일링 그룹 생성** 화면에서 아래 정보를 설정 후 **스케일링 그룹 생성**을 클릭합니다.
    * 인스턴스 템플릿
        * 사용: `사용 안 함`
    * 이미지
        * 개인 이미지 > 이미지 이름: `linux-server-basic-image` 클릭
        > [참고]
        >
        >`linux-server-basic-image` 개인 이미지는 [9.백업 및 복구](https://docs.nhncloud.com/ko/quickstarts/ko/backup-restore/)의 **단계 1**을 통해 생성할 수 있습니다.
    * 인스턴스 정보
        * 가용성 영역: `임의의 가용성 영역`
        * 인스턴스 이름: `linux-server-autoscale`
        * 인스턴스 타입 > **인스턴스 타입 선택** 클릭 > 인스턴스 타입 이름: `t2.c1m1` 클릭 후 **선택** 클릭
        * 키페어 > `MyKey`
            * 키페어 생성에 자세한 내용은 키페어 사용자 가이드를 참고하세요.
    * 루트 블록 스토리지
        * 블록 스토리지 타입: `HDD`
        * 블록 스토리지 크기(GB): `20` GB
    * 네트워크 설정: `네트워크 인터페이스 생성` 선택
    * 네트워크
        * 사용 가능한 서브넷 항목에서 `MySubnet (192.168.0.0/24)` 리소스를 클릭하여 선택된 서브넷으로 사용
    * 플로팅 IP: `사용 안 함`
    * 보안 그룹
        * **보안 그룹 선택:**  `MySG-HTTP`를 선택
    * 추가 블록 스토리지: `사용 안 함 (기본)`
    * 사용자 스크립트: `빈 칸 (기본)`
    * 스케일링 그룹 정보
        * 이름: `MyASGroup`
        * 최소 인스턴스: `1`
        * 최대 인스턴스: `3`
        * 구동 인스턴스: `2`
    * 증설 정책
        * 조건: `instance` `cpu` > `50` % 상태가 `1 분` 동안 지속 시
        * 인스턴스 조정: `1 개` 의 인스턴스가 생성됩니다.
        * 재사용 대기 시간: 인스턴스 생성 후 `1 분` 동안 생성을 시도하지 않습니다.
    * 감축 정책
        * 조건: `instance` `cpu` < `10` % 상태가 `1 분` 동안 지속 시
        * 인스턴스 조정: `1 개`의 인스턴스가 삭제됩니다.
        * 재사용 대기 시간: 인스턴스 생성 후 `1 분` 동안 생성을 시도하지 않습니다.
    * 자동 복구 정책
        * 자동 복구: `사용`
    * 로드 밸런서
        * 사용 가능한 로드 밸런서 항목에서 `MyLB` 리소스를 클릭하여 선택된 로드 밸런서로 사용
    * Deploy 연계: `사용 안 함 (기본)`

4. 성공 창에서 **확인**을 클릭합니다.
5. **Auto Scale** 화면의 이미지 목록에서 `MyASGroup` 이 생성 중인 것을 확인합니다. 생성이 완료되면 해당 스케일링 그룹의 상태 표시등이 초록색으로 표시됩니다.
6. 콘솔 창 왼쪽 메뉴 중 **Compute - Instance** 를 클릭합니다.
7. Instance 화면에서 인스턴스 목록 중 `linux-server-autoscale`가 2개 생성된 것을 확인합니다.

### 단계 3. 로드밸런서를 사용해 트래픽 분산하기

> `MyLB` 로드밸런서를 사용해 복수의 인스턴스로 트래픽을 분산하는 방법을 알아봅니다.

1. 콘솔 창 왼쪽 메뉴 중 **Netowork - Floating IP**를 클릭합니다.
2. 플로팅 IP 리소스 목록 중 연결된 장치가 `MyLB`인 IP 주소를 **복사** 후 **기록**합니다.
3. 웹 브라우저에서 새 창을 열어서 `http://복사한 MyLB 플로팅 IP 주소`를 입력하여 변경된 웹 페이지에 접속합니다.
4. 웹 페이지 본문의 **Server IP Address 값**이 생성한 `linux-server-autoscale`인스턴스의 가상 사설 IP 주소와 일치하는지 확인합니다.
5. 웹 브라우저 새로고침을 반복해  **Server IP Address 값**이 또 다른 `linux-server-autoscale`인스턴스의 가상 사설 IP 주소로 변경되는지 확인합니다.

> [참고]
> <details markdown="1">
> <summary><u>결과 화면 보기</u></summary>
>
> <p>
> **결과 화면 1**
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94_%EB%8B%A8%EA%B3%843-1.png">
> </p>
> <p>
> **결과 화면2**
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94_%EB%8B%A8%EA%B3%843-2.png">
> </p>

!!! tip "알아두기"
    * **Server IP Address 값 변경 값 확인**
        * `linux-server-autoscale` 인스턴스는 오토 스케일 그룹의 증설/감축 정책 설정에 따라 상태가 수시로 변경될 수 있습니다. 이에 2개의 인스턴스에서 1개의 인스턴스로 감축되어 Server IP Address가 한 개의 주소만 나올 수 있습니다.
        * 오토 스케일 그룹과 연결된 로드 밸런서가 그룹에 속한 인스턴스의 상태 값을 주기적으로 확인하여 접속합니다. 이에 따른 상태값 정보가 갱신되면서 일정 시간이 지난 후에 바뀐 Server IP Address 값이 노출될 수 있습니다.
    * **변경된 웹 페이지가 보이지 않을 경우**
        * 웹 페이지가 변경되었음에도 불구하고 이전 페이지가 표시된다면, 웹 브라우저의 캐시로 인해 발생한 문제일 수 있습니다. 이 경우, 브라우저에서 제공하는 새로고침(F5) 또는 강력 새로고침을 사용하여 변경된 페이지를 확인할 수 있습니다. 강력 새로고침은 다음 방법으로 수행할 수 있습니다.
            * **Windows**: `Ctrl + F5` 또는 `Shift + F5`
            * **Mac**: `Cmd + Shift + R`


### 단계 4. 오토 스케일 그룹 증설 및 감축 정책 적용하기

> 애플리케이션 수요에 따라 인스턴스 수를 자동 조절하는 방법을 알아봅니다.

1. 웹 브라우저에서 새 창을 열어서 `http://복사한 MyLB 플로팅 IP 주소`를 입력해 접속합니다.
2. 웹 페이지 본문의 **Start Stress Test**를 클릭 후 2분 동안 대기합니다.
> [참고]
>
> * Stress Test
>     * 해당 기능은 웹 서버에 임의적 CPU 부하를 발생하여 오토 스케일 그룹에 생성한 증설 정책이 동작하도록 합니다.
3. 콘솔 창 왼쪽 메뉴 중 **Compute - Instance**를 클릭합니다.
4. Instance 화면에서 인스턴스 목록 중 `linux-server-autoscale`이 추가로 생성되는 것을 확인합니다.
5. 웹 브라우저에서 새 창을 열어서 `http://복사한 MyLB 플로팅 IP 주소`를 입력해 웹 페이지를 확인합니다.
6. 웹 페이지 본문의 **Server IP Address 값**이 생성한 `linux-server-autoscale`인스턴스의 가상 사설 IP주소가 맞는지 확인합니다.
7. 웹 브라우저 새로고침을 반복해 **Server IP Address 값**이 또 다른 `linux-server-autoscale`인스턴스의 가상 사설 IP 주소로 변경되는지 확인합니다.
    * **Compute - Instance** 메뉴에서 `linux-server-autoscale` 인스턴스를 클릭하여 선택 후 아래 분할창에서 **모니터링** 탭을 클릭하면 CPU 사용률을 확인할 수 있습니다.
    * **Compute - Auto Scale** 메뉴에서 `MyASGroup` 스케일링 그룹을 클릭해 선택 후 아래 분할창에서 **통계** 탭을 클릭하면 CPU 사용률을 확인할 수 있습니다.
8. `linux-server-autoscale` 인스턴스가 자동으로 증설되었다면 다시 2분 간 대기합니다.
9. Instance 화면에서 인스턴스 목록 중 `linux-server-autoscale`이 제거되는 것을 확인합니다.
10. 웹 브라우저 새로고침을 반복해  **Server IP Address 값**이 유지 중인`linux-server-autoscale` 인스턴스의 가상 사설 IP주소로 출력되는지 확인합니다.

## 참고 자료

* [Auto Scale](https://docs.nhncloud.com/ko/Compute/Auto%20Scale/ko/overview/)
* [Load Balancer](https://docs.nhncloud.com/ko/Network/Load%20Balancer/ko/overview/)
* [로드 밸런싱 방식](https://docs.nhncloud.com/ko/Network/Load%20Balancer/ko/overview/#_1)
* [Load balancing (computing)](https://en.wikipedia.org/wiki/Load_balancing_(computing))
* [Transport Layer(L4)](https://en.wikipedia.org/wiki/Transport_layer)
* [Application Layer(L7)](https://en.wikipedia.org/wiki/Application_layer)
* [Autoscaling](https://en.wikipedia.org/wiki/Autoscaling)

## 이전 단계

* [9. 백업 및 복구](https://docs.nhncloud.com/ko/quickstarts/ko/backup-restore/)

## 다음 단계

* [11. 비용 관리](https://docs.nhncloud.com/ko/quickstarts/ko/cost-management/)