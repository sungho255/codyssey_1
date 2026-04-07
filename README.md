# CodySsey Mission 1-1

## 1. 프로젝트 개요
- 터미널, Docker, Git을 직접 세팅하여 "내 컴퓨터에서만 동작하는" 문제를 해결하고 누구에게나 동일하게 실행되는 재현 가능한 표준 개발 환경을 구축합니다.
- 환경 격리, 네트워크 포트 매핑, 데이터 영속성 등 인프라의 구조적 원리를 실습으로 직접 검증하며 설계 역량을 체득합니다.
- 이 과정에서 쌓은 기초는 단순 세팅을 넘어 리눅스 트러블슈팅, CI/CD, 클라우드 운영 등 고급 엔지니어링 단계로 확장되는 핵심 토대가 됩니다.

## 2. 실행 환경
```
- OS : macOS 15.7.4
- Shell : Zsh
- Docker : 28.5.2
- Git : 2.53
```

## 3. 수행 항목 체크리스트
- [ ] 터미널 기본 조작 및 폴더 구성
- [ ] 권한 변경 실습
- [ ] Docker 설치/점검
- [ ] hello-world 실행
- [ ] Dockerfile 빌드/실행
- [ ] 포트 매핑 접속(2회)
- [ ] 바인드 마운트 반영
- [ ] 볼륨 영속성
- [ ] Git 설정 + GitHub 연동

## 4. 터미널 조작 로그 기록
```
# 현재 위치 확인
$ pwd
/Users/kb0534045653

# 목록 확인
$ ls -a
.			.ssh			Movies
..			.vscode			Music
.CFUserTextEncoding	.zsh_sessions		OrbStack
.DS_Store		Desktop			Pictures
.Trash			Documents		Public
.docker			Downloads
.orbstack		Library

# 이동
$ cd Desktop
kb0534045653@c4r4s7 Desktop %

# 생성
# 1. 파일 생성
$ touch test.txt
$ ls
Desktop		Downloads	Movies		OrbStack	Public
Documents	Library		Music		Pictures	test.txt

# 2. 폴더 생성
$ mkdir testdir
$ ls
Desktop		Library		OrbStack	 test.txt  Documents	Movies
Pictures	testdir   Downloads	 Music		 Public

# 복사
$ cp test.txt copiedtext.txt
$ ls
Desktop		Library		OrbStack	copiedtext.txt
Documents	Movies		Pictures	test.txt
Downloads	Music		Public		testdir

# 이동/이름 변경
$ mv test.txt Desktop/renamed_test.txt
$ cd Descktop
$ ls
codyssey_1		renamed_test.txt	test.txt

# 삭제
$ rm renamed_test.txt
$ ls
codyssey_1	test.txt

# 파일 내용 확인, 빈 파일 생성
$ touch test.txt
$ echo "Hello World" > test.txt
$ cat test.txt
Hello World
```
### 절대 경로 / 상대 경로
| 구분 | 절대 경로 (Absolute) |	상대 경로 (Relative) |
| :--- | :---: | :---: |
| 기점	| 프로젝트 루트 (Project Root) |	현재 파일 위치 (Current File) |
| 가독성 |	명확함 (어디서 왔는지 한눈에 보임) |	위치에 따라 복잡해짐 (../../..) |
| 이식성 |	환경 설정(Path)에 의존함 |	폴더 구조만 유지되면 어디서든 작동 |
| 권장 상황 |	대규모 프로젝트, 공통 모듈 |	작은 단위의 기능 모듈, 라이브러리 배포 |

### 1. 절대 경로
- **외부 라이브러리 사용 시:**

  pip나 npm으로 설치한 패키지는 무조건 절대 경로로 가져옵니다.
- **대규모 프로젝트의 핵심 모듈:**

  프로젝트 어디서든 공통으로 쓰이는 utils, models, config 등은 절대 경로를 써서 소속을 명확히 합니다.
- **가독성 확보:**

  파일이 깊은 폴더(src/a/b/c/d/file.ts)에 있을 때, 상위로 여러 번 올라가는(../../../../) 복잡함을 피하기 위해 사용합니다.

### 2. 상대 경로
- **밀접하게 연관된 컴포넌트 간:**

  특정 기능을 구현하기 위해 쪼개놓은 작은 파일들끼리는 상대 경로가 직관적입니다.

- **패키지 배포 시:**

  내가 만든 라이브러리를 다른 사람이 어디에 설치할지 알 수 없으므로 내부 모듈 간의 연결은 상대 경로로 고정해야 이식성이 유지됩니다.

