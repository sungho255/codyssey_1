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


## 10. 포트 매핑 및 접속 증거

## 11. Docker 볼륨 영속성 검증

## 12. Git 설정 및 Github 연동

## 13. 보안 및 개인정보 보호

