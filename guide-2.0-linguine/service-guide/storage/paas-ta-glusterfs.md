# PaaS-TA GlusterFS 서비스팩 설치 가이드

## 1. 문서 개요

#### 1.1. 목적

본 문서\(GlusterFS 서비스팩 설치 가이드\)는 전자정부 표준 프레임워크 기반의 PaaS-TA에서 제공되는 서비스팩인 GlusterFS 서비스팩을 Bosh를 이용하여 설치 하는 방법과 PaaS-TA의 SaaS 형태로 제공하는 Application 에서GlusterFS 서비스를 사용하는 방법을 기술하였다.

#### 1.2. 범위

설치 범위는 GlusterFS 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

#### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도이다. Mysql Server, GlusterFS 서비스 브로커로 최소사항을 구성하였고 서비스 백엔드는 외부에 구성되어 있다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_01.png)

| 구분 | 스펙 |
| :--- | :--- |
| paasta-glusterfs-broker | 1vCPU / 1GB RAM / 8GB Disk |
| mysql | 1vCPU / 1GB RAM / 8GB Disk |

#### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs)   
 [**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

## 2. GlusterFS 서비스팩 설치

#### 2.1. 설치전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI가 설치 되어 있어야 하고 BOSH 에 로그인 및 target 설정이 되어 있어야 한다. BOSH CLI가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다.

* PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaSTA-Deployment.zip, PaaSTA-Sample-Apps.zip, PaaSTA-Services.zip\)
* 다운로드 위치

  > PaaSTA-Services : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip)  
  > PaaSTA-Deployment : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip)  
  > PaaSTA-Sample-Apps : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip)

#### 2.2. GlusterFS 서비스 릴리즈 업로드

* PaaSTA-Services.zip 파일 압축을 풀고 폴더안에 있는 GlusterFS 서비스 릴리즈 paasta-glusterfs-2.0.tgz 파일을 확인한다.

  ```text
  $ ls --all
  ```

  ```text
  .  cf236      paasta-cubrid-2.0.tgz    paasta-mysql-2.0.tgz    paasta-portal-object-storage-2.0.tgz paasta-redis-2.0.tgz
  .. cf-release paasta-glusterfs-2.0.tgz paasta-pinpoint-2.0.tgz paasta-rabbitmq-2.0.tgz              paasta-web-ide-2.0.tgz
  ```

* 업로드 되어 있는 릴리즈 목록을 확인한다.

  ```text
  $ bosh releases
  ```

  \`\`\`

  RSA 1024 bit CA certificates are loaded due to old openssl compatibility

  Acting as user 'admin' on 'my-bosh'

+-----------------+----------+-------------+ \| Name \| Versions \| Commit Hash \| +-----------------+----------+-------------+ \| empty-release \| 2.0 \| 870201f29+ \| +-----------------+----------+-------------+ \(+\) Uncommitted changes

Releases total: 1

```text
GlusterFS 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인

