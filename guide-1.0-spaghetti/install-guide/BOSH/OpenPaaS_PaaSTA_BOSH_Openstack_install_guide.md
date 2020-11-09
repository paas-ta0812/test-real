## Table of Contents
1. [문서 개요](#1--문서-개요)
     * [목적](#11--목적)
     * [범위](#12--범위)
     * [참고자료](#13--참고자료)
2. [Basic BOSH Workflow](#2--basic-bosh-workflow)
3. [BOSH 설치 패키지 구성 요소](#3--bosh-설치-패키지-구성-요소)
4. [Install BOSH CLI](#4--install-bosh-cli)
     * [RUBY 설치](#41--ruby-설치)
     * [BOSH INIT 설치](#42--bosh-init-설치)
     * [BOSH CLI 설치](#43--bosh-cli-설치)
5. [Install MicroBOSH](#5--install-microbosh)
     * [설치 절차](#51--설치-절차)
     * [OpenStack 환경설정](#52--openstack-환경설정)     
     * [배포 Manifest 작성](#53--배포-manifest-작성)
     * [MicroBOSH 배포](#54--microbosh-배포)
6. [Install BOSH](#6--install-bosh)
     * [설치 절차](#61--설치-절차)
     * [BOSH Stemell 업로드](#62--bosh-stemell-업로드)
     * [BOSH Release 업로드](#63--bosh-release-업로드)
     * [배포 Manifest 작성](#64--배포-manifest-작성)
     * [BOSH 배포](#65--bosh-배포)



# 1.  문서 개요 

## 1.1.  목적

클라우드 환경에 서비스 시스템을 배포할 수 있는 BOSH는 릴리즈 엔지니어링, 개발, 소프트웨어 라이프사이클 관리를 통합한 오픈소스 프로젝트로 본 문서에서는 로컬 환경에 MicroBOSH와 BOSH를 설치하여 기능을 테스트할 수 있는 환경을 구축하는 데 목적이 있다.


## 1.2.  범위

본 가이드에서는 오프라인 Linux 환경(Ubuntu 14.04)을 기준으로 설치환경 구성 및 BOSH CLI 설치하고, 이를 이용하여 MicroBOSH 및 BOSH를 설치하는 것을 기준으로 작성하였다.


## 1.3.  참고자료

본 문서는 Cloud Foundry의 BOSH Document를 참고로 작성하였다.

BOSH Document: [http://bosh.io](http://bosh.io)


# 2.  Basic BOSH Workflow

본 문서의 목적에서도 언급했듯이 BOSH는 클라우드 환경에 서비스를 배포 관리하는 소프트웨어로 BOSH자체도 클라우드에 배포되어야 하는 서비스로 설치 환경이 구성되어 있는 서버로부터 MicroBOSH만을 설치하여 운영하는 방법과 MiroBOSH 로부터 배포된 BOSH를 이용하는 방법 2가지 유형이 있으니 자신의 로컬 설치 환경에 따라 선택할 수 있다.

![bosh-workflow](./../images/deploy-bosh/bosh-workflow.png "bosh-workflow")

MicroBOSH (Single-VM BOSH)는 소프트웨어 배포 및 관리를 하나의 VM에서 수행하는 소프트웨어로 MicroBOSH를 배포하기 위해서는 Stemcell 이미지와 배포 Manifest파일이 필요하고 별도의 소프트웨어 Release는 필요로하지 않는다. (설치에 필요한 Release들은 Stemcell내부에 탑재되어 있다. )

MicroBOSH를 설치한 후에는 선택에 따라 MicroBOSH를 이용하여 BOSH (Multi-VM BOSH)를 배포하거나 또는 다른 서비스들을 배포할 수도 있다.


# 3.  BOSH 설치 패키지 구성 요소

다음의 설치 패키지는 MicroBOSH와 BOSH를 설치하기 위한 패키지로 아래의 구성 항목을 로컬 설치 환경의 기준 디렉토리(“workspace”)로 복사한다.

[다운로드](./../../Download_Page.md)

|**구분**|**디렉토리**|**파일명**|**설명**|
|-------|-----------|----------|--------|
|공통|OpenPaaS-Env|bosh-init-0.0.81-linux-amd64|BOSH init 실행 파일|
|공통|OpenPaaS-Env|bosh\_gem\_package.tar|BOSH CLI 설치 패키지|
|공통|OpenPaaS-Env|ruby-2.1.6.tar.gz|Ruby 2.1.6 버전|
|공통|OpenPaaS-Env/bosh\_env\_packages|디렉토리 전체|BOSH 실행 환경 설치 패키지|
|공통|OpenPaaS-IaaS-Controller|bosh-233.tgz|BOSH v233 릴리즈|
|OpenStack|OpenPaaS-IaaS-Controller|bosh-aws-cpi-release-36.tgz|BOSH Plugin CPI|
|OpenStack|OpenPaaS-Stemcells|bosh-stemcell-3147-openstack-kvm-ubuntu-trusty-go\_agent.tgz|OpenStack v3147 스템셀 이미지|
|OpenStack|OpenPaaS-Deployment|openpaas-bosh-init-openstack-1.0.yml|OpenStack MicroBOSH 배포 Manifest Sample 파일|
|OpenStack|OpenPaaS-Deployment|openpaas-bosh-openstack-1.0.yml|OpenStack BOSH 배포 Manifest Samle 파일|


# 4.  Install BOSH CLI 

BOSH는 Ruby언어로 구현되어 있으므로 Ruby개발 환경 설치 후 BOSH CLI를 설치해야 한다. BOSH CLI설치 가능한 Ruby버전은 1.9.3 이상이며, 개발 환경에 이미 설치가 되어 있다면 4.1절 Ruby설치는 건너뛰면 된다.

# 4.1.  Ruby 설치

BOSH CLI가 지원하는 Ruby버전 중 2.1.6버전을 설치 대상으로 기술하였고, 상세 설치 절차는 다음과 같다.

1.  Bosh Dependency를 설치한다.

	※ **온라인인 경우(권장)**

	**- Ubuntu의 경우**

		$ sudo apt-get update
		$ sudo apt-get install -y build-essential zlibc zlib1g-dev ruby ruby-dev openssl libxslt-dev libxml2-dev libssl-dev libreadline6 libreadline6-dev libyaml-dev libsqlite3-dev sqlite3 libxslt1-dev libpq-dev libmysqlclient-dev

	**- CentOS의 경우**

		$ sudo yum install gcc ruby ruby-devel mysql-devel postgresql-devel postgresql-libs sqlite-devel libxslt-devel libxml2-devel yajl-ruby

	**- OSX의 경우**

		$ xcode-select --install
		xcode-select: note: install requested for command line developer tools

	※ **오프라인인 경우**

		# 설치환경이 ubuntu (trusty 버전)인 경우 다음의 스크립트를 실행한다.
		$ cd <다운로드 디렉토리>/OpenPaaS-Env/bosh_env_packages
		$ chmod +x ./bosh-packages-install.sh
		$ ./bosh-packages-install.sh

	※ BOSH 설치 패키지의 구성은 다음과 같다.

	|No   |패키지명                       |패키지 파일명 (Ubuntu trusty amd64)|
	|---- |----------------------------- |--------------------------------------------------------|
	|1    |libcomerr2                    |libcomerr2_1.42.9-3ubuntu1.3_amd64.deb|
	|2    |libssl1.0.0                   |libssl1.0.0_1.0.1f-1ubuntu2.16_amd64.deb|
	|3    |libkrb5support0               |libkrb5support0_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|4    |libk5crypto3                  |libk5crypto3_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|5    |libkrb5-3                     |libkrb5-3_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|6    |libgssapi-krb5-2              |libgssapi-krb5-2_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|7    |libxml2                       |libxml2_2.9.1+dfsg1-3ubuntu4.6_amd64.deb|
	|8    |libgssrpc4                    |libgssrpc4_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|9    |libkadm5clnt-mit9             |libkadm5clnt-mit9_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|10   |libkdb5-7                     |libkdb5-7_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|11   |libkadm5srv-mit9              |libkadm5srv-mit9_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|12   |mysql-common                  |mysql-common_5.5.46-0ubuntu0.14.04.2_all.deb|
	|13   |libmysqlclient18              |libmysqlclient18_5.5.46-0ubuntu0.14.04.2_amd64.deb|
	|14   |libxslt1.1                    |libxslt1.1_1.1.28-2build1_amd64.deb|
	|15   |libyaml-0                     |libyaml-0-2_0.1.4-3ubuntu3.1_amd64.deb|
	|16   |openssl_1.0.1f-1ubuntu2.16    |openssl_1.0.1f-1ubuntu2.16_amd64.deb|
	|17   |libstdc++-4.8-dev             |libstdc++-4.8-dev_4.8.4-2ubuntu1~14.04_amd64.deb|
	|18   |g++-4.8                       |g++-4.8_4.8.4-2ubuntu1~14.04_amd64.deb|
	|19   |g++                           |g++_4%3a4.8.2-1ubuntu6_amd64.deb|
	|20   |libdpkg-perl                  |libdpkg-perl_1.17.5ubuntu5.5_all.deb|
	|21   |dpkg-dev                      |dpkg-dev_1.17.5ubuntu5.5_all.deb|
	|22   |build-essential               |build-essential_11.6ubuntu6_amd64.deb|
	|23   |libfakeroot                   |libfakeroot_1.20-3ubuntu2_amd64.deb|
	|24   |fakeroot                      |fakeroot_1.20-3ubuntu2_amd64.deb|
	|25   |comerr-dev                    |comerr-dev_2.1-1.42.9-3ubuntu1.3_amd64.deb|
	|26   |krb5-multidev                 |krb5-multidev_1.12+dfsg-2ubuntu5.2_amd64.deb|
	|27   |libalgorithm-diff-perl        |libalgorithm-diff-perl_1.19.02-3_all.deb|
	|28   |libalgorithm-diff-xs-perl     |libalgorithm-diff-xs-perl_0.04-2build4_amd64.deb|
	|29   |libalgorithm-merge-perl       |libalgorithm-merge-perl_0.08-2_all.deb|
	|30   |libtinfo-dev                  |libtinfo-dev_5.9+20140118-1ubuntu1_amd64.deb|
	|31   |libreadline6-dev              |libreadline6-dev_6.3-4ubuntu2_amd64.deb|
	|32   |libfile-fcntllock-perl        |libfile-fcntllock-perl_0.14-2build1_amd64.deb|
	|33   |zlib1g-dev                    |zlib1g-dev_1%3a1.2.8.dfsg-1ubuntu1_amd64.deb|
	|34   |libmysqlclient-dev            |libmysqlclient-dev_5.5.46-0ubuntu0.14.04.2_amd64.deb|
	|35   |libpq5                        |libpq5_9.3.10-0ubuntu0.14.04_amd64.deb|
	|36   |libssl-dev                    |libssl-dev_1.0.1f-1ubuntu2.16_amd64.deb|
	|37   |libpq-dev                     |libpq-dev_9.3.10-0ubuntu0.14.04_amd64.deb|
	|38   |libsqlite3-dev                |libsqlite3-dev_3.8.2-1ubuntu2.1_amd64.deb|
	|39   |libssl-doc                    |libssl-doc_1.0.1f-1ubuntu2.16_all.deb|
	|40   |libxml2-dev                   |libxml2-dev_2.9.1+dfsg1-3ubuntu4.6_amd64.deb|
	|41   |libxslt1-dev                  |libxslt1-dev_1.1.28-2build1_amd64.deb|
	|42   |sqlite3                       |sqlite3_3.8.2-1ubuntu2.1_amd64.deb|
	|43   |Zlibc                         |zlibc_0.9k-4.1_amd64.deb|
	|44   |libyaml-dev                   |libyaml-dev_0.1.4-3ubuntu3.1_amd64.deb|
	|45   |libruby1.9.1                  |libruby1.9.1_1.9.3.484-2ubuntu1.2_amd64.deb|
	|46   |ruby1.9.1-dev                 |ruby1.9.1-dev_1.9.3.484-2ubuntu1.2_amd64.deb|
	|47   |ruby-dev                      |ruby-dev_1%3a1.9.3.4_all.deb|
	|48   |ruby                          |ruby_1%3a1.9.3.4_all.deb|
	|49   |ruby1.9.1                     |ruby1.9.1_1.9.3.484-2ubuntu1.2_amd64.deb|


	※ CentOS, RHEL 또는 OSX의 경우는 표에 해당하는 패키지를 설치한다.

2.  Ruby 설치

		$ tar -xvzf <다운로드 디렉토리>/OpenPaaS-Env/ruby-2.1.6.tar.gz
		$ cd ruby-2.1.6/
		$ ./configure
		$ make
		$ sudo make install

	※ Ruby를 컴파일하여 설치할 경우 기본으로 /usr/local/lib에 설치한다.

	※ 온라인의 경우, rvm 또는 rbenv 등을 이용하여 ruby를 설치할 수 있다. 단, ruby 1.9.3버전 이상을 설치하도록 한다.

3.  설치 확인

		# Ruby 버전 확인 (터미널창을 새로 열어서 확인한다.)
		$ ruby -v

		# Ruby 환경설정 확인
		$ gem env

		$ gem env
		RubyGems Environment:
		- RUBYGEMS VERSION: 2.1.6
		- RUBY VERSION: 2.1.6 (2015-04-13 patchlevel 336) [x86_64-linux]
		- INSTALLATION DIRECTORY: /usr/local/lib/ruby/gems/2.1.0			<- gem 설치 디렉토리
		- RUBY EXECUTABLE: /usr/local/bin/ruby
		- EXECUTABLE DIRECTORY: /usr/local/bin
		- SPEC CACHE DIRECTORY: /home/ubuntu/.gem/specs
		- RUBYGEMS PLATFORMS:
			- ruby
			- x86_64-linux
		- GEM PATHS:
			- /usr/local/lib/ruby/gems/2.1.0
			- /home/ubuntu/.gem/ruby/2.1.0
		- GEM CONFIGURATION:
			- :update_sources => true
			- :verbose => true
			- :backtrace => false
			- :bulk_threshold => 1000
		- REMOTE SOURCES:
			- https://rubygems.org/
		- SHELL PATH:
			- /usr/local/sbin
			- /usr/local/bin
			- /usr/sbin
			- /usr/bin
			- /sbin
			- /bin
			- /usr/games
			- /usr/local/games

## 4.2.  BOSH INIT 설치

Bosh-init은 MicroBOSH를 설치에 사용하는 도구이다. Bosh-init을 설치하는 절차는 다음과 같다.

1.  Bosh\_init 설치

	**- 온라인의 경우**

	※ 다음의 웹사이트에서 설치환경에 맞는 bosh-init 바이너리 파일을 다운로드한다.

	[https://bosh.io/docs/install-bosh-init.html](https://bosh.io/docs/install-bosh-init.html)

	![bosh-init-webpage](./../images/deploy-bosh/bosh-init-webpage.png "bosh-init-webpage")


	※ 다운로드 받은 bosh-init에 실행 권한을 부여하고 설치한다. (~/Download 디렉토리에 파일을 다운로드 받았다고 가정)

		$ chmod +x ~/Downloads/bosh-init-*
		$ sudo mv ~/Downloads/bosh-init-* /usr/local/bin/bosh-init
 

	**- 오프라인의 경우**

		# OpenPaaS 패키지의 bosh-init을 설치
		$ chmod +x ~/workspace/OpenPaaS-Env/bosh-init-0.0.81-linux-amd64
		$ sudo mv ~/workspace/OpenPaaS-Env/bosh-init-* /usr/local/bin/bosh-init


2.  Bosh-init 설치 확인

		$ bosh-init -v


## 4.3.  BOSH CLI 설치

BOSH CLI를 설치하는 절차는 다음과 같다.

1.  Bosh\_cli 설치

	**- 온라인의 경우**

		#bosh_cli 패키지 설치
		$ sudo gem install bosh_cli
		※ gem 설치 디렉토리 소유자가 로그인한 사용자와 동일한 경우는 ‘sudo’를 사용하지 않는다.
 
	**- 오프라인의 경우**

		#bosh_cli 의존 패키지 압축해제
		$ sudo tar -xvf ~/workspace/OpenPaaS-Env/bosh_gem_package.tar -C <gem 설치 디렉토리>/cache
		※ gem 설치 디렉토리는 ‘gem env’를 실행하여 확인한다. (예: /usr/local/lib/ruby/gems/2.1.0)


		#압축해제한 gem 설치
		$ sudo gem install --force --local --no-ri --no-rdoc <gem 설치 디렉토리>/cache/*.gem
		※ gem설치 디렉토리 소유자가 로그인한 사용자와 동일한 경우는 ‘sudo’를 사용하지 않는다.

		#참고: 기존에 설치한 gem 삭제
		$ sudo gem uninstall -aIx


2.  Bosh 설치 확인

		$ bosh


# 5.  Install MicroBOSH

본 장에서는 단일 VM에서 동작하는 MicroBOSH를 OpenStack에 설치하는 절차를 기술한다.

## 5.1.  설치 절차

MicroBOSH를 배포하기 위해서는 아래와 같이 몇가지 준비 사항들이 사전에 준비되어 되어있어야 한다.

-   IaaS 환경설정: OpenStack

-   배포 Manifest 파일 작성

-   MicroBOSH 배포


## 5.2.  OpenStack 환경설정

MicroBOSH를 배포하기 위한 OpenStack 사전 준비사항 및 필요한 사항을 아래의 가이드를 참조하여 설정한다.

-   사전 준비 및 확인 사항

-   지원 가능한 OpenStack Release 버전: Havana, IceHouse(권장), Juno

-   OpenStack 서비스 사용 가능 확인

    -   Identity: 인증 및 OpenStack 서비스들의 Endpoint 조회

    -   Compute: VM 생성과 IP 할당, 볼륨 생성 및 Attach

    -   Image: 이미지 서비스를 이용한 Stemcell 저장

-   OpenStack 네트워크

    -   외부 또는 내부 네트워크의 Subnet

	-   Floating IP

-   OpenStack 프로젝트 생성


### -   Key Pair 생성

1.  오픈스택 horizon의 왼쪽 메뉴에서 Compute -> Access & Security 선택

	![list-keypair](./../images/iaas-setup/openstack/list-keypair.png "list-keypair")

2.  “Key Pairs” Tab 선택

3.  화면 상단의 “Create Key Pair” 선택

4.  Key pair 이름 입력 후 “Create Key Pair” 선택 및

	![create-keypair](./../images/iaas-setup/openstack/create-keypair.png "create-keypair")

5.  Key를 생성하면 로컬에 다운로드 한다. (예제에서는 microbosh.pem 파일을 다운로드)

6.  다운로드한 Key를 로컬의 키 디렉토리(~/.ssh) 에 이동시키고 권한을 400으로 변경한다.

    	$ mv ~/Download/microbosh.pem ~/.ssh/microbosh.pem
  		$ chmod 400 ~/.ssh/microbosh.pem


### -   Security Group 생성 및 구성

1.  오픈스택 horizon의 왼쪽 메뉴에서 Compute -> Access & Security 선택

2.  Security Groups Tab 선택

3.  “Create Security Group” 선택

	![list-security-group](./../images/iaas-setup/openstack/list-security-group.png "list-security-group")


4.  생성할 Security Group의 Name, Description 입력 후 “Create Security Group” 선택 (예제에서는 bosh-security 그룹을 생성)

	![create-security-group](./../images/iaas-setup/openstack/create-security-group.png "create-security-group")


5.  Security Group 목록에서 생성된 Security Group의 “Manage Rules”

	![manage-rules](./../images/iaas-setup/openstack/manage-rules.png "manage-rules")


6.  다음 표와 같이 Bosh 관련Security Rule을 추가한다.

	![security-rules.png](./../images/iaas-setup/openstack/security-rules.png "security-rules")


	(예제에서는 모든 원격지에서 ssh접근을 허용하게 설정되어 있다. 설치 환경에 맞게 접근 할 수 있는 원격지를 제한 할 수 있다.)



## 5.3.  배포 Manifest 작성

MicroBOSH 배포 Manifest는 배포에 필요한 컴포넌트 및 속성 정보를 YAML 파일 형식으로 정의한다.


1.  Manifest파일을 저장할 Local 배포 디렉토리를 생성한다.

2.  생성한 디렉토리에서 배포 Manifest YAML 파일을 생성하고, 아래의 MicroBOSH 배포 Manifest Template을 복사하여 붙여넣고, 수정이라 쓴 부분을 자신의 OpenStack 환경에 맞게 편집 후 저장한다.

		---
		name: bosh 												# 배포명

		releases:
		- name: bosh 											# bosh 릴리즈
		  url: file:///<path_to_file>/<release_file_name> 		# 수정: bosh 릴리즈 파일이 있는 경로 및 파일명
		- name: bosh-openstack-cpi 								# bosh-openstack-cpi 릴리즈
		  url: file:///<path_to_file>/<release_file_name>	 	# 수정: bosh-openstack-cpi 릴리즈 파일이 있는 경로 및 파일명

		resource_pools:
		- name: vms
		  network: private
		  stemcell:
		    url: file:///<path_to_file>/<stemcell_file_name>	# 수정: stemcell 파일이 있는 경로 및 파일명
		  cloud_properties:
			instance_type: m1.xlarge 							# VM 타입 (cpu, ram, disk)

		disk_pools:
		- name: disks
		  disk_size: 20_000

		networks:
		- name: private
		  type: manual
		  subnets: 												# MicroBOSH를 설치할 오픈스택 내부망 정보
			- range: PRIVATE-CIDR								# 수정: 네트워크 대역
		      gateway: PRIVATE-GATEWAY-IP						# 수정: 내부망 게이트웨이
			  dns: [DNS-IP]										# 수정: DNS 목록
		  cloud_properties: {net_id: NETWORK-UUID} 				# 수정: 오픈스택 내부망 아이디
		- name: public
		  type: vip

		jobs:
		- name: bosh
		  instances: 1 											# 설치 VM 수

		  templates: 											# 설치할 템플릿 목록 및 템플릿이 참조할 릴리즈명
		  - {name: nats, release: bosh}
		  - {name: redis, release: bosh}
		  - {name: postgres, release: bosh}
		  - {name: blobstore, release: bosh}
		  - {name: director, release: bosh}
		  - {name: health_monitor, release: bosh}
		  - {name: registry, release: bosh}
		  - {name: openstack_cpi, release: bosh-openstack-cpi}

		  resource_pool: vms 									# 참조할 리소스풀 명칭
		  persistent_disk_pool: disks 							# 참조할 디스크풀 명칭

		  networks: 											# job에 설정할 네트워크 정보
		  - name: private
		    static_ips: [PRIVATE-IP] 							# 수정: VM에 설정할 내부망 아이피
			default: [dns, gateway]
		  - name: public
			static_ips: [FLOATING-IP] 							# 수정: VM에 설정할 외부망 아이피

		  properties:											# job에 설정할 프로퍼티 정보
			nats: 												# nats 프로퍼티 정보
			  address: 127.0.0.1
			  user: nats
			  password: nats-password

			redis: 												# redis 프로퍼티 정보
			  listen_address: 127.0.0.1
			  address: 127.0.0.1
			  password: redis-password

			postgres: &db 										# postgres db 프로퍼티 정보
			  listen_address: 127.0.0.1
			  host: 127.0.0.1
			  user: postgres
		      password: postgres-password
			  database: bosh
			  adapter: postgres

		    registry:											# registry 프로퍼티 정보
			  address: PRIVATE-IP								# 수정: VM에 설정할 내부망 아이피
			  host: PRIVATE-IP									# 수정: VM에 설정할 내부망 아이피
			  db: *db
			  http: {user: admin, password: admin, port: 25777}
			  username: admin
			  password: admin
			  port: 25777

			blobstore:											# blobstore 프로퍼티 정보
			  address: PRIVATE-IP								# 수정: VM에 설정할 내부망 아이피
			  port: 25250
			  provider: dav
			  director: {user: director, password: director-password}
			  agent: {user: agent, password: agent-password}

			director:											# 디렉터 프로터피 정보
			  address: 127.0.0.1
			  name: my-bosh										# 디렉터명
			  db: *db
			  cpi_job: openstack_cpi 							# 플러그인 cpi명
			  max_threads: 3 									# 디렉터의 worker thread 개수
			  user_management:
				provider: local
				local:
				  users:
				  - {name: admin, password: admin}
				  - {name: hm, password: hm-password}

			hm: 												# hm 프로퍼티
			  director_account: {user: hm, password: hm-password}
			  resurrector_enabled: true 						# job이 중지된 경우, 해당 job에 대한 재시작 여부 설정

			openstack: &openstack
			  auth_url: IDENTITY-API-ENDPOINT					# 수정: 오픈스택 API 엔드포인트
			  tenant: OPENSTACK-TENANT							# 수정: 오픈스택 Tenant명
			  username: OPENSTACK-USERNAME						# 수정: 오픈스택 사용자아이디
			  api_key: OPENSTACK-PASSWORD						# 수정: 오픈스택 사용자 비밀번호
			  default_key_name: microbosh						# 수정: VM에 적용할 키명
			  default_security_groups: [bosh]					# 수정: VM에 적용할 시큐리티 그룹명

			agent: {mbus: "nats://nats:nats-password@PRIVATE-IP:4222"}	# 수정: VM에 설정할 내부망 아이피

			ntp: &ntp [0.pool.ntp.org, 1.pool.ntp.org]			# 수정: ntp 서버 주소

		cloud_provider:
		  template: {name: openstack_cpi, release: bosh-openstack-cpi}

		  ssh_tunnel:
			host: FLOATING-IP									# 수정: VM에 설정할 외부망 아이피
			port: 22											# VM에 설정할 ssh 포트
			user: vcap											# VM에 설정할 사용자 아이디
			private_key: ~/.ssh/microbosh.pem					# 수정: VM과 연결할 수 있는 개인키 정보

		  mbus: "https://mbus:mbus-password@FLOATING-IP:6868"	# 수정: VM에 설정할 외부망 아이피

		  properties:
			openstack: *openstack
			agent: {mbus: "https://mbus:mbus-password@0.0.0.0:6868"}
			blobstore: {provider: local, path: /var/vcap/micro_bosh/data/cache}
			ntp: *ntp


## 5.4.  MicroBOSH 배포

Bosh-init을 이용해 MicroBOSH를 배포한다.

1.  터미널 상에 bosh-init의 deploy를 실행한다.

		$ bosh-init deploy <작성한 manifest>

	※ Deploy를 실행하면 manifest파일 있는 디렉토리에 <manifest명>-state.json 파일이 생긴다. MicroBOSH를 삭제할 경우 필요한 파일이므로 삭제하지 않는다.


2.  배포가 성공하면 다음과 같이 Target 설정하면서 기본 계정인 admin/admin으로 로그인한다.

		$ bosh target <FLOATING-IP>


3.  MicroBOSH의 상태정보를 확인한다.


		$ bosh status

		Config
			/home/ubuntu/.bosh_config 					# BOSH CLI 설정파일 위치
		Director 										# 현재 설정된 Director 정보
		  Name my-bosh 									# Director 이름
		  URL https://<FLOATING-IP>:25555				# Director URL
		  Version 1.3001.0 (00000000)					# Director 버전
		  User admin									# Director 로그인 계정
		  UUID dfb961e2-2b0a-4fce-b201-dff4ea3137bc		# Director UUID
		  CPI openstack_cpi								# Director의 CPI
		  dns enabled (domain_name: microbosh)			# Director의 DNS
		  compiled_package_cache disabled				# Package캐싱 여부 설정
		  snapshots disabled							# Snapshot 기능 설정 여부
		Deployment
		  not set


4.  deploy를 삭제할 경우

		$ bosh-init delete <작성한 manifest>


5.  bosh-init 배포에 대한 상세한 로그를 출력하고 싶은 경우, 다음과 같이 설정하고 bosh-init을 실행한다.

		#bosh-init 로그출력 레벨 설정
		# 설정 가능한 로그 레벨은 debug, info, warn, error, none로 none이 기본 설정되어 있다.
		$ export BOSH_INIT_LOG_LEVEL=<로그 레벨>

		# 로그를 파일로 출력할 경우
		$ export BOSH_INIT_LOG_PATH=<로그를 출력할 경로>/<로그 파일명>


# 6.  Install BOSH

본 장에서는 이전 장에서 설치한 MicroBOSH 이용하여 BOSH를 설치하는 절차를 기술한다.

## 6.1.  설치 절차

BOSH CLI를 5장에서 설치한 MicroBOSH로 타겟 설정한 이후 아래의 절차로 설치를 진행한다.

-   BOSH Stemcell 업로드

-   BOSH Release 업로드

-   배포 Manifest 작성

-   BOSH 배포


## 6.2.  BOSH Stemell 업로드

Stemcell 업로드는 MicroBOSH가 BOSH VM인스턴스를 생성하기 위한 기반 이미지를 등록하는 절차로 기준 디렉토리(“workspace”)에 다운로드 받은 Stemcell을 MicroBOSH에 업로드 한다.

**BOSH CLI ->	$ bosh upload stemcell [Stemcell 파일명]**

	$ bosh upload stemcell ~/workspace/OpenPaaS-Stemcells/<stemcell 파일명>
 
Stemcell 업로드가 성공하면 나면 bosh stemcells 명령어을 실행하여 stemcell을 확인한다.

	$ bosh stemcells


## 6.3.  BOSH Release 업로드

기준 디렉토리(“workspace”)에 다운로드 받은 release파일을 아래와 같이 CLI를 이용하여 MicroBOSH에 업로드한다.

**BOSH CLI ->	$ bosh upload release [release 파일명]**

	$ bosh upload release ~/workspace/OpenPaaS-IaaS-Controller/<release 파일명>

release 업로드가 성공하면 나면 bosh releases 명령어을 실행하여 release를 확인한다.

	$ bosh releases


## 6.4.  배포 Manifest 작성

MicroBOSH 배포 Manifest파일 작성과 동일하게 로컬에 YAML파일을 생성하고, 아래의 BOSH 배포 Manifest을 복사하여 붙여넣고, 수정이라 쓴 부분을 자신의 OpenStack 환경에 맞게 편집 후 저장한다. 입력값은 ‘OpenPaaS-Deployment/openpaas-bosh-openstack-1.0.yml’ 참조

	---
	name: bosh												# bosh 배포명
	director_uuid: 6f7e18b2-52b3-429f-ba5a-7a7d2397a893		# 수정: MicroBOSH uuid

	release:												# bosh 릴리즈 정보
	  name: bosh											# 릴리즈명
	  version: 233											# 수정: 릴리즈 버전

	compilation:
	  cloud_properties:
		instance_type: m1.medium							# 설치 패키지를 컴파일 할 VM의 타입 정보
	  network: default										# 설치 패키지를 컴파일 할 VM에 할당할 네트워크 정보
	  reuse_compilation_vms: true
	  workers: 3											# 설치 패키지를 컴파일 할 VM 개수

	update:
	  canaries: 1											# job설치를 시험하기 위한 생성 VM 수
	  canary_watch_time: 3000-120000						# 시험용 VM의 생성 응답대기 시간 (밀리초)
	  max_in_flight: 4										# 동일 Job에 대한 VM을 동시에 생성할 최대 수
	  update_watch_time: 3000-120000						# 시험용 VM의 업데이트 응답대기 시간 (밀리초)

	networks:
	- name: elastic											# 네트워크 이름 설정
	  type: vip												# 네트워크 타입 설정 (타입: manual, dynamic, vip)
	  cloud_properties: {}
	- name: default											# 네트워크 이름 설정
	  type: manual											# 네트워크 타입 설정 (타입: manual, dynamic, vip)
	  subnets:												# 내부망 네트워크 서브넷 정보 설정
	  - range: 192.168.20.0/24								# 수정: 내부망 네트워크 대역 설정
		dns: [8.8.8.8]										# 수정: 내부망 네트워크 DNS 설정
		gateway: 192.168.20.1								# 수정: 내부망 네트워크 게이트웨이 설정
		static:												# 수정: 내부망에 생성할 VM에 할당할 아이피 대역 설정
		- 192.168.20.6 - 192.168.20.12
		cloud_properties:
		  net_id: d8032a1c-xxxx-xxxx-xxxx-6d7282352381		# 수정: 내부망에 대한 오픈스택 네트워크 아이디

	resource_pools:											# VM에 할당할 리소스풀
	- name: small											# small 타입 리소스
	  network: default										# small 타입 리소스에 할당할 네트워크 이름
	  size: 6												# small 타입의 job(=VM) 개수
	  stemcell:												# small 타입의 job에 적용할 stemcell 정보
		name: bosh-openstack-kvm-ubuntu-trusty-go_agent		# stemcell 이름
		version: 3147										# 수정: stemcell 버전
	  cloud_properties:
		instance_type: m1.small								# 리소스에 적용할 오픈스택 flavor 타입
	- name: medium											# medium 타입 리소스
	  network: default
	  size: 1
	  stemcell:
		name: bosh-openstack-kvm-ubuntu-trusty-go_agent
		version: 3147										# 수정: stemcell 버전
	  env:
		bosh:												# 수정: medium 타입의 VM에 적용할 sha-512로 암호화된 비밀번호
		  password: $6$JA/VRhS7guR2t$kruB3wpqcgyi7Ql2IZIfVC7WP.ZTxjRJ9hbyJV4Xn/wr.mcaOR0MwqAulcKY5gxkFQ0/DJul7UELCKsQ3LrZ71
	  cloud_properties:
		instance_type: m1.medium

	jobs:
	- instances: 1											# 생성할 VM 수
	  name: nats											# job(=VM) 이름
	  networks:												# job에 할당할 네트워크 설정 정보
	  - default:											# 기본 설정 네트워크 정보
		- dns												# 참조 대상의 DNS (네트워크 이름이 default의 DNS를 참조)
		- gateway											# 참조 대상의 게이트웨이 (네트워크 이름이 default의 게이트웨이를 참조)
		name: default										# 참조 대상의 네트워크 이름
		static_ips:
		- 192.168.20.6										# 수정: job에 할당할 아이피
	  resource_pool: small									# job에 할당할 리소스 타입의 이름
	  template: nats										# job 생성시에 참조할 템플릿 이름
	- instances: 1
	  name: redis
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.7										# 수정: job에 할당할 아이피
	  resource_pool: small
	  template: redis
	- instances: 1
	  name: postgres
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.8										# 수정: job에 할당할 아이피
	  persistent_disk: 16384								# VM에 persistent 디스크를 할당 할 경우, 디스크 사이즈 설정
	  resource_pool: small
	  template: postgres
	- instances: 1
	  name: blobstore
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.9										# 수정: job에 할당할 아이피
	  persistent_disk: 51200
	  resource_pool: small
	  template: blobstore
	- instances: 1
	  name: director
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.10										# 수정: job에 할당할 아이피
	  - name: elastic
		static_ips:
		- 172.16.100.11										# 수정: 디렉터의 경우 관리를 위해 외부에서 접속할 수 있는 아이피를 할당
	  persistent_disk: 16384
	  resource_pool: medium
	  templates:
	  - name: powerdns
		release: bosh
	  - name: director
		release: bosh
	- instances: 1
	  name: registry
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.11										# 수정: job에 할당할 아이피
	  resource_pool: small
	  template: registry
	- instances: 1
	  name: health_monitor
	  networks:
	  - default:
		- dns
		- gateway
		name: default
		static_ips:
		- 192.168.20.12										# 수정: job에 할당할 아이피
	  resource_pool: small
	  template: health_monitor  

	properties:
	  blobstore:
		address: 192.168.20.9								# 수정: blobstore에 할당한 아이피(job에서 설정한 아이피와 동일)
		agent:												# agent 설정 정보
		  password: agent									# agent 비밀번호
		  user: agent										# agent 사용자 아이디
		director:											# 디렉터 설정 정보
		  password: director								# 디렉터 비밀번호
		  user: director									# 디렉터 사용자 아이디
	  director:
		address: 192.168.20.10								# 수정: 디렉터에 할당한 아이피(job에서 설정한 아이피와 동일)
		db:
		  database: bosh									# 데이터베이스 이름
		  host: 192.168.20.8								# 수정: postgres에 할당한 아이피(job에서 설정한 아이피와 동일)
		  password: postgres								# 데이터베이스 접속 비밀번호
		  user: postgres									# 데이터베이스 사용자 아이디
		name: mDirector										# 디렉터 이름
	  dns:
		address: 192.168.20.10								# 수정: DNS에 할당한 아이피(job에서 설정한 아이피와 동일)
		db:
		  database: bosh
		  host: 192.168.20.8								# 수정: postgres에 할당한 아이피(job에서 설정한 아이피와 동일)
		  password: postgres
		  user: postgres
		recursor: 172.16.100.10								# 수정: MicroBOSH에 할당한 아이피
	  hm:
		director_account:
		  password: admin
		  user: admin
		http:
		  password: hm
		  user: hm
		resurrector_enabled: true							# job이 중지된 경우, 해당 job에 대한 재시작 여부 설정
	  nats:
		address: 192.168.20.6								# 수정: nats에 할당한 아이피(job에서 설정한 아이피와 동일)
		password: nats
		port: 4222
		user: nats
	  postgres:
		database: bosh
		host: 192.168.20.8									# 수정: postgres에 할당한 아이피(job에서 설정한 아이피와 동일)
		password: postgres
		user: postgres
	  redis:
		address: 192.168.20.7								# 수정: redis 에 할당한 아이피(job에서 설정한 아이피와 동일)
		password: redis
	  registry:
		address: 192.168.20.11								# 수정: registry에 할당한 아이피(job에서 설정한 아이피와 동일)
		db:
		  database: bosh
		  host: 192.168.20.8								# 수정: postgres에 할당한 아이피(job에서 설정한 아이피와 동일)
		  password: postgres
		  user: postgres
		http:
		  password: registry
		  user: registry

	  ntp: [1.kr.pool.ntp.org, 0.asia.pool.ntp.org, 1.asia.pool.ntp.org]	# 수정: NTP 서버 설정

	  openstack:
		auth_url: IDENTITY-API-ENDPOINT						# 수정: OpenStack Identity API End Point 설정
		tenant: TENANT										# 수정: OpenStack Tenant 이름 설정
		username: USERNAME									# 수정: OpenStack 계정 이름 설정
		api_key: PASSWORD									# 수정: OpenStack 계정 비밀번호 설정
		default_key_name: KEYPAIR-NAME						# 수정: Key-Pair 이름 설정
		private_key: KEYPAIR-PATH							# 수정: Key-Pair 경로 설정
		default_security_groups: [SECURITY_GROUP_NAME]		# 수정: Security Group 설정
  

※ VM의 vcap 아이디의 비밀번호는 다음의 절차를 통해 변경할 수 있다.

	# 1. 암호화된 비밀번호 생성도구 설치
	$ sudo apt-get install whois

	# 2. vcap 비밀번호 생성
	$ mkpasswd -m sha-512
	Password: <PASSWORD>
	$6$4A2jmWvsaYv2O37d$WtT5ul2L4IeRcYcGljeYpd9NHuA3dbdUWcHuDInLG9542g0sU.j4/fj.0uVfEQuutTxw3FW4DANgIqn1OGVTq1

	# 3. 생성한 암호화된 비밀번호를 manifest의 env.bosh.password에 적용


## 6.5.  BOSH 배포

다음의 절차를 이용해서 BOSH를 배포한다.

1.  터미널에서 작성한 BOSH 배포 Manifest파일을 CLI에 설정한다.

		$ bosh deployment <작성한 manifest>


2.  아래와 같이 bosh deploy명령어를 이용하여 배포를 실행한다.

		$ bosh -n deploy


3.  Target 설정하면서 기본 계정인 admin/admin으로 로그인한다.

		$ bosh target <BOSH-FLOATING-IP>


4.  배포한 BOSH의 상태정보를 확인한다.

		$ bosh status


5.  BOSH를 삭제할 경우

		$ bosh target <MicroBOSH-FLOATING-IP>
		$ bosh delete deployment <BOSH-DEPLOYMENT-NAME>


6.  BOSH 배포에서 오류가 발생한 경우, 아래의 명령어를 이용하여 로그를 확인한다.

		$ bosh task <BOSH 배포 task 번호> --debug > <로그 출력 경로>/<로그 파일명> 
