# 보안 설정
**Quickstarts > 5. 보안 설정**

이번 학습 모듈에서는 NHN Cloud에서 보안을 설정하고 관리하는 기본 개념과 주요 기능을 단계별로 안내하여, 안전하고 신뢰할 수 있는 클라우드 환경을 구축하는 방법을 학습합니다. NHN Cloud는 사용자의 데이터를 안전하게 보호하고, 클라우드 리소스를 효율적으로 관리할 수 있는 다양한 보안 기능을 제공합니다.
![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95.png)

## 학습 목표

이번 학습 모듈에서 배울 내용은 다음과 같습니다.

* **네트워크 보안 설정**
    * VPC 및 서브넷 구성을 통한 네트워크 분리 및 보호
    * 보안 그룹과 ACL(Access Control List)를 통한 허가된 트래픽만 허용
* **데이터 보안**
    * NHN Cloud에서 제공하는 데이터 암호화 방식 및 스토리지 보안 설정
    * 데이터 손실 대비 위한 백업 및 복구 전략
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/diagram/%EB%AA%A8%EB%93%88%205.%20%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95.png)

<p style="text-align: center; color: black;">최종 구성도</p>

## 시작하기 전에

NHN Cloud를 시작하기 위해서는 다음 사항을 준비해야 합니다.

* **표준 인터넷 브라우저**
    * Google Chrome, Microsoft Edge, Firefox, Safari 등의 최신 버전 브라우저가 설치되어 있어야 합니다.
    * 브라우저 설정에서 JavaScript 및 쿠키가 활성화되어 있어야 합니다.
* **인터넷 연결 환경**
    * 안정적인 인터넷 연결이 필요하며, 권장 대역폭은 최소 5Mbps 이상입니다.
    * HTTPS를 통한 안전한 통신이 가능해야 합니다.