- **빠른 프로토타이핑:**

  설정(tsconfig.json이나 sys.path)을 건드리지 않고 바로 옆의 파일을 가져올 때 편리합니다.

## 5. 권한 실습 및 증거 기록
| 권한 | 기호 | 값 | 설명 |
| :--- | :---: | :---: | :--- |
| **Read** | `r` | **4** | 파일 읽기, 디렉토리 목록 확인 |
| **Write** | `w` | **2** | 파일 수정, 디렉토리 내 파일 생성/삭제 |
| **Execute** | `x` | **1** | 파일 실행, 디렉토리 접근 (`cd`) |
| **None** | `-` | **0** | 권한 없음 |


### 5-1. 파일
```
# 권한 변경 전 
$ touch my_script.sh
$ ls -l my_script.sh
-rw-r--r--  1 kb0534045653  kb0534045653  0 Apr  1 13:56 my_script.sh

# 권한 변경 후
$ chmod 755 my_script.sh
$ ls -l my_script.sh
-rwxr-xr-x  1 kb0534045653  kb0534045653  0 Apr  1 13:56 my_script.sh
```

### 5-2. 폴더
```
# 권한 변경 전 
$ mkdir my_dir
$ ls -ld my_dir
drwxr-xr-x  2 kb0534045653  kb0534045653  64 Apr  1 14:07 my_dir

# 권한 변경 후
$ chmod 775 my_dir 
$ ls -ld my_dir
drwxrwxr-x  2 kb0534045653  kb0534045653  64 Apr  1 14:07 my_dir
```


## 6. Docker 설치 및 기본 점검
```
# Docker 버전 확인 결과를 기록한다.
$ docker --version
Docker version 28.5.2, build ecc6942

# Docker 데몬 동작 여부 확인 결과를 기록한다.
$ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/kb0534045653/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/kb0534045653/.docker/cli-plugins/docker-compose
...

```


## 7. Docker 기본 운영 명령 수행
```
# 이미지: 다운로드/목록 확인
$ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

# 컨테이너: 실행/중지/목록 확인
$ docker ps
$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 운영: 로그 확인
$ docker logs
docker: 'docker logs' requires 1 argument

Usage:  docker logs [OPTIONS] CONTAINER

# 리소스 확인
$ docker stats
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS

```

### 컨테이너가 갑자기 종료되는 경우( docker images나 docker ps 에서 확인이 안될 때)

-> docker ps -a 을 사용하여 컨테이너가 언제 죽었는지를 확인할 수가 있다.

그렇게 해서 알아낸 컨테이너의 아이디를 파악한 후
```
$ docker logs [ID]
```

위 명령어를 통해 실제 에러 원인을 확인 할 수 있다



## 8. 컨테이너 실행 실습
### Docker run 옵션 
| 축약형 (Short) | 풀 네임 (Long-form) | 역할 및 설명 |
| :---: | :--- | :--- |
| **`-d`** | `--detach` | 컨테이너를 백그라운드 모드로 실행 |
| **`-p`** | `--publish` | 호스트와 컨테이너의 포트를 매핑 (예: 8080:80) |
| **`-it`** | `-i --tty` | 컨테이너 내부 터미널로 대화형 접속 |
| **`-v`** | `--volume` | 호스트와 컨테이너 간의 데이터 공유 (마운트) |
| **`-e`** | `--env` | 환경 변수 설정 |
| **(없음)** | **`--name`** | 컨테이너 이름 지정 (축약형 없음) |
| **(없음)** | **`--rm`** | 컨테이너 종료 시 자동으로 삭제 (축약형 없음) |

```
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
```
로컬에 hello-world라는 이미지가 없으므로 docker hub에서 hello-world라는 이미지를 가져온다 