<br>
-    GlusterFS 서비스 릴리즈 파일을 업로드한다.
```

$ bosh upload release paasta-glusterfs-2.0.tgz

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'my-bosh'

Verifying manifest... Extract manifest OK Manifest exists OK Release name/version OK

File exists and readable OK Read package 'openjdk' \(1 of 4\) OK Package 'openjdk' checksum OK Read package 'op-gluster-java-broker' \(2 of 4\) OK Package 'op-gluster-java-broker' checksum OK Read package 'cli' \(3 of 4\) OK Package 'cli' checksum OK Read package 'mariadb' \(4 of 4\) OK Package 'mariadb' checksum OK Package dependencies OK Checking jobs format OK Read job 'op-glusterfs-java-broker' \(1 of 4\), version 4b573f84329911cc1f253796d102cf965e9d59c4 OK Job 'op-glusterfs-java-broker' checksum OK Extract job 'op-glusterfs-java-broker' OK Read job 'op-glusterfs-java-broker' manifest OK Check template 'bin/op-glusterfs-java-broker\_ctl.erb' for 'op-glusterfs-java-broker' OK Check template 'bin/monit\_debugger' for 'op-glusterfs-java-broker' OK Check template 'config/datasource.properties.erb' for 'op-glusterfs-java-broker' OK Check template 'config/glusterfs.properties.erb' for 'op-glusterfs-java-broker' OK Check template 'config/logback.xml.erb' for 'op-glusterfs-java-broker' OK Check template 'data/properties.sh.erb' for 'op-glusterfs-java-broker' OK Check template 'helpers/ctl\_setup.sh' for 'op-glusterfs-java-broker' OK Check template 'helpers/ctl\_utils.sh' for 'op-glusterfs-java-broker' OK Job 'op-glusterfs-java-broker' needs 'openjdk' package OK Job 'op-glusterfs-java-broker' needs 'op-gluster-java-broker' package OK Monit file for 'op-glusterfs-java-broker' OK Read job 'broker-deregistrar' \(2 of 4\), version b5f6f776d46eb1ac561ab1e8f58d8ddedb97f86e OK Job 'broker-deregistrar' checksum OK Extract job 'broker-deregistrar' OK Read job 'broker-deregistrar' manifest OK Check template 'errand.sh.erb' for 'broker-deregistrar' OK Job 'broker-deregistrar' needs 'cli' package OK Monit file for 'broker-deregistrar' OK Read job 'mysql' \(3 of 4\), version 8afab204ac5fc544319c81645e506eb32163f01e OK Job 'mysql' checksum OK Extract job 'mysql' OK Read job 'mysql' manifest OK Check template 'bin/mariadb\_ctl.erb' for 'mysql' OK Check template 'bin/monit\_debugger' for 'mysql' OK Check template 'data/properties.sh.erb' for 'mysql' OK Check template 'helpers/ctl\_setup.sh' for 'mysql' OK Check template 'helpers/ctl\_utils.sh' for 'mysql' OK Check template 'config/my.cnf.erb' for 'mysql' OK Check template 'config/mariadb\_init.erb' for 'mysql' OK Job 'mysql' needs 'mariadb' package OK Monit file for 'mysql' OK Read job 'broker-registrar' \(4 of 4\), version e1f5e30b87e70e916ea74ea8eb63a7b6ff6ff643 OK Job 'broker-registrar' checksum OK Extract job 'broker-registrar' OK Read job 'broker-registrar' manifest OK Check template 'errand.sh.erb' for 'broker-registrar' OK Job 'broker-registrar' needs 'cli' package OK Monit file for 'broker-registrar' OK

### Release info

Name: paasta-glusterfs Version: 2.0

Packages

* openjdk \(566dfae383c61dff0c9e82bee373bb68bac3e10e\)
* op-gluster-java-broker \(e281dffd1a22142658f57509183afa9be6be2983\)
* cli \(24305e50a638ece2cace4ef4803746c0c9fe4bb0\)
* mariadb \(76d00089f1c7ee1122f6b584d26d21a14254e1f0\)

Jobs

* op-glusterfs-java-broker \(4b573f84329911cc1f253796d102cf965e9d59c4\)
* broker-deregistrar \(b5f6f776d46eb1ac561ab1e8f58d8ddedb97f86e\)
* mysql \(8afab204ac5fc544319c81645e506eb32163f01e\)
* broker-registrar \(e1f5e30b87e70e916ea74ea8eb63a7b6ff6ff643\)

License

* none

Checking if can repack release for faster upload... openjdk \(566dfae383c61dff0c9e82bee373bb68bac3e10e\) UPLOAD op-gluster-java-broker \(e281dffd1a22142658f57509183afa9be6be2983\) UPLOAD cli \(24305e50a638ece2cace4ef4803746c0c9fe4bb0\) UPLOAD mariadb \(76d00089f1c7ee1122f6b584d26d21a14254e1f0\) UPLOAD op-glusterfs-java-broker \(4b573f84329911cc1f253796d102cf965e9d59c4\) UPLOAD broker-deregistrar \(b5f6f776d46eb1ac561ab1e8f58d8ddedb97f86e\) UPLOAD mysql \(8afab204ac5fc544319c81645e506eb32163f01e\) UPLOAD broker-registrar \(e1f5e30b87e70e916ea74ea8eb63a7b6ff6ff643\) UPLOAD Uploading the whole release

Uploading release paasta-gluste: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 135.0MB 34.7MB/s ETA: 00:00:00 Director task Started extracting release &gt; Extracting release. Done \(00:00:01\)

Started verifying manifest &gt; Verifying manifest. Done \(00:00:00\)

Started resolving package dependencies &gt; Resolving package dependencies. Done \(00:00:00\)

Started creating new packages Started creating new packages &gt; openjdk/566dfae383c61dff0c9e82bee373bb68bac3e10e. Done \(00:00:01\) Started creating new packages &gt; op-gluster-java-broker/e281dffd1a22142658f57509183afa9be6be2983. Done \(00:00:00\) Started creating new packages &gt; cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0. Done \(00:00:00\) Started creating new packages &gt; mariadb/76d00089f1c7ee1122f6b584d26d21a14254e1f0. Done \(00:00:01\) Done creating new packages \(00:00:02\)

Started creating new jobs Started creating new jobs &gt; op-glusterfs-java-broker/4b573f84329911cc1f253796d102cf965e9d59c4. Done \(00:00:00\) Started creating new jobs &gt; broker-deregistrar/b5f6f776d46eb1ac561ab1e8f58d8ddedb97f86e. Done \(00:00:00\) Started creating new jobs &gt; mysql/8afab204ac5fc544319c81645e506eb32163f01e. Done \(00:00:00\) Started creating new jobs &gt; broker-registrar/e1f5e30b87e70e916ea74ea8eb63a7b6ff6ff643. Done \(00:00:00\) Done creating new jobs \(00:00:00\)

Started release has been created &gt; paasta-glusterfs/2.0. Done \(00:00:00\)

Task 379 done

Started 2017-01-16 07:24:34 UTC Finished 2017-01-16 07:24:37 UTC Duration :00:03 paasta-gluste: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 135.4MB 16.9MB/s Time: 00:00:08

Release uploaded

```text
<br>
-    업로드 된 GlusterFS 릴리즈를 확인한다.
```

$ bosh releases

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'my-bosh'

+------------------+----------+-------------+ \| Name \| Versions \| Commit Hash \| +------------------+----------+-------------+ \| paasta-glusterfs \| 2.0 \| 85e3f01e+ \| +------------------+----------+-------------+ \(+\) Uncommitted changes

Releases total: 1

```text
<br>
<div id='9'></div>
###   2.3. glusterfs 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용 할 것인지와 Release (Software packages, Config templates, Scripts)의 이름과 버전, VMs 용량, Jobs params 등이 정의 되어 있다.