**본 가이드는 [4. 네트워크 설정과 인스턴스 생성](https://docs.nhncloud.com/ko/quickstarts/ko/network-setup/) 이후 단계부터 시작합니다.**

> 이번 학습 모듈에서는 4가지 시나리오를 통해 다양한 보안 설정 방법을 학습합니다.

## 시나리오 1. 웹 서버 인스턴스에 보안 규칙 적용해 외부 접근 허용하기

1. NHN Cloud 콘솔 상단 메뉴에서 실습에 사용할 조직(`MyORG`), 프로젝트(`MyPRJ`), 그리고 `한국(평촌) 리전`을 선택합니다.
2. 왼쪽 메뉴에서 **Network - Floating IP**를 클릭합니다.
3. 플로팅 IP 리소스 목록 중 연결된 장치가 `linux-server-basic`인 IP 주소를 **복사** 후 **기록**합니다.
4. 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-basic 플로팅 IP 주소`를 입력해 접속을 확인합니다.
> [참고]
> **해당 웹 서버에 접속하면 웹 페이지가 보이지 않습니다.** 이는 해당 웹 서버의 네트워크 인터페이스에 보안 그룹을 설정하지 않아 모든 통신이 차단된 상태이기 때문입니다.
5. 웹 브라우저에서 콘솔 창으로 돌아온 후 왼쪽 메뉴에서 **Network - Security Groups**를 클릭합니다.
6. **Security Groups** 화면에서 **+ 보안 그룹 생성**을 클릭합니다.
7. **보안 그룹 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 이름: `MySG-HTTP`
    * 보안 규칙 추가에  **+**을 클릭하여 다음 설정의 보안 규칙을 추가
        * 방향: `수신`
        * IP 프로토콜: `HTTP`
        * Ether: `IPv4`
        * 원격: `CIDR - 0.0.0.0/0`
8. 성공 창에서 **확인**을 클릭합니다.
9. 왼쪽 메뉴에서 **Compute - Instance**를 클릭합니다.
10. **Instance** 화면에서 `linux-server-basic` 인스턴스를 클릭하여 선택합니다.
11. 하단 분할 화면에서 **네트워크** 탭을 클릭합니다.
12. 인스턴스 생성 시 함께 **자동으로 생성된 네트워크 인터페이스 리소스(네트워크 인터페이스 ID는 임의의 ID로 지정됨)**을 선택합니다.
13. 네트워크 인터페이스 목록 바로 위에 **보안 그룹 변경**을 클릭합니다.
14. **보안 그룹 변경** 창에서 보안 그룹 선택 목록 중 `MySG-HTTP`를 선택하여 추가 후 **확인**을 클릭합니다.
15. 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-basic 플로팅 IP 주소`를 입력하여 접속을 확인합니다.
16. 웹 페이지가 정상적으로 출력되는 것을 확인합니다.
<br></br>
![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%851%20-%20%EB%B3%B5%EC%82%AC%EB%B3%B8.png)

## 시나리오 2. 웹 서버 인스턴스에 허용된 IP만 SSH, ICMP(Ping 등) 통신 허용하기

1. 왼쪽 메뉴 중 **Network - Security Groups**를 클릭합니다.
2. **Security Groups** 화면에서 `MySG-SSH`를 클릭하여 선택합니다.
3. 하단 분할 화면 **보안 규칙** 탭에서 포트 범위가 **22(SSH)**인 규칙을 선택 후 오른쪽에 **변경**을 클릭합니다.
4. **보안 규칙 변경** 창에서 원격 항목에 `CIDR`을 클릭해 드롭다운 메뉴에서 `내 IP`를 선택 후 **확인**을 클릭합니다.
5. 성공 창에서 **확인**을 클릭합니다.
6. **+ 보안 규칙 생성**을 클릭합니다.
7. **보안 규칙 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 방향: `수신`
    * IP 프로토콜: `ALL ICMP`
    * Ether: `IPv4`
    * 원격: `내 IP`
8. 성공 창에서 **확인**을 클릭합니다.
9. 새로운 **터미널** 또는 **PowerShell**을 실행합니다.
10. 아래 명령어로 통신 테스트를 진행합니다.

```
#bash
ping (linux-server-basic 플로팅 IP 주소)
```
Ping 통신이 허용된 것을 확인합니다.

## 시나리오 3. Network ACL 설정으로 특정 네트워크 대역 차단하기 

1. 콘솔 창 왼쪽 메뉴 중 **Network - Network ACL**을 클릭합니다.
2. **Network ACL > 관리 화면**에서 **+ Network ACL 생성**을 클릭합니다.
3. **Network ACL 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 이름: `MyACL`
4. 성공 창에서 **확인**을 클릭합니다.
5. 생성된 `MyACL`을 클릭하여 선택합니다.
6. 하단 분할 화면에서 **ACL Rule** 탭을 클릭합니다.
7. MyACL의 ACL Rule 목록에서 설명에 **"default allow rule"**이 표시된 **101** 순서의 ACL Rule을 선택 후 **ACL Rule 삭제**를 클릭합니다.
8. 성공 창에서 **확인**을 클릭합니다.
9. 화면 상단에 **Network ACL 탭 중 바인딩** 탭을 클릭합니다.
10. **+ ACL 바인딩 생성**을 클릭합니다.
11. ACL 바인딩 생성 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * Network ACL: `MyACL`
    * VPC: `MyVPC`
12. 성공 창에서 **확인**을 클릭합니다.
13. **터미널** 또는 **PowerShell**에서 아래 명령어를 실행하여 통신 테스트를 진행합니다.

```bash
ping (linux-server-basic 플로팅 IP 주소)
```

* Ping 통신이 차단된 것을 확인합니다.

* 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-basic 플로팅 IP 주소`를 입력하여 접속을 확인합니다.
* Http 통신이 차단된 것을 확인합니다.


## 시나리오 4. Network ACL Rule 추가 적용해 외부 접근 허용하기

1. 콘솔 창 왼쪽 메뉴 중 **Network - Network ACL**을 클릭합니다.
2. `MyACL`을 클릭한 뒤 하단 분할창에서 **ACL Rule** 탭을 클릭합니다.
3. **+ ACL Rule 생성**을 클릭합니다.
4. **ACL Rule 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * IP 프로토콜: `ICMP`
    * 출발지 CIDR: `0.0.0.0/0`
    * 목적지 CIDR: `linux-server-basic 플로팅 IP 주소/32`
    * 순서: `110`
    * 적용 방법: `차단`
5. 성공 창에서 **확인**을 클릭합니다.
6. **+ ACL Rule 생성**을 클릭합니다.
7. **ACL Rule 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * IP 프로토콜: `ICMP`
    * 출발지 CIDR: `작업하는 컴퓨터의 IP 주소/32`

        > [참고] 
        >
        > * 작업하는 컴퓨터의 IP 주소 확인 방법
        >     * 터미널 또는 PowerShell을 실행 후 `curl ifconfig.me`를 실행하면 **본인이 작업하는 컴퓨터의 IP 주소**를 확인할 수 있습니다.

    * 목적지 CIDR: `linux-server-basic 플로팅 IP 주소/32`
    * 순서: `109`
    * 적용 방법: `허용`
8. **ACL Rule 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * IP 프로토콜: `전체적용`
    * 출발지 CIDR: `0.0.0.0/0`
    * 목적지 CIDR: `0.0.0.0/0`
    * 순서: `32764`
    * 적용 방법: `허용`
9. **터미널** 또는 **PowerShell**에서 아래 명령어를 실행하여 통신 테스트를 진행합니다.

```
#bash
ping (linux-server-basic 플로팅 IP 주소)
```

* 작업하는 컴퓨터의 IP에서만 Ping 통신이 허용된 것을 확인합니다.

* 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-basic 플로팅 IP 주소`를 입력하여 접속을 확인합니다.
* 모든 IP에서 Http 통신이 허용된 것을 확인합니다.
<br></br>
![pic2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%854.png)

## 참고 자료

* [보안 그룹](https://docs.nhncloud.com/ko/Network/Security%20Groups/ko/overview/)
* [Network ACL](https://docs.nhncloud.com/ko/Network/Network%20ACL/ko/overview/)
* [Network Port](https://en.wikipedia.org/wiki/Port_(computer_networking))
* [ACL](https://en.wikipedia.org/wiki/Access-control_list)
* [Whitelist](https://en.wikipedia.org/wiki/Whitelist)
* [Blacklist](https://en.wikipedia.org/wiki/Blacklist_(computing))
* [TCP](https://en.wikipedia.org/wiki/TCP)
* [ICMP(Internet Control Message Protocol)](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol)
* [ping](https://en.wikipedia.org/wiki/Ping_(networking_utility))

## 이전 단계

* [4. 네트워크 설정과 인스턴스 생성](https://docs.nhncloud.com/ko/quickstarts/ko/network-setup/)

## 다음 단계

* [6. 데이터베이스 생성 및 연결](https://docs.nhncloud.com/ko/quickstarts/ko/create-database/)
