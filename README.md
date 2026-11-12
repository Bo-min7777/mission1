# 🚀 개발 워크스테이션 구축 미션

## 1. 프로젝트 개요

개발은 코드를 작성하는 순간이 아니라, 환경을 세팅하는 순간부터 시작됩니다.  
본 프로젝트는 윈도우 WSL2 환경에서 리눅스 CLI, Docker, Git/GitHub를 직접 손으로 세팅하고,  
재현 가능한 개발 환경을 구축하는 것을 목표로 합니다.

**핵심 도구**
- WSL2 (Ubuntu) — 리눅스 터미널 환경
- Docker Desktop — 컨테이너 실행 및 관리
- Git / GitHub — 버전 관리 및 협업 플랫폼
- VS Code — 코드 에디터 및 GitHub 연동

---

## 2. 실행 환경

| 항목 | 내용 |
|------|------|
| OS | Windows 11 / WSL2 Ubuntu 22.04 LTS |
| Shell | Bash |
| Terminal | Windows Terminal |
| Docker Version | (실습 후 입력: `docker --version` 결과) |
| Git Version | (실습 후 입력: `git --version` 결과) |

---

## 3. 수행 항목 체크리스트

- [ ] 터미널 기본 조작 (pwd, ls, mkdir, cd, cp, mv, rm, touch, cat)
- [ ] 절대 경로 / 상대 경로 개념 정리
- [ ] 파일 및 디렉토리 권한 확인 및 변경 (chmod, ls -l)
- [ ] Docker 설치 및 기본 점검 (docker --version, docker info)
- [ ] Docker 기본 운영 명령 (images, ps, ps -a, logs, stats)
- [ ] hello-world 컨테이너 실행
- [ ] ubuntu 컨테이너 실행 및 내부 진입
- [ ] Dockerfile 기반 커스텀 웹 서버 이미지 제작
- [ ] 포트 매핑 및 브라우저 접속 확인
- [ ] 바인드 마운트 변경 반영 확인
- [ ] Docker 볼륨 생성 및 영속성 검증
- [ ] Git 사용자 정보 및 기본 브랜치 설정
- [ ] GitHub 저장소 생성 및 VS Code 연동

---

## 4. 터미널 조작 로그

### 4-1. 현재 위치 및 목록 확인

```bash
# 현재 위치 확인
pwd
# 출력 결과 (실습 후 입력)

# 숨김 파일 포함 목록 확인
ls -al
# 출력 결과 (실습 후 입력)
```

### 4-2. 디렉토리 생성 및 이동

```bash
# 작업 디렉토리 생성
mkdir workspace
cd workspace
pwd
# 출력 결과 (실습 후 입력)
```

### 4-3. 파일 생성, 내용 확인, 복사, 이동, 삭제

```bash
# 빈 파일 생성
touch mission.txt

# 파일 내용 작성 및 확인
echo "Hello Docker" > mission.txt
cat mission.txt
# 출력 결과 (실습 후 입력)

# 파일 복사
cp mission.txt backup.txt
ls -l
# 출력 결과 (실습 후 입력)

# 파일 이름 변경
mv backup.txt old_mission.txt
ls -l
# 출력 결과 (실습 후 입력)

# 파일 삭제
rm old_mission.txt
ls -l
# 출력 결과 (실습 후 입력)
```

---

## 5. 절대 경로 vs 상대 경로

| 구분 | 설명 | 예시 |
|------|------|------|
| 절대 경로 | 루트(/)부터 시작하는 전체 경로. 어디서 실행해도 동일한 위치를 가리킴 | `/home/user/workspace/mission.txt` |
| 상대 경로 | 현재 위치를 기준으로 하는 경로. 현재 위치에 따라 달라짐 | `./mission.txt` 또는 `../workspace` |

```bash
# 절대 경로로 이동
cd /home/user/workspace

# 상대 경로로 상위 폴더 이동
cd ..

# 현재 폴더(.) 기준 파일 확인
cat ./mission.txt
```

