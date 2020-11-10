# PaaS-TA Redis 서비스팩 설치 가이드

### Table of Contents

1. [문서 개요](paas-ta-redis.md#1)
   * [1.1. 목적](paas-ta-redis.md#2)
   * [1.2. 범위](paas-ta-redis.md#3)
   * [1.3. 시스템 구성도](paas-ta-redis.md#4)
   * [1.4. 참고자료](paas-ta-redis.md#5)
2. [Redis 서비스팩 설치](paas-ta-redis.md#6)
   * [2.1. 설치 전 준비 사항](paas-ta-redis.md#7)
   * [2.2. Redis 서비스 릴리즈 업로드](paas-ta-redis.md#8)
   * [2.3. Redis 서비스 Deployment 파일 수정 및 배포](paas-ta-redis.md#9)
   * [2.4. Redis 서비스 브로커 등록](paas-ta-redis.md#10)
3. [Redis 연동 Sample App 설명](paas-ta-redis.md#11)
   * [3.1. Sample App 구조](paas-ta-redis.md#12)
   * [3.2. PaaS-TA에서 서비스 신청](paas-ta-redis.md#13)
   * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](paas-ta-redis.md#14)
4. [Redis Client 툴 접속](paas-ta-redis.md#15)
   * [4.1. Redis Desktop Manager 설치 및 연결](paas-ta-redis.md#16)

## 1. 문서 개요

#### 1.1. 목적

본 문서\(Redis 서비스팩 설치 가이드\)는 전자정부표준프레임워크 기반의 PaaS-TA에서 제공되는 서비스팩인 Redis 서비스팩을 Bosh를 이용하여 설치하는 방법과 PaaS-TA의 SaaS 형태로 제공하는 Application에서 Redis 서비스를 사용하는 방법을 기술하였다.

#### 1.2. 범위

설치 범위는 Redis서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

#### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. Redis dedicated-node\(2대\), Redis 서비스 브로커로 최소사항을 구성하였다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_01.png)

| 구분 | 스펙 |
| :--- | :--- |
| paasta-redis-broker | 1vCPU / 1GB RAM / 8GB Disk+4GB\(영구적 Disk\) |
| dedicated-node | 1vCPU / 1GB RAM / 8GB Disk+4GB\(영구적 Disk\) |

#### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs) [**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

## 2. Redis 서비스팩 설치

#### 2.1. 설치 전 준비 사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI가 설치 되어 있어야 하고 BOSH 에 로그인 및 target 설정이 되어 있어야 한다. BOSH CLI가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다.

* PaaS-TA에서 제공하는 압축된 릴리즈 파일들을 다운받는다. \(PaaSTA-Deployment.zip, PaaSTA-Sample-Apps.zip, PaaSTA-Services.zip\)
* 다운로드 위치

  > PaaSTA-Services : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Services.zip)  
  > PaaSTA-Deployment : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Deployment.zip)  
  > PaaSTA-Sample-Apps : [https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip](https://paas-ta.kr/data/packages/2.0/PaaSTA-Sample-Apps.zip)

#### 2.2. Redis 서비스 릴리즈 업로드

* PaaSTA-Services.zip 파일 압축을 풀고 폴더안에 있는 Redis 서비스 릴리즈 paasta-redis-2.0.tgz 파일을 확인한다.

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

  Acting as user 'admin' on 'bosh'

+--------------------------+----------+-------------+ \| Name \| Versions \| Commit Hash \| +--------------------------+----------+-------------+ \| cflinuxfs2-rootfs \| 1.40.0 _\| 19fe09f4+ \| \| empty-release \| 1+dev.1_ \| 00000000 \| \| etcd \| 86 _\| 2dfbef00+ \| \| paasta-container \| 2.0_ \| b857e171 \| \| paasta-controller \| 2.0 _\| 0f315314 \| \| paasta-garden-runc \| 2.0_ \| ea5f5d4d+ \| \| paasta-influxdb-grafana \| 2.0 _\| 00000000 \| \| paasta-logsearch \| 2.0_ \| 00000000 \| \| paasta-metrics-collector \| 2.0 _\| 00000000 \| +--------------------------+----------+-------------+ \(_\) Currently deployed \(+\) Uncommitted changes

Releases total: 9

```text
Redis 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인


-    Redis 서비스 릴리즈 파일을 업로드한다.
```

$ bosh upload release paasta-redis-2.0.tgz

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'bosh'

+--------------------------+----------+-------------+ \| Name \| Versions \| Commit Hash \| +--------------------------+----------+-------------+ \| cflinuxfs2-rootfs \| 1.40.0 _\| 19fe09f4+ \| \| empty-release \| 1+dev.1_ \| 00000000 \| \| etcd \| 86 _\| 2dfbef00+ \| \| paasta-container \| 2.0_ \| b857e171 \| \| paasta-controller \| 2.0 _\| 0f315314 \| \| paasta-garden-runc \| 2.0_ \| ea5f5d4d+ \| \| paasta-influxdb-grafana \| 2.0 _\| 00000000 \| \| paasta-logsearch \| 2.0_ \| 00000000 \| \| paasta-metrics-collector \| 2.0 _\| 00000000 \| +--------------------------+----------+-------------+ \(_\) Currently deployed \(+\) Uncommitted changes

Releases total: 9 inception@inception-new:~/bosh-space/kimdojun/redis$ bosh upload release paasta-redis-2.0.tgz RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'bosh'

Verifying manifest... Extract manifest OK Manifest exists OK Release name/version OK

File exists and readable OK Read package 'cf-cli' \(1 of 8\) OK Package 'cf-cli' checksum OK Read package 'cf-redis-broker' \(2 of 8\) OK Package 'cf-redis-broker' checksum OK Read package 'cf-redis-nginx' \(3 of 8\) OK Package 'cf-redis-nginx' checksum OK Read package 'cf-redis-smoke-tests' \(4 of 8\) OK Package 'cf-redis-smoke-tests' checksum OK Read package 'go' \(5 of 8\) OK Package 'go' checksum OK Read package 'redis-common' \(6 of 8\) OK Package 'redis-common' checksum OK Read package 'redis' \(7 of 8\) OK Package 'redis' checksum OK Read package 'ruby' \(8 of 8\) OK Package 'ruby' checksum OK Package dependencies OK Checking jobs format OK Read job 'broker-deregistrar' \(1 of 6\), version fd74f060a430d793ed734328638db0a5fee34395 OK Job 'broker-deregistrar' checksum OK Extract job 'broker-deregistrar' OK Read job 'broker-deregistrar' manifest OK Check template 'errand.sh.erb' for 'broker-deregistrar' OK Job 'broker-deregistrar' needs 'cf-cli' package OK Monit file for 'broker-deregistrar' OK Read job 'broker-registrar' \(2 of 6\), version 77bba8ba0e8fa8b06cc8be6e8ef373d285fa4daf OK Job 'broker-registrar' checksum OK Extract job 'broker-registrar' OK Read job 'broker-registrar' manifest OK Check template 'errand.sh.erb' for 'broker-registrar' OK Job 'broker-registrar' needs 'cf-cli' package OK Monit file for 'broker-registrar' OK Read job 'cf-redis-broker' \(3 of 6\), version a8cc45566d5ebc66dd90bce0737a9833e7f3fcb8 OK Job 'cf-redis-broker' checksum OK Extract job 'cf-redis-broker' OK Read job 'cf-redis-broker' manifest OK Check template 'pre-start.erb' for 'cf-redis-broker' OK Check template 'cf-redis-broker\_ctl.erb' for 'cf-redis-broker' OK Check template 'health\_check.sh.erb' for 'cf-redis-broker' OK Check template 'process-watcher\_ctl.erb' for 'cf-redis-broker' OK Check template 'process-destroyer\_ctl.erb' for 'cf-redis-broker' OK Check template 'nginx\_ctl.erb' for 'cf-redis-broker' OK Check template 'broker.yml.erb' for 'cf-redis-broker' OK Check template 'nginx.conf.erb' for 'cf-redis-broker' OK Check template 'redis.conf.erb' for 'cf-redis-broker' OK Check template 'drain.sh' for 'cf-redis-broker' OK Job 'cf-redis-broker' needs 'cf-redis-broker' package OK Job 'cf-redis-broker' needs 'redis-common' package OK Job 'cf-redis-broker' needs 'cf-redis-nginx' package OK Job 'cf-redis-broker' needs 'redis' package OK Monit file for 'cf-redis-broker' OK Read job 'dedicated-node' \(4 of 6\), version 660e3ceee60783b8dcbc01fef065371370fe2265 OK Job 'dedicated-node' checksum OK Extract job 'dedicated-node' OK Read job 'dedicated-node' manifest OK Check template 'agent.yml.erb' for 'dedicated-node' OK Check template 'redis.conf.erb' for 'dedicated-node' OK Check template 'nginx.conf.erb' for 'dedicated-node' OK Check template 'redis\_ctl.erb' for 'dedicated-node' OK Check template 'redis-agent\_ctl.erb' for 'dedicated-node' OK Check template 'nginx\_ctl.erb' for 'dedicated-node' OK Check template 'redis-agent.pem.erb' for 'dedicated-node' OK Check template 'redis-agent.key.erb' for 'dedicated-node' OK Check template 'drain-redis.sh' for 'dedicated-node' OK Job 'dedicated-node' needs 'redis' package OK Job 'dedicated-node' needs 'redis-common' package OK Job 'dedicated-node' needs 'cf-redis-nginx' package OK Job 'dedicated-node' needs 'cf-redis-broker' package OK Monit file for 'dedicated-node' OK Read job 'smoke-tests' \(5 of 6\), version e9a839a060cbac2985bc79cfbcabfc2c59ad0c8c OK Job 'smoke-tests' checksum OK Extract job 'smoke-tests' OK Read job 'smoke-tests' manifest OK Check template 'config.json.erb' for 'smoke-tests' OK Check template 'errand.sh.erb' for 'smoke-tests' OK Job 'smoke-tests' needs 'go' package OK Job 'smoke-tests' needs 'cf-redis-smoke-tests' package OK Job 'smoke-tests' needs 'cf-cli' package OK Monit file for 'smoke-tests' OK Read job 'syslog-configurator' \(6 of 6\), version e74e363f8b9ac570f30b32d024e41ef2c89db03f OK Job 'syslog-configurator' checksum OK Extract job 'syslog-configurator' OK Read job 'syslog-configurator' manifest OK Check template 'syslog-configurator\_ctl.erb' for 'syslog-configurator' OK Check template 'syslog\_forwarder.conf.erb' for 'syslog-configurator' OK Job 'syslog-configurator' needs 'redis-common' package OK Monit file for 'syslog-configurator' OK

### Release info

Name: paasta-redis Version: 2.0

Packages

* cf-cli \(33a64fb1b0ca68b3403fe5b0254e86ec7d672dba\)
* cf-redis-broker \(f530f8b2135eac4a888c2da20177082eb081ee65\)
* cf-redis-nginx \(dd15c82027671c74b108c52bcecb64fcaf9c0d38\)
* cf-redis-smoke-tests \(b347f491c873fdd9e878c90defd276a82f980023\)
* go \(32629593cd827ebaf88981b56d205bea6c8b7c18\)
* redis-common \(3747d5011f5405b1f8033653dae31b28e6839451\)
* redis \(c6226fd977b4bcb4693823d32ddeb4c9c2c0c76f\)
* ruby \(9b59d2f2700da81a98c38c73cd27b6ccf26f188c\)

Jobs

* broker-deregistrar \(fd74f060a430d793ed734328638db0a5fee34395\)
* broker-registrar \(77bba8ba0e8fa8b06cc8be6e8ef373d285fa4daf\)
* cf-redis-broker \(a8cc45566d5ebc66dd90bce0737a9833e7f3fcb8\)
* dedicated-node \(660e3ceee60783b8dcbc01fef065371370fe2265\)
* smoke-tests \(e9a839a060cbac2985bc79cfbcabfc2c59ad0c8c\)
* syslog-configurator \(e74e363f8b9ac570f30b32d024e41ef2c89db03f\)

License

* license \(443041add743bce9c52077d8f3d2e130c08340c5\)

Checking if can repack release for faster upload... cf-cli \(33a64fb1b0ca68b3403fe5b0254e86ec7d672dba\) UPLOAD cf-redis-broker \(f530f8b2135eac4a888c2da20177082eb081ee65\) UPLOAD cf-redis-nginx \(dd15c82027671c74b108c52bcecb64fcaf9c0d38\) UPLOAD cf-redis-smoke-tests \(b347f491c873fdd9e878c90defd276a82f980023\) UPLOAD go \(32629593cd827ebaf88981b56d205bea6c8b7c18\) UPLOAD redis-common \(3747d5011f5405b1f8033653dae31b28e6839451\) UPLOAD redis \(c6226fd977b4bcb4693823d32ddeb4c9c2c0c76f\) UPLOAD ruby \(9b59d2f2700da81a98c38c73cd27b6ccf26f188c\) UPLOAD broker-deregistrar \(fd74f060a430d793ed734328638db0a5fee34395\) UPLOAD broker-registrar \(77bba8ba0e8fa8b06cc8be6e8ef373d285fa4daf\) UPLOAD cf-redis-broker \(a8cc45566d5ebc66dd90bce0737a9833e7f3fcb8\) UPLOAD dedicated-node \(660e3ceee60783b8dcbc01fef065371370fe2265\) UPLOAD smoke-tests \(e9a839a060cbac2985bc79cfbcabfc2c59ad0c8c\) UPLOAD syslog-configurator \(e74e363f8b9ac570f30b32d024e41ef2c89db03f\) UPLOAD Uploading the whole release

Uploading release paasta-redis-: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 104.3MB 22.8MB/s ETA: 00:00:00 Director task Started extracting release &gt; Extracting release. Done \(00:00:01\)

Started verifying manifest &gt; Verifying manifest. Done \(00:00:00\)

Started resolving package dependencies &gt; Resolving package dependencies. Done \(00:00:00\)

Started creating new packages Started creating new packages &gt; cf-cli/33a64fb1b0ca68b3403fe5b0254e86ec7d672dba. Done \(00:00:00\) Started creating new packages &gt; cf-redis-broker/f530f8b2135eac4a888c2da20177082eb081ee65. Done \(00:00:00\) Started creating new packages &gt; cf-redis-nginx/dd15c82027671c74b108c52bcecb64fcaf9c0d38. Done \(00:00:00\) Started creating new packages &gt; cf-redis-smoke-tests/b347f491c873fdd9e878c90defd276a82f980023. Done \(00:00:00\) Started creating new packages &gt; go/32629593cd827ebaf88981b56d205bea6c8b7c18. Done \(00:00:02\) Started creating new packages &gt; redis-common/3747d5011f5405b1f8033653dae31b28e6839451. Done \(00:00:00\) Started creating new packages &gt; redis/c6226fd977b4bcb4693823d32ddeb4c9c2c0c76f. Done \(00:00:00\) Started creating new packages &gt; ruby/9b59d2f2700da81a98c38c73cd27b6ccf26f188c. Done \(00:00:00\) Done creating new packages \(00:00:02\)

Started creating new jobs Started creating new jobs &gt; broker-deregistrar/fd74f060a430d793ed734328638db0a5fee34395. Done \(00:00:00\) Started creating new jobs &gt; broker-registrar/77bba8ba0e8fa8b06cc8be6e8ef373d285fa4daf. Done \(00:00:00\) Started creating new jobs &gt; cf-redis-broker/a8cc45566d5ebc66dd90bce0737a9833e7f3fcb8. Done \(00:00:00\) Started creating new jobs &gt; dedicated-node/660e3ceee60783b8dcbc01fef065371370fe2265. Done \(00:00:00\) Started creating new jobs &gt; smoke-tests/e9a839a060cbac2985bc79cfbcabfc2c59ad0c8c. Done \(00:00:00\) Started creating new jobs &gt; syslog-configurator/e74e363f8b9ac570f30b32d024e41ef2c89db03f. Done \(00:00:00\) Done creating new jobs \(00:00:00\)

Started release has been created &gt; paasta-redis/2.0. Done \(00:00:00\)

Task 2337 done

Started 2017-01-13 06:03:46 UTC Finished 2017-01-13 06:03:49 UTC Duration :00:03 paasta-redis-: 96% \|oooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo \| 104.6MB 10.7MB/s Time: 00:00:09

Release uploaded

```text
-    업로드 된 Redis 릴리즈를 확인한다.
```

$ bosh releases

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on 'bosh'

+--------------------------+----------+-------------+ \| Name \| Versions \| Commit Hash \| +--------------------------+----------+-------------+ \| cflinuxfs2-rootfs \| 1.40.0 _\| 19fe09f4+ \| \| empty-release \| 1+dev.1_ \| 00000000 \| \| etcd \| 86 _\| 2dfbef00+ \| \| paasta-container \| 2.0_ \| b857e171 \| \| paasta-controller \| 2.0 _\| 0f315314 \| \| paasta-garden-runc \| 2.0_ \| ea5f5d4d+ \| \| paasta-influxdb-grafana \| 2.0 _\| 00000000 \| \| paasta-logsearch \| 2.0_ \| 00000000 \| \| paasta-metrics-collector \| 2.0 _\| 00000000 \| \| paasta-redis \| 2.0 \| 2d766084+ \| +--------------------------+----------+-------------+ \(_\) Currently deployed \(+\) Uncommitted changes

Releases total: 10

```text
### <div id='9'>  2.3. Redis 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용 할 것인지와 Release (Software packages, Config templates, Scripts)의 이름과 버전, VMs 용량, Jobs params 등이 정의 되어 있다.

-    PaaSTA-Deployment.zip 파일 압축을 풀고 폴더안에 있는 IaaS별 Redis Deployment 파일을 복사한다. 
예) vsphere 일 경우 paasta_redis_vsphere_2.0.yml를 복사

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


-    Deployment 파일을 서버 환경에 맞게 수정한다. (vsphere 용으로 설명, 다른 IaaS는 해당 Deployment 파일의 주석내용을 참고)

```yaml
# paasta-redis-service 설정 파일 내용

name: paasta-redis-service                             # 서비스 배포이름(필수)
director_uuid: 873c784d-3e91-4c78-95a3-b492024c389f    # bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- name: paasta-redis                                   # 서비스 릴리즈 이름(필수)
  version: 2.0                                         # 서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                                          # canary 인스턴스 수(필수)
  canary_watch_time: 30000-180000                      # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 6                                     # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-180000                      # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

compilation:                                           # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:            # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    cpu: 2
    disk: 4096
    ram: 4096
  network: default                       # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true            # 컴파일지 VM 재사용 여부(옵션)
  workers: 6                             # 컴파일 하는 가상머신의 최대수(필수)

jobs:
- instances: 1
  name: paasta-redis-broker                # 작업 이름(필수)
  networks:
  - name: default
    static_ips:
    - 10.30.60.71                          # 사용할 IP addresses 정의(필수)
  persistent_disk: 4096                    # 영구적 디스크 사이즈 정의(옵션)
  properties:
    nats:                                  # paas-ta nats 정보
      machines:
      - 10.30.110.31
      password: nats
      port: 4222
      user: nats
  resource_pool: services-small
  templates:
  - name: cf-redis-broker
    release: paasta-redis
  - name: syslog-configurator
    release: paasta-redis
- instances: 3
  name: dedicated-node                        # 전용 노드
  networks:
  - name: default
    static_ips:                              # 전용 노드 IP 목록(필수)
    - 10.30.60.72
    - 10.30.60.73
    - 10.30.60.74
  persistent_disk: 4096
  resource_pool: services-small
  templates:
  - name: dedicated-node
    release: paasta-redis
  - name: syslog-configurator
    release: paasta-redis

- instances: 1
  lifecycle: errand                         # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar
  networks:
  - name: default
  properties:
    broker:
      host: 10.30.60.71
      name: paasta-redis-service
      password: admin
      username: admin
  resource_pool: services-small
  templates:
  - name: broker-registrar
    release: paasta-redis

- instances: 1
  lifecycle: errand
  name: broker-deregistrar
  networks:
  - name: default
  properties:
    broker:
      host: 10.30.60.71
      name: paasta-redis-service
      password: admin
      username: admin
  resource_pool: services-small
  templates:
  - name: broker-deregistrar
    release: paasta-redis
- instances: 1
  lifecycle: errand
  name: smoke-tests
  networks:
  - name: default
  resource_pool: services-small
  templates:
  - name: smoke-tests
    release: paasta-redis

meta:
  apps_domain: 115.68.46.186.xip.io
  broker:                                                      # broker 정보 : 디폴트 포트 12350
    host: paasta-redis-broker.115.68.46.186.xip.io
    name: redis
    password: admin
    username: admin
  cf:                                                          # paas-ta 정보
    admin_password: admin
    admin_username: admin
    api_url: https://api.115.68.46.186.xip.io
    apps_domain: 115.68.46.186.xip.io
    skip_ssl_validation: false
    system_domain: 115.68.46.186.xip.io
  deployment_name: paasta-redis-service
  director_uuid: d363905f-eaa0-4539-a461-8c1318498a32          # uuid 정보 bosh status
  external_domain: 115.68.46.186.xip.io
  nats:                                                        # nats 정보
    machines:
    - 10.30.110.31
    password: nats
    port: 4222
    user: nats
  redis:
    bg_save_command: yetanotherrandomstring
    broker:                                                       # broker 정보
      dedicated_vm_plan_id: 48b35349-d3de-4e19-bc4a-66996ae07766
      name: redis
      service_id: 7aba7e52-f61b-4263-9de1-14e9d11fb67d
      shared_vm_plan_id: 78bf886c-bc50-4f31-a03c-cb786a158286
      subdomain: paasta-redis-broker
    config_command: configalias
    dedicated_plan:
      instance_count: 3                                        # 전용노드 수
    save_command: anotherrandomstring
    shared_plan:
      instance_limit: 10                                       # 공유 노드 수
      max_memory: 262144000
  release_name: paasta-redis
  route_name: paasta-redis-broker
  service_name: redis
  syslog_aggregator: null
networks:
- name: default
  subnets:
  - cloud_properties:
      name: Internal                                            # vsphere 에서 사용하는 network 이름(필수)
    dns:                                                        # dns 정보
    - 8.8.8.8
    gateway: 10.30.20.23
    name: default_unused
    range: 10.30.0.0/16
    reserved:                                                   # 설치시 제외할 IP 설정
    - 10.30.0.1 - 10.30.10.254
    - 10.30.40.1 - 10.30.60.70
    - 10.30.60.81 - 10.30.254.254
    static:                                                     # 사용 가능한 IP 설정
    - 10.30.60.71 - 10.30.60.80
properties:
  broker:                                                       # broker 정보
    host: paasta-redis-broker.115.68.46.186.xip.io
    name: redis
    password: admin
    username: admin
  cf:                                                           # paas-ta 정보
    admin_password: admin
    admin_username: admin
    api_url: https://api.115.68.46.186.xip.io
    apps_domain: 115.68.46.186.xip.io
    skip_ssl_validation: false
    system_domain: 115.68.46.186.xip.io
  redis:
    agent:
      backend_port: 54321
    bg_save_command: yetanotherrandomstring
    broker:
      auth:
        password: admin
        username: admin
      backend_host: 10.30.60.71
      backend_port: 12345
      dedicated_nodes:
      - 10.30.60.72
      - 10.30.60.73
      - 10.30.60.74
      dedicated_vm_plan_id: 48b35349-d3de-4e19-bc4a-66996ae07766
      enable_service_access: true
      host: 10.30.60.71
      name: redis
      network: default
      route_name: paasta-redis-broker
      service_id: 7aba7e52-f61b-4263-9de1-14e9d11fb67d
      service_instance_limit: 20                                    # 최대 서비스 인스턴스 개수
      service_name: redis
      shared_vm_plan_id: 78bf886c-bc50-4f31-a03c-cb786a158286
      subdomain: redis-broker
    config_command: configalias
    host: 10.30.60.71
    maxmemory: 262144000
    save_command: anotherrandomstring
  syslog_aggregator: null

resource_pools:
- cloud_properties:
    cpu: 1
    disk: 8192
    ram: 1024
  name: services-small
  network: default
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: "3309"
```

* Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

  ```text
  $ bosh deployment paasta_redis_vsphere_2.0.yml
  ```

  ```text
  RSA 1024 bit CA certificates are loaded due to old openssl compatibility
  Deployment set to '/home/inception/bosh-space/kimdojun/redis/paasta_redis_vsphere_2.0.yml'
  ```

* Redis 서비스팩을 배포한다.

  ```text
  $ bosh deploy
  ```

  \`\`\` RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on deployment 'paasta-redis-service' on 'bosh' Getting deployment properties from director...

### Detecting deployment changes

...&lt;manifest 파일 내용 출력// 생략 &gt;...

Please review all changes carefully

### Deploying

Are you sure you want to deploy? \(type 'yes' to continue\): yes

Director task Deprecation: Ignoring cloud config. Manifest contains 'networks' section.

Started preparing deployment &gt; Preparing deployment. Done \(00:00:02\)

Started preparing package compilation &gt; Finding packages to compile. Done \(00:00:00\)

Started compiling packages Started compiling packages &gt; cf-cli/33a64fb1b0ca68b3403fe5b0254e86ec7d672dba Started compiling packages &gt; cf-redis-smoke-tests/b347f491c873fdd9e878c90defd276a82f980023 Started compiling packages &gt; redis/c6226fd977b4bcb4693823d32ddeb4c9c2c0c76f Started compiling packages &gt; cf-redis-nginx/dd15c82027671c74b108c52bcecb64fcaf9c0d38 Started compiling packages &gt; redis-common/3747d5011f5405b1f8033653dae31b28e6839451 Started compiling packages &gt; go/32629593cd827ebaf88981b56d205bea6c8b7c18 Done compiling packages &gt; redis-common/3747d5011f5405b1f8033653dae31b28e6839451\(00:01:17\) Done compiling packages &gt; cf-redis-smoke-tests/b347f491c873fdd9e878c90defd276a82f980023\(00:01:20\) Done compiling packages &gt; cf-cli/33a64fb1b0ca68b3403fe5b0254e86ec7d672dba\(00:01:22\) Done compiling packages &gt; go/32629593cd827ebaf88981b56d205bea6c8b7c18\(00:01:57\) Started compiling packages &gt; cf-redis-broker/f530f8b2135eac4a888c2da20177082eb081ee65 Done compiling packages &gt; redis/c6226fd977b4bcb4693823d32ddeb4c9c2c0c76f\(00:02:20\) Done compiling packages &gt; cf-redis-nginx/dd15c82027671c74b108c52bcecb64fcaf9c0d38\(00:02:21\) Done compiling packages &gt; cf-redis-broker/f530f8b2135eac4a888c2da20177082eb081ee65\(00:00:26\) Done compiling packages \(00:02:23\)

Started creating missing vms Started creating missing vms &gt; paasta-redis-broker/113f1267-9b53-4e1c-94db-2e29abfbd687 \(0\) Started creating missing vms &gt; dedicated-node/a90f1ed2-d7cd-437d-9671-b49901b1d7b3 \(0\) Started creating missing vms &gt; dedicated-node/55611f55-db59-4c58-b74e-bf14deaea025 \(1\) Started creating missing vms &gt; dedicated-node/c74d92c7-2d0f-4d51-9868-d7aad55167ee \(2\) Done creating missing vms &gt; dedicated-node/a90f1ed2-d7cd-437d-9671-b49901b1d7b3 \(0\)\(00:01:05\) Done creating missing vms &gt; dedicated-node/c74d92c7-2d0f-4d51-9868-d7aad55167ee \(2\)\(00:01:06\) Done creating missing vms &gt; dedicated-node/55611f55-db59-4c58-b74e-bf14deaea025 \(1\)\(00:01:07\) Done creating missing vms &gt; paasta-redis-broker/113f1267-9b53-4e1c-94db-2e29abfbd687 \(0\)\(00:01:09\) Done creating missing vms \(00:01:09\)

Started updating instance paasta-redis-broker&gt; paasta-redis-broker/113f1267-9b53-4e1c-94db-2e29abfbd687 \(0\) \(canary\). Done \(00:01:3\) Started updating instance dedicated-node Started updating instance dedicated-node&gt; dedicated-node/a90f1ed2-d7cd-437d-9671-b49901b1d7b3 \(0\) \(canary\). Done \(00:01:36\) Started updating instance dedicated-node&gt; dedicated-node/55611f55-db59-4c58-b74e-bf14deaea025 \(1\) Started updating instance dedicated-node&gt; dedicated-node/c74d92c7-2d0f-4d51-9868-d7aad55167ee \(2\). Done \(00:01:31\) Done updating instance dedicated-node&gt; dedicated-node/55611f55-db59-4c58-b74e-bf14deaea025 \(1\)\(00:01:32\) Done updating instance dedicated-node\(00:03:08\)

Task 2551 done

Started 2017-01-13 09:15:17 UTC Finished 2017-01-13 09:24:08 UTC Duration :08:51

Deployed 'paasta-redis-service' to 'bosh'

```text
-    배포된 Redis 서비스팩을 확인한다.
```

$ bosh vms

```text

```

RSA 1024 bit CA certificates are loaded due to old openssl compatibility Acting as user 'admin' on deployment 'paasta-redis-service' on 'bosh'

Director task

Task 2415 done

+--------------------------------------------------------------+---------+-----+----------------+-------------+ \| VM \| State \| AZ \| VM Type \| IPs \| +--------------------------------------------------------------+---------+-----+----------------+-------------+ \| dedicated-node/0 \(a1017de7-dbd9-4eeb-9790-996b69a9f06c\) \| running \| n/a \| services-small \| 10.30.60.72 \| \| dedicated-node/1 \(4020a083-6bfa-431e-a047-2f567775cfbb\) \| running \| n/a \| services-small \| 10.30.60.73 \| \| dedicated-node/2 \(904bd212-43dc-45ef-876e-37a9cad54d36\) \| running \| n/a \| services-small \| 10.30.60.74 \| \| paasta-redis-broker/0 \(b1ed5994-741d-4e7c-9bf9-2406621b10ec\) \| running \| n/a \| services-small \| 10.30.60.71 \| +--------------------------------------------------------------+---------+-----+----------------+-------------+

VMs total: 4

```text
### <div id='10'> 2.4. Redis 서비스 브로커 등록
Redis 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Redis 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시에는 PaaS-TA에서 서비스 브로커를 등록할 수 있는 사용자로 로그인하여야 한다


- 서비스 브로커 목록을 확인한다.
```

$ cf service-brokers

```text

```

Getting service brokers as admin...

name url paasta-pinpoint-broker [http://10.30.70.82:8080](http://10.30.70.82:8080)

```text
- Redis 서비스 브로커를 등록한다.
```

cf create-service-broker {서비스브로커 이름} {서비스브로커 사용자ID} {서비스브로커 사용자비밀번호} [http://{서비스브로커](http://{서비스브로커) 호스트}:{서비스브로커 포트}

* 서비스브로커 이름 : 서비스 브로커 관리를 위해 PaaS-TA에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
* 서비스브로커 사용자ID / 비밀번호 : 서비스 브로커에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
* 서비스브로커 URL : 서비스 브로커가 제공하는 API를 사용할 수 있는 URL을 입력한다.

  ```text

  ```

  $ cf create-service-broker redis-service-broker admin admin [http://10.30.40.171:12350](http://10.30.40.171:12350)

  ```text

  ```

  Creating service broker paasta-redis-brokeras admin...

  OK

  \`\`\`

* 등록된 Redis 서비스 브로커를 확인한다.

```text
$ cf service-brokers
```

```text
Getting service brokers as admin...

name                     url
paasta-pinpoint-broker  http://10.30.70.82:8080
paasta-redis-broker     http://10.30.60.71:12350
```

* 접근 가능한 서비스 목록을 확인한다.

```text
$ cf service-access
```

```text
Getting service access as admin...
broker: paasta-redis-broker
  service   plan           access   orgs
  redis     shared-vm      none
  redis     dedicated-vm   none

broker: paasta-pinpoint-broker
  service    plan                access   orgs
  Pinpoint   Pinpoint_standard   none
```

서비스 브로커 등록시 최초에는 접근을 허용하지 않는다. 따라서 access는 none으로 설정된다.

* 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. \(전체 조직\)

```text
$ cf enable-service-access redis
```

```text
$ cf service-access
```

```text
Getting service access as admin...
broker: paasta-redis-broker
  service   plan           access   orgs
  redis     shared-vm      all
  redis     dedicated-vm   all

broker: paasta-pinpoint-broker
  service    plan                access   orgs
  Pinpoint   Pinpoint_standard   none
```

## 3. Redis연동 Sample App 설명

본 Sample App은 PaaS-TA에 배포되며 Redis의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

#### 3.1. Sample App 구조

Sample App은 PaaS-TA에 App으로 배포가 된다. 배포 완료 후 정상적으로 App이 구동되면 curl 명령어로 배포된 도메인 정보를 이용하여 Redis에 특정 key 에 값을 저장/조회/삭제를 한다.

Sample App 구조는 다음과 같다.

| 이름 | 설명 |
| :--- | :--- |
| lib | Sample 소스디렉토리 |
| manifest.yml | PaaS-TA에 app 배포시 필요한 설정을 저장하는 파일 |
| Gemfile | Sample App 구동시 필요한 ruby gem 설정 파일 |
| config.ru | Sample App 구동 파일 |

* PaaSTA-Sample-Apps.zip 파일의 압축을 풀고 Service 폴더 안에 있는 Redis Sample Web App인 redis-example-app을 복사한다.

```text
$ ls -all
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_02.png)

#### 3.2. PaaS-TA에서 서비스 신청

Sample App에서 Redis 서비스를 사용하기 위해서는 서비스 신청\(Provision\)을 해야 한다. \*참고: 서비스 신청시 PaaS-TA에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

* 먼저 PaaS-TA Marketplace에서 서비스가 있는지 확인을 한다.

```text
$ cf marketplace
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_03.png)

* Marketplace에서 원하는 서비스가 있으면 서비스 신청\(Provision\)을 한다.

```text
$ cf create-service {서비스명} {서비스 플랜} {내 서비스명}
- 서비스명 : redis로 Marketplace에서 보여지는 서비스 명칭이다.
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Redis 서비스는 shared-vm 과 dedicated-vm 플랜을 지원한다.
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경 설정 정보를 가져온다.
```

```text
$ cf create-service redis shared-vm redis-service-instance
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_04.png)

* 생성된 Redis 서비스 인스턴스를 확인한다.

```text
$ cf services
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_05.png)

#### 3.3. Sample App에 서비스 바인드 신청 및 App 확인

서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Redis 서비스를 이용한다. \*참고: 서비스 Bind 신청시 PaaS-TA에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

* Sample App 디렉토리로 이동하여 manifest 파일을 확인한다

```text
$ cd redis-example-app
```

```text
$ vi manifest.yml
```

```yaml
---
applications:
- name: redis-example-app    # 배포할 App 이름
  memory: 256M               # 배포시 메모리 크기
  instances: 1               # 배포시인스턴스 수
path: .                      # 배포시 구동되는 Path
```

* --no-start 옵션으로 App을 배포한다.

  --no-start: App 배포시 구동은 하지 않는다.

```text
$ cf push --no-start
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_06.png)

* 배포된 Sample App을 확인하고 로그를 수행한다.

```text
$ cf apps
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_07.png)

```text
$ cf logs redis-example-app
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_08.png)

* Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

```text
$ cf bind-service redis-example-app redis-service-instance
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_09.png)

* 바인드를 적용하기 위해서 App을 재기동한다.

```text
$ cf restart redis-example-app
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_10.png)

* App이 정상적으로 Redis 서비스를 사용하는지 확인한다.
* curl 로 확인

**Redis Application 을 이용하여 Key 값으로 데이터 저장**

```text
$ export APP=redis-example-app.controller.open-paas.com
```

```text
$ curl -X PUT $APP/foo -d 'data=bar'
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_11.png)

**Key 값을 이용하여 Redis 에 저장되어 있는 Value 값 Get**

```text
$ curl -X GET $APP/foo
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_12.png)

**Key 값을 이용하여 Redis 에 저장되어 있는 Value 삭제**

```text
$ curl -X DELETE $APP/foo
```

![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_13.png)

## 4. Redis Client 툴 접속

Application에 바인딩 된 Redis 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Redis Client 툴에서 직접 연결할 수 없다. 따라서 Redis Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Redis Client 툴로써는 오픈 소스인 Redis Desktop Manager로 가이드한다. Redis Desktop Manager 에서 접속하기 위해서 먼저 SSH 터널링할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 PaaS-TA에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 OpenStack 관리자 및 PaaS-TA 운영자에게 문의하여 구성한다. vsphere 에서 구성한 인스턴스는 공개키\(.pem\) 로 접속을 해야 하므로 공개키는 운영 담당자에게 문의하여 제공받는다. 참고\) 개인키\(.ppk\)로는 접속이 되지 않는다.

#### 4.1. Redis Desktop Manager 설치 및 연결

Redis Desktop Manager 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

* Redis Desktop Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다. [**http://redisdesktop.com/download**](http://redisdesktop.com/download) ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_14.png)
* 다운로드한 설치파일을 실행한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_15.png)
* Redis Desktop Manager 설치를 위한 안내사항이다. Next 버튼을 클릭한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_16.png)
* 프로그램 라이선스에 관련된 내용이다. I Agree 버튼을 클릭한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_17.png)
* Redis Desktop Manager를 설치할 경로를 설정 후 Install 버튼을 클릭한다. 별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_18.png)
* 설치 완료 후 Next 버튼을 클릭하여 다음 과정을 진행한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_19.png)
* Finish 버튼 클릭으로 설치를 완료한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_20.png)
* Redis Desktop Manager를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect to Redis Server 버튼을 클릭한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_21.png)
* Connection 탭에서 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_22.png)
* 서버 정보는 Application에 바인드 되어 있는 서버 정보를 입력한다. cfenv 명령어로 이용하여 확인한다. 예\) $ cfenvredis-example-app ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_23.png)
* SSH Tunnel탭을 클릭하고 PaaS-TA 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력하고 공개키\(.pem\) 파일을 불러온다. Test Connection 버튼을 클릭하면 Redis 서버에 접속이 되는지 테스트 하고 OK 버튼을 눌러 Redis 서버에 접속한다. \(참고\) 만일 공개키 없이 ID/Password로 접속이 가능한 경우에는 공개키 대신 사용자 이름과 암호를 입력한다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_24.png)
* 접속이 완료되고 좌측 서버 정보를 더블 클릭하면 좌측에 스키마 정보가 나타난다. ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_25.png)
* 신규 키 등록후 확인 ![](https://github.com/jhuhm13579/trans-test/tree/c3fa60c3f2804eba4cf4bb19f90449a85a66a625/images/paasta-service/redis/redis_image_26.png)

