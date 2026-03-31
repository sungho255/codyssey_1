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


## 6. Docker 설치 및 기본 점검

## 7. Docker 기본 운영 명령 수행

## 8. 컨테이너 실행 실습

## 9. 기존 Docker 기반 커스텀 이미지 제작

## 10. 포트 매핑 및 접속 증거

## 11. Docker 볼륨 영속성 검증

## 12. Git 설정 및 Github 연동

## 13. 보안 및 개인정보 보호