---

## 6. 파일 권한 실습 로그

### 6-1. 권한 표기 해석

```
-rwxr-xr-x
 ↑↑↑↑↑↑↑↑↑
 │└──┘└──┘└──┘
 │ 소유자  그룹  기타
 └ 파일 종류 (- : 파일, d : 디렉토리)

r = 읽기(4), w = 쓰기(2), x = 실행(1)
755 = rwxr-xr-x (소유자: 7=rwx, 그룹: 5=r-x, 기타: 5=r-x)
644 = rw-r--r-- (소유자: 6=rw-, 그룹: 4=r--, 기타: 4=r--)
```

### 6-2. 파일 권한 변경 실습

```bash
# 변경 전 권한 확인
ls -l mission.txt
# 출력 결과 (실습 후 입력)

# 755로 변경
chmod 755 mission.txt
ls -l mission.txt
# 출력 결과 (실습 후 입력)

# 644로 변경
chmod 644 mission.txt
ls -l mission.txt
# 출력 결과 (실습 후 입력)
```

### 6-3. 디렉토리 권한 변경 실습

```bash
# 변경 전 권한 확인
ls -ld workspace
# 출력 결과 (실습 후 입력)

# 700으로 변경 (나만 접근 가능)
chmod 700 workspace
ls -ld workspace
# 출력 결과 (실습 후 입력)

# 755로 복구
chmod 755 workspace
ls -ld workspace
# 출력 결과 (실습 후 입력)
```

---

## 7. Docker 설치 및 기본 점검

### 7-1. 버전 및 데몬 확인

```bash
docker --version
# 출력 결과 (실습 후 입력)

docker info
# 출력 결과 (실습 후 입력)
```

### 7-2. hello-world 컨테이너 실행

```bash
docker run hello-world
# 출력 결과 (실습 후 입력)
```

### 7-3. ubuntu 컨테이너 실행 및 내부 진입

```bash
# ubuntu 컨테이너 실행 및 bash 진입
docker run -it ubuntu bash

# 컨테이너 내부에서 실행
ls
echo "I am inside the container"
exit
# 출력 결과 (실습 후 입력)
```

> **attach vs exec 차이 정리**  
> - `docker attach`: 실행 중인 컨테이너의 메인 프로세스에 붙음. exit 시 컨테이너가 종료됨  
> - `docker exec -it [컨테이너명] bash`: 실행 중인 컨테이너에 새 프로세스로 접속. exit 해도 컨테이너 유지됨

---

## 8. Docker 기본 운영 명령 로그

```bash
# 이미지 목록 확인
docker images
# 출력 결과 (실습 후 입력)

# 실행 중인 컨테이너 확인
docker ps
# 출력 결과 (실습 후 입력)

# 전체 컨테이너 확인 (종료된 것 포함)
docker ps -a
# 출력 결과 (실습 후 입력)

# 컨테이너 로그 확인
docker logs [컨테이너명 또는 ID]
# 출력 결과 (실습 후 입력)

# 리소스 사용량 확인
docker stats --no-stream
# 출력 결과 (실습 후 입력)
```

---

## 9. Dockerfile 기반 커스텀 웹 서버 제작

### 9-1. 프로젝트 구조

```
my-web-server/
├── Dockerfile
└── app/
    └── index.html
```

### 9-2. index.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>My Docker Web Server</title>
</head>
<body>
  <h1>🚀 Docker 커스텀 웹 서버 동작 중!</h1>
  <p>WSL2 + Docker Desktop 환경에서 실행 중입니다.</p>
</body>
</html>
```

### 9-3. Dockerfile

```dockerfile
# 베이스 이미지: Nginx 공식 이미지
FROM nginx:latest

# 커스텀 HTML 파일을 Nginx 기본 경로에 복사
COPY ./app/index.html /usr/share/nginx/html/index.html

