
## Table of Contents
1. [개요](#1)
    * [1.1. 문서 목적](#2)
    * [1.2. 범위](#3)
    * [1.3. 참고자료](#4)
2. [Prerequisites](#5)
     * [2.1. OpenPaas Controller 설치확인](#6)
3. [Open Paas Container 설치](#7)
     * [3.1. Release Upload](#8)
     * [3.2. Deployment Manifest 파일 수정하기](#9)
     * [3.2.1. Name & Release](#10)
     * [3.2.2. Networks](#11)
     * [3.2.3. Compilation](#12)
     * [3.2.4. Resource Pools](#13)
     * [3.2.5. Update](#14)
     * [3.2.6. Jobs](#15)
     * [3.2.7. Properties](#16)
     * [3.3. Deployment Manifest 지정](#17)
     * [3.4. Bosh Deploy](#18)
     * [3.5. 설치형상 확인](#19)
     * [3.6. Trobleshooting](#20)
4. [설치 검증](#21)
     * [4.1. CF Login](#22)
     * [4.2. Application Deploy](#23)
     * [4.3. Application Access](#24)
     
     
     
# <a name="1"/>1. 개요
### <a name="2"/>1.1. 문서 목적
본 문서(설치가이드)는, 현 시점에서 지원되는 IaaS(Infrastructure as a Service) 중 하나인 VSphere 환경에서 개방형클라우드플랫폼(OpenPaas Container) 을 설치하기 위한 가이드를 제공하는데 그 목적이 있다.

#### <a name="3"/>1.2. 범위
본 문서의 범위는 개방형클라우드플랫폼을 VSphere 기반에 설치하기 위한 내용으로 한정되어 있다. Openstack/AWS와 같은 다른 IaaS 환경에서의 설치는 그에 맞는 가이드 문서를 참고해야 하며, Bosh/CF release 설치 또한 해당 가이드 문서를 별도로 참조해야 한다.

#### <a name="4"/>1.1.2. 참고 자료
[**https://github.com/cloudfoundry-incubator/diego-release**](https://github.com/cloudfoundry-incubator/diego-release)



# <a name="5"/>2. Prerequisites
### <a name="6"/>2.1. OpenPaas Controller 설치확인
개방형클라우드플랫폼 (OpenPaas Container) 를 설치하기 위해서는 사전에 OpenPaas Controller가 설치되어 있어야 한다.

확인하는 방법은 bosh deployments를 통해 배포된 리스트 목록으로 확인한다.

>![container_vsphere_install_guide_01]

# <a name="7"/>3. Open Paas Container 설치
### <a name="8"/>3.1. Release Upload
배포된 설치 패키지의 OpenPaaS-Container 폴더에 있는 Open PaaS Container Bosh Release와 의존관계에 있는 garden-linux 및 etcd 를 Bosh Server로 아래와 같은 명령으로 Upload 한다.
````
bosh upload release $INSTALL_PACKAGE/OpenPaaS-Container/garden-linux-release-0.329.0.tgz
bosh upload release $INSTALL_PACKAGE/OpenPaaS-Container/etcd-release-20.tgz
bosh upload release $INSTALL_PACKAGE/OpenPaaS-Container/openpaas-container-1.0.tgz
````
Release Upload는 상황에 따라 다소 차이는 있으나 보통 20-30분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.

>![container_vsphere_install_guide_02]

[주의] Release Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

Bosh Sever에 Release가 정상적으로 Upload 되었는지는 “bosh releases” 명령으로 확인한다.
````
bosh releases
````

>![container_vsphere_install_guide_03]

### <a name="9"/>3.2. Deployment Manifest 파일 수정하기
배포된 설치 패키지에 포함된 Sample Deployment Manifest File($INSTALL_PACKAGE/OpenPaaS-Deployment/openpaas-container-vsphere-1.0.yml)을 아래의 순서대로 설치환경에 적합하게 수정한다. 

#### <a name="10"/>3.2.1.  Name & Release
````
name: openpaas-container-vsphere-1.0                # Deployment Name
director_uuid: 0bc8d3c2-e032-4c7e-a99c-e23eea7091fc  # Bosh Director UUID
releases:
- name: openpaas-container      # container Release Name
  version: latest                 # container Release Version 
- name: openpaas-controller     # controller Release Name
version: latest                 # controller Release Version
- name: garden-linux            # garden-linux Release Name
  version: latest                # garden-linux Release Version 
- name: etcd                   # etcd Release Name
  version: latest                # etcd Release Version 
````
Deployment Name은 설치자가 임의로 부여하는데, IaaS와 Version을 표시할 것을 권장한다. Bosh Director UUID는 “bosh status” 명령을 실행하면 출력되는 UUID 값을 넣는다.

※ Controller, Container, Garden-linux, etcd의 Release Name과 Version은 “bosh releases” 명령의 결과로 나오는 값들을 입력하도록 한다. 본 가이드에서는 각 하나의 release가 업로드 되어 있으므로 명시적 버전 대신 업로드 된 릴리즈 버전 중 최신 버전인 latest 로 지정하여 사용한다. 

#### <a name="11"/>3.2.2. Networks
````
networks:
- name: openpaas-container-network         # Platform이 설치될 Network Name
  subnets:
  - cloud_properties:
      name: Internal                       # VSphere Network 이름
    dns:
    - 8.8.8.8
    gateway: 10.30.20.23                    # Gateway IP Address
    range: 10.30.0.0/16                      # Network CIDR
    reserved:
- 10.30.0.1 - 10.30.20.22
    - 10.30.20.24 - 10.30.49.255
    - 10.30.50.1 - 10.30.50.9
    static:
    - 10.30.50.10 - 10.30.50.50             # VM에 할당될 Static IP 주소 대역
  type: manual
````

Network Name은 설치자가 임의로 부여 가능하다. Network ID, Security_groups, Gateway, DNS Server, Network CIDR은 VSphere 구성을 직접 확인하거나 인프라 담당자에게 문의하여 정보를 얻도록 한다. Static IP 주소는 Platform을 설치할 때 개별 VM에 할당될 IP의 주소 대역으로 마찬가지로 인프라 담당자에게 할당을 받아야 한다.
	
#### <a name="12"/>3.2.3. Compilation
````
compilation:
  cloud_properties:
cpu: 2                                # Compile시 사용할 CPU 개수
    disk: 8192                            # Compile시 사용할 Disk 크기 
    ram: 1024                            # Compile시 사용할 Memory 크기 
network: openpaas-container-network     # 네트워크 설정에 사용된 것과 동일한 이름 
  reuse_compilation_vms: true              # compilation VMs 재사용 여부
workers: 6                              # 동시 동작하는 VM 수
````
Network Name은 3.3.2에서 정의한 것과 동일한 이름을 줘야 한다. Workers는 동시에 Compile을 수행하는 VM의 개수로 별다른 환경적 특성이 없다면 Default 값을 사용토록 한다.

#### <a name="13"/>3.2.4. Resource Pools
````
resource_pools:
- name: access_z1                                # Resource Name
  cloud_properties:
    cpu: 1                                       # Resource 정보 중  CPU 개수
    disk: 2048                                   # Resource 정보 중  Disk 크기 
    ram: 1024                                   # Resource 정보 중  Memory 크기 
network: openpaas-container-network                     # Network Name
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent         # Stemcell Name
    version: latest                                          # Stemcell Version 
    
- name: brain_z1
cloud_properties:
    cpu: 1                                       
    disk: 2048 
    ram: 1024 
network: openpaas-container-network                     
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent         
    version: latest 
    
- name: cc_bridge_z1
 cloud_properties:
    cpu: 1                                       
    disk: 2048 
    ram: 1024 
network: openpaas-container-network                     
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent         
    version: latest                                          

- name: cell _z1
 cloud_properties:
    cpu: 4                                       
    disk: 32768                           # Cell 영역의 VM은 기본적으로 5G Disk
    ram: 8024                            # 를 필요로하기 때문에 App 배포 영역을
network: openpaas-container-network     # 고려하여 최소 10G 이상의 크기를 지정한다.
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent         # Stemcell Name
    version: latest                                          # Stemcell Version
 
- name: database_z1
  cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  network: openpaas-container-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3147

- name: route_emitter_z1
  cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  network: openpaas-container-network
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3147
````

Resource pool 정보는 Jobs 영역에서 각 VM들이 사용하기 위한 Resource를 사전 정의한 영역으로, 각 VM 영역의 이름으로 명명되어 있으며, 필요 크기에 따라 cpu, disk, ram 정보를 수정한다. Stemcell Name과 Version은 “bosh stemcells” 명령어 결과로 출력되는 값들을 입력하도록 한다.

#### <a name="14"/>3.2.5. Update
````
update:
  canaries: 1			   # Canary instance 개수
  canary_watch_time: 5000-120000	   # Canary instance 의 health 상태 점검 대기 시간
  max_in_flight: 1			   # update instance들의 최대 병렬 처리 개수 
  serial: false			   # VM 순차적 Update 
  update_watch_time: 5000-120000  # canary instance 테스트 후 실제 instance update 하면서 health 상태 점검 대기 시간
````

Default 값들을 수정 없이 사용한다.

#### <a name="15"/>3.2.6. Jobs
아래 Sample Jobs를 참고하여 설치 환경에 맞게 수정한다.
````
jobs:
- instances: 1                     # VM Instance 개수
  name: database_z1              # Job Name
  networks:
- name: openpaas-container-network      # Network Name
  static_ips:
  - 10.30.50.11

persistent_disk: 1024
  properties:
    consul:
      agent:
        services:
          bbs: {}
          etcd: {}
    metron_agent:
      zone: z1
resource_pool: database_z1             # Resource Name
  templates:
  - name: etcd                  # database VM에 실행될 컴포넌트 (job template정보)
    release: etcd                # etcd job template이 존재하는 release 정보
- name: bbs
    release: openpaas-container
  - name: consul_agent
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update:
    max_in_flight: 1             # update instance들의 최대 병렬처리 개수
    serial: true                 # 순차적 실행여부

- instances: 1
  name: brain_z1
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.30.50.12             # brain 노드 ip 지정
properties:
consul:
    agent:
      services:
        auctioneer: {}
    metron_agent:
      zone: z1                      # Zone 영역(설치되는 VM 그룹)
  resource_pool: brain
  templates:
- name: consul_agent
  release: openpaas-controller
- name: auctioneer
  release: openpaas-container
- name: converger
  release: openpaas-container
- name: metron_agent
  release: openpaas-controller
update:
    max_in_flight: 1
    serial: true

- instances: 1
  name: cell_z1
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.30.50.13      # cell 노드 ip 지정
  properties:
    diego:
      rep:
        zone: z1
    metron_agent:
      zone: z1
  resource_pool: cell_z1
  templates:
- name: rep
    release: openpaas-container
  - name: consul_agent
    release: openpaas-controller
  - name: garden
    release: garden-linux
  - name: rootfses
    release: openpaas-container
  - name: metron_agent
    release: openpaas-controller
update:
    max_in_flight: 1
    serial: false

- instances: 1
  name: cc_bridge_z1
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.30.50.14   # cc_bridge 노드 ip 지정
  properties:
    consul:
      agent:
services:
          cc_uploader: {}
          nsync: {}
          stager: {}
          tps: {}
    metron_agent:
      zone: z1
  resource_pool: cc_bridge_z1
  templates:
- name: stager
    release: openpaas-container
  - name: nsync
    release: openpaas-container
  - name: tps
    release: openpaas-container
  - name: cc_uploader
    release: openpaas-container
  - name: consul_agent
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller update:
update:
    max_in_flight: 1
    serial: false

- instances: 1
  name: route_emitter_z1
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.30.50.16        # route_emitter 노드 ip 지정
  properties:
    metron_agent:
      zone: z1
  resource_pool: route_emitter_z1
  templates:
  - name: route_emitter
    release: openpaas-container
  - name: consul_agent
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update:
    max_in_flight: 1
    serial: false

- instances: 1
  name: access_z1
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.30.50.15       # access 노드 ip 지정
  properties:
    consul:
      agent:
        services:
file_server: {}
          ssh_proxy: {}
    metron_agent:
      zone: z1
  resource_pool: access_z1
  templates:
  - name: ssh_proxy
    release: openpaas-container
  - name: consul_agent
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: receptor
    release: openpaas-container
  update:
    max_in_flight: 1
    serial: false
````

#### <a name="16"/>3.2.7. Properties
아래 Sample Manifest를 참조하여 설치 환경에 맞게 값을 수정한다.
````
properties:
  consul:                                 # consul 속성 정의
    agent:
      datacenter: null
      log_level: null
      servers:
        lan:
        - 10.30.40.16			# controller에서 정의한 consul의 static_ips
    agent_cert: |				# controller에서 정의한 consul의 agent_cert
     -----BEGIN CERTIFICATE-----
     -----END CERTIFICATE-----

    agent_key:  |				# controller에서 정의한 consul의 agent_key
     -----BEGIN RSA PRIVATE KEY-----
-----END RSA PRIVATE KEY-----

    ca_cert: |				# controller에서 정의한 consul의 ca_cert
     -----BEGIN CERTIFICATE-----
     -----END CERTIFICATE-----
    encrypt_keys:			# controller에서 정의한 consul의 encrypt_keys
     - t66mLrBhJ5kpofLwoJpH5A==

    require_ssl: true
    server_cert: |				# controller에서 정의한 consul의 server_cert
     -----BEGIN CERTIFICATE-----
     -----END CERTIFICATE-----

    server_key: |				# controller에서 정의한 consul의 server_key
-----BEGIN CERTIFICATE-----
     -----END CERTIFICATE-----

  diego:
    auctioneer:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+					#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null	 # TLS client 캐시 크기
        max_idle_conns_per_host: null      # Idel http 요청 최대 수
        require_ssl: true		 	 # bbs 통시 시 ssl 사용
      cell_state_timeout: null		 # Cell State endpoint 에 HTTP 요청 시 타임아웃 시간(디폴트 1초)
      log_level: null			 # 로그 레벨(디폴트 info)
    bbs:
      active_key_label: key1		# database 쓰기 시 사용될 보안 키 명
      auctioneer:
        api_url: http://auctioneer.service.cf.internal:9016  # Auctioneer API 주소
      ca_cert: |+				#  diego-ca.crt 인증서
        -----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
      encryption_keys:			# 사용할 암호화 키
      - label: key1
        passphrase: DtFRfvja56znEDc0HRRTuw==
      etcd:
        ca_cert: |+			#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
        client_cert: |+			#  etcd-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+			#  etcd-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        machines:
        - etcd.service.cf.internal
        max_idle_conns_per_host: null
        require_ssl: true
      log_level: null
      require_ssl: true
      server_cert: |+			#  bbs-certs/server.crt 인증서
        -----BEGIN CERTIFICATE-----
        -----END CERTIFICATE-----
      server_key: |+			#  bbs-certs/server.key 
        -----BEGIN RSA PRIVATE KEY-----
        

        -----END RSA PRIVATE KEY-----
    cc_uploader:
      cc:
        base_url: https://api.controller.open-paas.com
        basic_auth_password: admin
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      log_level: null
    converger:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      log_level: debug
    executor:
      disk_capacity_mb: null
      drain_timeout_in_seconds: 0
      garden:
        address: 127.0.0.1:7777
        network: tcp
      log_level: debug
      memory_capacity_mb: null
    file_server:
      log_level: null
    nsync:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      cc:
        base_url: https://api.controller.open-paas.com
        basic_auth_password: admin
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      log_level: debug
    rep:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      evacuation_timeout_in_seconds: 60
      log_level: debug
    route_emitter:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      log_level: debug
      nats:
        machines:
        - 10.30.40.11			# controller에서 정의한 nats의 static_ips
        password: admin   		# controller에서 정의한 nats의 password
        port: 4222
        user: nats        		# controller에서 정의한 nats의 user id
    ssh_proxy:
      bbs:
        api_location: bbs.service.cf.internal:8889	# BBS server 도메인 주소 
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      diego_credentials: null
      enable_cf_auth: true
      enable_diego_auth: false
      host_key: |+		# ssh_proxy 인증서. 호스트 식별을 위한 RSA 키
        -----BEGIN RSA PRIVATE KEY-----
        -----END RSA PRIVATE KEY-----
      servers:
      - 10.30.50.31			# ssh_proxy 서버 ip 지정
      uaa_secret: ssh-proxy-secret  # UAA에서 ssh-proxy 인증 시 사용할 OAuth client 비밀키
      uaa_token_url: https://uaa.controller.open-paas.com/oauth/token # UAA 토큰 엔드 포인트
    ssl:
      skip_cert_verify: true
    stager:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      cc:
        base_url: https://api.controller.open-paas.com
        basic_auth_password: admin        		# controller에서 정의한 cc의 internal_api_password
        staging_upload_password: admin    		# controller에서 정의한 cc의 staging_upload_password
        staging_upload_user: staging_upload_user  	# controller에서 정의한 cc의 staging_upload_user
      insecure_docker_registry_list: null 
      log_level: null
    tps:
      bbs:
        api_location: bbs.service.cf.internal:8889
        ca_cert: |+				#  diego-ca.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_cert: |+				#  bbs-certs/client.crt 인증서
          -----BEGIN CERTIFICATE-----
          -----END CERTIFICATE-----
        client_key: |+				#  bbs-certs/client.key 
          -----BEGIN RSA PRIVATE KEY-----
          -----END RSA PRIVATE KEY-----
        client_session_cache_size: null
        max_idle_conns_per_host: null
        require_ssl: true
      cc:
        base_url: https://api.controller.open-paas.com
        basic_auth_password: admin
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      log_level: null
      traffic_controller_url: wss://doppler.controller.open-paas.com:443
  etcd:
    ca_cert: |+					#  diego-ca.crt 인증서
      -----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----
    client_cert: |+				#  etcd-certs/client.crt 인증서
      -----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----
    client_key: |+					#  etcd-certs/client.key 
      -----BEGIN RSA PRIVATE KEY-----
      -----END RSA PRIVATE KEY-----
    cluster:
    - instances: 1
      name: database_z1
    - instances: 0
      name: database_z2
    - instances: 0
      name: database_z3
    - instances: 0
      name: colocated_z1
    - instances: 0
      name: colocated_z2
    - instances: 0
      name: colocated_z2
    election_timeout_in_milliseconds: null
    heartbeat_interval_in_milliseconds: null
    machines:
    - etcd.service.cf.internal
    peer_ca_cert: |+				#  etcd-peer-ca.crt 인증서
      -----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
    peer_cert: |+					#  etcd-certs/peer.crt 인증서
      -----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----
    peer_key: |+					#  etcd-certs/peer.key 
      -----BEGIN RSA PRIVATE KEY-----
      -----END RSA PRIVATE KEY-----
    peer_require_ssl: true
    require_ssl: true
    server_cert: |+				#  etcd-certs/server.crt 인증서
      -----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----
    server_key: |+				#  etcd-certs/server.key 
      -----BEGIN RSA PRIVATE KEY-----
      -----END RSA PRIVATE KEY-----
  garden:
    allow_host_access: null
    allow_networks: null
    deny_networks:
    - 0.0.0.0/0
    disk_quota_enabled: null
    enable_graph_cleanup: true
    insecure_docker_registry_list: null
    listen_address: 0.0.0.0:7777
    listen_network: tcp
    log_level: debug
    mount_btrfs_loopback: null
    network_mtu: null
    persistent_image_list:
    - /var/vcap/packages/rootfs_cflinuxfs2/rootfs
  loggregator:
    etcd:
      machines:
      - 10.30.40.24		# controller에서 정의한 etcd의 static_ips
  loggregator_endpoint:
    shared_secret: admin  	# controller에서 정의한 loggregator_endpoint의 shared_secret
  metron_agent:
    deployment: openpaas-vsphere-1.0
  metron_endpoint:
    shared_secret: admin  # changeit: controller에서 정의한 metron_endpoint의 shared_secret
  nats:
    machines:
    - 10.30.40.11				# controller에서 정의한 nats의 static_ips
    password: admin   			# controller에서 정의한 nats의 password
    port: 4222
    user: nats        			# controller에서 정의한 nats의 user id
  syslog_daemon_config:
    address: null
    port: null
````

Sample Manifest에서 사용한 인증서 생성 방법은 다음과 같다. 
````
export GOPATH=$(pwd)/certstrap
export PATH=$PATH:$GOPATH/bin

cd $OPEN_PAAS_CONTAINER

mkdir -p $GOPATH

dir_cert=$(pwd)/diego-certs
. scripts/generate-diego-ca-certs

. scripts/generate-etcd-certs \
    etcd-peer-ca $dir_cert diego-ca $dir_cert

. scripts/generate-bbs-certs \
    diego-ca $dir_cert


SSH_PROXY_KEY=ssh_proxy
ssh-keygen -b 2048 -t rsa -f $dir_cert/$SSH_PROXY_KEY -q -N ""
ssh-keygen -lf $dir_cert/${SSH_PROXY_KEY}.pub | cut -f2 -d " " > $dir_cert/${SSH_PROXY_KEY}.fin

````

위 인증서 생성 명령을 실행 후 최종 인증서의 생성 위치는 다음과 같다.
- diego-cert/diego-ca.key
- diego-cert/etcd-peer-ca.crt
- diego-cert/etcd-certs/peer.cert 
- diego-cert/etcd-certs/peer.key
- diego-cert/etcd-certs/client.crt
- diego-cert/etcd-certs/client.key
- diego-cert/bbs-certs/peer.cert 
- diego-cert/bbs-certs/peer.key
- diego-cert/bbs-certs/client.crt
- diego-cert/bbs-certs/client.key
- diego-certs/ssh_proxy

ssh_proxy 접속을 위한 키(diego-certs/ssh_proxy.fin)는 아래와 같이 openpaas-controller의 manifest에 반영해야 한다. 
````
properties:
  …
  app_ssh:
    host_key_fingerprint: 17:2b:92:3e:03:7b:6c:3a:31:31:1e:f7:49:63:24:b0  # diego-certs/ssh_proxy.fin 파일 내용으로 치환
    oauth_client_id: ssh-proxy
````

### <a name="17"/>3.3. Deployment Manifest 지정
````
bosh deployment openpaas-container-vsphere-1.0.yml
````

“bosh deployment” 명령어로 생성한 Deployment Manifest File을 지정하고, 아래의 그림과 같이 동일한 명령어로 정상 지정 되었는지를 확인한다 
	
>![container_vsphere_install_guide_04]

### <a name="18"/>3.4. Bosh Deploy
Diego module에 대한 bosh upload 과정이 끝났으면, deploy 과정을 통해 Diego 관련 VM을 생성한다.
````
$ bosh deploy
````
>![container_vsphere_install_guide_05]

### <a name="19"/>3.5. 설치형상 확인
설치가 정상적으로 완료된 후 “bosh vms” 명령으로 설치된 Platform의 형상을 확인한다.
````
$ bosh vms
````
아래 그림과 같이 Deployment Name, Virtual Machine, IP 주소 등의 정보를 확인할 수 있다.

>![container_vsphere_install_guide_06]

### <a name="20"/>3.6. Trobleshooting
Container 파일을 Deploy를 한 후 다음 사진과 같이 “database_z1 > database_z1/0 (canary). Failed: Volume”이라는 에러가 발생하는 경우 사용하고 있는 OpenStack에 접속하여 리소스가 부족하지 않은지 확인해 보고 필요하지 않은 것들은 Delete를 해 준다.

>![container_vsphere_install_guide_07]

Container 파일을 Deploy를 한 후 다음 사진과 같이 “database_z1 > database_z1/0 (canary). Failed: ‘database_z1/0’ is not running after update”라는 에러가 발생하는 경우가 있다.

>![container_vsphere_install_guide_08]

다음과 같이 bosh ssh를 통해 database_z1/0에 접근한다. “Choose an instance”에서 database_z1/0을 선택하면 된다.

>![container_vsphere_install_guide_09]

다음 그림과 같이 sudo su를 통해 접속하면 ‘etcd’가 ‘not monitored’ 상태인 것을 확인할 수 있다.

>![container_vsphere_install_guide_10]

monit summary를 통해 먼저 프로세스의 상태를 확인한다. 프로세스를 없애고 다시 상태를 확인한다. 다음의 명령어들을 하나씩 실행하면서 프로세스의 상태를 확인한다.
````
monit summary
killall etcd
ps –ef | grep etcd
kill -9 pid
monit quit etcd
monit summary
````

monit quit etcd까지 다 실행하고 monit summary를 실행하면 ‘etcd’가 running으로 바뀐 것을 볼 수 있다.

>![container_vsphere_install_guide_11]

‘etcd’를 확인한 후 종료하고 OpenStack 서버에서 다음과 같은 명령어를 실행하면 database_z1/0가 running 상태인 것을 확인할 수 있다.

>![container_vsphere_install_guide_12]



# <a name="21"/>4. 설치 검증
### <a name="22"/>4.1. CF Login
````
cf api http://api.115.68.46.29.xip.io --skip-ssl-validation  # cf target 지정
…
cf login
Email> admin
Password> admin
OK
…
cf create-org open-paas
cf target -o open-paas
cf create-space dev
cf target -o open-paas -s dev
…
````

CF Target을 지정하고, Login을 수행한다. 이 때 계정은 admin/admin을 사용한다.
Application을 Deploy할 ORG와 Space를 생성하고, 해당하는 ORG/Space로 Targetting 한다.

※ admin 계정의 패스워드 설정을 바꾸고 싶다면, CF-Release deploy시 manifest 설정 파일에서 변경하야 한다.

###<a name="23"/>4.2. Application Deploy
개방형클라우드플랫폼 패키지와 함께 배포된 Sample Application이 위치하는 디렉토리로 이동하고 Application을 Deploy 한다.
````
cd $PACKAGE_ROOT/apps/hello-java
cf push “application-name” –i “instance_count” –m “memory_size” 
````

※ Application 배포시 Disk 관련 옵션 (-k)을 지정하지 않은 경우에는 기본적으로 6G 크기의 디스크 사용량이 지정된다.

>![container_vsphere_install_guide_13]
>![container_vsphere_install_guide_14]
>![container_vsphere_install_guide_15]

### <a name="24"/>4.3. Application Access
Deploy한 Application URL을 Browser 또는 curl 명령어로 Access하여 정상 접근 되는지를 확인한다.

사용법) 배포된 App URL이 hello-spring-test.controller.open-paas.com 일 경우
curl –L http:// hello-spring-test.controller.open-paas.com


[container_vsphere_install_guide_01]:/images/openpaas-container/container_vsphere_install_guide_01.png
[container_vsphere_install_guide_02]:/images/openpaas-container/container_vsphere_install_guide_02.png
[container_vsphere_install_guide_03]:/images/openpaas-container/container_vsphere_install_guide_03.png
[container_vsphere_install_guide_04]:/images/openpaas-container/container_vsphere_install_guide_04.png
[container_vsphere_install_guide_05]:/images/openpaas-container/container_vsphere_install_guide_05.png
[container_vsphere_install_guide_06]:/images/openpaas-container/container_vsphere_install_guide_06.png
[container_vsphere_install_guide_07]:/images/openpaas-container/container_vsphere_install_guide_07.png
[container_vsphere_install_guide_08]:/images/openpaas-container/container_vsphere_install_guide_08.png
[container_vsphere_install_guide_09]:/images/openpaas-container/container_vsphere_install_guide_09.png
[container_vsphere_install_guide_10]:/images/openpaas-container/container_vsphere_install_guide_10.png
[container_vsphere_install_guide_11]:/images/openpaas-container/container_vsphere_install_guide_11.png
[container_vsphere_install_guide_12]:/images/openpaas-container/container_vsphere_install_guide_12.png
[container_vsphere_install_guide_13]:/images/openpaas-container/container_vsphere_install_guide_13.png
[container_vsphere_install_guide_14]:/images/openpaas-container/container_vsphere_install_guide_14.png
[container_vsphere_install_guide_15]:/images/openpaas-container/container_vsphere_install_guide_15.png