<br>
-    PaaSTA-Deployment.zip 파일 압축을 풀고 폴더안에 있는 IaaS별 GlusterFS Deployment 파일을 복사한다.
예) vsphere 일 경우 paasta_glusterfs_vsphere_2.0.yml를 복사

다운로드 받은 Deployment Yml 파일을 확인한다.
```

ls –all

```text

```

. openpaas-mysql-aws-1.0.yml paasta\_mysql\_aws\_2.0.yml paasta\_portal\_object\_storage\_vsphere\_2.0.yml paasta\_web\_ide\_aws\_2.0.yml .. paasta\_cubrid\_vsphere\_2.0.yml paasta\_pinpoint\_cluster\_aws\_2.0.yml paasta\_rabbitmq\_vsphere\_2.0.yml cf236 paasta-glusterfs-vsphere-2.0.yml paasta\_pinpoint\_vsphere\_2.0.yml paasta\_redis\_vsphere\_2.0.yml

```text
<br>
-    Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기 위한 현재 대상 BOSH Director의 UUID와 일치해야 한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할 수 있다.
```

$ bosh status

```text

```

Config /home/inception/.bosh\_config

Director RSA 1024 bit CA certificates are loaded due to old openssl compatibility Name bosh URL [https://10.30.40.105:25555](https://10.30.40.105:25555) Version .1.0 \(00000000\) User admin UUID d363905f-eaa0-4539-a461-8c1318498a32 CPI vsphere\_cpi dns disabled compiled\_package\_cache disabled snapshots disabled

Deployment Manifest /home/inception/crossent-deploy/paasta-logsearch.yml

```text
<br>
-    Deploy시 사용할 Stemcell을 확인한다.
```

$ bosh stemcells

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'bosh'

+------------------------------------------+---------------+---------+-----------------------------------------+ \| Name \| OS \| Version \| CID \| +------------------------------------------+---------------+---------+-----------------------------------------+ \| bosh-vsphere-esxi-ubuntu-trusty-go\_agent \| ubuntu-trusty \| 3263.8 _\| sc-af443b65-9335-43b1-9b64-6d1791a10428 \| \| bosh-vsphere-esxi-ubuntu-trusty-go\_agent \| ubuntu-trusty \| 3309_ \| sc-e00c788b-ac6b-4089-bc43-f56a3ffdb55a \| +------------------------------------------+---------------+---------+-----------------------------------------+

\(\*\) Currently in-use

Stemcells total: 2

```text
Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell을 업로드 해야 한다.

