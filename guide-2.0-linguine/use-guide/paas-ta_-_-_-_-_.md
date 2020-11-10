# PaaS-TA\_플랫폼\_설치\_자동화\_사용\_가이드

## Table of Contents

1. [문서 개요](paas-ta_-_-_-_-_.md#1)
   * [목적](paas-ta_-_-_-_-_.md#2)
   * [범위](paas-ta_-_-_-_-_.md#3)
   * [참고자료](paas-ta_-_-_-_-_.md#4)
2. [플랫폼 설치 자동화 메뉴얼](paas-ta_-_-_-_-_.md#5)
   * [플랫폼 설치 자동화 화면 설명](paas-ta_-_-_-_-_.md#6)
     * [로그인](paas-ta_-_-_-_-_.md#7)
     * [환경설정 및 관리 -&gt; 설치관리자 설정](paas-ta_-_-_-_-_.md#8)
     * [환경설정 및 관리 -&gt; 스템셀 관리](paas-ta_-_-_-_-_.md#9)
     * [환경설정 및 관리 -&gt; 릴리즈 관리](paas-ta_-_-_-_-_.md#10)
     * [플랫폼 설치 자동화 관리 -&gt; 코드 관리](paas-ta_-_-_-_-_.md#11)
     * [플랫폼 설치 자동화 관리 -&gt; 권한 관리](paas-ta_-_-_-_-_.md#12)
     * [플랫폼 설치 자동화 관리 -&gt; 사용자 관리](paas-ta_-_-_-_-_.md#13)
     * [플랫폼 설치 -&gt; BOOTSTRAP 설치](paas-ta_-_-_-_-_.md#14)
     * [플랫폼 설치 -&gt; BOSH 설치](paas-ta_-_-_-_-_.md#15)
     * [플랫폼 설치 -&gt; CF 설치](paas-ta_-_-_-_-_.md#16)
     * [플랫폼 설치 -&gt; DEIGO 설치](paas-ta_-_-_-_-_.md#17)
     * [플랫폼 설치 -&gt; CF & DEIGO 통합 설차](paas-ta_-_-_-_-_.md#18)
     * [플랫폼 설치 -&gt; 서비스팩 설치](paas-ta_-_-_-_-_.md#19)
     * [정보조회 -&gt; 스템셀 업로드](paas-ta_-_-_-_-_.md#20)
     * [정보조회 -&gt; 릴리즈 업로드](paas-ta_-_-_-_-_.md#21)
     * [정보조회 -&gt; 배포 정보](paas-ta_-_-_-_-_.md#22)
     * [정보조회 -&gt; Task 정보](paas-ta_-_-_-_-_.md#23)
     * [정보조회 -&gt; VM 관리](paas-ta_-_-_-_-_.md#24)
     * [정보조회 -&gt; Property 관리](paas-ta_-_-_-_-_.md#25)
     * [정보조회 -&gt; 스냅샷 관리](paas-ta_-_-_-_-_.md#26)
     * [정보조회 -&gt; Manifest 관리](paas-ta_-_-_-_-_.md#27)
3. [플랫폼 설치 자동화 활용](paas-ta_-_-_-_-_.md#28)
   * [플랫폼 설치 자동화 파일 관리](paas-ta_-_-_-_-_.md#29)
   * [코드 관리](paas-ta_-_-_-_-_.md#30)
   * [권한 관리](paas-ta_-_-_-_-_.md#31)
   * [사용자 관리](paas-ta_-_-_-_-_.md#32)
   * [스템셀과 릴리즈](paas-ta_-_-_-_-_.md#33)
   * [BOOTSTRAP 설치하기](paas-ta_-_-_-_-_.md#34)
     * [스템셀 다운로드](paas-ta_-_-_-_-_.md#35)
     * [릴리즈 다운로드](paas-ta_-_-_-_-_.md#36)
     * [BOOTSTRAP 설치](paas-ta_-_-_-_-_.md#37)
     * [설치 관리자 설정](paas-ta_-_-_-_-_.md#38)
   * [BOSH 설치하기](paas-ta_-_-_-_-_.md#39)
     * [스템셀 업로드](paas-ta_-_-_-_-_.md#40)
     * [릴리즈 업로드](paas-ta_-_-_-_-_.md#41)
     * [BOSH 설치](paas-ta_-_-_-_-_.md#42)
     * [설치 관리사 설정](paas-ta_-_-_-_-_.md#43)
   * [CF 설치하기](paas-ta_-_-_-_-_.md#44)
     * [스템셀 업로드](paas-ta_-_-_-_-_.md#45)
     * [릴리즈 업로드](paas-ta_-_-_-_-_.md#46)
     * [CF 설치](paas-ta_-_-_-_-_.md#47)
   * [DIEGO 설치하기](paas-ta_-_-_-_-_.md#48)
     * [스템셀 업로드](paas-ta_-_-_-_-_.md#49)
     * [릴리즈 업로드](paas-ta_-_-_-_-_.md#50)
     * [DIEGO 설치](paas-ta_-_-_-_-_.md#51)
   * [CF & DIEGO 통합 설치](paas-ta_-_-_-_-_.md#52)
     * [스템셀 업로드](paas-ta_-_-_-_-_.md#53)
     * [릴리즈 업로드](paas-ta_-_-_-_-_.md#54)
     * [CF & DIEGO 설치](paas-ta_-_-_-_-_.md#55)
   * [서비스팩 설치](paas-ta_-_-_-_-_.md#56)
     * [스템셀 업로드](paas-ta_-_-_-_-_.md#57)
     * [릴리즈 업로드](paas-ta_-_-_-_-_.md#58)
     * [Manifest 업로드](paas-ta_-_-_-_-_.md#59)
     * [서비스팩 설치](paas-ta_-_-_-_-_.md#60) 
   * [프로퍼티 관리](paas-ta_-_-_-_-_.md#61)           

## 1.  문서 개요

### 1.1.  목적

본 문서는 플랫폼 설치 자동화 시스템의 사용 절차에 대해 기술하였다.

### 1.2.  범위

본 문서에서는 Linux 환경\(Ubuntu 14.04\)을 기준으로 플랫폼 설치 자동화를 사용하는 방법에 대해 작성되었다.

### 1.3.  참고자료

본 문서는 Cloud Foundry의 Document를 참고로 작성하였다.

BOSH Document: [**http://bosh.io**](http://bosh.io) CF & Diego Document: [http://docs.cloudfoundry.org/](http://docs.cloudfoundry.org/)

## 2.  플랫폼 설치 자동화 매뉴얼

플랫폼 설치 관리자는 설치관리자 등록정보 관리 및 기본 설치관리자를 지정하는 환경 설정하는 부분과 기본 설치관리자로부터 필요한 정보를 조회/업로드를 수행하는 부분 그리고 설치관리자를 이용해서 PaaS-TA를 설치하는 부분으로 구성되어 있다.

| 분류 | 메뉴 | 설명 |
| :--- | :--- | :--- |
| 환경설정 및 관리 | 설치관리자 설정 | BOSH 디렉터\(설치관리자\) 정보를 관리하는 화면 |
| 스템셀 관리 | BOSH Public 스템셀 등록/삭제하는 화면 |  |
| 릴리즈 관리 | 릴리즈 등록/삭제하는 화면 |  |
| 플랫폼 설치 자동화 관리 | 코드 관리 | 공통 코드를 등록/수정/삭제 등 관리하는 화면 |
| 권한 관리 | 권한 정보를 등록/수정/삭제 등 관리하는 화면 |  |
| 사용자 관리 | 사용자 정보를 등록/수정/삭제 등 관리하는 화면 |  |
| 플랫폼 설치 | BOOTSTRAP 설치 | BOOTSTRAP를 설치하는 화면 |
| BOSH 설치 | BOSH를 설치하는 화면 |  |
| CF 설치 | CF를 설치하는 화면 |  |
| Diego 설치 | Diego를 설치하는 화면 |  |
| CF 및 Diego 설치 | CF 및 Diego를 설치하는 화면 |  |
| 서비스팩 설치 | 서비스팩을 설치하는 화면 |  |
| 배포 정보 조회 및 관리 | 스템셀 업로드 | 기본 설치관리자에 스템셀 업로드 및 삭제하는 화면 |
| 릴리즈 업로드 | 기본 설치관리자에 릴리즈 업로드 및 삭제하는 화면 |  |
| 배포 정보 | 기본 설치관리자에 배포된 배포목록을 확인하는 화면 |  |
| Task 정보 | 기본 설치관리자가 수행한 Task 정보를 확인하는 화면 |  |
| VM 관리 | 기본 설치관리자에 배포된 배포의 VM을 관리하는 화면 |  |
| Property 관리 | 기본 설치관리자에 배포된 배포의 Property를 관리하는 화면 |  |
| 스냅샷 관리 | 기본 설치관리자에 배포된 배포의 스냅샷을 관리하는 화면 |  |
| Manifest 관리 | 서비스팩 설치에 필요한 Manifest를 관리하는 화면 |  |

### 2.1.  플랫폼 설치 자동화 화면 설명

본 장에서는 플랫폼 설치 자동화를 구성하는 20개의 메뉴 및 로그인에 대한 설명을 기술한다.

#### 2.1.1. _**로그인**_

“로그인” 화면은 플랫폼 설치 자동화 관리자가 로그인하는 화면으로 사용자 정보 생성 후 최초 로그인을 했을 경우 비밀번호 변경 화면을 통해 비밀번호 정보를 변경한다.

**1.  로그인**

* 플랫폼 설치 관리자는 로그인 첫 아이디와 비밀번호를\(admin/admin\) 입력 후 로그인 버튼을 클릭한다.

![](../../.gitbook/assets/login%20%289%29.png)

**2.  비밀번호 변경**

* 비밀번호 변경 화면을 통해 비밀번호를 수정할 수 있다.

![](../../.gitbook/assets/passwordChange%20%282%29.png)

**3.  플랫폼 설치 자동화 접속**

![](../../.gitbook/assets/Dashboard%20%282%29.png)

#### 2.1.2. _**환경설정 및 관리 -&gt; 설치관리자 설정**_

“설치관리자 설정” 화면은 BOSH의 디렉터 정보 관리 및 설정하는 화면으로 BOOTSTRAP\(Microbosh\) 또는 BOSH의 디렉터 정보를 관리하는 화면이다.

※ 설치 관리자에서 설정을 추가하기 위해서는 먼저 BOOTSTRAP을 설치 해야한다.

![](../../.gitbook/assets/DirectorConfig%20%282%29.png)

**1.  설정 추가**

* 설치 관리자 정보를 등록하는 기능으로 BOSH 디렉터의 IP, 포트번호,계정, 비밀번호 입력 후 확인 버튼을 클릭한다.

![](../../.gitbook/assets/DirectorConfigAdd%20%282%29.png)

**2.  설정 수정**

* 설치 관리자 등록 목록에서 선택된 설치 관리자 정보를 수정하는 기능으로 계정과 비밀번호를 수정할 수 있다.

**3.  설정 삭제**

* 설치 관리자 등록 목록에서 선택된 설치 관리자 정보를 삭제하는 기능

**4.  기본 설치 관리자로 설정**

* 설치 관리자 등록 목록에서 선택된 설치 관리자를 기본 설치 관리자로 설정하는 기능

**5.  설치 관리자 목록**

* 등록된 설치 관리자 목록을 보여준다.

**6.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

#### 2.1.3.  _**환경설정 및 관리 -&gt; 스템셀 관리**_

다운로드한 스템셀 목록을 조회하고, 필요한 스템셀을 등록 및 삭제 할 수 있는 화면이다.

![](../../.gitbook/assets/StemcellConfig%20%282%29.png)

**1.  등록**

* 등록할 스템셀 정보를 입력하여 스템셀 정보를 저장하고 스템셀 파일을 플랫폼 설치 자동화의 스템셀 디렉토리\(~/.bosh\_plugin/stemcell\)로 다운로드를 수행한다.

**2.  삭제**

* 플랫폼 설치 자동화에 다운로드 된 스템셀을 삭제하는 기능을 수행한다.

#### 2.1.4.  환경설정 및 관리 -&gt; 릴리즈 관리

다운로드한 릴리즈 목록을 조회하고, 필요한 릴리즈를 등록/삭제 할 수 있는 화면이다.

![](../../.gitbook/assets/ReleaseConfig%20%282%29.png)

**1.  등록**

* 등록할 릴리즈 정보를 입력하여 릴리즈 정보를 저장 하고 플랫폼 설치 자동화의 릴리즈 디렉토리\(~/.bosh\_plugin/release\)로 다운로드를 수행한다.

**2.  삭제**

* 플랫폼 설치 자동화에 등록된 릴리즈를 삭제하는 기능을 수행한다.

#### 2.1.5.  _**플랫폼 설치 자동화 관리 -&gt; 코드 관리**_

코드 관리 코드 그룹, 코드 목록을 조회하고, 필요한 코드 그룹을 등록, 수정, 삭제 할 수 있고 해당 코드 그룹의 하위 코드를 등록, 수정, 삭제 할 수 있는 화면이다.

![](../../.gitbook/assets/CodeManagement%20%282%29.png)

**1.  코드 그룹 조회**

* 플랫폼 설치 자동화에 등록 된 상위 공통 코드를 조회 한다.

**2.  코드 조회**

* 선택 한 코드 그룹에 해당 하는 플랫폼 설치 자동화에 등록 된 하위 공통 코드를 조회 한다.

**3.  코드 그룹 등록**

* 코드 그룹 정보를 등록 하는 기능으로 코드 그룹 명, 코드 그룹 값, 설명을 입력 하고 확인 버튼을 클릭한다.

**4.  코드 그룹 수정**

* 코드 그룹 목록에서 선택 된 코드 그룹 정보를 수정하는 기능으로 코드 그룹 명과 설명을 수정 할 수 있다.

**5.  코드 그룹 삭제**

* 코드 그룹 목록에서 선택 된 코드 그룹을 삭제 하는 기능

**6.  코드 등록**

* 코드를 등록 하는 기능으로 하위 그룹, 코드명\(영문\), 코드명\(한글\), 코드 값, 설명을 입력 하고 확인 버튼을 클릭 한다.

**7.  코드 수정**

* 코드 목록에서 선택 된 코드 정보를 수정하는 기능으로 하위 그룹, 코드명\(영문\), 코드명\(한글\), 설명을 수정 할 수 있다.

**8.  코드 삭제**

* 코드 목록에서 선택 된 코드를 삭제 하는 기능

#### 2.1.6.  _**플랫폼 설치 자동화 관리 -&gt; 권한 관리**_

권한 관리 권한 그룹, 권한 목록을 조회하고, 필요한 권한 그룹을 등록, 수정, 삭제 할 수 있고 해당 권한 그룹의 상세 권한을 등록할 수 있는 화면이다.

![](../../.gitbook/assets/AuthManagement%20%282%29.png)

**1.  권한 그룹 조회**

* 플랫폼 설치 자동화에 등록 된 권한 그룹을 조회 한다.

**2.  상세 권한 조회**

* 선택 한 권한 그룹에 해당 하는 플랫폼 설치 자동화에 등록 된 상세 권한을 조회 한다.

**3.  권한 그룹 등록**

* 권한 그룹 정보를 등록 하는 기능으로 권한 그룹 명, 설명을 입력 하고 확인 버튼을 클릭 한다.

**4.  권한 그룹 수정**

* 권한 그룹 목록에서 선택 된 권한 그룹 정보를 수정하는 기능으로 권한 그룹 명과 설명을 수정 할 수 있다.

**5.  권한 그룹 삭제**

* 권한 그룹 목록에서 선택 된 권한 그룹을 삭제 하는 기능

**6.  상세 권한 등록**

* 권한 그룹 목록에서 선택 된 권한 그룹에 해당하는 상세 권한 목록을 등록 하는 기능으로 권한 설정 허용/거부를 입력 후 확인 버튼을 클릭 한다.

#### 2.1.7.  _**플랫폼 설치 자동화 관리 -&gt; 사용자 관리**_

사용자 관리 사용자 목록을 조회하고 사용자를 등록, 수정, 삭제 할 수 있는 화면이다.

![](../../.gitbook/assets/UseManagement%20%282%29.png)

**1.  사용자 조회**

* 플랫폼 설치 자동화에 등록 된 사용자 목록을 조회 한다.

**2.  사용자 등록**

* 사용자 정보를 등록 하는 기능으로 사용자 아이디, 이름, Email, 권한 그룹을 입력하고 확인 버튼을 클릭 한다.

**3.  사용자 수정**

* 사용자 목록에서 선택 된 사용자 정보를 수정 하는 기능으로 비밀번호, 이름, Email, 권한을 수정 할 수 있다.

**4.  사용자 삭제**

* 사용자 목록에서 선택 된 사용자 정보를 삭제 하는 기능이다.

#### 2.1.8.  _**플랫폼 설치 -&gt; BOOTSTRAP 설치**_

클라우드 환경에 BOOTSTRAP\(Microbosh\)를 설치하는 화면으로 상단의 버튼을 이용해서 설치/수정/삭제 기능을 제공한다.

![](../../.gitbook/assets/BootStrapInstall%20%282%29.png)

**1.  설치**

* BOOTSTRAP 설치할 수 있는 기능을 수행한다.

**2.  수정**

* BOOTSTRAP 목록에서 선택된 BOOTSTRAP 정보 확인 및 수정 후 재설치하는 기능을 수행한다.

**3.  삭제**

* BOOTSTRAP 목록에서 선택된 BOOTSRAP을 삭제하는 기능을 수행한다.

#### 2.1.9.  _**플랫폼 설치 -&gt; BOSH 설치**_

클라우드 환경에 BOSH를 설치하는 화면으로 상단의 버튼을 이용해서 설치/수정/삭제 기능을 제공한다.

![](../../.gitbook/assets/BoshInstall%20%282%29.png)

**1.  설치**

* BOSH를 설치할 수 있는 기능을 수행한다.

**2.  수정**

* BOSH 목록에서 선택된 BOSH 정보 확인 및 수정 후 재설치하는 기능을 수행한다.

**3.  삭제**

* BOSH 목록에서 선택된 BOSH을 삭제하는 기능을 수행한다.

**4.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**5.  BOSH 목록**

* BOSH 설치 목록을 조회한다.

#### 2.1.10. _**플랫폼 설치 -&gt; CF 설치**_

설치 관리자\(BOOTSTRAP 또는 BOSH\)를 이용해서 PaaS-TA Controller인 CF를 설치하는 화면으로 상단의 버튼을 이용해서 설치/수정/삭제 기능을 제공한다.

![](../../.gitbook/assets/CfInstall%20%282%29.png)

**1.  설치**

* CF를 설치할 수 있는 기능을 수행한다.

**2.  수정**

* CF 목록에서 선택된 CF 정보 확인 및 수정 후 재설치하는 기능을 수행한다.

**3.  삭제**

* CF 목록에서 선택된 CF를 삭제하는 기능을 수행한다.

**4.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**5.  CF 목록**

* CF 설치 목록을 조회한다.

#### 2.1.11. _**플랫폼 설치 -&gt; DIEGO 설치**_

설치 관리자\(BOOTSTRAP 또는 BOSH\)를 이용해서 PaaS-TA Container인 DIEGO를 설치하는 화면으로 상단의 버튼을 이용해서 설치/수정/삭제 기능을 제공한다.

![](../../.gitbook/assets/DiegoInstall%20%282%29.png)

**1.  설치**

* DIEGO를 설치할 수 있는 기능을 수행한다.

**2.  수정**

* DIEGO 목록에서 선택된 DIEGO 정보 확인 및 수정 후 재설치하는 기능을 수행한다.

**3.  삭제**

* DIEGO 목록에서 선택된 DIEGO를 삭제하는 기능을 수행한다.

**4.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**5.  DIEGO 목록**

* DIEGO 설치 목록을 조회한다.

#### 2.1.12. _**플랫폼 설치 -&gt; CF & DIEGO 통합 설치**_

설치 관리자\(BOOTSTRAP 또는 BOSH\)를 이용해서 PaaS-TA Controller인 CF와 PaaS-TA Container인 Diego를 통합 설치하는 화면으로 상단의 버튼을 이용해서 설치/수정/삭제 기능을 제공한다.

![](../../.gitbook/assets/Cf_DiegoInstall%20%282%29.png)

**1.  설치**

* CF 및 Diego를 통합 설치할 수 있는 기능을 수행한다.

**2.  수정**

* CF & Diego 통합 설치 목록에서 선택된 CF & Diego 정보 확인 및 수정 후 재설치하는 기능을 수행한다.

**3.  삭제**

* CF & Diego 통합 설치 목록에서 선택된 CF & Diego를 삭제하는 기능을 수행한다.

**4.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**5.  CF & Diego 통합 설치 목록**

* CF & Diego 통합 설치 목록을 조회한다.

#### 2.1.13. _**플랫폼 설치 -&gt; 서비스팩 설치**_

설치 관리자\(BOOTSTRAP 또는 BOSH\)를 이용해서 PaaS-TA Controller인 CF에 서비스팩을 설치하는 화면으로 상단의 버튼을 이용해서 설치/삭제 기능을 제공한다.

![](../../.gitbook/assets/ServicePackInstall%20%282%29.png)

**1.  설치**

* CF에 서비스팩을 설치할 수 있는 기능을 수행한다.

**2.  삭제**

* 서비스팩 목록에서 선택된 서비스팩을 삭제하는 기능을 수행한다.

**3.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**4.  서비스팩 목록**

* 서비스팩 설치 목록을 조회한다.

#### 2.1.14. _**정보조회 -&gt; 스템셀 업로드**_

설치 관리자로부터 스템셀 정보를 조회/업로드/삭제할 수 있는 기능을 제공하는 화면이다.

![](../../.gitbook/assets/StemcellUpload%20%284%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  스템셀 삭제**

* 설치 관리자에 업로드 된 스템셀을 삭제하는 기능을 제공한다.

**3.  업로드 된 스템셀 목록**

* 설치 관리자에 업로드 된 스템셀 목록을 보여준다.

**4.  스템셀 업로드**

* 설치 관리자로 스템셀을 업로드할 수 있는 기능을 제공한다.

**5.  다운로드 된 스템셀 목록**

* 플랫폼 설치 자동화에 다운로드 된 스템셀을 목록을 보여준다.

#### 2.1.15. _**정보조회 -&gt; 릴리즈 업로드**_

설치 관리자로부터 릴리즈 정보를 조회/업로드/삭제할 수 있는 기능을 제공하는 화면이다.

![](../../.gitbook/assets/ReleaseUpload%20%284%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  업로드 된 릴리즈 목록**

* 설치 관리자에 업로드 된 업로드 된 릴리즈 목록을 보여준다.

**3.  다운로드 된 릴리즈 목록**

* 플랫폼 설치 자동화에 다운로드 된 릴리즈 목록을 보여준다.

**4.  릴리즈 삭제**

* 설치 관리자에 업로드 된 업로드 된 릴리즈를 삭제 하는 기능을 제공

  한다.

**5.  릴리즈 업로드**

* 설치 관리자로 릴리즈 업로드할 수 있는 기능을 제공한다.

#### 2.1.16. _**정보조회 -&gt; 배포정보**_

설치 관리자로부터 배포된 배포 정보를 조회하는 기능을 제공하는 화면이다.

![](../../.gitbook/assets/DeploymentInfo%20%282%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  설치 목록**

* 설치 관리자를 이용해서 배포된 배포 목록 정보를 보여준다.

#### 2.1.17. _**정보조회 -&gt; Task 정보**_

설치 관리자가 수행한 Task 작업들에 대한 목록 조회 및 상세 로그 정보를 확인하는 기능을 제공하는 화면이다.

![](../../.gitbook/assets/TaskInfo%20%282%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  Task 실행 이력**

* 설치 관리자가 수행한 Task의 작업 목록을 보여준다.

**3.  디버그 로그**

* 선택된 Task 작업에 대한 디버그 로그를 보여준다.

**4.  이벤트 로그**

* 선택된 Task 작업에 대한 이벤트 로그를 보여준다.

#### 2.1.18. _**정보조회 -&gt; VM 관리**_

VM 관리 기본 설치 관리자를 통해 배포한 VM을 조회, 관리 하는 기능을 제공 하는 화면 이다.

![](../../.gitbook/assets/VmInfo%20%282%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  배포 명 목록**

* 기본 설치 관리자가 배포한 VM의 배포명 목록을 보여준다.

**3.  VM 목록**

* VM의 상세 명칭, 상태 값, Type, AZ, IPs, Load, Cpu 타입 등을 보여 준다.

**4.  배포 명 조회**

* 배포명 목록에서 선택 된 배포명을 통해 해당 배포명을 갖는 VM의 상세 목록을 조회하는 기능

**5.  로그 다운로드**

* VM 목록에서 선택 된 VM의 Agent 로그, Job 로그를 선택 하여 다운로드 할 수 있는 기능

**6.  Job 시작**

* VM 목록에서 선택 된 중지 상태 중인 VM을 시작하는 기능

**7.  Job 중지**

* VM 목록에서 선택 된 시작 상태 중인 VM을 중지하는 기능

**8.  Job 재시작**

* VM 목록에서 선택 된 VM을 재시작 하는 기능

**9.  Job 재생성**

* VM 목록에서 선택 된 VM을 재생성 하는 기능

#### 2.1.19. 정보조회 -&gt; Property 관리

Property 관리 설치 관리자가 배포한 VM 정보의 Property를 조회, 생성, 수정, 삭제, 상세보기 할 수 있는 기능을 제공하는 화면

![](../../.gitbook/assets/PropertyInfo%20%282%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  배포 명 목록**

* 기본 설치 관리자가 배포한 VM의 배포명 목록을 보여준다.

**3.  배포 명 조회 기능**

* 배포명 목록에서 선택 된 배포명을 통해 해당 배포명을 갖는 Property의 상세 목록을 조회하는 기능

**4.  Property 목록 조회**

* 배포 명을 통해 조회 된 해당 배포 정보의 Property 명, Property 값을 보여 준다.

**5.  Property 생성**

* Property 정보를 저장하는 기능으로 Property 명, Property 값을 입력 하고 저장 버튼을 클릭 한다.

**6.  Property 수정**

* 선택 된 Property 정보를 수정하는 기능으로 Property 값을 수정하고 수정 버튼을 클릭 한다.

**7.  Property 삭제**

* 선택 된 Property 정보를 삭제하는 기능

**8.  Property 상세 보기**

* 선택 된 Property를 상세 보기 할 수 있는 기능

#### _**2.1.20. 정보조회 -&gt; 스냅샷 관리**_

스냅샷 관리 설치 관리자가 배포한 VM 정보의 스냅샷을 조회, 삭제, 전체 삭제 할 수 있는 기능을 제공하는 화면

![](../../.gitbook/assets/SnapshotInfo%20%282%29.png)

**1.  설치 관리자**

* 기본 설치 관리자로 설정된 설치 관리자 정보를 보여준다.

**2.  배포 명 목록**

* 기본 설치 관리자가 배포한 VM의 배포명 목록을 보여준다.

**3.  배포 명 조회 기능**

* 배포명 목록에서 선택 된 배포명을 통해 해당 배포명을 갖는 스냅샷의 상세 목록을 조회하는 기능

**4.  스냅샷 목록 조회**

* 배포 명을 통해 조회 된 해당 배포 정보의 JobName, Uuid, SnapshotCid 등 스냅샷 상세 정보를 보여 준다

**5.  스냅샷 삭제**

* 선택 된 스냅샷을 삭제 하는 기능

**6.  스냅샷 전체 삭제**

* 조회 된 스냅샷을 전체 삭제 하는 기능.

#### _**2.1.21. 정보조회 -&gt; Manifest 관리**_

Manifest 관리 서비스팩 설치에 필요한 Manifest를 플랫폼 설치 자동화에 업로드, 수정, 삭제, 업로드 된 Manifest 파일을 로컬에 다운로드 할 수 있는 기능을 제공 하는 화면

![](../../.gitbook/assets/ManifestInfo%20%282%29.png)

**1.  Manifest 목록**

* 플랫폼 설치 자동화에 업로드 된 Manifest 파일 목록을 보여준다.

**2.  Manifest 업로드**

* 로컬에 있는 Manifest 파일을 플랫폼 설치 자동화의 Manifest 관리 디렉토리\(~/.bosh\_plugin/deployment/manifest\)로 업로드를 실행하는 기능.

IaaS, 설명, 파일을 입력 후 업로드 버튼을 클릭 한다.

**3.  Manifest 다운로드**

* 선택 한 업로드 된 Manifest 파일을 로컬 다운로드 폴더 경로로 다운로드 하는 기능

**4.  Manifest 수정**

* 선택 한 업로드 된 Manifest 파일을 상세 보기하여 수정 할 수 있는 기능

**5.  Manifest 삭제**

* 선택 한 업로드 된 Manifest 파일을 삭제 하는 기능

## 3.  플랫폼 설치 자동화 활용

BOSH는 클라우드 환경에 서비스를 배포하고 소프트웨어 릴리즈를 관리해주는 오픈 소스로 BooStrap은 하나의 VM에 설치 관리자의 모든 컴포넌트를 설치한 것으로 PaaS-TA 설치를 위한 관리자 기능을 담당한다.

플랫폼 설치 자동화를 이용해서 클라우드 환경에 PaaS-TA를 설치하기 위해서는 **스템셀**과 **소프트웨어 릴리즈**, 배포 **Manifest 파일** 3가지 요소가 필요하다. 스템셀은 클라우드 환경에 VM을 생성하기 위해 사용할 기본 이미지이고, 소프트웨어 릴리즈는 VM에 설치할 소프트웨어 패키지들을 묶어 놓은 파일이고, 배포 Manifest파일은 스템셀과 소프트웨어 릴리즈를 이용해서 서비스를 어떤 식으로 구성할지를 정의해 놓은 명세서이다. 다음 그림은 BOOTSTRAP과 BOSH를 이용하여 PaaS-TA를 설치하는 절차이다.

![](../../.gitbook/assets/PlatformProcess%20%282%29.png)

### 3.1.  _**플랫폼 설치 자동화 파일 관리**_

플랫폼 설치 관리자에서 파일 관리라 함은 배포에 필요한 스템셀과 릴리즈 그리고 배포 파일 관리를 의미한다. 플랫폼 설치 관리자 실행 시 실행 계정의 Home 디렉토리에 .bosh\_plugin 디렉토리를 생성하고 배포에 필요한 스템셀, 릴리즈, 배포파일을 관리하도록 기준 디렉토리가 결정되어 있다.

| 설정 디렉토리 | 설명 |
| :--- | :--- |
| {HOME}/.bosh\_plugin | 플랫폼 설치 자동화가 사용하는 기준 디렉토리 |
| {HOME}/.bosh\_plugin/stemcell | 스템셀 관리 디렉토리 |
| {HOME}/.bosh\_plugin/release | 릴리즈 관리 디렉토리 |
| {HOME}/.bosh\_plugin/deployment | 배포 관리 디렉토리 |
| {HOME}/.bosh\_plugin/deployment/manifest | 서비스팩 Manifest 관리 디렉토리 |
| {HOME}/.bosh\_plugin/key | CF 및 Diego 키 관리 디렉토리 |
| {HOME}/.bosh\_plugin/lock | 스템셀, 릴리즈, 배포 등을 수행 시 lock 관리 디렉토리 |
| {HOME}/.bosh\_plugin/temp | 임시 디렉토리 |

플랫폼 설치 자동화를 이용해서 다운로드 된 스템셀과 생성된 배포 파일은 해당 디렉토리에 각각 다운로드 또는 생성되어 관리된다.

### 3.2.  _**코드 관리**_

플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치 자동화 관리” -&gt; “코드 관리” 메뉴로 이동한다. 플랫폼 설치 자동화는 “코드 관리” 메뉴에서 기본적으로 배포 유형 및 배포 상태 / 릴리즈 유형 / 권한 / IaaS 유형 / 국가 코드 / OS 유형 등의 코드 정보를 제공한다. \(코드 관리 화면 설명은 코드 관리 화면 설명은 [2.1.5](paas-ta_-_-_-_-_.md#11) 참고\)

**1.  코드 그룹 등록**

* 코드 그룹 “등록” 버튼을 클릭 후 코드 그룹 정보를 입력하고 “확인” 버튼을 클릭한다.
* 중복된 코드 그룹 값은 등록할 수 없다.

![](../../.gitbook/assets/codeGroupAdd%20%282%29.png)

**2.  코드 그룹 수정**

* 코드 그룹 “수정” 버튼을 클릭 후 코드 그룹 정보를 수정하고 “확인” 버튼을 클릭한다.

![](../../.gitbook/assets/codeGroupModify%20%282%29.png)

**3.  코드 등록**

* 코드 “등록” 버튼을 클릭 후 코드 정보를 입력하고 “확인” 버튼을 클릭한다.
* 하위 그룹을 선택하지 않을 경우 해당 코드 그룹의 상위 코드가 등록된다.
* 하위 그룹을 선택했을 경우 해당 코드 그룹의 선택한 하위 그룹의 하위 코드가 등록된다.

![](../../.gitbook/assets/codeAdd%20%282%29.png)

**4.  코드 수정**

* 코드 “수정” 버튼을 클릭 후 코드 정보를 수정하고 “확인” 버튼을 클릭한다.

![](../../.gitbook/assets/codeModify%20%282%29.png)

### 3.3.  _**권한 관리**_

플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치 자동화 관리” -&gt; “권한 관리” 메뉴로 이동한다. 플랫폼 설치 자동화는 “권한 관리” 메뉴에서 기본적으로 플랫폼 설치 사용자 / 플랫폼 설치 자동화 관리자 등의 권한 그룹 정보 및 해당 권한 그룹의 상세 권한 정보를 제공한다. \(권한 관리 화면 설명은 [2.1.6](paas-ta_-_-_-_-_.md#12) 참고\)

상세 권한 정보는 “코드 관리” 화면에서 코드 그룹 명 “ROLE”의 하위 코드를 통해 관리할 수 있다.

**1.  코드 등록**

* 코드 그룹 목록에서 “ROLE”을 선택 후 코드 “등록” 버튼을 클릭하고 코드 등록 화면에서 권한 코드 정보를 입력 후 “확인” 버튼을 클릭한다.

![](../../.gitbook/assets/authCodeADD%20%282%29.png)

**2.  권한 그룹 등록**

* 권한 그룹 “등록” 버튼을 클릭 후 권한 그룹 정보를 입력하고 “확인” 버튼을 클릭한다.
* 권한 그룹명은 중복해서 등록할 수 없다.

![](../../.gitbook/assets/authGroupadd%20%282%29.png)

**3.  권한 그룹 수정**

* 권한 그룹 “수정” 버튼을 클릭 후 권한 그룹 정보를 수정하고 “확인” 버튼을 클릭한다.

![](../../.gitbook/assets/authGroupModify%20%282%29.png)

**4.  권한 상세 등록/수정**

* 권한 상세 “등록” 버튼을 클릭 후 권한 상세 정보를 등록/수정하고 “확인” 버튼을 클릭한다.
* 권한 설정 항목에서 대시보드 / 기본 시스템 사용자 / 기본 시스템 조회 등의 권한은 기본적으로 허용으로 설정되어 있고, 그 외의 권한은 거부로 설정되어 있다.

![](../../.gitbook/assets/authDetailAdd%20%282%29.png)

### 3.4.  _**사용자 관리**_

플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치 자동화 관리” -&gt; “사용자 관리” 메뉴로 이동한다. 플랫폼 설치 자동화는 “사용자 관리” 메뉴에서 기본적으로 플랫폼 설치 자동화 관리자 정보\(admin/admin\)를 제공한다. \(사용자 관리 화면 설명은 [2.1.7](paas-ta_-_-_-_-_.md#13) 참고\)

**1.  사용자 등록**

* 사용자 “등록” 버튼을 클릭 후 사용자 정보 입력 및 해당 사용자의 권한을 선택하여 “확인” 버튼을 클릭한다.
* 사용자 등록 후 초기 비밀번호는 “1234” 이며, 최초 로그인 후 비밀번호를 변경할 수 있다.

![](../../.gitbook/assets/userAdd%20%282%29.png)

**2.  사용자 수정**

* 사용자 “수정” 버튼을 클릭 후 사용자 정보 및 해당 권한을 수정하여 “확인” 버튼을 클릭한다.
* 관리자는 선택한 사용자의 아이디는 수정할 수 없지만 비밀번호를 변경할 수 있다.

![](../../.gitbook/assets/userModify%20%282%29.png)

### 3.5.  _**스템셀과 릴리즈**_

플랫폼 설치 자동화 설치 테스트를 위해 사용한 스템셀과 릴리즈 정보는 다음과 같다.

| BootStrap | 스템셀 | aws | bosh-stemcell-3312.12-aws-xen-ubuntu-trusty-go\_agent.tgz |
| :--- | :--- | :--- | :--- |
| 오픈스택 | bosh-stemcell-3312.12-openstack-kvm-ubuntu-trusty-go\_agent.tgz |  |  |
| Vsphere | bosh-stemcell-3312.12-vsphere-esxi-ubuntu-trusty-go\_agent.tgz |  |  |
| 릴리즈 | BOSH | bosh-256.tgz |  |
| BOSH CPI | bosh-openstack-cpi-30.tgz |  |  |
| BOSH | 스템셀 | aws | bosh-stemcell-3312.12-aws-xen-ubuntu-trusty-go\_agent.tgz |
| 오픈스택 | bosh-stemcell-3312.12-openstack-kvm-ubuntu-trusty-go\_agent.tgz |  |  |
| Vsphere | bosh-stemcell-3312.12-vsphere-esxi-ubuntu-trusty-go\_agent.tgz |  |  |
| 릴리즈 | BOSH | bosh-256.tgz |  |
| Controller | 스템셀 | aws | bosh-stemcell-3312.12-aws-xen-ubuntu-trusty-go\_agent.tgz |
| 오픈스택 | bosh-stemcell-3312.12-openstack-kvm-ubuntu-trusty-go\_agent.tgz |  |  |
| Vsphere | bosh-stemcell-3312.12-vsphere-esxi-ubuntu-trusty-go\_agent.tgz |  |  |
| 릴리즈 | Controller | paasta-controller-2.0.tgz |  |
| cf-release-247.tgz |  |  |  |
| Container | 스템셀 | aws | bosh-stemcell-3312.12-aws-xen-ubuntu-trusty-go\_agent.tgz |
| 오픈스택 | bosh-stemcell-3312.12-openstack-kvm-ubuntu-trusty-go\_agent.tgz |  |  |
| Vsphere | bosh-stemcell-3312.12-vsphere-esxi-ubuntu-trusty-go\_agent.tgz |  |  |
| 릴리즈 | Container | paasta-container-2.0.tgz |  |
| diego-release-1.1.0.tgz |  |  |  |
| Garden-runc | paastapaasta-garden-runc-2.0.tgz |  |  |
| garden-runc-1.0.4.tgz |  |  |  |
| Cflinuxfs2-root | paasta-cflinuxfs2-rootfs-2.0.tgz |  |  |
| cflinuxfs2-root-release-1.40.0.tgz |  |  |  |
| ETCD | bosh-256.tgz |  |  |
| etcd-release-86.tgz |  |  |  |

#### 3.6.  _**BOOTSTRAP 설치하기**_

플랫폼 설치 자동화를 이용하여 BOOTSTRAP 설치하고, 설치 관리자로 등록하는 절차는 다음과 같다.

![](../../.gitbook/assets/BootStrapProcess%20%282%29.png)

#### 3.6.1.  _**스템셀 다운로드**_

플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -&gt; “스템셀 관리” 메뉴로 이동한다. “스템셀 관리” 메뉴에서는 Cloud Foundry에서 제공하는 공개 스템셀을 다운로드할 수 있는 기능을 제공한다.  
 상단에 위치한 “등록” 버튼을 클릭 후 스템셀 정보를 입력하고 “등록” 버튼을 클릭한다. \([2.1.3](paas-ta_-_-_-_-_.md#9) 참고\)

```text
  ※ 공개 스템셀 참조 사이트
  http://bosh.cloudfoundry.org/stemcells 
      AWS 환경의 BOOTSTRAP 설치 시에는 반드시 Lite 유형의 스템셀을 다운로드 하여야 한다.
```

![](../../.gitbook/assets/StemcellAdd%20%282%29.png)

* 본 가이드에서는 버전 3312.12을 다운로드 하였다.

#### 3.6.2.  _**릴리즈 다운로드**_

BOOTSTRAP을 설치하기 위해서는 BOSH 릴리즈와 BOSH CPI릴리즈 2개의 릴리즈가 필요하다  
 릴리즈를 다운로드하기 위해 플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -&gt; “릴리즈 관리” 메뉴로 이동 후 상단에 위치한 “등록” 버튼을 클릭하고, 릴리즈 등록 팝업 화면에서 릴리즈 정보 입력 후 “등록” 버튼을 클릭한다. \(릴리즈 관리 화면 설명은 [2.1.4](paas-ta_-_-_-_-_.md#10) 참고\)

**1.  BOSH 릴리즈**

* 릴리즈 등록 팝업화면에서BOSH 릴리즈 정보를 입력하고, “등록” 버튼클릭한다.
* BOSH 릴리즈 참조 사이트

  ```text
  http://bosh.io/releases/github.com/cloudfoundry/bosh?all=1
  ```

![](../../.gitbook/assets/releaseAdd%20%282%29.png)

※ 본 가이드에서는 v256을 다운로드 하였다.

**2.  BOSH CPI 릴리즈**

* 릴리즈 등록 팝업화면에서 BOSH CPI릴리즈 정보를 입력하고, “등록” 버튼 클릭한다.
* BOSH-CPI 릴리즈 참조 사이트

  ```text
  ※ aws의 경우
  http://bosh.io/releases/github.com/cloudfoundry-incubator/bosh-aws-cpi-release?all=1

  ※ openstack의 경우
  http://bosh.io/releases/github.com/cloudfoundry-incubator/bosh-openstack-cpi-release?all=1

  ※ vsphere의 경우
  http://bosh.io/releases/github.com/cloudfoundry-incubator/bosh-vsphere-cpi-release?all=1
  ```

![](../../.gitbook/assets/releaseAdd2%20%282%29.png)

※ 본 가이드에서는 v30을 다운로드 하였다.

#### 3.6.3.  _**BOOTSTRAP 설치**_

BOOTSTRAP 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “BOOTSTRAP 설치” 메뉴로 이동 후 상단에 위치한 “설치”버튼을 클릭한다. \(BOOTSTRAP설치 화면 설명은 [2.1.8](paas-ta_-_-_-_-_.md#14) 참고\)

**3.  클라우드 환경 선택**

* 설치할 클라우드 환경을 선택하는 팝업화면에서 설치할 클라우드를 선택하고, “확인” 버튼 클릭한다.

![](../../.gitbook/assets/BootStrapIaasSelect%20%282%29.png)

**4.  BOOTSTRAP 설치 – 선택한 클라우드 환경 정보**

* 오픈스택 클라우드 환경을 선택한 경우 오픈스택의 인증정보/시큐리티 그룹/키 파일 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapOpenstackInfo%20%282%29.png)

* AWS 클라우드 환경을 선택한 경우 AWS의 정보 및 키 파일 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapAWSInfo%20%282%29.png)

* VSPHERE 클라우드 환경을 선택한 경우 VSPHERE의 정보 및 키 파일 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapVsphereInfo%20%282%29.png)

**5.  BOOTSTRAP 설치 – 기본 정보**

* BOOTSTRAP의 배포명 / 디렉터명 / NTP / BOSH 릴리즈 / BOSH CPI 릴리즈 / 스냅샷기능 사용여부 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapDefaultInfo%20%282%29.png)

**6.  BOOTSTRAP 설치 – 클라우드 환경 별 네트워크 정보**

* AWS/오픈스택 클라우드 환경을 선택한 경우 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapOpenstackNetworkInfo%20%282%29.png)

* VSPHERE 클라우드 환경을 선택한 경우 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapVsphereNetworkInfo%20%282%29.png)

**7.  BOOTSTRAP 설치 – 리소스 정보**

* AWS/오픈스택 클라우드 환경을 선택한 경우 스템셀 / 인스턴스 유형 / VM 비밀번호 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapAwsOpenstackResourceInfo%20%282%29.png)

* VSPHERE 클라우드 환경을 선택한 경우 스템셀 / 리소스 풀 CPU / 리소스 풀 RAM / 리소스 풀 DISK / VM 비밀번호 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BootStrapVsphereResourceInfo%20%282%29.png)

**8.  BOOTSTRAP 설치 - 배포 파일 정보**

* 입력한 정보를 기준으로 생성한 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/BootStrapDeployInfo%20%282%29.png)

**9.  BOOTSTRAP 설치 - 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 BOOTSTRAP설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

#### 3.6.4.  _**설치 관리자 설정**_

BOOTSTRAP설치가 완료되면 BOOTSTRAP 디렉터 정보\(디렉터IP, 포트번호, 계정, 비밀번호\)를 이용해서 플랫폼 설치 자동화의 설치 관리자로 설정한다. \(설치 관리자 설정 화면 설명은 [2.1.2](paas-ta_-_-_-_-_.md#8) 참고\)

![](../../.gitbook/assets/BootStrapDirectorAdd%20%282%29.png)

### 3.7.  _**BOSH 설치하기**_

BOOTSTRAP\(Microbosh\)을 설치 관리자로 설정 완료 후 BOSH를 설치하는 절차는 다음과 같다.

![](../../.gitbook/assets/BoshInstallProcess%20%282%29.png)

#### 3.7.1.  _**스템셀 업로드**_

플랫폼 설치 자동화 웹 화면에서 “배포 정보 조회 및 관리” -&gt; “스템셀 업로드”를 선택한다. “스템셀 업로드” 화면의 하단에 [3.6.1](paas-ta_-_-_-_-_.md#35) “스템셀 다운로드” 메뉴에서 다운로드 받은 3312.12 버전의 스템셀을 선택하고, “스템셀 업로드” 버튼을 클릭하여 설치 관리자에 스템셀을 업로드 한다.

![](../../.gitbook/assets/StemcellUpload%20%285%29.png)

#### 3.7.2.  _**릴리즈 업로드**_

플랫폼 설치 자동화 웹 화면에서 “정보 조회” -&gt; “릴리즈 업로드”를 선택한다. “릴리즈 업로드” 화면의 하단에 [3.6.2](paas-ta_-_-_-_-_.md#36) “릴리즈 다운로드”에서 다운로드 한 256버전의 BOSH 릴리즈\(bosh-256.tgz\)를 선택하고, “릴리즈 업로드” 버튼을 클릭하여 설치 관리자에 릴리즈를 업로드한다.

![](../../.gitbook/assets/ReleaseUpload%20%285%29.png)

#### 3.7.3.  _**BOSH 설치**_

BOSH설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “BOSH설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다. \(BOSH 설치 화면 설명은 [2.1.9](paas-ta_-_-_-_-_.md#15) 참고\)

**1.  BOSH 설치 – 기본 설치 관리자에 따른 클라우드 환경 정보**

* 오픈스택 클라우드 환경일 경우 오픈스택의 인증정보 / 시큐리티 그룹 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshOpenstackInfo%20%282%29.png)

* AWS 클라우드 환경일 경우 AWS의 인증정보 / 시큐리티 그룹 / 키 파일 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshAwsInfo%20%282%29.png)

* VSPHERE 클라우드 환경일 경우 VSPHERE의 인증정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshVsphereInfo%20%282%29.png)

**2.  BOSH 설치 – 기본 정보**

* BOSH의 배포명 / 디렉터명 / NTP / BOSH 릴리즈 / 스냅샷 사용 여부 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshDefaultInfo%20%282%29.png)

**3.  BOSH 설치 – 클라우드 환경 별 네트워크 정보**

* 오픈스택 환경일 경우 오픈스택의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshOpenstackNetworkInfo%20%282%29.png)

* AWS 환경일 경우 AWS의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshAwsNetworkInfo%20%282%29.png)

* VSPHERE 환경일 경우 VSPHERE 의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshVsphereNetworkInfo%20%282%29.png)

**4.  BOSH 설치 – 클라우드 환경 별 리소스 정보**

* 오픈스택/AWS 환경일 경우 오픈스택 또는 AWS의 스템셀 / 인스턴스 유형 / VM 비밀번호 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshAwsOpenstackResourceInfo%20%282%29.png)

* VSPHERE 환경일 경우 VSPHERR의 스템셀 / 리소스 유형 / VM 비밀번호 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/BoshVsphereResourceInfo%20%282%29.png)

**5.  BOSH 설치 – 배포파일 정보**

* 입력한 정보를 기준으로 생성한 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/BoshDeployInfo%20%282%29.png)

**6.  BOSH 설치 – 설치 정보**

* 생성된 배포 Manifest파일 정보를 이용하여 BOSH설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/BoshInstallInfo%20%282%29.png)

#### 3.7.4.  _**설치 관리자 설정**_

BOSH설치가 완료되면 BOSH 디렉터 정보\(디렉터IP, 포트번호, 계정, 비밀번호\)를 이용해서 플랫폼 설치 자동화의 설치 관리자로 설정한다. \(설치 관리자 설정 화면 설명은 [2.1.2](paas-ta_-_-_-_-_.md#8) 참고\)

### 3.8.  _**CF 설치하기**_

BOSH를 설치하고 플랫폼 설치 자동화의 설치 관리자로 설정이 완료되면 CF를 설치할 준비가 된 상태로 CF를 설치하는 절차는 다음과 같다.

![](../../.gitbook/assets/CfProcess%20%282%29.png)

#### 3.8.1.  _**스템셀 업로드**_

[3.7.1](paas-ta_-_-_-_-_.md#40) “스템셀 업로드”에서 수행했던 것과 동일하게 BOSH 설치 관리자에 3312.12버전의 스템셀을 업로드 합니다.

#### 3.8.2.  _**릴리즈 업로드**_

PaaS-TA개발팀에서 제공하는 PaaS-TA Controller 2.0버전의 릴리즈\(passta-controller-2.0.tgz\) 또는 247버전의 cf-release\(cf-release-247.tgz\)를 [3.6.2](paas-ta_-_-_-_-_.md#36) “릴리즈 다운로드”와 동일하게 플랫폼 설치 자동화에 다운로드한다. 그리고 [3.7.2](paas-ta_-_-_-_-_.md#41) “릴리즈 업로드”와 동일하게 설치 관리자로 업로드한다.

| 릴리즈 명 | 버전 | 다운로드 링크 |
| :--- | :--- | :--- |
| CF 릴리즈 | 247 | [http://bosh.io/releases/github.com/cloudfoundry/cf-release?all=1](http://bosh.io/releases/github.com/cloudfoundry/cf-release?all=1) |
| PaaS-TA | Controller    2.0 | paasta-controller-2.0.tgz |

#### 3.8.3.  _**CF 설치**_

CF설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “CF설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다. \(CF 설치 화면 설명은 [2.1.10](paas-ta_-_-_-_-_.md#16) 참고\)

**1.  Diego 사용 여부 선택**

* Diego 사용 여부 팝업화면에서 예를 선택하고, “확인” 버튼 클릭한다.

![](../../.gitbook/assets/CfDiegoCheck%20%282%29.png)

**2.  CF 설치 – 기본정보 입력**

* 배포에 필요한 기본정보와 도메인 / 로그인 비밀번호를 입력 후 “다음” 버튼을 클릭한다.
* SSH 핑거프린트는 입력 항목은 Diego 설치 팝업 화면에서 Key 생성 후 입력한다. \(below\)

![](../../.gitbook/assets/CfDefaultInfo%20%284%29.png)

**3.  CF 설치 – 클라우드 환경 별 네트워크 정보**

* 오픈스택 환경일 경우 오픈스택의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfOpenstackNetworkInfo%20%284%29.png)

* AWS 환경일 경우 AWS의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfAwsNetworkInfo%20%284%29.png)

* VSPHERE 환경일 경우 VSPHERE의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfVsphereNetworkInfo%20%284%29.png)

**4.  CF 설치 – Key 생성**

* Key 생성 정보 입력 후 “Key 생성” 버튼을 클릭하고, Key 생성 확인 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfKeyInfo%20%284%29.png)

**5.  CF 설치 – 클라우드 환경 별 리소스 정보**

* 오픈스택/AWS 환경일 경우 오픈스택 및 AWS의 스템셀 / VM 비밀번호 / Flavor 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfAwsOpenstackResourceInfo%20%284%29.png)

* VSPHERE 환경일 경우 오픈스택 및 VSPHERE의 스템셀 / VM 비밀번호 / 리소스 유형 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfVsphereResourceInfo%20%284%29.png)

**6.  CF 설치 – 배포 파일 정보**

* 입력한 정보를 기준으로 생성한 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/CfDeployInfo%20%284%29.png)

**7.  CF 설치 – 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 PaaS-TA Controller\(CF\) 설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/CfInstallInfo%20%284%29.png)

### 3.9.  _**DIEGO 설치하기**_

CF설치가 완료되면 DIEGO를 설치할 준비가 된 상태로 DIEGO를 설치하는 절차는 다음과 같다.

![](../../.gitbook/assets/DiegoProcess%20%282%29.png)

#### 3.9.1.  _**스템셀 업로드**_

[3.7.1](paas-ta_-_-_-_-_.md#40) “스템셀 업로드”에서 수행했던 것과 동일하게 BOSH 설치 관리자에 3312.12버전의 스템셀을 업로드 합니다.

#### 3.9.2.  _**릴리즈 업로드**_

DIEGO설치를 위해서는 Container 역할을 하는 릴리즈와 의존 관계에 있는 릴리즈를 다운로드, 업로드 하여야 한다. 2.0 버전의 PaaS-TA Container 릴리즈와 2.0 버전의 PaaS-TA cflinuxfs2-root 릴리즈와 2.0 버전의 PaaS-TA garden-runc릴리즈와 2.0 버전의 PaaS-TA etcd 릴리즈를 [3.6.2](paas-ta_-_-_-_-_.md#36) “릴리즈 다운로드”와 동일하게 플랫폼 설치 자동화에 다운로드한다. 그리고 [3.7.2](paas-ta_-_-_-_-_.md#41) “릴리즈 업로드”와 동일하게 설치 관리자로 업로드한다.

| 릴리즈 명 | 버전 | 다운로드 링크 |
| :--- | :--- | :--- |
| PaaS-TA Container \(DIEGO 릴리즈\) | 2.0 | paasta-container-2.0.tgz |
| 1.1.0 | http://bosh.io/releases/github.com/cloudfoundry/diego-release?all=1 |  |
| PaaS-TA cflinuxfs2-root \(cflinuxfs2-root 릴리즈\) | 2.0 | paasta-cflinuxfs2-rootfs-2.0.tgz |
| 1.40.0 | http://bosh.io/releases/github.com/cloudfoundry/cflinuxfs2-rootfs-release?all=1 |  |
| PaaS-TA garden-runc  \(garden-runc릴리즈\) | 2.0 | paasta-garden-runc-2.0.tgz |
| 1.0.4 | http://bosh.io/releases/github.com/cloudfoundry/garden-runc-release?all=1 |  |
| PaaS-TA etcd \(etcd릴리즈\) | 2.0 | paasta-etcd-2.0.tgz |
| 86 | http://bosh.io/releases/github.com/cloudfoundry-incubator/etcd-release?all=1 |  |

#### 3.9.3.  _**DIEGO 설치**_

DIEGO를 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “DIEGO설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다. \(DIEGO 설치 화면 설명은 [2.1.11](paas-ta_-_-_-_-_.md#17) 참고\)

**1.  DIEGO 설치 – 기본 정보**

* 배포에 필요한 기본정보와 릴리즈 정보를 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoDefaultInfo%20%285%29.png)

**2.  DIEGO – 클라우드 환경 별 네트워크 정보**

* 오픈스택 환경일 경우 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoOpenstackNetworkInfo%20%285%29.png)

* AWS 환경일 경우 AWS의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoAwsNetworkInfo%20%285%29.png)

* VSPHERE 환경일 경우 VSPHERE의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoVsphereNetworkInfo%20%285%29.png)

**3.  DIEGO 설치 – Key 생성**

* “Key 생성” 버튼을 클릭 후 생성된 ssh-key-fingerprint를 복사한다.

![](../../.gitbook/assets/DiegoCreateKey%20%282%29.png)

**4.  DIEGO 설치 – CF 재설치**

* DIEGO 설치에 연동 할 CF정보를 수정한다.
* 복사한 fingerprint 정보를 CF 기본 정보 저장 팝업 화면에서 입력 후 CF를 재설치 한다. \(CF 설치 화면 설명은 [2.1.10](paas-ta_-_-_-_-_.md#16) 참고\)

**5.  DIEGO 설치 – 클라우드 환경 별 리소스 정보**

* 오픈스택/AWS 환경일 경우 오픈스택 및 AWS의 스템셀 / VM 비밀번호 / Flavor 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoAwsOpenstackResourceinfo%20%285%29.png)

* VSPHERE 환경일 경우 오픈스택 및 VSPHERE의 스템셀 / VM 비밀번호 / 리소스 유형 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoVsphereResourceinfo%20%285%29.png)

**6.  DIEGO 설치 – 배포 파일 정보**

* 입력한 정보를 기준으로 생성한 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/DiegoDeployinfo%20%285%29.png)

**7.  DIEGO 설치 – 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 PaaS-TA Container\(DIEGO\) 설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/DiegoInstallInfo%20%285%29.png)

### 3.10.  _**CF & DIEGO 통합 설치 하기**_

BOSH를 설치하고 플랫폼 설치 자동화의 설치 관리자로 설정이 되면 CF & DIEGO 통합 설치가 준비 된 상태로 실행 절차는 CF와 DIEDO 설치와 유사 하다.

![](../../.gitbook/assets/CfDiegoProcess%20%282%29.png)

#### 3.10.1.  _**스템셀 업로드**_

[3.7.1](paas-ta_-_-_-_-_.md#40) “스템셀 업로드”에서 수행했던 것과 동일하게 BOSH 설치 관리자에 3312.12버전의 스템셀을 업로드 합니다.

#### 3.10.2.  _**릴리즈 업로드**_

CF & DIEGO 설치를 위해서는 Controller역할을 담당하는 PaaS-TA Controller\(CF\) 릴리즈와 PaaS-TA Container\(DIEGO\) 릴리즈, 의존 관계 릴리즈가 필요하다. 이 릴리즈는 PaaS-TA개발팀에서 제공 받거나 아래 다운로드 링크를 통해 [3.6.2](paas-ta_-_-_-_-_.md#36) ”릴리즈 다운로드” 플랫폼 설치 자동화에 다운로드 한다. 그리고 [3.7.2](paas-ta_-_-_-_-_.md#41) “릴리즈 업로드”와 동일하게 설치 관리자로 업로드한다.

| 릴리즈 명 | 버전 | 다운로드 링크 |
| :--- | :--- | :--- |
| PaaS-TA Controller \(CF 릴리즈\) | 2.0 | paasta-controller-2.0.tgz |
| 247 | http://bosh.io/releases/github.com/cloudfoundry/cf-release?all=1 |  |
| PaaS-TA Container \(DIEGO 릴리즈\) | 2.0 | paasta-container-2.0.tgz |
| 1.1.0 | http://bosh.io/releases/github.com/cloudfoundry/diego-release?all=1 |  |
| PaaS-TA cflinuxfs2-root \(cflinuxfs2-root 릴리즈\) | 2.0 | paasta-cflinuxfs2-rootfs-2.0.tgz |
| 1.40.0 | http://bosh.io/releases/github.com/cloudfoundry/cflinuxfs2-rootfs-release?all=1 |  |
| PaaS-TA garden-runc  \(garden-runc릴리즈\) | 2.0 | paasta-garden-runc-2.0.tgz |
| 1.0.4 | http://bosh.io/releases/github.com/cloudfoundry/garden-runc-release?all=1 |  |
| PaaS-TA etcd \(etcd릴리즈\) | 2.0 | paasta-etcd-2.0.tgz |
| 86 | http://bosh.io/releases/github.com/cloudfoundry-incubator/etcd-release?all=1 |  |

#### 3.10.3.  _**CF & DIEGO 통합 설치**_

CF & DIEGO를 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “CF & DIEGO설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다. \(CF & DIEGO 설치 화면 설명은 [2.1.12](paas-ta_-_-_-_-_.md#18) 참고\)

**1.  CF & DIEGO 설치 – CF 기본 정보 입력**

* 배포에 필요한 기본정보와 도메인 정보/로그인 비밀번호를 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfDefaultInfo%20%285%29.png)

**2.  CF & DIEGO 설치 – 클라우드 환경 별 네트워크 정보**

* 오픈스택 환경일 경우 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfOpenstackNetworkInfo%20%285%29.png)

* AWS 환경일 경우 AWS의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfAwsNetworkInfo%20%285%29.png)

* VSPHERE 환경일 경우 VSPHERE의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfVsphereNetworkInfo%20%285%29.png)

**3.  CF & DIEGO 설치 – CF 키 생성**

* CF 설치에 필요 한 Key 를 생성하기 위해 키 정보를 입력 후 “key 생성” 버튼을 클릭 한다. 키가 생성 되면 “다음” 버튼을 클릭 한다.

![](../../.gitbook/assets/CfKeyInfo%20%285%29.png)

**4.  CF & DIEGO 설치 – 클라우드 환경 별 리소스 정보**

* 오픈스택/AWS 환경일 경우 오픈스택 및 AWS의 스템셀 / VM 비밀번호 / Flavor 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfAwsOpenstackResourceInfo%20%285%29.png)

* VSPHERE 환경일 경우 오픈스택 및 VSPHERE의 스템셀 / VM 비밀번호 / 리소스 유형 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/CfVsphereResourceInfo%20%285%29.png)

**5.  CF & DIEGO 설치 – CF 배포 파일 정보**

* 입력한 정보를 기준으로 생성한 CF 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/CfDeployInfo%20%285%29.png)

**6.  CF & DIEGO 설치 – DIEGO 기본 정보 입력**

* DIEGO 배포에 필요한 기본정보와 릴리즈 정보를 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoDefaultInfo%20%284%29.png)

**7.  CF & DIEGO 설치 – 클라우드 환경 별 네트워크 정보**

* 오픈스택 환경일 경우 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoOpenstackNetworkInfo%20%284%29.png)

* AWS 환경일 경우 AWS의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoAwsNetworkInfo%20%284%29.png)

* VSPHERE 환경일 경우 VSPHERE의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoVsphereNetworkInfo%20%284%29.png)

**8.  CF & DIEGO 설치 – 클라우드 환경 별 리소스 정보**

* 오픈스택/AWS 환경일 경우 오픈스택 및 AWS의 스템셀 / VM 비밀번호 / Flavor 정보 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoAwsOpenstackResourceinfo%20%284%29.png)

* VSPHERE 환경일 경우 오픈스택 및 VSPHERE의 스템셀 / VM 비밀번호 / 리소스 유형 입력 후 “다음” 버튼을 클릭한다.

![](../../.gitbook/assets/DiegoVsphereResourceinfo%20%284%29.png)

**9.  CF & DIEGO 설치 – DIEGO 배포 파일 정보**

* 입력한 정보를 기준으로 생성한 DIEGO 배포 Manifest파일 정보를 확인한다.

![](../../.gitbook/assets/DiegoDeployinfo%20%284%29.png)

**10. CF & DIEGO 설치 – CF 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 PaaS-TA Controller\(CF\) 설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/CfInstallInfo%20%285%29.png)

**11. CF & DIEGO 설치 – DIEGO 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 PaaS-TA Container\(DIEGO\) 설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/DiegoInstallInfo%20%284%29.png)

### 3.11.  _**서비스팩 설치**_

CF 설치가 성공적으로 완료되고 배포할 Manifest를 업로드하면 서비스팩을 설치할 준비가 된 상태로 서비스팩을 설치하는 절차는 다음과 같다.

![](../../.gitbook/assets/ServicePackProcess%20%282%29.png)

#### 3.11.1.  _**스템셀 업로드**_

[3.7.1](paas-ta_-_-_-_-_.md#40) “스템셀 업로드”에서 수행했던 것과 동일하게 BOSH 설치 관리자에 설치할 서비스팩의 스템셀을 업로드 합니다.

#### 3.11.2.  _**릴리즈 업로드**_

PaaS-TA개발팀에서 제공하는 PaaS-TA 서비스 릴리즈\(passta-mysql-2.0.tgz\) 에서 [3.6.2](paas-ta_-_-_-_-_.md#36) “릴리즈 다운로드”를 통해 다운 받는다. 그리고 [3.7.2](paas-ta_-_-_-_-_.md#41) “릴리즈 업로드”와 동일하게 설치 관리자로 업로드한다.

#### 3.11.3.  _**Manifest 업로드**_

Manifest를 업로드 하기 위해 플랫폼 설치 자동화 웹 화면에서 “배포 정보 조회 및 관리” -&gt; “Manifest 관리” 메뉴로 이동 후 상단의 “업로드” 버튼을 클릭한다. \(Manifest설치 화면 설명은 [2.1.21](paas-ta_-_-_-_-_.md#27) 참고\)

**1.  Manifest 업로드 – 업로드**

* 서비스팩 설치를 위해서는 배포 정보를 가지고 있는 Manifest 파일이 필요하다. 서비스팩 설치에 필요한 Manifest를 작성하여 플랫폼 설치 자동화에 업로드 한다.

![](../../.gitbook/assets/ServicePackManifestUpload%20%282%29.png)

* 본 가이드에서는 PaaS-TA 서비스 MySQL Manifest를 업로드 하였다.

#### 3.11.4.  _**서비스팩 설치**_

서비스팩을 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -&gt; “서비스팩 설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다. \(서비스팩 설치 화면 설명은 [2.1.13](paas-ta_-_-_-_-_.md#19) 참고\)

**1.  서비스팩 설치 – Manifest 등록**

* 배포에 필요한 Manifest 파일을 선택하고 “설치” 버튼을 클릭 한다.

![](../../.gitbook/assets/ServicePackManifestInfo%20%282%29.png)

**2.  서비스팩 설치 – 설치**

* 생성된 배포 Manifest파일 정보를 이용하여 서비스팩 설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](../../.gitbook/assets/ServicePackInstallInfo%20%282%29.png)

### 3.12.  _**Property 관리**_

Property를 생성하기 위해 플랫폼 설치 자동화 웹 화면에서 “배포 정보 조회 및 관리” -&gt; “Property 관리” 메뉴로 이동 후 배포명을 선택하고 “조회” 버튼을 클릭한다. \(Property 관리 화면 설명은 [2.1.19](paas-ta_-_-_-_-_.md#25) 참고\)

**1.  Property 생성**

* 배포명 선택 콤보 박스에서 배포명을 선택하고 “조회” 버튼을 클릭 후 “Property 생성” 버튼을 클릭 하고 Property 정보 입력 후 “저장” 버튼을 클릭한다.

![](../../.gitbook/assets/PropertyAdd%20%282%29.png)

**2.  Property 수정**

* “Property 수정” 버튼을 클릭 후 Property 값을 수정하고 “수정” 버튼을 클릭한다.

![](../../.gitbook/assets/PropertyModify%20%282%29.png)