```
# hello-world 실행 성공을 기록한다.
$ docker run -it ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
817807f3c64e: Pull complete 
Digest: sha256:186072bba1b2f436cbb91ef2567abca677337cfc786c86e107d25b7072feef0c
Status: Downloaded newer image for ubuntu:latest

root@4948d5041e87:/#

# ubuntu 컨테이너를 실행하고 내부 진입 후 간단 명령(예: ls , echo ) 수행 결과를 기록
$ root@4948d5041e87:/# ls -l
total 16
lrwxrwxrwx   1 root root   7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   1 root root   0 Apr 22  2024 boot
drwxr-xr-x   5 root root 340 Apr  1 05:57 dev
drwxr-xr-x   1 root root  56 Apr  1 05:57 etc
drwxr-xr-x   1 root root  12 Feb 17 02:09 home
lrwxrwxrwx   1 root root   7 Apr 22  2024 lib -> usr/lib
lrwxrwxrwx   1 root root   9 Apr 22  2024 lib64 -> usr/lib64
drwxr-xr-x   1 root root   0 Feb 17 02:02 media
drwxr-xr-x   1 root root   0 Feb 17 02:02 mnt
drwxr-xr-x   1 root root   0 Feb 17 02:02 opt
dr-xr-xr-x 230 root root   0 Apr  1 05:57 proc
drwx------   1 root root  30 Feb 17 02:09 root
drwxr-xr-x   1 root root  22 Feb 17 02:09 run
lrwxrwxrwx   1 root root   8 Apr 22  2024 sbin -> usr/sbin
drwxr-xr-x   1 root root   0 Feb 17 02:02 srv
dr-xr-xr-x  11 root root   0 Apr  1 05:57 sys
drwxrwxrwt   1 root root   0 Feb 17 02:09 tmp
drwxr-xr-x   1 root root  10 Feb 17 02:02 usr
drwxr-xr-x   1 root root  90 Feb 17 02:09 var
 
$ echo "Hello from Ubuntu Container"
Hello from Ubuntu Container

# 컨테이너 종료/유지(attach/exec 등)의 차이를 스스로 관찰하고 간단히 정리한다.
# 1. EXEC
# 기존 터미널
$ docker docker run -it ubuntu
root@f216c3455a97:/# echo $$
1

$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
4948d5041e87   ubuntu    "bash"    30 minutes ago   Up 30 minutes             frosty_northcutt

# 새 터미널
$ docker exec -it frosy_northcutt bash
root@f216c3455a97:/# echo $$
12

# 2. ATTACH
# 새 터미널 
$ docker attach frosy_northcutt
root@4948d5041e87:/# echo $$
1

```

### exec VS attach

| 항목 | `docker attach` | `docker exec` |
| :--- | :---: | :---: |
| **PCB 개수** | **1개 (공유)** | **2개 이상 (독립)** |
| **변수 수정** | **원본 직접 수정** | **내 프로세스 복사본만 수정** |
| **비유** | **하나의 칠판에 같이 적기** | **각자의 공책에 따로 적기** |

### run VS start

| 항목 | `docker run` | `docker start` |
| :--- | :--- | :--- |
| **기본 정의** | **Create + Start** (생성 후 즉시 실행) | **Restart** (중지된 컨테이너 재시작) |
| **대상 (Target)** | **이미지 (Image)** | **컨테이너 (Container)** |
| **컨테이너 ID** | **매번 새로운 ID**가 생성됨 | **기존 ID**를 그대로 유지함 |

### stop VS kill

| 항목 | `docker stop` (Graceful) | `docker kill` (Forced) |
| :--- | :--- | :--- |
| **송신 시그널** | `SIGTERM` (15) → `SIGKILL` (9) | `SIGKILL` (9) |
| **종료 메커니즘** | 프로세스에게 정리(Cleanup) 시간 허용 | 즉시 프로세스 강제 종료 |
| **기본 대기 시간** | 10초 (설정 가능) | 0초 (즉시) |
| **데이터 안전성** | 높음 (DB 커넥션 종료, 파일 저장 가능) | 낮음 (데이터 유실 및 인덱스 오염 위험) |
| **종료 코드 (Exit)** | 정상 종료 시 `0`, 타임아웃 시 `137` | 항상 `137` |
| **OS 비유** | 시작 메뉴 -> '시스템 종료' 클릭 | 본체의 '전원 버튼' 길게 눌러 강제 종료 |

## 9. 기존 Docker 기반 커스텀 이미지 제작

### 커스텀 Dockerfile
```
FROM nginx:slim

# 1. 기존의 기본 설정 파일 삭제 (매우 중요)
RUN rm /etc/nginx/conf.d/default.conf

# 2. 사용자가 작성한 위 설정을 해당 위치로 복사
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 3. HTML 파일 복사 (파일명이 my-project.html인지 확인 필수)
COPY ./html/ /usr/share/nginx/html/

# 권한 설정 (실행 권한 문제 예방)
# RUN chmod -R 755 /usr/share/nginx/html
```

### 커스텀 포인트
### 1. 정적 콘텐츠
```
<h1>Hello World!</h1>
```
### 2. 결과 스크린샷
<img src="./img/CustomImageResult.jpg"/>


