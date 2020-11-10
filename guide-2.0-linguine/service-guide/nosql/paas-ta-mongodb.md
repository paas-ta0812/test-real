# Table of Contents

1. [문서 개요](paas-ta-mongodb.md#1)
   * [1.1. 목적](paas-ta-mongodb.md#2)
   * [1.2. 범위](paas-ta-mongodb.md#3)
   * [1.3. 시스템 구성도](paas-ta-mongodb.md#4)
   * [1.4. 참고자료](paas-ta-mongodb.md#5)
2. [Mongodb 서비스팩 설치](paas-ta-mongodb.md#6)
   * [2.1. 설치전 준비사항](paas-ta-mongodb.md#7)
   * [2.2. Mongodb 서비스 릴리즈 업로드](paas-ta-mongodb.md#8)
   * [2.3. Mongodb 서비스 Deployment 파일 수정 및 배포](paas-ta-mongodb.md#9)
   * [2.4. Mongodb 서비스 브로커 등록](paas-ta-mongodb.md#10)
3. [Mongodb 연동 Sample App 설명](paas-ta-mongodb.md#11)
   * [3.1. Sample App 구조](paas-ta-mongodb.md#12)
   * [3.2. PaaS-TA에서 서비스 신청](paas-ta-mongodb.md#13)
   * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](paas-ta-mongodb.md#14)
4. [Mongodb Client 툴 접속](paas-ta-mongodb.md#15)
   * [4.1. MongoChef 설치 & 연결](paas-ta-mongodb.md#16)

## 1.  문서 개요

### 1.1. 목적

본 문서\(Mongodb 서비스팩 설치 가이드\)는 전자정부표준프레임워크 기반의 PaaS-TA에서 제공되는 서비스팩인 Mongodb 서비스팩을 Bosh를 이용하여 설치 하는 방법과 PaaS-TA의 SaaS 형태로 제공하는 Application 에서 Mongodb 서비스를 사용하는 방법을 기술하였다.

### 1.2. 범위

설치 범위는 Mongodb 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. Mongodb Server, Mongodb 서비스 브로커로 최소사항을 구성하였다.

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_02.png)

| 구분 | 스펙 |
| :--- | :--- |
| PaaS-TA-mongodb-broker | 1vCPU / 1GB RAM / 8GB Disk |
| Mongos | 1vCPU / 1GB RAM / 8GB Disk |
| Mongo Config | 1vCPU / 1GB RAM / 8GB Disk+4GB\(영구적 Disk\) |
| Mongod | 1vCPU / 1GB RAM /8GB Disk+4GB\(영구적 Disk\) |

### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs)

[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

## 2.  Mongodb 서비스팩 설치

### 2.1.  설치전 준비사항

본 서비스팩 설치를 위해서는 먼저 BOSH CLI가 설치 되어 있어야 하고 BOSH 에 로그인 및 target 설정이 되어 있어야 한다. BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다. PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaS-TA-Services.zip, PaaS-TA-Deployment.zip, PaaS-TA-Sample-Apps.zip\)

* 다운로드 위치

  > PaaSTA-Services : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip)  
  > PaaSTA-Deployment : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip)  
  > PaaSTA-Sample-Apps : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip)

### 2.2. Mongodb 서비스 릴리즈 업로드

* PaaS-TA-Services.zip 파일 압축을 풀고 폴더안에 있는 Mongodb 서비스 릴리즈 pasta-mongodb-shard-2.0.tgz 파일을 확인한다.

```text
$ ls –all
```

```text
-rw-rw-r-- 1 ubuntu ubuntu 121273779 Jan 16 04:05 paasta-mongodb-shard-2.0.tgz
```

* 업로드 되어 있는 릴리즈 목록을 확인한다.

  ```text
  $ bosh releases
  ```

  ```text
  +--------------------+----------------+-------------+
  | Name               | Versions       | Commit Hash |
  +--------------------+----------------+-------------+
  | cf-monitoring      | 0+dev.1*       | 00000000    |
  | cflinuxfs2-rootfs  | 1.40.0*        | 19fe09f4+   |
  | etcd               | 86*            | 2dfbef00+   |
  | logsearch          | 203.0.0+dev.1* | 00000000    |
  | metrics-collector  | 0+dev.1*       | 00000000    |
  | paasta-container   | 0+dev.1*       | b857e171    |
  | paasta-controller  | 0+dev.1*       | 0f315314    |
  | paasta-garden-runc | 2.0*           | ea5f5d4d+   |
  +--------------------+----------------+-------------+
  (*) Currently deployed
  (+) Uncommitted changes
  ```

  Mongodb 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인

* Mongodb 서비스 릴리즈를 업로드한다.

  ```text
  $ bosh upload release paasta-mongodb-shard-2.0.tgz
  ```

  \`\`\` Uploading release paasta-mongod: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 111.0MB 22.9MB/s ETA: 00:00:00 Director task 692 Started extracting release &gt; Extracting release. Done \(00:00:01\)

  Started verifying manifest &gt; Verifying manifest. Done \(00:00:00\)

  Started resolving package dependencies &gt; Resolving package dependencies. Done \(00:00:00\)

  Started creating new packages Started creating new packages &gt; mongodb\_broker/d547d39098e73acb70d58ab2be2c18c2410dfa5b. Done \(00:00:01\) Started creating new packages &gt; java7/cb28502f6e89870255182ea76e9029c7e9ec1862. Done \(00:00:01\) Started creating new packages &gt; cli/24305e50a638ece2cace4ef4803746c0c9fe4bb0. Done \(00:00:00\) Started creating new packages &gt; mongodb/b355ac045b257e6a0cec85874c6fb6e7abe92b6d. Done \(00:00:00\) Done creating new packages \(00:00:02\)

  Started creating new jobs Started creating new jobs &gt; mongodb\_slave/cd18c5187f44f8e3d1d2c7937047cc748a851a43. Done \(00:00:00\) Started creating new jobs &gt; mongodb\_broker/10da2f3c0e374b01f818b28ff5ecb8044fd0cd1a. Done \(00:00:00\) Started creating new jobs &gt; mongodb\_config/dcb9c707d4e9757a150f540ee5af39efb8580f04. Done \(00:00:01\) Started creating new jobs &gt; mongodb\_master/adfc199c9d2f3aceaf31fe56e553e15faf605ee7. Done \(00:00:00\) Started creating new jobs &gt; mongodb\_broker\_deregistrar/d797f068e89265313436b7c6439d93288d0fafbe. Done \(00:00:00\) Started creating new jobs &gt; mongodb\_shard/a549bee23d326211549a2dce9def42d85b655e4d. Done \(00:00:00\) Started creating new jobs &gt; mongodb\_broker\_registrar/a4892a7dfec7acdc7ba0cd2618a79ee3b2f80d9b. Done \(00:00:00\) Done creating new jobs \(00:00:01\)

  Started release has been created &gt; paasta-mongodb-shard/2.0. Done \(00:00:00\)

Task 692 done

Started 2017-01-16 04:16:20 UTC Finished 2017-01-16 04:16:24 UTC Duration 00:00:04 paasta-mongod: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 111.3MB 11.0MB/s Time: 00:00:10

Release uploaded

```text
-   업로드 되어 있는 릴리즈 목록을 확인한다.
```

$ bosh releases

```text

```

Acting as user 'admin' on 'my-bosh'

+----------------------+----------------+-------------+ \| Name \| Versions \| Commit Hash \| +----------------------+----------------+-------------+ \| cf-monitoring \| 0+dev.1 _\| 00000000 \| \| cflinuxfs2-rootfs \| 1.40.0_ \| 19fe09f4+ \| \| etcd \| 86 _\| 2dfbef00+ \| \| logsearch \| 203.0.0+dev.1_ \| 00000000 \| \| metrics-collector \| 0+dev.1 _\| 00000000 \| \| paasta-container \| 0+dev.1_ \| b857e171 \| \| paasta-controller \| 0+dev.1 _\| 0f315314 \| \| paasta-garden-runc \| 2.0_ \| ea5f5d4d+ \| \| paasta-mongodb-shard \| 2.0 _\| 85e3f01e+ \| +----------------------+----------------+-------------+ \(_\) Currently deployed \(+\) Uncommitted changes

Releases total: 9

```text
Mongodb 서비스 릴리즈가 업로드 되어 있는 것을 확인



### <div id='9'> 2.3. Mongodb 서비스 Deployment 파일 수정 및 배포

BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용 할 것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

- PaaS-TA-Deployment.zip 파일 압축을 풀고 폴더안에 있는 vSphere 용 Mongodb Deployment 파일인 paasta-mongodb-shard-vsphere-2.0.yml 를 복사한다.
다운로드 받은 Deployment Yml 파일을 확인한다. (paasta-mongodb-shard-vsphere-2.0.yml)
```

$ ls –all

```text
![mongodb_image_03]


- Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기 위한 현재 대상 BOSH Director의 UUID와 일치해야 한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director에 target 되어 있는 UUID를 확인할 수 있다.
```

$ bosh status

```text
![mongodb_image_04]


- Deploy시 사용할 Stemcell을 확인한다.
```

$ bosh stemcells

```text
![mongodb_image_05]
Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell을 업로드 해야 한다.


-    paasta-mongodb-shard-vsphere-2.0.yml Deployment 파일을 서버 환경에 맞게 수정한다. (빨간색으로 표시된 부분 특히 주의)
```

$ vi paasta-mongodb-shard-vsphere-2.0.yml

```text
```yaml
# paasta-mongodb-shard-vsphere 설정 파일 내용

---
name: paasta-mongodb-shard-service  # 서비스 배포이름(필수)
director_uuid ##################    # bosh status 에서 확인한 Director UUID을 입력(필수)

release:
  name: paasta-mongodb-shard        # 서비스 릴리즈 이름(필수)
  version: 2.0                      # 서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전

compilation:                        # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:                 # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)
    instance_type: monitoring       # 인스턴스 타입: Flavors 타입 (필수)
  network: default                  # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true       # 컴파일지 VM 재사용 여부(옵션)
  workers: 4                        # 컴파일 하는 가상머신의 최대수(필수)

# this section describes how updates are handled
update:
  canaries: 1                        # canary 인스턴스 수(필수)
  canary_watch_time: 30000           # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 30000           # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  max_in_flight: 4
networks:                            # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: default
  subnets:
  - cloud_properties:
      net_id: b7c8c08f-2d3b-4a86-bd10-641cb6faa317
      security_groups: [bosh]
    dns:                             # DNS 정보
    - 10.244.3.4
    - 8.8.8.8
    gateway: 10.244.3.1
    range: 10.244.3.0/24             # 사용할 네트워크 범위
    reserved:                        # 설치시 제외할 IP 설정
    - 10.244.3.2 - 10.244.3.140
    static:
    - 10.244.3.141 - 10.244.3.170    # 사용 가능한 IP 설정
resource_pools:                      # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique >해야함(필수)
- cloud_properties:
    instance_type: monitoring
  env:
    bosh: #password: dhvms09!
      password: $6$mwZOg/kA$r64mds4/xoqhW2tR8ck7oxmhqGiCBsDS5SWW/j8vgahvpdHkKJrb25/Wc2..CT3ja02qLgh0JB60RTP2ndjAh0
    # bosh:
    #  password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  name: small               # 고유한 resource pool 이름
  network: default
  stemcell:
    name:  bosh-openstack-kvm-ubuntu-trusty-go_agent     # stemcell 이름(필수)
    version: 3309                    # stemcell 버전(필수)


jobs:
  - name: mongodb_slave1             # 작업 이름(필수): mongodb replica set의 slave 서버
    template: mongodb_slave          # job template 이름(필수)
    instances: 1                     # job 인스턴스 수(필수)
    resource_pool: small             # resource_pools block에 정의한 resource pool 이름(필수)
    persistent_disk: 9000            # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:                        # 네트워크 구성정보
    - name: default                  # Networks block에서 선언한 network 이름(필수)
      static_ips:                    # 사용할 IP addresses 정의(필수)
      - 10.244.3.142
    properties:
      replSetName: op1               # replicaSet1 의 이름

  - name: mongodb_master1            # 작업 이름(필수): mongodb replica set의 master 서버
    template: mongodb_master         # job template 이름(필수)
    instances: 1                     # job 인스턴스 수(필수)
    resource_pool: small             # resource_pools block에 정의한 resource pool 이름(필수)
    persistent_disk: 9000            # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:                        # 네트워크 구성정보
    - name: default                  # Networks block에서 선언한 network 이름(필수)
      static_ips:
      - 10.244.3.141                 # 사용할 IP addresses 정의(필수)
    properties:
      replSet_hosts: ["10.244.3.141","10.244.3.142"]      # 첫번째 Host는 replicaSet1의 master
      replSetName: op1               # replicaSet1 의 이름

  - name: mongodb_config
    template: mongodb_config
    instances: 1
    resource_pool: small
    persistent_disk: 9000            # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:
    - name: default
      static_ips:
      - 10.244.3.150

  - name: mongodb_shard
    template: mongodb_shard
    instances: 1
    resource_pool: small
    persistent_disk: 9000            # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:
    - name: default
      static_ips:
      - 10.244.3.170
    properties:
      bindIp: 0.0.0.0
      configsvr_hosts:               # mongodb_config hosts
      - 10.244.3.150

      repl_name_host_list:           # mongodb_master properties
      - op1/10.244.3.141             # replicaSet1 의 이름/host
      # - op2/10.244.3.144            # replicaSet2 의 이름/host
      # - op3/10.244.3.147            # replicaSet3 의 이름/host

  - name: mongodb_broker                # 작업 이름(필수): mongodb 서비스 브로커
    template: mongodb_broker            # job template 이름(필수)
    instances: 1                        # job 인스턴스 수(필수)
    resource_pool: small                # resource_pools block에 정의한 resource pool 이름(필수)
    networks:                           # 네트워크 구성정보
    - name: default                     # Networks block에서 선언한 network 이름(필수)
      static_ips:                       # 사용할 IP addresses 정의(필수)
      - 10.244.3.154

  - name : mongodb_broker_registrar       # 작업 이름: 서비스 브로커 등록
    template : mongodb_broker_registrar
    instances: 1
    lifecycle: errand             # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
    resource_pool: small
    networks:
    - name: default
    properties:
      broker:                              # 서비스 브로커 설정 정보
        host: 10.244.3.154                 # 서비스 브로커 IP
        name: Mongo-DB                     # CF에서 서비스 브로커를 생성시 생기는 서비스 이름 브로커에 고정되어있는 값
        password: cloudfoundry             # 브로커 접근 아이디 비밀번호(필수)
        username: admin                    # 브로커 접근 아이디(필수)
        protocol: http
        port: 8080                         # 브로커 포트
      cf:
        admin_password: admin                           # CF 사용자의 패스워드
        admin_username: admin                           # CF 사용자 이름
        api_url: https://api.api.115.68.151.188.xip.io  # CF 설치시 설정한 api uri 정보(필수)
    release: paasta-mongodb-shard

  - name : mongodb_broker_deregistrar                   # 작업 이름: 서비스 브로커 삭제
    template : mongodb_broker_deregistrar
    instances: 1
    lifecycle: errand
    resource_pool: small
    networks:
    - name: default
    properties:
      broker:
        host: 10.244.3.154
        name: Mongo-DB
        password: cloudfoundry
        username: admin
        protocol: http
        port: 8080
      cf:
        admin_password: admin
        admin_username: admin
        api_url: https://api.115.68.151.188.xip.io
    release: paasta-mongodb-shard

meta:
  apps_domain: api.115.68.151.188.xip.io                # CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: api.115.68.151.188.xip.io            # CF 설치시 설정한 외부 도메인 정보
  nats:                                                 # CF 설치시 설정한 nats 정보
    machines:
    - 10.244.3.11
    password: admin
    port: 4222
    user: nats
  syslog_aggregator: null

properties:
  mongodb:            # mongodb shard release의 여러 job에서 공통적으로 허용하는 properties
                      # key는 shard를 구성할 때 mongos와 각 replicaSet의 인증을 하기위해 사용
    key: |
      +Qy+1icfeV8D2WXIfCojRjvYlryMVI2Ry+dAi8mYZ1H1Z9pDstRkOC0/oJYs0L/i
      +Dj/3PurWo8MJuqBYrWVGsRnsx31um0SVAgFZM2GQEKvHIByX5hq/MuHlulSLM0h
      GKkMT19zqDwFBFIN53jN0PLuuOnJ6FxZSb4cTLymfWM543WGpYx/31b8ehPYyeRp
      T7P2o2vUd9hecb8mQFxcjsBN7PTLwuPb5lK0BRL4Ze7rh6qeC8j7M3zimV8lX2X5
      9EtWlQP0ORYIlFpqijatZhS8Bf5AfI1EW6kZgfqwycl2ghxmSIbeleiqyQgYZNKQ
      yBXV9disuBXcKy4tsOjSFvKw7y61kjjQOn8KXElefokefdLbcrpeARP6LR9WwR1v
      ZTHcChfzWA4apHo6gJZkoqGVPjF4ArXTYxZfC+hHrsa5oe3XZjNapwV6XQfBNCuQ
      EihT3Td/B7iAUWJnGQvugFJwYKJ5EYOYubhk8QtO9QIvoZxQPDq9tgUsVgiQ6gty
      ZT83oxFAIgm3vky9l3uPwYi6jQ2FvsEJvDyiZl7gulOaC5UD/BdcM4Y5n/dxy/6Y
      qphWWuPsJwnYBXLJgwtTZ/NkYDYyX/tL9gyzXGPkpMMD7DofFjWEpJvHlVRKIxp1
      /zlxbVOMAmASgZDaqFperSQQyrfQqpuvAA8pRkWgorROyrsiRYYWlJZWWa4qHlI9
      OZ1dDp8o71l3v0SqsKbEtxINpdiUNx4OdafsMNN/KVxw9oGdrPXnDl5DomtmAoKZ
      uaCf3AQ3RsDeymgVX3j5EpLCHBhcPj+0B5tv4Yln652HAzDissOUKPyDf+PJaVRo
      OfDOkUvmuqnwl45DOoOtZ0BMw7hXGdgm6Xfv5jEmtSjJzQ1pfwHOOfiY+zZWhHAi
      ow/WNvLtUgNUhobi+OQb11bMMNNtmGWe+cZft6QzBsnd2xa/tAYTZDfAJ8OCvYQK
      e46UrHd54ZJFzdzicRZ8DeuU9G4K
    user: root               # admin 권한 사용자이름
    passwd: paasta           # admin 권한 사용자 비밀번호
#    replSetName: op
#    bindIp: 10.244.3.153    # shard server ip
    port: 27017              # mongodb port

  mongodb_broker:
    db_name: mongodb-broker   # mongodb broker 관리용 데이터베이스
    authsource: admin         # mongodb broker 관리용 데이터베이스에 접근할 때 인증정보가 있는 데이터베이스
    hosts: 10.244.3.153       # mongodb Host
```

* Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

```text
$ bosh deployment {Deployment manifest 파일 PATH}
```

```text
Deployment set to '/home/ubuntu/workspace/bd_test/paasta-mongodb-shard-2.0.yml'
```

* Mongodb 서비스팩을 배포한다.

```text
$ bosh deploy
```

```text
Acting as user 'admin' on deployment 'paasta-mongodb-shard-service' on 'my-bosh'
Getting deployment properties from director...
Unable to get properties list from director, trying without it...

Detecting deployment changes
----------------------------
resource_pools:
…
중략
…
Please review all changes carefully

Deploying
---------
Are you sure you want to deploy? (type 'yes' to continue): yes

Director task 756
Deprecation: Ignoring cloud config. Manifest contains 'networks' section.

  Started preparing deployment > Preparing deployment. Done (00:00:01)

  Started preparing package compilation > Finding packages to compile. Done (00:00:00)

  Started creating missing vms
  Started creating missing vms > mongodb_slave1/0 (66bbef0c-e135-417c-ba20-d61195fb7cfd)
  Started creating missing vms > mongodb_master1/0 (6090417a-2183-4d98-ac5b-9883172f2e0c)
  Started creating missing vms > mongodb_config/0 (2409b059-873e-45d1-b452-05fd5a336fff)
     Done creating missing vms > mongodb_master1/0 (6090417a-2183-4d98-ac5b-9883172f2e0c) (00:01:20)
  Started creating missing vms > mongodb_shard/0 (3e7db12a-0c39-4cb3-9e31-04a647206c00)
     Done creating missing vms > mongodb_slave1/0 (66bbef0c-e135-417c-ba20-d61195fb7cfd) (00:01:24)
  Started creating missing vms > mongodb_broker/0 (9de2c4f3-abd0-4cf3-91cb-674ae7d3b598)
     Done creating missing vms > mongodb_config/0 (2409b059-873e-45d1-b452-05fd5a336fff) (00:01:24)
     Done creating missing vms > mongodb_shard/0 (3e7db12a-0c39-4cb3-9e31-04a647206c00) (00:01:21)
     Done creating missing vms > mongodb_broker/0 (9de2c4f3-abd0-4cf3-91cb-674ae7d3b598) (00:01:21)
     Done creating missing vms (00:02:45)

  Started updating job mongodb_slave1 > mongodb_slave1/0 (66bbef0c-e135-417c-ba20-d61195fb7cfd) (canary). Done (00:01:11)
  Started updating job mongodb_master1 > mongodb_master1/0 (6090417a-2183-4d98-ac5b-9883172f2e0c) (canary). Done (00:01:07)
  Started updating job mongodb_config > mongodb_config/0 (2409b059-873e-45d1-b452-05fd5a336fff) (canary). Done (00:01:15)
  Started updating job mongodb_shard > mongodb_shard/0 (3e7db12a-0c39-4cb3-9e31-04a647206c00) (canary). Done (00:01:13)
  Started updating job mongodb_broker > mongodb_broker/0 (9de2c4f3-abd0-4cf3-91cb-674ae7d3b598) (canary). Done (00:00:48)

Task 756 done

Started        2017-01-16 09:19:33 UTC
Finished    2017-01-16 09:27:53 UTC
Duration    00:08:20

Deployed 'paasta-mongodb-shard-service' to 'my-bosh'
```

* 배포된 Mongodb 서비스팩을 확인한다.

```text
$ bosh vms
```

```text
Acting as user 'admin' on deployment 'paasta-mongodb-shard-service' on 'my-bosh'

Director task 764

Task 764 done

+----------------------------------------------------------+---------+-----+---------+--------------+
| VM                                                       | State   | AZ  | VM Type | IPs          |
+----------------------------------------------------------+---------+-----+---------+--------------+
| mongodb_broker/0 (9de2c4f3-abd0-4cf3-91cb-674ae7d3b598)  | running | n/a | small   | 10.244.3.154 |
| mongodb_config/0 (2409b059-873e-45d1-b452-05fd5a336fff)  | running | n/a | small   | 10.244.3.150 |
| mongodb_master1/0 (6090417a-2183-4d98-ac5b-9883172f2e0c) | running | n/a | small   | 10.244.3.141 |
| mongodb_shard/0 (3e7db12a-0c39-4cb3-9e31-04a647206c00)   | running | n/a | small   | 10.244.3.170 |
| mongodb_slave1/0 (66bbef0c-e135-417c-ba20-d61195fb7cfd)  | running | n/a | small   | 10.244.3.142 |
+----------------------------------------------------------+---------+-----+---------+--------------+

VMs total: 5
```

### 2.4. Mongodb 서비스 브로커 등록

Mongodb 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Mongodb 서비스 브로커를 등록해 주어야 한다.

서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

* 서비스 브로커 목록을 확인한다.

```text
$ cf service-brokers
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_06.png)

* Mongodb 서비스 브로커를 등록한다.

```text
$cf create-service-broker {서비스브로커 이름} {서비스브로커 사용자ID} {서비스브로커 사용자비밀번호} http://{서비스브로커 URL(IP)}
```

```text
-   서비스브로커 이름 : 서비스브로커 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.

-   서비스브로커 사용자ID / 비밀번호 : 서비스 브로커에 접근할 수 있는 사용자 ID입니다. 서비스브로커도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.

-   서비스브로커 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.
```

```text
$ cf create-service-broker mongodb-shard-service-broker admin cloudfoundry http://10.30.60.54:8080
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_07.png)

* 등록된 mongodb 서비스 브로커를 확인한다.

```text
$ cf service-brokers
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_08.png)

* 접근 가능한 서비스 목록을 확인한다.

```text
$ cf service-access
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_09.png) 서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

* 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

```text
$ cf enable-service-access Mongo-DB
$ cf service-access
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_10.png)

## 3. Mongodb 연동 Sample Web App 설명

본 Sample Web App은 PaaS-TA에 배포되며 Mongodb의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

### 3.1. Sample App 구조

Sample Web App은 PaaS-TA에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 Mongodb 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우저나 curl로 해당 App에 접속 하여 Mongodb 환경정보\(서비스 연결 정보\)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.

| 이름 | 설명 |
| :--- | :--- |
| src | Sample 소스디렉토리 |
| manifest | PaaS-TA에 app 배포시 필요한 설정을 저장하는 파일 |
| build.gradle | gradle project 설정 파일 |
| build | gradle 빌드시 생성되는 디렉토리\(war 파일, classes 폴더 등\) |

* PaaS-TA-Sample-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 Mongodb Sample Web App인 hello-spring-mongodb를 복사한다.

  ```text
  $ ls -all
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_11.png)

### 3.2. PaaS-TA에서 서비스 신청

Sample Web App에서 Mongodb 서비스를 사용하기 위해서는 서비스 신청\(Provision\)을 해야 한다. \*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

* 먼저 PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

```text
$ cf marketplace
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_12.png)

* Marketplace에서 원하는 서비스가 있으면 서비스 신청\(Provision\)을 한다.

  ```text
  $ cf create-service {서비스명} {서비스플랜} {내서비스명}
  ```

  \`\`\`

* 서비스명 : Mongo-DB로 Marketplace에서 보여지는 서비스 명칭이다.
* 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다.
* 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.

  \`\`\`

```text
$ cf create-service Mongo-DB default-plan mongodb-service-instance
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_13.png)

* 생성된 Mongodb 서비스 인스턴스를 확인한다.

  ```text
  $ cf services
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_14.png)

### 3.3. Sample App에 서비스 바인드 신청 및 App 확인

서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Mongodb 서비스를 이용한다. \*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

* Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

```text
$ cd hello-spring-mongodb
$ vi manifest.yml
```

```text
---
applications:
- name: hello-spring-mongodb #배포할 App 이름
  memory: 512M # 배포시 메모리 사이즈
  instances: 1 # 배포 인스턴스 수
  path: ./build/libs/hello-spring-mongodb.war #배포하는 App 파일 PATH
```

참고: ./build/libs/hello-spring-mongodb.war 파일이 존재 하지 않을 경우 gradle 빌드를 수행 하면 파일이 생성된다.

* --no-start 옵션으로 App을 배포한다.

  --no-start: App 배포시 구동은 하지 않는다.

```text
$ cf push --no-start
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_15.png)

* 배포된 Sample App을 확인하고 로그를 수행한다.

  ```text
  $ cf apps
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_16.png)

```text
$ cf logs {배포된 App명}
$ cf logs hello-spring-mongodb
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_17.png)

* Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

  ```text
  $ cf bind-service hello-spring-Mongodb Mongodb-service-instance
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_42.png)

* 바인드가 적용되기 위해서 App을 재기동한다.

  ```text
  $ cf restart hello-spring-mongodb
  ```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_18.png)

\(참고\) 바인드 후 App구동시 Mongodb 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.

* rule.json 파일을 만들고 아래와 같이 내용을 넣는다.

  ```text
  $ vi rule.json
  ```

  ```text
  [
  {
    "protocol": "tcp",
    "destination": "10.20.0.153",
    "ports": "27017"
  }
  ]
  ```

* 보안 그룹을 생성한다.

  ```text
  $ cf create-security-group Mongo-DB rule.json
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_19.png)

* 모든 App에 Mongodb 서비스를 사용할 수 있도록 생성한 보안 그룹을 적용한다.

  ```text
  $ cf bind-running-security-group Mongo-DB
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_20.png)

* App을 리부팅 한다.

  ```text
  $ cf restart hello-spring-Mongodb
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_21.png)

* App이 정상적으로 Mongodb 서비스를 사용하는지 확인한다.

  **curl 로 확인**

  ```text
  $ curl hello-spring-Mongodb.115.68.46.30.xip.io
  ```

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_22.png)

* 브라우에서 확인

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_23.png)

## 4.  Mongodb Client 툴 접속

Application에 바인딩된 Mongodb 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Mongodb Client 툴에서 직접 연결할수 없다. 따라서 SSH 터널, Proxy 터널 등을 제공하는 Mongodb Client 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Mongodb Client 툴로써는 MongoChef 로 가이드한다. MongoChef 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 PaaS-TA에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 OpenStack관리자 및 PaaS-TA 운영자에게 문의하여 구성한다.

## 4.1.  MongoChef 설치 & 연결

MongoChef 프로그램은 무료로 사용할 수 있는 소프트웨어이다.

* MongoChef을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.

  [**http://3t.io/mongochef/download/platform/**](http://3t.io/mongochef/download/platform/)

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_24.png)

* 다운로드한 설치파일을 실행한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_25.png)