# 컨테이너가 사용할 포트 명시
EXPOSE 80
```

> **커스텀 포인트 설명**  
> - `FROM nginx:latest`: 이미 검증된 Nginx 이미지를 베이스로 사용하여 웹 서버 설정 생략  
> - `COPY`: 호스트의 정적 파일을 이미지 내부로 복사하여 컨테이너 어디서든 동일한 파일 제공  
> - `EXPOSE 80`: 컨테이너가 80번 포트를 사용함을 명시 (포트 매핑 시 기준점)

### 9-4. 빌드 및 실행

```bash
# 이미지 빌드
docker build -t my-web-server .
# 출력 결과 (실습 후 입력)

# 포트 매핑으로 컨테이너 실행
docker run -d -p 8080:80 --name web-container my-web-server
# 출력 결과 (실습 후 입력)

# 실행 확인
docker ps
# 출력 결과 (실습 후 입력)
```

---

## 10. 포트 매핑 접속 확인

> **포트 매핑이 필요한 이유**  
> Docker 컨테이너는 기본적으로 외부와 격리된 네트워크 환경에서 실행됩니다.  
> `-p 8080:80` 옵션은 호스트의 8080번 포트를 컨테이너의 80번 포트에 연결하여,  
> 외부(브라우저)에서 컨테이너 내부 서비스에 접근할 수 있도록 통로를 열어줍니다.

- **접속 주소**: `http://localhost:8080`
- **접속 증거**: (브라우저 스크린샷 첨부 또는 아래 curl 결과 입력)

```bash
curl http://localhost:8080
# 출력 결과 (실습 후 입력)
```

---

## 11. 바인드 마운트 변경 반영 확인

```bash
# 바인드 마운트로 컨테이너 실행
docker run -d -p 8081:80 \
  --name bind-container \
  -v $(pwd)/app:/usr/share/nginx/html \
  nginx:latest
# 출력 결과 (실습 후 입력)
```

**변경 전**: `http://localhost:8081` 접속 화면 (스크린샷 첨부)

```bash
# 호스트에서 index.html 내용 수정
echo "<h1>바인드 마운트 반영 확인!</h1>" > ./app/index.html
```

**변경 후**: `http://localhost:8081` 새로고침 화면 (스크린샷 첨부)

> 호스트 파일을 수정하면 컨테이너 재시작 없이 즉시 반영됨을 확인

---

## 12. Docker 볼륨 영속성 검증

```bash
# 볼륨 생성
docker volume create my-volume
# 출력 결과 (실습 후 입력)

# 볼륨 연결하여 컨테이너 실행 및 데이터 쓰기
docker run -it --rm \
  -v my-volume:/data \
  ubuntu bash -c "echo '볼륨 데이터 저장' > /data/test.txt && cat /data/test.txt"
# 출력 결과 (실습 후 입력)

# 컨테이너 삭제 후 새 컨테이너에서 데이터 확인
docker run -it --rm \
  -v my-volume:/data \
  ubuntu bash -c "cat /data/test.txt"
# 출력 결과 (실습 후 입력)
```

> **Docker 볼륨의 필요성**  
> 컨테이너는 삭제되면 내부 데이터도 함께 사라집니다.  
> Docker 볼륨은 컨테이너 외부(호스트)에 데이터를 저장하므로,  
> 컨테이너가 삭제되어도 데이터가 유지됩니다.

---

## 13. Git 설정 및 GitHub 연동

### 13-1. Git 사용자 정보 설정

```bash
# 사용자 이름 설정
git config --global user.name "홍길동"

# 이메일 설정
git config --global user.email "example@email.com"

# 기본 브랜치 이름 설정
git config --global init.defaultBranch main

# 설정 확인
git config --list
# 출력 결과 (실습 후 입력 / 이메일·토큰 등 민감정보는 *** 로 마스킹)
```

### 13-2. 로컬 저장소 초기화 및 첫 커밋

