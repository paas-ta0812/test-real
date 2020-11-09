## Table Contents
1. [개요](#11111)
	* [문서 목적](#1-목적)
	* [문서 범위](#1-범위)
	* [참고 자료](#1-자료)

1. [BOSH CLI 기본 사용법](#22222)

1. [BOSH CLI - micro](#33333)
	* [micro deployment](#micro-deployment)
	* [micro deployments](#micro-deployments)
	* [micro deploy](#micro-deploy)
	* [micro status](#micro-status)
	* [micro agent](#micro-agent)
	* [micro apply](#micro-apply)
	* [micro delete](#micro-delete)

1. [BOSH CLI - Deployment](#44444)
	* [deployment](#deployment)
	* [deployments](#deployments)
	* [edit deployment](#edit-deployment)
	* [deploy](#deploy)
	* [download manifest](#download-manifest)
	* [diff](#diff)
	* [validate jobs](#validate-jobs)

1. [BOSH CLI - Release](#55555)
	* [create release](#create-release)
	* [delete release](#delete-release)
	* [verify release](#verify-release)
	* [upload release](#upload-release)
	* [releases](#releases)
	* [reset release](#reset-release)
	* [init release](#init-release)
	* [generate job](#generate-job)
	* [generate package](#generate-package)

1. [BOSH CLI - Stemcell](#66666)
	* [upload stemcell](#upload-stemcell)
	* [verify stemcell](#verify-stemcell)
	* [stemcells](#stemcells)
	* [delete stemcell](#delete-stemcell)
	* [public stemcells](#public-stemcells)
	* [download public stemcell](#download-public-stemcell)

1. [BOSH CLI - Job](#77777)
	* [start](#start)
	* [stop](#stop)
	* [restart](#restart)
	* [recreate](#recreate)

1. [BOSH CLI - User](#88888)
	* [create user](#create-user)
	* [delete user](#delete-user)
	* [login](#login)
	* [logout](#logout)

1. [BOSH CLI - Task](#99999)
	* [task](#task)
	* [tasks](#tasks)
	* [tasks recent](#tasks-recent)
	* [cancel task](#cancel-task)

1. [BOSH CLI - Property](#aaaaa)
	* [set property](#set-property)
	* [get property](#get-property)
	* [properties](#properties)
	* [unset property](#unset-property)

1. [BOSH CLI - Log](#bbbbb)
	* [logs](#logs)

1. [BOSH CLI - Maintenance](#ccccc)
	* [cleanup](#cleanup)
	* [cloudcheck](#cloudcheck)

1. [BOSH CLI – Remote Access](#ddddd)
	* [ssh](#ssh)
	* [scp](#scp)

1. [BOSH CLI - Blob](#eeeee)
	* [upload blob](#upload-blobs)
	* [add blob](#add-blob)
	* [sync blobs](#sync-blobs)
	* [blobs](#blobs)

1. [BOSH CLI - Snapshot](#fffff)
	* [take snapshot](#take-snapshot)
	* [delete snapshot](#delete-snapshot)
	* [delete snapshots](#delete-snapshots)
	* [snapshots](#snapshots)

1. [BOSH CLI - Misc](#ggggg)
	* [status](#status)
	* [target](#target)
	* [targets](#targets)
	* [vms](#vms)
	* [locks](#locks)
	* [alias](#alias)
	* [aliases](#aliases)
	* [export compiled_package](#export-compiled_package)
	* [vm resurrection](#vm-resurrection)

<div id='11111'/>
## 문서 개요

<div id='1-목적'/>
### 문서 목적 
본 문서는 MicroBOSH/BOSH에 대한 설치 및 운영 관리를 위한 도구인 BOSH CLI에 대해 기본 사용법 및 사용 예시를 통해 BOSH를 이해하는데 목적이 있다. 

<div id='1-범위'/>
### 문서 범위 

본 문서에서는 BOSH CLI 사용법에 대해서 작성하였습니다.

<div id='1-자료'/>
### 참고 자료 

본 문서는 Cloud Foundry의 BOSH Document([http://bosh.io](http://bosh.io))를 참고로 작성하였습니다.

<div id='22222'/>
## BOSH CLI 기본 사용법

CLI는 BOSH 배포와 Release를 관리하기 위해 도움을 주는 커맨드 라인 명령어로 아래와 같이 2가지 형태로 구분된다.

- bosh : BOSH (Multi-VM BOSH)를 관리하기 위한 CLI
- bosh micro : MicroBOSH (Single-VM BOSH)를 관리하기 위한 CLI


- **기본 Syntax**

		$ bosh [<options>] <command> [<args>]
		$ bosh micro [<options>] <command> [<args>]

	bosh 또는 bosh micro 명령어에 대괄호로 묶인 options 과 args는 명령어에 따라 선택적으로 사용되고, command는 필수인자이다.

- **Options**

	|**번호**   |**옵션**                  |**설명**|
	|----------|-------------------------|--------------------------------|
	|1          |-c, --config              |BOSH configuration file 지정|
	|2          |--parallel MAX            |병렬 다운로드 최대 개수 설정|
	|3          |--[no-]color              |Toggle colorized output|
	|4          |-v, --verbose             |Show additional output|
	|5          |-q, --quiet               |suppress all output|
	|6          |-n, --non-interactive     |Don’t ask for user input|
	|7          |-N, --no-track            |Return task ID and don’t track|
	|8          |-P, --poll INTERVAL       |Director task polling interval|
	|9          |-t, --target URL          |타겟 디렉터 지정|
	|10         |-u, --user USER           |BOSH 사용자 아이디|
	|11         |-p, --password PASSWORD   |BOSH 사용자 비밀번호|
	|12         |-d, --deployment FILE     |BOSH 배포파일 지정|
	|13         |-h, --help                |Help 메시지 보기|

<div id='33333'/>
##  BOSH CLI - micro

<div id='micro-deployment'/>
### ***micro deployment***

- **기본 Syntax**

		$ bosh micro deployment [<manifest-filename>]

- **설명**

	bosh micro에 설정된 배포 파일을 확인하거나 또는 설정하기 위한 명령어

- **파라미터**

	|**파라미터 명** |**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|manifest-filename| MicroBOSH 배포 Manifest파일|X|

<div id='micro-deployments'/>
### ***micro deployments***

- **기본 Syntax**

		$ bosh micro deployments

- **설명**

	bosh micro에서 배포한 목록 출력

- **파라미터**

	없음

<div id='micro-deploy'/>
### ***micro deploy*** 

- **기본 Syntax**

		$ bosh micro deploy [<stemcell>] [--update] [--update-if-exists]

- **설명**

	MicroBOSH Instance 배포

- **파라미터**


	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|stemcell|Stemcell 파일|O|
	|--update|update existing instance|X|
	|--update-if-exists|create new or update existing instance|X|

<div id='micro-status'/>
### ***micro status*** 

- **기본 Syntax**

		$ bosh micro status

- **설명**

	MicroBOSH 등록 정보 출력

- **파라미터**

-   없음


<div id='micro-agent'/>
### ***micro agent***

- **기본 Syntax**

		$ bosh micro agent <args>

- **설명**

	MicroBOSH Agent에게 요청 Message 전달

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|args|메시지 유형|O|

	**메시지 유형**
	- start : MicroBOSH 에 구동중인 모든 Job 시작
	- stop : MicroBOSH 에 구동중인 모든 Job 종료
	- ping : Agent 응답 여부 확인
	- drain TYPE SPEC
		-   TYPE : ‘shutdown’, ‘update’, ‘status’ 중 하나
		-   SPEC : 사용할 drain spec
	- state [full] : 시스템의 상태 출력
	- list_disk : MicroBOSH에 마운트된 디스크 CID목록 출력
	- migrate_disk OLD NEW : 디스크 Migrate
	- mount_disk CID : 마운트 디스크
	- unmount_disk CID : 언마운트 디스크

<div id='micro-apply'/>
### ***micro apply***

- **기본 Syntax**

		$ bosh micro apply <spec>

- **설명**

	MicroBOSH Instance에 지정된 Spec정보 적용

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|spec|MicroBOSH Instance에 적용할 spec 파일|O|


<div id='micro-delete'/>
### ***micro delete*** 

- **기본 Syntax**

		$ bosh micro delete

- **설명**

	MicroBOSH Instance 삭제

- **파라미터**

-   없음

<div id='44444'/>
## BOSH CLI - Deployment

<div id='deployment'/>
### ***deployment***

- **기본 Syntax**

		$ bosh deployment [<manifest-filename>]

- **설명**

	bosh에 설정된 배포 파일을 확인하거나 또는 설정하기 위한 명령어

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|manifest-filename|BOSH 배포 Manifest파일|X|


<div id='deployments'/>
### ***deployments***

- **기본 Syntax**

		$ bosh deployments

- **설명**

	bosh에서 배포한 목록 출력

- **파라미터**

	없음



<div id='edit-deployment'/>
### ***edit deployment***

- **기본 Syntax**

		$ bosh edit deployment

- **설명**

	현재 설정된 배포 Manifest 파일 편집기를 이용해서 수정

- **파라미터**

	없음

<div id='deploy'/>
### ***deploy***

- **기본 Syntax**

		$ bosh deploy [--recreate] [--redact-diff]

- **설명**

	배포 수행

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--recreate|모든 VM 재생성|X|
	|--redact-diff|Manifest내 변경된 정보는 무시|X|



<div id='download-manifest'/>
### ***download manifest***

- **기본 Syntax**

		$ bosh download manifest <deployment_name> [<save_as>]

- **설명**

	BOSH Manifest 파일 다운로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|deployment_name|다운로드할 배포명 설정|O|
	|save_as|저장할 파일명 설정|X|

<div id='diff'/>
### ***diff***

- **기본 Syntax**

		$ bosh diff <template>

- **설명**

	현재 Deployment 설정된 정보와 지정된 Manifest파일과의 비교하여 차이 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|template|비교 대상 Manifst 파일|O|


<div id='validate-jobs'/>
### ***validate jobs***

- **기본 Syntax**

		$ bosh validate jobs

- **설명**

	현재 배포 Manifest를 사용하여 Release에서 모든 Job의 유효 여부 확인

- **파라미터**

	없음

<div id='55555'/>
## BOSH CLI - Release

<div id='create-release'/>
### ***create release***

- **기본 Syntax**

		$ bosh create release [--force] [--final] [--with-tarball] [--dry-run]
		[--name NAME] [--version VERSION]

- **설명**

	Release 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--force|bypass git dirty state check|X|
	|--final|create final release|X|
	|--with-tarball|create release tarball|X|
	|--dry-run|stop before writing release manifest|X|
	|--name NAME|specify a custom release name|X|
	|--version VERSION|specify a custom version number|X|

<div id='delete-release'/>
### ***delete release***

- **기본 Syntax**

		$ bosh delete release <name> [<version>] [--force]

- **설명**

	등록된 Release 삭제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|삭제할 Release 이름|O|
	|version|삭제할 Release 버전|X|
	|--force|Release 삭제하는 동안 에러 무시|X|

<div id='verify-release'/>
### ***verify release***

- **기본 Syntax**

		$ bosh verify release <tarball_path>

- **설명**

	Release 유효성 체크

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|tarball_path|Release 파일|O|

<div id='upload-release'/>
### ***upload release***

- **기본 Syntax**

		$ bosh upload release [<release_file>] [--rebase] [--skip-if-exists]

- **설명**

	Release 업로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|release_file|업로드할 Release 파일|O|
	|--rebase|최신 버전 Release|X|
	|--skip-if-exists|Release가 존재하는 경우 무시|X|

<div id='releases'/>
### ***releases***

- **기본 Syntax**

		$ bosh releases

- **설명**

	Release 목록 출력

- **파라미터**

	없음

<div id='reset-release'/>
### ***reset release***

 **기본 Syntax**

		$ bosh reset release

- **설명**

	Reset Dev release

- **파라미터**

	없음

<div id='init-release'/>
### ***init release***

- **기본 Syntax**

		$ bosh init release [<base>] [--git]

- **설명**

	Release작성하기 위한 템플릿 디렉토리 및 파일 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|base|Release 작성을 위한 기준 디렉토리 지정(지정하지 않을 경우 현재 디렉토리에 생성됨)|X|
	|--git|git repository 초기화|X|

<div id='generate-job'/>
### ***generate job***

- **기본 Syntax**

		$ bosh generate job <name>

- **설명**

	Job Template 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|생성할 Job Template 이름|O|

<div id='generate-package'/>
### ***generate package***

- **기본 Syntax**

		$ bosh generate package <name>

- **설명**

	Package 템플릿 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|생성할 패키지 이름|O|

<div id='66666'/>
## BOSH CLI - Stemcell 

<div id='upload-stemcell'/>
### ***upload stemcell***

- **기본 Syntax**

		$ bosh upload stemcell <stemcell_location> [--skip-if-exists]

- **설명**

	BOSH stemcell 업로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|stemcell_location|BOSH Stemcell 파일 경로|O|
	|--skip-if-exists|존재하는 BOSH Stemcell이면 Skip|X|

<div id='verify-stemcell'/>
### ***verify stemcell***

- **기본 Syntax**

		$ bosh verify stemcell <tarball_path>

- **설명**

	BOSH stemcell 유효성 검사

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|tarball_path|BOSH Stemcell 파일 경로|O|

<div id='stemcells'/>
### ***stemcells***

- **기본 Syntax**

		$ bosh stemcells

- **설명**

	Director에 업로드한 Stemcell 목록 출력

- **파라미터**

	없음

<div id='delete-stemcell'/>
### ***delete stemcell***

- **기본 Syntax**

		$ bosh delete stemcell <name> <version> [--force]

- **설명**

	BOSH stemcell 삭제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|Stemcell 이름|O|
	|version|Stemcell 버전|O|
	|--force|Stemcell 삭제하는 동안 에러 무시|X|

<div id='public-stemcells'/>
### ***public stemcells***

- **기본 Syntax**

		$ bosh public stemcells [--full] [--all]

- **설명**

	public stemcell 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--full|다운로드 URL 출력|X|
	|--all|모든 Stemcell 출력|X|

<div id='download-public-stemcell'/>
### ***download public stemcell***

- **기본 Syntax**

		$ bosh download public stemcell <stemcell_filename>

- **설명**

	public stemcell 다운로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|stemcell_filename|다운로드 받을 Stemcell 파일명|O|

<div id='77777'/>
## BOSH CLI - Job

<div id='start'/>
### ***start*** 

- **기본 Syntax**

		$ bosh start <job> [<index>] [--force]

- **설명**

	Job/Instance 시작

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|X|
	|--force|수행중 발생하는 에러 무시|X|

<div id='stop'/>
### ***stop***

- **기본 Syntax**

		$ bosh stop <job> [<index>] [--soft] [--hard] [--force]

- **설명**

	Job/Instance 종료

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|X|
	|--soft|프로세스만 종료|X|
	|--hard|VM 종료|X|
	|--force|수행중 발생하는 에러 무시|X|


<div id='restart'/>
### ***restart***

- **기본 Syntax**

		$ bosh restart <job> [<index>] [--force]

- **설명**

	Job/Instance (Soft stop+start) 재시작

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|X|
	|--force|수행중 발생하는 에러 무시|X|

<div id='recreate'/>
### ***recreate***

- **기본 Syntax**

		$ bosh recreate <job> [<index>] [--force]

- **설명**

	Job/Instance (Soft stop+start) 재생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|X|
	|--force|수행중 발생하는 에러 무시|X|

	
<div id='88888'/>
## BOSH CLI - User

<div id='create-user'/>
### ***create user***

- **기본 Syntax**

		$ bosh create user [<username>] [<password>]

- **설명**

	BOSH 사용자 등록

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|username|사용자 아이디|X|
	|password|사용자 비밀번호|X|

<div id='delete-user'/>
### ***delete user***

- **기본 Syntax**

		$ bosh delete user [<username>]

- **설명**

	BOSH 사용자 삭제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|username|사용자 아이디|X|

<div id='login'/>
### ***login*** 

- **기본 Syntax**

		$ bosh login [<username>] [<password>]

- **설명**

	BOSH 사용자 등록

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|username|사용자 아이디|X|
	|password|사용자 비밀번호|X|

<div id='logout'/>
### ***logout***

- **기본 Syntax**

		$ bosh logout

- **설명**

	BOSH 로그 아웃

- **파라미터**

	없음

<div id='99999'/>
## BOSH CLI - Task

<div id='task'/>
### ***task***

- **기본 Syntax**

		$ bosh task [<task_id>] [--event] [--cpi] [--debug] [--result] [--raw] [--no-filter]

- **설명**

	BOSH Task 수행 로그 출력

- **파라미터**
	
	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|task_id|Task ID|O|
	|--event|Event Log 출력|X|
	|--cpi|CPI Log 출력|X|
	|--debug|Debug Log 출력|X|
	|--result|Result Log 출력|X|
	|--raw|raw 로그 출력|X|
	|--no-filter|모든 Task 유형(ssh, logs, vms, etc) 포함해서 로그 출력|X|

<div id='tasks'/>
### ***tasks***

- **기본 Syntax**

		$ bosh tasks [--no-filter]

- **설명**

	BOSH 수행중인 Task 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--no-filter|모든 Task 유형(ssh, logs, vms, etc) 포함해서 로그 출력|X|

<div id='tasks-recent'/>
### ***tasks recent***

- **기본 Syntax**

		$ bosh tasks recent [<count>] [--no-filter]

- **설명**

	수행 완료한 Task 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|count|Task ID|O|
	|--no-filter|모든 Task 유형(ssh, logs, vms, etc) 포함해서 출력|X|

<div id='cancel-task'/>
### ***cancel task***

- **기본 Syntax**

		$ bosh cancel task <task_id>

- **설명**

	수행중인 Task 취소

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|task_id|수행중인 Task ID|O|

<div id='aaaaa'/>
## BOSH CLI - Property

<div id='set-property'/>
### ***set property***

- **기본 Syntax**

		$ bosh set property <name> <value>

- **설명**

	Deployment Property 설정

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|Property 이름|O|
	|value|Property 값|O|

<div id='get-property'/>
###  ***get property***

- **기본 Syntax**

		$ bosh get property <name>

- **설명**

	Deployment Property 확인

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|Property 이름|O|

<div id='properties'/>
### ***properties***

- **기본 Syntax**

		$ bosh properties [--terse]

- **설명**

	Deployment Property 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--terse|간결한 형식으로 출력X|

<div id='unset-property'/>
### ***unset property***

- **기본 Syntax**

		$ bosh unset property <name>

- **설명**

	Deployment Property 설정 해제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|Property 이름|O|

<div id='bbbbb'/>
## BOSH CLI - Log

<div id='logs'/>
### ***logs***

- **기본 Syntax**

		$ bosh logs <job> [<index>] [--agent] [--job] [--only filter1,filter2,...] [--dir destination_directory] [--all]

- **설명**

	BOSH에서 배포된 VM에서 Agent나 Job 수행 로그 다운로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|X|
	|--agent|Agent 로그 출력|X|
	|--job|Job 로그 출력|X|
	|--only filter1,filter2|지정된 Filter에 대한 로그만 출력|X|
	|--dir destination_directory|로그 다운로드 경로 지정|X|
	|--all|Deprecated|X|

<div id='ccccc'/>
## BOSH CLI - Maintenance

<div id='cleanup'/>
### ***cleanup***

- **기본 Syntax**

		$ bosh cleanup [--all]

- **설명**

	사용되지 않은 오래된(2일) release와 stemcell 삭제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--all|사용되지 않은 Release와 Stemcell 모두 삭제|X|

<div id='cloudcheck'/>
### ***cloudcheck***

- **기본 Syntax**

		$ bosh cloudcheck [<deployment_name>] [--auto] [--report]

- **설명**

	배포 Manifest기준으로 차이가 있는 문제 검출 및 해결

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|deployment_name|배포 이름|X|
	|--auto|자동으로 Problem 해결|X|
	|--report|Report 생성|X|

<div id='ddddd'/>
## BOSH CLI – Remote Access

<div id='ssh'/>
### ***ssh***

- **기본 Syntax**

		$ bosh ssh [--public_key FILE] [--gateway_host HOST] [--gateway_user USER] [--gateway_identity_file FILE] [--default_password PASSWORD]

- **설명**

	배포된 VM ssh 세션 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--public_key FILE|인증서 파일 설정|X|
	|--gateway_host HOST|Gateway Host IP|X|
	|--gateway_user USER|Gateway User|X|
	|--gateway_identity_file FILE|Gateway Identity File|X|
	|--default_password PASSWORD|Default 비밀번호|X|

<div id='scp'/>
### ***scp***

- **기본 Syntax**

		$ bosh scp <job> [--index][--download] [--upload] [--public_key FILE] [--gateway_host HOST] [--gateway_user USER] [--gateway_identity_file FILE]

- **설명**

	배포된 VM으로부터 파일 다운로드 또는 업로드

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|--download|다운로드할 파일|X|
	|--upload|업로드할 파일|X|
	|--public_key FILE|인증서 파일 설정|X|
	|--gateway_host HOST|Gateway Host IP|X|
	|--gateway_user USER|Gateway User|X|
	|--gateway_identity_file FILE|Gateway Identity File|X|

<div id='eeeee'/>
## BOSH CLI - Blob

<div id='upload-blobs'/>
### ***upload blobs***

- **기본 Syntax**

		$ bosh upload blobs

- **설명**

	blobstore에 blob 업로드

- **파라미터**

	없음

<div id='add-blob'/>
### ***add blob*** 

- **기본 Syntax**

		$ bosh add blob <local_path> [<blob_dir>]

- **설명**

	Blobstore에 로컬 blob 추가

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|local_path|로컬 Blob디렉토리|O|
	|blob_dir|Release내 blob 디렉토리|X|

<div id='sync-blobs'/>
### ***sync blobs***

- **기본 Syntax**

		$ bosh sync blobs

- **설명**

	blobstore의 blob 동기화

- **파라미터**

	없음

<div id='blobs'/>
### ***blobs***

- **기본 Syntax**

		$ bosh blobs

- **설명**

	현재 blob 상태 출력

- **파라미터**

	없음

<div id='fffff'/>
## BOSH CLI - Snapshot

<div id='take-snapshot'/>
### ***take snapshot***

- **기본 Syntax**

		$ bosh take snapshot [<job>] [<index>]

- **설명**

	Snapshot 생성

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|O|
	|index|Job 번호|O|

<div id='delete-snapshot'/>
### ***delete snapshot***

- **기본 Syntax**

		$ bosh delete snapshot <snapshot_cid>

- **설명**

	지정된 Snapshot CID의 Snapshot 삭제

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|snapshot_cid|snapshot_cid|O|

<div id='delete-snapshots'/>
### ***delete snapshots***

- **기본 Syntax**

		$ bosh delete snapshots

- **설명**

	설정된 배포내의 모든 Snapshot 삭제

- **파라미터**

	없음

<div id='snapshots'/>
### ***snapshots***

- **기본 Syntax**

		$ bosh snapshots [<job>] [<index>]

- **설명**

	모든 Snapshot 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|X|
	|index|Job 번호|X|

<div id='ggggg'/>
## BOSH CLI - Misc

<div id='status'/>
### ***status***

- **기본 Syntax**

		$ bosh status [--uuid]

- **설명**

	BOSH 타겟 Director 설정

- **파라미터**
	
	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|--uuid|BOSH Director 등록 정보 출력|X|

<div id='target'/>
### ***target***

- **기본 Syntax**

		$ bosh target [<director_url>] [<name>] [--ca-cert FILE]

- **설명**

	BOSH 타겟 Director 설정

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|director_url|BOSH Director URL|O|
	|name|BOSH Director의 Alias 지정|X|
	|--ca-cert FILE|UAA server에서 제공된 인증서|X|

<div id='targets'/>
### ***targets***

- **기본 Syntax**

		$ bosh targets

- **설명**

	BOSH 타겟 Director 목록 출력

- **파라미터**

	없음

<div id='vms'/>
### ***vms***

- **기본 Syntax**

		$ bosh vms [<deployment_name>] [--details] [--dns] [--vitals]

- **설명**

	BOSH에서 배포된 VM 목록 출력

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|deployment_name|배포이름|X|
	|--details|VM 상세정보 출력|X|
	|--dns|DNS 레코드 포함 출력|X|
	|--vitals|VM 자원 사용 상태 출력|X|

<div id='locks'/>
### ***locks***

- **기본 Syntax**

		$ bosh locks

- **설명**

	Lock된 VM 목록 출력

- **파라미터**

	없음

<div id='alias'/>
### ***alias***

- **기본 Syntax**

		$ bosh alias <name> <command>

- **설명**

	BOSH 커맨드 Alias 등록

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|name|Alias 이름|O|
	|command|등록할 BOSH Command|O|

<div id='aliases'/>
### ***aliases***

- **기본 Syntax**

		$ bosh aliases

- **설명**

	등록된 Alias 목록 출력

- **파라미터**

	없음

<div id='export-compiled_package'/>
### ***export compiled_package***

- **기본 Syntax**

		$ bosh export compiled_packages <release> <stemcell> <download_dir>

- **설명**

	compiled Package 내보내기

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|release|Release 이름과 버전|O|
	|stemcell|Stemcell 이름과 버전|O|
	|download_dir|내보내기 디렉토리|O|

<div id='vm-resurrection'/>
### ***vm resurrection***

- **기본 Syntax**

		$ bosh vm resurrection [<job>] [<index>] <new_state>

- **설명**

	배포된 VM의 재시작 여부 설정

- **파라미터**

	|**파라미터 명**|**설명**|**필수****(O/X)**|
	|----------|-------------------------|--------------------------------|
	|job|Job 이름|X|
	|index|Job 번호|X|
	|new_state|‘on’ or ‘off’, ‘yes’ or ‘no’, ‘enable’ or ‘disable’|O|