* MongoChef 설치를 위한 안내사항이다. Next 버튼을 클릭한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_26.png)

* 프로그램 라이선스에 관련된 내용이다. 동의\(I accept the terms in the License Agreement\)에 체크 후 Next 버튼을 클릭한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_27.png)

* MongoChef 을 설치할 경로를 설정 후 Next 버튼을 클릭한다. 별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_28.png)

* Install 버튼을 클릭하여 설치를 진행한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_29.png)

* Finish 버튼 클릭으로 설치를 완료한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_30.png)

* MongoChef를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect버튼을 클릭한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_31.png)

* 새로운 접속 정보를 작성하기 위해New Connection 버튼을 클릭한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_32.png)

* Server에 접속하기 위한 Connection 정보를 입력한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_33.png)

서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env  명령어로 이용하여 확인한다.

```text
$ cf env hello-spring-mongodb
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_34.png)

* Authentication탭으로 이동하여 mongodb 의 인증정보를 입력한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_35.png)

SSH 터널 탭을 클릭하고 PaaS-TA 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_36.png)

모든 정보를 입력했으면 Test Connection 버튼을 눌러 접속 테스트를 한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_37.png) 모두 OK 결과가 나오면 정상적으로 접속이 된다는 것이다. OK 버튼을 누른다.

* Save 버튼을 눌러 작성한 접속정보를 저장한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_38.png)

* 방금 저장한 접속정보를 선택하고 Connect 버튼을 클릭하여 접속한다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_39.png)

* 접속이 완료되면 좌측에 스키마 정보가 나타난다. 컬럼을 더블클릭 해보면 우측에 적재되어있는 데이터가 출력된다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_40.png)

* 우측 화면에 쿼리 항목에 Query문을 작성한 후 실행 버튼\(삼각형\)을 클릭한다. Query문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.

  ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/mongodb/mongodb_image_41.png)