### 명령어
| 지시어 | 설명 | 비고 (Best Practice & Engineering Insight) |
| :--- | :--- | :--- |
| **FROM** | 베이스 이미지를 지정 | `alpine`이나 `slim` 버전을 사용하여 이미지 크기를 최소화하고, 공격 표면(Attack Surface)을 줄여 보안성을 확보 |
| **WORKDIR** | 컨테이너 내 작업 디렉토리 설정 | `mkdir` 대신 이 명령어를 사용하면 폴더가 자동 생성되며, 이후 모든 상대 경로의 기준점(Context)이 되어 경로 실수를 방지 |
| **COPY / ADD** | 호스트 파일을 컨테이너로 복사 | 레이어 캐싱 효율을 극대화하기 위해 소스 코드 전체를 복사하기 전, **의존성 파일(`package.json`, `requirements.txt` 등)**을 먼저 복사 |
| **RUN** | 빌드 시점에 실행할 명령어 | 여러 개의 `RUN`은 `&&`로 묶어 **단일 레이어**로 생성하세요. 레이어 수가 적을수록 이미지 전송 속도와 저장 효율이 비약적으로 상승 |
| **ENV** | 환경 변수 설정 | 빌드 및 런타임 환경 변수를 정의합니다. 다만, API Key와 같은 민감 정보는 `ENV` 대신 Docker Secret이나 가상화된 환경 변수 주입 방식을 권장 |
| **EXPOSE** | 문서화용 포트 개방 | 실제 호스트 포트를 개방하는 기능은 없으며 해당 컨테이너가 어떤 포트를 사용하는지 명시하는 **메타데이터(Metadata)** 역할을 수행하여 협업 효율을 높입니다. |
| **CMD / ENTRYPOINT** | 컨테이너 시작 시 실행될 명령 | **CMD**는 기본 인자를 설정하여 실행 시 덮어쓰기가 유연하고, **ENTRYPOINT**는 컨테이너를 특정 실행 파일(Executable)처럼 고정하여 사용할 때 아키텍처적으로 유리합니다. |

### alpine VS slim
| 구분 | Slim (권장) | Alpine (특수 목적) |
| :--- | :---: | :---: |
| **기반 OS** | Debian (최소화 버전) | Alpine Linux (초경량) |
| **C 라이브러리** | "glibc (표준, 고성능)" | "musl-libc (경량, 표준 준수)" |
| **이미지 크기** | 보통 (~150MB) | 매우 작음 (~50MB) |
| **패키지 도구** | apt-get | apk |
| **Python 호환성** | 최상 (대부분의 Wheel 지원) | 낮음 (대부분 직접 컴파일 필요) |
| **빌드 속도** | 빠름 (Binary 기반 설치) | 매우 느림 (Source 기반 빌드) |
| **실행 성능** | 최적화된 연산 성능 보장 | 주의 (메모리/멀티스레드 병목 가능) |
| **권장 용도** | LLM, 데이터 분석 복잡한 API | 단순 Proxy, Go/Rust 정적 바이너리 |

## 10. 포트 매핑 및 접속 증거

## 11. Docker 볼륨 영속성 검증

## 12. Git 설정 및 Github 연동

## 13. 보안 및 개인정보 보호

## 14. 트러블 슈팅

### 1. found character that cannot start any token (vi 편집기에서 YAML 파싱 에러: TAB 문자 사용 문제)
### 문제
&emsp;**현상 :** docker-compose.yml 실행 시 found chracter that cannot start any token 에러 발생

&emsp;**영향 :** 도커 컴포즈 엔진이 설정 파일을 해석(parsing)하지 못해 컨테이너 서비스 배포 중단

### 원인 가설
&emsp;- AML 표준 명세(Spec)상 들여쓰기에 탭(\t) 문자가 포함되어 파서가 이를 유효하지 않은 토큰으로 인식했을 것이다.
  
### 확인
&emsp;가설을 검증하기 위해 vi 편집기 내부에서 제어 문자를 시각화하여 전수 조사를 실시

&emsp;**검증 방법:** vi 명령 모드에서 다음 명령을 실행하여 유령 문자를 노출시킵니다.

```
:set list
```

&emsp;**판독 기준:**
  
&emsp;라인 시작 부분에 ^I 기호가 보인다면 탭 문자가 존재한다는 증거
  
&emsp;라인 끝에 $ 외에 다른 기호가 보인다면 특수문자 삽입

### 해결/대안

&emsp;i 명령 모드에서 다음 명령을 실행
```
set nocompatible    " 구형 vi와의 호환성 해제 (현대적 기능 사용)
set tabstop=4
set shiftwidth=4
set expandtab       " 탭을 공백으로 변환
set number          " 줄 번호 표시 (설정 적용 확인용)
```