<br>
-    Deployment 파일을 서버 환경에 맞게 수정한다. (vsphere 용으로 설명, 다른 IaaS는 해당 Deployment 파일의 주석내용을 참고)

```yaml
# paasta-glusterfs-vsphere 설정 파일 내용
--
name: paasta-glusterfs-service                       # 서비스 배포이름(필수)
director_uuid: d363905f-eaa0-4539-a461-8c1318498a32  # bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- name: paasta-glusterfs             # 서비스 릴리즈 이름(필수)
  version: 2.0                       # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                        # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000    # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                   # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000    # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

compilation:                      # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:               # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    cpu: 4
    disk: 20480
    ram: 4096
  network: paasta_network         # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true     # 컴파일지 VM 재사용 여부(옵션)
  workers: 2                      # 컴파일 하는 가상머신의 최대수(필수)

jobs:
- instances: 1                    # job 인스턴스 수(필수)
  name: mysql                     # 작업 이름(필수): MySQL 서버
  networks:                       # 네트워크 구성정보
  - name: paasta_network          # Networks block에서 선언한 network 이름(필수)
    static_ips: 10.30.120.196     # 사용할 IP addresses 정의(필수): MySQL 서버 IP
#  persistent_disk: 1024          # 영구적 디스크 사이즈 정의(옵션): 10G, 상황에 맞게 수정
  properties:                     # job에 대한 속성을 지정(필수)
    admin_username: root          # MySQL 어드민 계정
    admin_password: admin         # MySQL 어드민 패스워드
  release: paasta-glusterfs       # 서비스 릴리즈 이름(필수)
  resource_pool: services-small   # Resource Pools block에 정의한 resource pool 이름(필수)
  template: mysql                 # job template 이름(필수)

- instances: 1
  name: paasta-glusterfs-broker
  networks:
  - name: paasta_network
    static_ips: 10.30.120.197          # service broker IP(필수)
  properties:
    jdbc_ip: 10.30.120.196             # Mysql IP(필수)
    jdbc_pwd: admin                    # Mysql password(필수)
    jdbc_port: 3306                    # Mysql Port
    log_dir: paasta-glusterfs-broker   # Broker Log 저장 디렉토리 명
    log_file: paasta-glusterfs-broker  # Broker Log 저장 파일 명
    log_level: INFO                    # Broker Log 단계
    glusterfs_url: 52.201.48.51        # Glusterfs 서비스 주소
    glusterfs_tenantname: service      # Glusterfs 서비스 테넌트 이름
    glusterfs_username: swift          # Glusterfs 서비스 계정 아이디
    glusterfs_password: password       # Glusterfs 서비스 암호
  release: paasta-glusterfs
  resource_pool: services-small
  template: op-glusterfs-java-broker

- instances: 1
  lifecycle: errand                    # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar
  networks:
  - name: paasta_network
  properties:
    broker:
      host: 10.30.120.197          # Service Broker IP
      name: glusterfs-service      # Service Broker Name
      password: cloudfoundry       # Service Broker Auth Password
      username: admin              # Service Broker Auth Id
      protocol: http               # Service Broker Http Protocol
      port: 8080                   # Service Broker port
    cf:
      admin_password: admin                      # CF Paasword
      admin_username: admin                      # CF Id
      api_url: https://api.115.68.46.30.xip.io   # CF Target Url
      skip_ssl_validation: true                  # CF SSL 설정
  release: paasta-glusterfs
  resource_pool: services-small
  template: broker-registrar

- instances: 1
  lifecycle: errand
  name: broker-deregistrar
  networks:
  - name: paasta_network
  properties:
    broker:
      name: glusterfs-service
    cf:
      admin_password: admin
      admin_username: admin
      api_url: https://api.115.68.46.30.xip.io
      skip_ssl_validation: true
  release: paasta-glusterfs
  resource_pool: services-small
  template: broker-deregistrar

networks:                        # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수 있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: paasta_network
  subnets:
  - cloud_properties:
      name: Internal             # vsphere 에서 사용하는 network 이름(필수)
    dns:
    - 10.30.20.24
    - 8.8.8.8
    gateway: 10.30.20.23
    name: default_unused
    range: 10.30.0.0/16
    reserved:                           # 설치시 제외할 IP 설정
    - 10.30.0.1 - 10.30.0.5
    static:                             # 사용 가능한 IP 설정
    - 10.30.120.190 - 10.30.120.200
  type: manual                          # 네트워크 설정 타입
properties: {}
resource_pools:               # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)        
- cloud_properties:           # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    cpu: 1
    disk: 8192
    ram: 1024
  name: services-small        # 고유한 resource pool 이름
  #size: 5                    # resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  network: paasta_network       
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent    # stemcell 이름(필수)
    version: "3309"                                 # stemcell 버전(필수)
```

* Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

  ```text
  $ bosh deployment paasta_glusterfs_vsphere_2.0.yml
  ```

  ```text
  RSA 1024 bit CA certificates are loaded due to old openssl compatibility
  Deployment set to '/mnt/workspace/deployments/test-deployments/paasta_glusterfs_vsphere_2.0.yml'
  ```

* glusterfs 서비스팩을 배포한다.

  ```text
  bosh deploy
  ```

  \`\`\`

  RSA 1024 bit CA certificates are loaded due to old openssl compatibility

  Acting as user 'admin' on deployment 'paasta-glusterfs-service' on 'my-bosh'

  Getting deployment properties from director...

  Unable to get properties list from director, trying without it...

### Detecting deployment changes

...&lt; manifest 파일 내용 출력// 생략 &gt;...

Please review all changes carefully

### Deploying

Are you sure you want to deploy? \(type 'yes' to continue\): yes

Director task Deprecation: Ignoring cloud config. Manifest contains 'networks' section.

Started preparing deployment &gt; Preparing deployment. Done \(00:00:00\)

Started preparing package compilation &gt; Finding packages to compile. Done \(00:00:00\)

Started compiling packages Started compiling packages &gt; cf-cli/68cbd378bc91c97fd0847075d71528c3eb1f7928 Started compiling packages &gt; glusterfs-broker/66260363d9b0ee0c671adc4a9b5e38453652fc06 Done compiling packages &gt; cf-cli/68cbd378bc91c97fd0847075d71528c3eb1f7928\(00:01:28\) Started compiling packages &gt; glusterfs-common/a1ceb4166602035b5e8783f2d90a2048a654875f Done compiling packages &gt; glusterfs-broker/66260363d9b0ee0c671adc4a9b5e38453652fc06\(00:01:32\) Started compiling packages &gt; java/4cc1119c49707a07d69b3863914b6f1307e611b5 Done compiling packages &gt; glusterfs-common/a1ceb4166602035b5e8783f2d90a2048a654875f\(00:00:08\) Started compiling packages &gt; haproxy/f0f2335dc5e7cab5c8a7232fd4aa9d92cbc8dba8 Done compiling packages &gt; java/4cc1119c49707a07d69b3863914b6f1307e611b5\(00:00:22\) Started compiling packages &gt; glusterfs-syslog-aggregator/a1662acacf8b2ef2ff54707681b125370544c824. Done \(00:00:06\) Started compiling packages &gt; golang-1.6.1/7c83e83f822259c6324742e3dfc5d4aaae25e9e6 Done compiling packages &gt; haproxy/f0f2335dc5e7cab5c8a7232fd4aa9d92cbc8dba8\(00:00:38\) Started compiling packages &gt; glusterfs-server/c93764197128c6e731899062f1cc33aef9cef053. Done \(00:00:06\) Started compiling packages &gt; erlang/d689b1d899513421b211ded55d614407a1b51ad6 Done compiling packages &gt; golang-1.6.1/7c83e83f822259c6324742e3dfc5d4aaae25e9e6\(00:00:25\) Started compiling packages &gt; glusterfs-upgrade-preparation/3ef7571fb3896b0c2260fd8063e47dfd4802a81c. Done \(00:00:14\) Started compiling packages &gt; glusterfs-cluster-migration-tool/aa2db9b54d0d87e67b4b7ef7ae1854ebe6cef867. Done \(00:00:12\) Done compiling packages &gt; erlang/d689b1d899513421b211ded55d614407a1b51ad6\(00:04:24\) Done compiling packages \(00:06:44\)

Started creating missing vms Started creating missing vms &gt; rmq/48e1fdae-965a-4750-a8bb-1811878a3d98 \(0\) Started creating missing vms &gt; haproxy/5de67578-1315-405d-9721-5e655b1a7954 \(0\) Started creating missing vms &gt; paasta-rmq-broker/18d4ae42-db87-400b-9d18-95c7a146acc9 \(0\). Done \(00:01:29\) Done creating missing vms &gt; rmq/48e1fdae-965a-4750-a8bb-1811878a3d98 \(0\)\(00:01:29\) Done creating missing vms &gt; haproxy/5de67578-1315-405d-9721-5e655b1a7954 \(0\)\(00:01:29\) Done creating missing vms \(00:01:29\)

Started updating instance haproxy&gt; haproxy/5de67578-1315-405d-9721-5e655b1a7954 \(0\) \(canary\) Started updating instance rmq&gt; rmq/48e1fdae-965a-4750-a8bb-1811878a3d98 \(0\) \(canary\) Started updating instance paasta-rmq-broker&gt; paasta-rmq-broker/18d4ae42-db87-400b-9d18-95c7a146acc9 \(0\) \(canary\) Done updating instance haproxy&gt; haproxy/5de67578-1315-405d-9721-5e655b1a7954 \(0\) \(canary\)\(00:00:47\) Done updating instance rmq&gt; rmq/48e1fdae-965a-4750-a8bb-1811878a3d98 \(0\) \(canary\)\(00:00:52\) Done updating instance paasta-rmq-broker&gt; paasta-rmq-broker/18d4ae42-db87-400b-9d18-95c7a146acc9 \(0\) \(canary\)\(00:00:55\)

Task 373 done

Started 2017-01-16 05:37:31 UTC Finished 2017-01-16 05:47:18 UTC Duration :09:47

Deployed 'paasta-glusterfs-service' to 'my-bosh'

```text
<br>
-    배포된 glusterfs 서비스팩을 확인한다.
```

$ bosh vms

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on deployment 'paasta-glusterfs-service' on 'my-bosh'

Director task

Task 385 done

+------------------------------------------------------------------+---------+-----+----------------+------------+ \| VM \| State \| AZ \| VM Type \| IPs \| +------------------------------------------------------------------+---------+-----+----------------+------------+ \| mysql/0 \(9a02e0e1-359b-47f1-ade5-6c149c316e3a\) \| running \| n/a \| resource\_pools \| 10.0.0.196 \| \| paasta-glusterfs-broker/0 \(4586e374-3209-4851-a0af-ef5eb60db261\) \| running \| n/a \| resource\_pools \| 10.0.0.197 \| +------------------------------------------------------------------+---------+-----+----------------+------------+

VMs total: 2

```text
<br>
<div id='10'></div>
### 2.4. GlusterFS 서비스 브로커 등록
GlusterFS 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 GlusterFS 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시에는 PaaS-TA에서 서비스 브로커를 등록할 수 있는 사용자로 로그인 하여야 한다


<br>
- 서비스 브로커 목록을 확인한다.
```

$ cf service-brokers

```text
![glusterfs_image_02]

<br>
- GlusterFS 서비스 브로커를 등록한다.
```

cf create-service-broker {서비스브로커 이름} {서비스브로커 사용자ID} {서비스브로커 사용자비밀번호} [http://{서비스브로커](http://{서비스브로커) 호스트}:{서비스브로커 포트}

* 서비스브로커 이름 : 서비스 브로커 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
* 서비스브로커 사용자ID / 비밀번호 : 서비스 브로커에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
* 서비스브로커 URL : 서비스 브로커가 제공하는 API를 사용할 수 있는 URL을 입력한다.

  ```text

  ```

  $ cf create-service-broker glusterfs-service admin cloudfoundry [http://10.30.40.197:8080](http://10.30.40.197:8080)

  \`\`\`

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_03.png)

* 등록된 GlusterFS 서비스 브로커를 확인한다.

  ```text
  $ cf service-brokers
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_04.png)

* 접근 가능한 서비스 목록을 확인한다.

  ```text
  $ cf service-access
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_05.png)

  서비스 브로커 등록시 최초에는 접근을 허용하지 않는다. 따라서 access는 none으로 설정된다.

* 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

  ```text
  $ cf enable-service-access glusterfs
  ```

  ```text
  $ cf service-access
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_06.png)

## 3. GlusterFS 연동 Sample App 설명

본 Sample Web App은 PaaS-TA에 배포되며 GlusterFS의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

#### 3.1. Sample App 구조

Sample Web App은 PaaS-TA에 App으로 배포가 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우저나 curl로 해당 App에 접속 하여 GlusterFS 환경정보\(서비스 연결 정보\)와파일 업로드하고 확인하는 기능을 제공한다.

Sample App 구조는 다음과 같다.

| 이름 | 설명 |
| :--- | :--- |
| src | Sample 소스디렉토리 |
| manifest | PaaS-TA에 app 배포시 필요한 설정을 저장하는 파일 |
| pom.xml | maven project 설정 파일 |
| target | maven build시 생성되는 디렉토리\(war 파일, classes 폴더 등\) |

* PaaSTA-Sample-Apps.zip 파일 압축을 풀고 Service폴더안에 있는 GlusterFSSample Web App인 hello-spring-glusterfs를 복사한다.

```text
$ ls -all
```

> ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_07.png)

#### 3.2. PaaS-TA에서 서비스 신청

Sample App에서 GlusterFS 서비스를 사용하기 위해서는 서비스 신청\(Provision\)을 해야 한다. \*참고: 서비스 신청시 PaaS-TA에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

* 먼저 PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

```text
$ cf marketplace
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_08.png)

* Marketplace에서 원하는 서비스가 있으면 서비스 신청\(Provision\)을 한다.

```text
$ cf create-service {서비스명} {서비스 플랜} {내 서비스명}
- 서비스명 : glusterfs로 Marketplace에서 보여지는 서비스 명칭이다.
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. glusterfs 서비스는 standard plan만 지원한다.
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경 설정 정보를 가져온다.
```

```text
$ cf create-service glusterfs glusterfs-1000Mb glusterfs-service-instance
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_09.png)

* 생성된 GlusterFS 서비스 인스턴스를 확인한다.

```text
$ cf services
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_10.png)

#### 3.3. Sample App에 서비스 바인드 신청 및 App 확인

서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 GlusterFS 서비스를 이용한다.

* 참고: 서비스 Bind 신청시 PaaS-TA에서 서비스 Bind 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.
* Sample App 디렉토리로 이동하여 manifest 파일을 확인한다

```text
$ cd hello-spring-glusterfs
```

```text
$ vi manifest.yml
```

```yaml
---
applications:
- name: hello- spring-glustefs          # 배포할 App 이름
  memory: 512M                          # 배포시 메모리 사이즈
  instances: 1                          # 배포 인스턴스 수
path: target/hello-spring-glusterfs.war # 배포하는 App 파일 PATH
```

* --no-start 옵션으로 App을 배포한다. 

  --no-start: App 배포시 구동은 하지 않는다.

```text
$ cf push --no-start
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_11.png)

* 배포된 Sample App을 확인하고 로그를 수행한다.

```text
$ cf apps
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_12.png)

```text
$ cf logs {배포된 App명}
```

```text
$ cf logs hello-spring-glusterfs
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_13.png)

* Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다. 

```text
$ cf bind-service hello-spring-glusterfs glusterfs-service-instance
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_14.png)

* 바인드가 적용되기 위해서 App을 재기동한다.

```text
$ cf restart hello-spring-glusterfs
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_15.png)

* App이 정상적으로 GlusterFS  서비스를 사용하는지 확인한다.

**curl 로 확인**

```text
$ curl --form attchFile=@../../../Desert.jpg --form press=OK hello-spring-glusterfs.115.68.46.30.xip.io/swifttest
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_16.png)

**브라우에서 이미지 확인**

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/glusterfs/glusterfs_image_17.jpeg)