```bash
# 작업 디렉토리로 이동
cd ~/workspace

# Git 저장소 초기화
git init
# 출력 결과 (실습 후 입력)

# 파일 스테이징
git add .

# 첫 커밋
git commit -m "feat: 개발 워크스테이션 구축 미션 초기 커밋"
# 출력 결과 (실습 후 입력)

# 커밋 로그 확인
git log --oneline
# 출력 결과 (실습 후 입력)
```

### 13-3. GitHub 원격 저장소 연동

```bash
# 원격 저장소 연결
git remote add origin https://github.com/사용자명/저장소명.git

# 연결 확인
git remote -v
# 출력 결과 (실습 후 입력)

# 원격 저장소에 푸시
git push -u origin main
# 출력 결과 (실습 후 입력)
```

> **VS Code와 GitHub 연동 확인**  
> VS Code 좌측 사이드바 → 소스 제어(Source Control) 아이콘 클릭 후  
> 저장소가 정상 연결되었는지 확인 (스크린샷 첨부)

---

## 14. 트러블슈팅

### Case 1: (문제 상황 제목 입력)

| 항목 | 내용 |
|------|------|
| 문제 | (예: WSL2 터미널에서 docker 명령어 실행 시 오류 발생) |
| 오류 메시지 | (터미널 출력 내용 입력) |
| 원인 | (예: Docker Desktop WSL Integration 설정이 비활성화 상태) |
| 해결 방법 | (예: Docker Desktop → Settings → Resources → WSL Integration → Ubuntu 활성화 후 Apply & Restart) |
| 결과 | (해결 후 정상 출력 내용 입력) |

```bash
# 문제 재현 명령어
(입력)

# 해결 후 정상 실행 명령어 및 결과
(입력)
```

### Case 2: (문제 상황 제목 입력)

| 항목 | 내용 |
|------|------|
| 문제 | (예: 포트 매핑 후 브라우저에서 localhost:8080 접속 불가) |
| 오류 메시지 | (터미널 출력 내용 입력) |
| 원인 | (예: 호스트 80번 포트가 이미 다른 프로세스에 의해 점유됨) |
| 해결 방법 | (예: 호스트 포트를 8080으로 변경하여 docker run -p 8080:80 으로 재실행) |
| 결과 | (해결 후 정상 출력 내용 입력) |

```bash
# 문제 재현 명령어
(입력)

# 해결 후 정상 실행 명령어 및 결과
(입력)
```

---

## 15. 학습 요약 및 회고

### 절대 경로 vs 상대 경로
> (본인의 말로 작성)  
> 예시: 절대 경로는 지도에서 "서울특별시 종로구 ○○로 1번지"처럼 어디서든 동일한 위치를 가리키고,  
> 상대 경로는 "여기서 두 블록 앞"처럼 현재 내 위치를 기준으로 달라집니다.

### 파일 권한 (r/w/x, 755/644) 이해
> (본인의 말로 작성)  
> 예시: r(읽기)=4, w(쓰기)=2, x(실행)=1 을 더해서 표현합니다.  
> 755는 소유자는 모든 권한(7=rwx), 그룹과 기타 사용자는 읽기·실행만 가능(5=r-x)을 의미합니다.  
> 644는 소유자만 쓰기 가능(6=rw-), 나머지는 읽기만 가능(4=r--)합니다.

### Docker 볼륨의 필요성
> (본인의 말로 작성)  
> 예시: 컨테이너는 삭제하면 내부 데이터도 함께 사라집니다.  
> 볼륨을 사용하면 데이터를 컨테이너 바깥에 보관하므로,  
> 컨테이너를 지우고 새로 만들어도 데이터가 그대로 유지됩니다.

### 전체 미션 회고
> (본인의 말로 자유롭게 작성)  
> - 가장 어려웠던 부분:  
> - 새롭게 알게 된 점:  
> - 다음에 더 공부하고 싶은 것: