# 백업 및 복구
**Quickstarts > 9. 백업 및 복구**

이번 학습 모듈에서는 NHN Cloud 환경에서 애플리케이션과 데이터를 안전하게 보호하고 복구할 수 있는 방법을 학습합니다. 블록 스토리지 복제, 인스턴스 이미지 생성 및 이미지 기반 생성을 통해 데이터 유실을 방지하고 신속한 복구가 가능한 시스템을 구축합니다.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%B0%B1%EC%97%85%20%EB%B0%8F%20%EB%B3%B5%EA%B5%AC.png)
## 학습 목표

이번 학습 모듈에서 배울 내용은 다음과 같습니다.

* **인스턴스 이미지 생성**
    * 인스턴스 이미지 생성 기능을 활용해 서버 전체 이미지 주기적 백업
    * 운영 중인 인스턴스의 상태를 인스턴스 이미지로 저장 후 새로운 인스턴스 생성
* **블록 스토리지 복제 및 연결**
    * 블록 스토리지 복제 기능을 활용해 기존의 블록 스토리지를 복제한 뒤 인스턴스와 연결
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/diagram/%EB%AA%A8%EB%93%88%209.%20%EB%B0%B1%EC%97%85%20%EB%B0%8F%20%EB%B3%B5%EA%B5%AC.png)

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
    * NHN Cloud 홈페이지에 로그인 해야 합니다.

**본 가이드는 [8. 모니터링 설정](https://docs.nhncloud.com/ko/quickstarts/ko/configure-monitoring/) 이후 단계부터 시작됩니다.**

## 인스턴스 이미지를 통한 인스턴스 생성 및 블록 스토리지 연결

### 단계 1. 인스턴스 이미지 생성하기

> 앞의 학습 모듈에서 생성한 `linux-server-basic` 인스턴스의 이미지를 생성해 봅니다. 구동 중인 인스턴스는 이미지 생성 시 무결성을 보장하지 않으므로, 인스턴스를 중지한 뒤 이미지를 생성합니다.

1. NHN Cloud 콘솔 상단 메뉴에서 실습에 사용할 조직(`MyORG`), 프로젝트(`MyPRJ`), 그리고 `한국(평촌) 리전`을 선택합니다.
2. 콘솔 창 왼쪽 메뉴 중 **Compute - Instance**를 클릭합s니다.
3. 인스턴스 목록에서 `linux-server-basic` 인스턴스를 클릭하여 선택합니다.
4. 인스턴스 목록 위에 **···** 을 클릭 후 **인스턴스 중지**를 클릭합니다.
5. **인스턴스 중지** 창에서 **중지**를 클릭합니다.
6. 성공 창에서 **확인**을 클릭합니다.
7. 인스턴스 목록 위에 **이미지 생성**을 클릭합니다.
8. **이미지 생성** 창에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 이미지 이름: `linux-server-basic-image`
    * 삭제 보호: `사용 안 함`
    * **구동 중인 인스턴스의 이미지 생성은 무결성을 보장하지 않습니다. 진행하려면 체크박스를 선택해 주세요.** 체크박스를 선택합니다.
    > [주의]
    >
    > * 구동 중인 인스턴스 이미지 생성
    >     * 구동 중인 인스턴스를 이미지로 만들 경우 파일 시스템에 무결성을 보장받을 수 없습니다. 이에 따른 애플리케이션 구동에 문제가 발생할 경우도 있습니다. 가급적 인스턴스를 중지시킨 후 이미지를 생성할 수 있도록 권장합니다.
9. 성공 창에서 **확인**을 클릭합니다.
10. 콘솔 창 왼쪽 메뉴 중 **Compute - Image**를 클릭합니다.
11. Image 화면의 이미지 목록에서 `linux-server-basic-image`가 생성 중인 것을 확인합니다. 생성이 완료되면 해당 이미지의 상태표시 등이 초록색으로 표시됩니다.

### 단계 2. 인스턴스 이미지로 신규 인스턴스 생성하기

> 단계 1에서 생성한 `linux-server-basic-image` 인스턴스 이미지를 사용해 `linux-server-recovery` 인스턴스를 새로 생성해 봅니다.

1. 콘솔창 왼쪽 메뉴 중 **Compute - Instance**를 클릭합니다.
2. **인스턴스 생성**을 클릭합니다.
3. **인트턴스 생성** 창에서 아래 정보를 설정 후 **인스턴스 생성**을 클릭합니다.
    * 인스턴스 템플릿
        * 사용: `사용 안 함`
    * 이미지
        * **개인 이미지** 이미지 이름: `linux-server-basic-image` 선택
    * 인스턴스 정보
        * 가용성 영역: `임의의 가용성 영역`
        * 인스턴스 이름: `linux-server-recovery`
        * 인스턴스 타입: **인스턴스 타입 선택** > 인스턴스 타입 이름: `t2.c1m1` 클릭 후 **선택** 클릭
        * 인스턴스 수: `1`
        * 키페어: `MyKey`
    * 루트 블록 스토리지
        * 블록 스토리지 타입: `HDD`
        * 블록 스토리지 크기(GB): `20` GB
    * 네트워크 설정: `네트워크 인터페이스 생성` 선택
    * 네트워크
        * 사용 가능한 서브넷 항목에서 `MySubnet (192.168.0.0/24)` 리소스를 클릭하여 선택된 서브넷으로 사용
    * 플로팅 IP: **설정 변경** 클릭
        * 사용: `사용`
        * 삭제 보호: `사용 안 함`
        * 연결할 서브넷: `MySubnet (192.168.0.0/24)`
    * 보안 그룹
        * 보안 그룹 선택: `MySG-SSH, MySG-HTTP` 선택
    * 추가 블록 스토리지: 사용 안 함 (기본)
    * 사용자 스크립트: 빈 칸 (기본)
    * 삭제 보호: 사용 안 함 (기본)
4. 인스턴스 생성 정보 창에서 **인스턴스 생성**을 클릭합니다.
5. 인스턴스 생성 작업이 진행됩니다. 몇 분 내외로 인스턴스 생성이 완료됩니다.

### 단계 3. 생성한 인스턴스 접속하기

> 단계 2에서 생성한 `linux-server-recovery` 인스턴스의 플로팅 IP 주소를 통해 접속하는 방법을 알아봅니다.

1. 콘솔 창 왼쪽 메뉴 중 **Network - Floating IP** 를 클릭합니다.
2. 플로팅 IP 리소스 목록 중 연결된 장치가 `linux-server-recovery`인 IP 주소를 **복사** 후 **기록**합니다.
3. 웹 브라우저에서 새 창을 열어서 `http://복사한 linux-server-recovery 플로팅 IP 주소`를 입력하여 접속을 확인합니다.
4. 웹 페이지가 열리는 것을 확인합니다.
5. **터미널** 또는 **PowerShell**에서 아래 명령어를 실행하여 원격 접속 합니다.

```
#PowerShell
cd /(MyKey.pem 파일이 있는 디렉토리명)
    
ssh -i MyKey.pem ubuntu@복사한 linux-server-recovery 플로팅 IP 주소
```

* "Are you sure you want to continue connecting (yes/no/[fingerprint])?" 문구가 나오면 `yes`를 입력 후 엔터키를 입력합니다.
* lsb_release 명령어로 현재 접속한 Linux 버전을 확인합니다.

```
#bash
lsb_release -a
```

### 단계 4. 기존 블록 스토리지를 복제해 인스턴스에 연결하기

> 앞의 학습 모듈에서 생성한 `MyBS` 블록 스토리지를 복제한 뒤 `linux-server-recovery` 인스턴스와 연결하고 `MyBS` 블록 스토리지의 데이터를 조회해 봅니다.

1. 콘솔 창 왼쪽 메뉴 중 **Storage - Block Storage** 를 클릭합니다.
2. **Block Storage > 관리** 화면에 블록 스토리지 목록에서 `MyBS`를 선택합니다.
3. **블록 스토리지 복제**를 클릭합니다.
4. **블록 스토리지 복제** 화면에서 아래 정보를 설정 후 **확인**을 클릭합니다.
    * 대상 프로젝트: `동일 프로젝트`
    * 리전: 한국(평촌)
    * 블록 스토리지 이름: `MyBS-Duplicate`
    * 블록 스토리지 크기: `10GB`(고정)
    * 블록 스토리지 타입: `HDD`
    * 가용성 영역: 블록 스토리지 목록에서 연결정보가 `linux-server-recovery의 /dev/vda`인 블록 스토리지와 **동일한 가용성 영역**
    * **구동 중인 인스턴스의 블록 스토리지는 파일 시스템 무결성을 보장하지 않습니다. 진행하려면 체크 박스를 선택해 주세요.** 체크박스를 선택합니다.
5. 성공 창에서 **확인**을 클릭합니다.
6. Block Storage **관리 탭** 오른쪽에 있는 **복제 결과** 탭을 클릭합니다.
7. 복제 결과 목록에서 상태가 **SUCCESS**인 것을 확인합니다.
8. **관리** 탭을 클릭합니다.
9. `MyBS-Duplicate`를 선택 후 상단에 **연결 관리**를 클릭합니다.
10. **블록 스토리지 연결 관리** 창에서 아래 정보를 설정 후 **연결**을 클릭합니다.
    * 인스턴스에 연결: `linux-server-recovery`
11. 성공 창에서 **확인**을 클릭합니다.
12. `linux-server-recovery` 인스턴스에 원격 접속한 상태에서 아래 명령어를 통해 /mnt/vdb 경로에 다시 마운트합니다.

```bash
sudo mount -a
```

* 아래 명령어로 복제된 블록 스토리지의 값을 확인합니다.

```bash
cat /mnt/vdb/employees.csv
```

데이터베이스의 결과값이 csv 파일로 조회되는 것을 확인합니다.

## 참고 자료

* [이미지](https://docs.nhncloud.com/ko/Compute/Image/ko/overview/)
* [이미지 생성](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#_13)
* [블록 스토리지 복제](https://docs.nhncloud.com/ko/Storage/Block%20Storage/ko/console-guide/#_11)
* [Snapshot](https://en.wikipedia.org/wiki/Snapshot_(computer_storage))
* [Backup](https://en.wikipedia.org/wiki/Backup)

## 이전 단계

* [8. 모니터링 설정](https://docs.nhncloud.com/ko/quickstarts/ko/configure-monitoring/)

## 다음 단계

* [10. 확장성과 성능 최적화](https://docs.nhncloud.com/ko/quickstarts/ko/optimze-performance/)
