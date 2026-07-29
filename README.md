# Docker / Linux / Git 실습 과제

## 0. 프로젝트 개요

본 프로젝트는 개발 환경에서 기본적으로 필요한 Linux 터미널 조작, 파일/디렉토리 권한 관리, Docker 설치 및 컨테이너 운영, Dockerfile 기반 커스텀 이미지 제작, Docker 볼륨 영속성 검증, Git 및 GitHub 연동 과정을 실습하고 그 결과를 기록하는 것을 목표로 한다.

주요 수행 목표는 다음과 같다.

- 터미널 기본 명령어 실습
- 파일 및 디렉토리 권한 변경 실습
- Docker 설치 및 동작 확인
- Docker 이미지/컨테이너 기본 운영
- Dockerfile 기반 커스텀 이미지 제작 및 실행
- 포트 매핑 및 접속 확인
- Docker 볼륨을 이용한 데이터 영속성 검증
- Git 설정 및 GitHub 저장소 연동
- 민감정보 보호 및 기록 관리

---

## 1. 실행 환경

| 항목 | 내용 |
|---|---|
| OS | `Darwin 24.6.0` |
| Shell / Terminal | `zsh` |
| Docker Version | `28.5.2` |
| Git Version | `2.53.0` |
| 작업 디렉토리 | `/first_work` |

### 실행 환경 확인 명령

```bash
$ uname -a
출력 결과
Darwin c6r4s8.codyssey.kr 24.6.0 Darwin Kernel Version 24.6.0: Mon Jan 19 22:00:10 PST 2026; root:xnu-11417.140.69.708.3~1/RELEASE_X86_64 x86_64

$ echo $SHELL
출력 결과
/bin/zsh

$ docker --version
출력 결과
Docker version 28.5.2, build ecc6942

$ git --version
출력 결과
git version 2.53.0
```

---

## 수행 체크리스트

| 구분 | 수행 여부 |
|---|---|
| 터미널 기본 조작 | - [✅] 완료 |
| 파일/디렉토리 생성, 복사, 이동, 삭제 | - [✅] 완료 |
| 파일 내용 확인 및 빈 파일 생성 | - [✅] 완료 |
| 파일 권한 변경 실습 | - [✅] 완료 |
| 디렉토리 권한 변경 실습 | - [✅] 완료 |
| Docker 설치 및 버전 확인 | - [✅] 완료 |
| Docker 데몬 동작 확인 | - [✅] 완료 |
| Docker 이미지 다운로드 및 목록 확인 | - [✅] 완료 |
| Docker 컨테이너 실행/중지/목록 확인 | - [✅] 완료 |
| Docker 로그 확인 | - [✅] 완료 |
| Docker 리소스 확인 | - [✅] 완료 |
| hello-world 컨테이너 실행 | - [✅] 완료 |
| ubuntu 컨테이너 실행 및 내부 명령 수행 | - [✅] 완료 |
| attach / exec 차이 관찰 | - [✅] 완료 |
| Dockerfile 기반 커스텀 이미지 빌드 | - [✅] 완료 |
| 커스텀 컨테이너 실행 | - [✅] 완료 |
| 포트 매핑 및 접속 확인 | - [✅] 완료 |
| Docker 볼륨 생성 및 연결 | - [✅] 완료 |
| Docker 볼륨 영속성 검증 | - [✅] 완료 |
| Git 사용자 정보 설정 | - [✅] 완료 |
| Git 기본 브랜치 설정 | - [✅] 완료 |
| GitHub 저장소 연동 | - [✅] 완료 |
| 민감정보 마스킹 확인 | - [✅] 완료 |

---

## 2. 터미널 기본 조작 로그

### 2-1. 현재 위치 확인 - 절대경로

```bash
$ pwd
출력 결과
/Users/ajj05062587/first_work
```

### 2-2. 목록 확인 - 숨김 파일 미포함

```bash
$ ls
출력 결과
README.md
```

### 2-3. 숨김 파일 포함 목록 확인

```bash
$ ls -la
출력 결과
.               ..              .git            README.md
```

### 2-4. 디렉토리 생성

```bash
$ ls
README.md
$ mkdir test
$ ls
README.md       test
```

### 2-5. 디렉토리 이동
```bash
$ pwd
/Users/ajj05062587/first_work
$ cd test/
$ pwd
출력 결과
/Users/ajj05062587/first_work/test
```

### 2-6. 파일 생성

```bash
$ ls
README.md test
$ echo "hello world" > sample.txt
$ ls
출력 결과
README.md       sample.txt      test
```

### 2-7. 파일 내용 확인

```bash
$ cat sample.txt

출력 결과
hello world
```

### 2-8. 빈 파일 생성

```bash
$ ls
README.md       sample.txt      test
$ touch empty.txt
$ ls
출력 결과
empty.txt       README.md       sample.txt      test
```

### 2-9. 파일 복사

```bash
$ ls
empty.txt       README.md       sample.txt      test
$ cp sample.txt sample-copy.txt
$ ls
출력 결과
empty.txt       README.md       sample-copy.txt sample.txt      test
```

### 2-10. 파일 이동 / 이름 변경

```bash
파일 이동
$ ls
empty.txt       README.md       sample-copy.txt sample.txt      test
$ mv sample-copy.txt test/
$ ls
empty.txt       README.md       sample.txt      test
$ cd test
$ ls
sample-copy.txt


이름 변경
$ ls
sample-copy.txt
$ mv sample-copy.txt changed.txt
$ ls
출력 결과
changed.txt
```

### 2-11. 파일 삭제

```bash
$ pwd
/Users/ajj05062587/first_work/test
$ ls
changed.txt
$ rm changed.txt
$ ls -a
출력 결과
.       ..
```

---

## 3. 권한 실습 및 증거 기록

## 3-1. 파일 권한 변경 실습

### 변경 전 권한 확인

```bash
$ ls -l sample-copy.txt
출력 결과
-rw-r--r--  1 ajj05062587  ajj05062587  12  7 29 09:35 sample.txt
```

### 파일 권한 변경

```bash
$ chmod 755 sample.txt
```

### 변경 후 권한 확인

```bash
$ ls -l sample.txt

출력 결과
-rwxr-xr-x  1 ajj05062587  ajj05062587  12  7 29 09:35 sample.txt
```

### 파일 권한 비교

| 구분 | 권한 |
|---|---|
| 변경 전 | `-rw-r--r-- ` |
| 변경 후 | `-rwxr-xr-x` |

`chomod 755`의 의미는 파일 소유자에게는 읽기, 쓰기, 실행의 권한을 주며 그 외 사용자에게는 읽기, 쓰기만의 권한을 준다는 뜻이다.
여기서 파일의 권한은 읽기(r) 4 / 쓰기(w) 2 / 실행 (x) 1로 분류된다.
그리고 각각의 숫자는 0과 1을 조합한 권한을 의미하여 3개의 비트로 구성된다.

---

## 3-2. 디렉토리 권한 변경 실습

### 실습용 디렉토리 생성

```bash
$ mkdir permission-test
$ ls
empty.txt       permission-test README.md       sample.txt      test
```

### 변경 전 권한 확인

```bash
$ ls -l
출력 결과
total 56
-rw-r--r--  1 ajj05062587  ajj05062587      0  7 29 09:35 empty.txt
drwxr-xr-x  2 ajj05062587  ajj05062587     64  7 29 09:41 permission-test
-rw-r--r--@ 1 ajj05062587  ajj05062587  21975  7 29 09:41 README.md
-rwxr-xr-x  1 ajj05062587  ajj05062587     12  7 29 09:35 sample.txt
drwxr-xr-x  2 ajj05062587  ajj05062587     64  7 29 09:39 test
```

### 디렉토리 권한 변경

```bash
$ chmod 700 permission-test
```

### 변경 후 권한 확인

```bash
$ ls -l
total 56
-rw-r--r--  1 ajj05062587  ajj05062587      0  7 29 09:35 empty.txt
drw-r--r--  2 ajj05062587  ajj05062587     64  7 29 09:41 permission-test
-rw-r--r--@ 1 ajj05062587  ajj05062587  21975  7 29 09:41 README.md
-rwxr-xr-x  1 ajj05062587  ajj05062587     12  7 29 09:35 sample.txt
drwxr-xr-x  2 ajj05062587  ajj05062587     64  7 29 09:39 test
```

### 디렉토리 권한 비교

| 구분 | 권한 |
|---|---|
| 변경 전 | `drwxr-xr-x` |
| 변경 후 | `drw-r--r--` |

`chmod 644`는 폴더 소유자에게는 읽기, 쓰기의 권한이 부여되며, 그 외 Group, Other 사용자에게는 읽기의 권한만 부여된다.

---

## 4. Docker 설치 및 기본 점검

## 4-1. Docker 버전 확인

```bash
$ docker --version
출력 결과
Docker version 28.5.2, build ecc6942
```

## 4-2. Docker 데몬 동작 여부 확인

```bash
$ docker info
출력 결과
Docker Client Version: 28.5.2
Docker Server Version: 28.5.2
OSType: linux
Architecture: x86_64
Operating System: OrbStack
Storage Driver: overlay2
Cgroup Driver: cgroupfs
Cgroup Version: 2
```
![docker info image](/images/docker%20info.png)

확인 결과:

- Docker 데몬 동작 여부: `정상`
- 특이사항: `WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set`
    - iptable raw 테이블 사용이 비활성화된 상태이므로, 일부 네트워크 보안 규칙이 적용되지 않을 수 있음.

가설:
Docker가 실행되는 환경에서 `iptables raw` 기능을 사용할 수 없거나, 관련 환경변수가 설정되어 해당 경고가 출력된 것으로 추정

조치:
`docker info` 결과를 확인한 결과 Docker 데몬은 정상적으로 동작했으며, 컨테이너 실행 및 포트 매핑에도 문제가 없음을 확인.
따라서 실습 환경에서는 해당 경고를 기록하고 실습을 계속 진행하였다.
운영 환경에서는 iptables raw 기능 지원 여부와 Docker 설정을 추가로 확인하여 보안 정책이 정상적으로 적용되는지 점검하는 것이 바람직하다.

---

## 5. Docker 기본 운영 명령 수행

## 5-1. 이미지 다운로드

```bash
$ docker pull hello-world
출력 결과
Using default tag: latest
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:c3cbe1cc1aa588a64951ac6286e0df7b27fe2e6324b1001c619bb358770c0178
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

## 5-2. 이미지 목록 확인

```bash
$ docker images
출력 결과
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB
```

## 5-3. 컨테이너 실행

```bash
$ docker run hello-world
출력 결과
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/


$ docker ps -a
CONTAINER ID   IMAGE                 COMMAND                   CREATED          STATUS                      PORTS                                     NAMES
94f28e38b08a   hello-world           "/hello"                  10 seconds ago   Exited (0) 10 seconds ago                                          priceless_dubinsky
```

## 5-4. 실행 중인 컨테이너 목록 확인

```bash
$ docker ps
출력 결과
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 5-5. 전체 컨테이너 목록 확인

```bash
$ docker ps -a
출력 결과
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
ae8cc6ba4cad   hello-world   "/hello"   5 seconds ago   Exited (0) 4 seconds ago             stoic_roentgen
```

## 5-6. 컨테이너 로그 확인

```bash
$ docker logs hello-world
출력 결과
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 5-7. Docker 리소스 확인

```bash
$ docker stats --no-stream
출력 결과
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS 
 
```

---

## 6. 컨테이너 실행 실습

## 6-1. hello-world 실행 성공 기록

```bash
$ docker run hello-world
출력 결과

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

실행 결과 요약:

- `hello-world` 이미지가 정상적으로 실행됨
- Docker 클라이언트와 데몬이 정상적으로 동작함을 확인함

---

## 6-2. Ubuntu 컨테이너 실행 및 내부 진입

```bash
$ docker run -it ubuntu bash
출력 결과
root@53b903b82f0e:/# 
```

컨테이너 내부에서 명령 실행:

```bash
root@53b903b82f0e:/# ls
출력 결과
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr

root@53b903b82f0e:/# echo "hello ubuntu container"
출력 결과
hello ubuntu container
```

컨테이너 종료:

```bash
root@53b903b82f0e:/# exit
출력 결과
root@53b903b82f0e:/# exit 
exit
```

---

## 6-3. attach와 exec 차이 관찰

### exec 사용

```bash
$ docker run -d -p 2368:2368 --name ghost ghost:4.3-alpine

$ docker exec ghost pwd
출력 결과
/var/lib/ghost
```

### attach 사용

```bash
$ docker run -dit --name attach-test ubuntu bash
$ docker attach attach-test

출력 결과
root@a31262f819ab:/# 
```

### 관찰 내용 정리

| 명령 | 특징 |
|---|---|
| `docker exec` | 이미 실행 중인 컨테이너에 새로운 프로세스를 실행한다. 주로 컨테이너 내부에 접속해 명령을 실행할 때 사용한다. |
| `docker attach` | 컨테이너의 기존 메인 프로세스에 연결한다. attach 상태에서 종료 방식에 따라 메인 프로세스에 종료 신호가 전달되어 컨테이너가 함께 종료될 수 있다. |

내가 관찰한 차이:

> docker exec로 /bin/bash 를 실행한 뒤 exit를 입력해도 컨테이너는 종료되지 않았다. 하지만 docker attach로 연결한 후 ctrl + c로 종료하자 컨테이너도 같이 종료되는것을 확인했다.


---

## 7. 기존 Dockerfile 기반 커스텀 이미지 제작

## 7-1. 선택한 방식

선택 방식:

- [x] A. 웹 서버 베이스 이미지 활용
- [ ] B. Linux 베이스 이미지 활용

사용한 베이스 이미지:
```text
nginx:alpine
```

선택 이유:

`nginx:alpine` 이미지는 가볍고 정적 웹 페이지를 제공하기에 적합하다. 이번 실습에서는 기본 NGINX 이미지를 기반으로 정적 콘텐츠를 교체하여 커스텀 웹 서버 이미지를 제작하였다.

---

## 7-2. 프로젝트 구조

예상 구조:

```text
└── my-nginx
    └── Dockerfile
    └── index.html
```

---

## 7-3. Dockerfile

```Dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="custom-nginx-practice"
LABEL org.opencontainers.image.description="Dockerfile practice with nginx alpine"

ENV APP_ENV=dev

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### 커스텀 포인트

| 항목 | 목적 |
|---|---|
| `FROM nginx:alpine` | 가벼운 NGINX 웹 서버 이미지를 베이스로 사용 |
| `LABEL` | 이미지 정보 기록 |
| `ENV APP_ENV=dev` | 컨테이너 환경변수 설정 |
| `COPY site/ ...` | 기본 NGINX 페이지 대신 직접 작성한 정적 콘텐츠 배포 |
| `EXPOSE 80` | 컨테이너가 80번 포트를 사용함을 명시 |

---

## 7-4. 정적 페이지 작성

```bash
$ mkdir my-nginx

$ cat > my-nginx/index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom NGINX</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>커스텀 이미지</p>
</body>
</html>
EOF
```

---

## 7-5. 커스텀 이미지 빌드

```bash
$ docker build -t my-custom-nginx:1.0 .
결과 출력
[+] Building 4.7s (7/7) FINISHED                                         docker:orbstack
 => [internal] load build definition from Dockerfile                                0.1s
 => => transferring dockerfile: 279B                                                0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                     0.4s
 => [internal] load .dockerignore                                                   0.2s
 => => transferring context: 2B                                                     0.0s
 => [internal] load build context                                                   0.2s
 => => transferring context: 299B                                                   0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da03789  3.1s
 => => resolve docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da03789  0.2s
 => => sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aea 10.33kB / 10.33kB  0.0s
 => => sha256:1d40e3eb3bf4f138de1d67193f2aa5309fcaf343eb5ffadbf5e9 2.50kB / 2.50kB  0.0s
 => => sha256:f0ba77f796e57c6fa89ae7f4fdad1665d6fcbd8e3f21153512 12.32kB / 12.32kB  0.0s
 => => sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582f 627B / 627B  0.3s
 => => sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e5923899 1.89MB / 1.89MB  0.5s
 => => sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32 3.85MB / 3.85MB  0.7s
 => => sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6 957B / 957B  0.8s
 => => sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143 404B / 404B  0.8s
 => => extracting sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c3212343  0.1s
 => => sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982 20.31MB / 20.31MB  1.3s
 => => extracting sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12  0.1s
 => => sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe 1.21kB / 1.21kB  1.3s
 => => sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e75760 1.40kB / 1.40kB  1.1s
 => => extracting sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc  0.0s
 => => extracting sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c  0.0s
 => => extracting sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143d  0.0s
 => => extracting sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f87  0.0s
 => => extracting sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9  0.0s
 => => extracting sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010  0.3s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                          0.3s
 => exporting to image                                                              0.2s
 => => exporting layers                                                             0.1s
 => => writing image sha256:2be902a83b0f40b8d75bf0df7a9f74a12fab3a90757fb0d45e7e9e  0.0s
 => => naming to docker.io/library/my-custom-nginx:1.0                              0.0s
```

빌드 결과:

- 이미지 이름: `my-custom-nginx`
- 태그: `1.0`
- 빌드 성공 여부: `성공`

이미지 확인:

```bash
$ docker images
결과 출력
REPOSITORY        TAG          IMAGE ID       CREATED          SIZE
my-custom-nginx   1.0          2be902a83b0f   14 seconds ago   62.4MB
```

---

## 7-6. 컨테이너 실행

```bash
$ docker run -d -p 8080:80 --name my-nginx-8080 my-custom-nginx:1.0
출력 결과
6dd57872e9cb54f09e8448a0339ba64a45ee35a279c10b5700085a8cecc72dc3
```

실행 중인 컨테이너 확인:

```bash
$ docker ps
출력 결과
CONTAINER ID   IMAGE                 COMMAND                   CREATED          STATUS          PORTS                                         NAMES
6dd57872e9cb   my-custom-nginx:1.0   "/docker-entrypoint.…"   27 seconds ago   Up 27 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       my-nginx-8080
![핵심 결과](/images/7%20핵심결과%20스크린샷.png)
```

---

## 8. 포트 매핑 및 접속 증거
```bash
$ docker run -d -p 8080:80 --name my-nginx-8080 my-custom-nginx:1.0
```
컨테이너는 별도의 네트워크 네임스페이스를 사용하기 때문에 호스트와 네트워크가 분리되어 있다. 따라서 컨테이너 내부에서 실행중인 Nginx의 80번 포트에 호스트에서 접근하려면 -p 8080:80과 같은 포트 매핑이 필요하다.
포트를 공개하면 외부의 접근이 가능해지므로 필요한 포트만 노출해야 된다.

## 8-1. curl 접속 확인

```bash
$ curl http://localhost:8080
결과
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom NGINX</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>커스텀 이미지</p>
</body>
```
![브라우저 접속 기록](/images/브라우저%20접속%20기록.png)

## 8-2 포트 충돌 진단
```bash
포트 확인
$ lsof -i :[포트번호]
COMMAND     PID        USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
OrbStack  39392 ajj05062587   85u  IPv4 0x8cb473601b48df4f      0t0  TCP *:http-alt (LISTEN)
OrbStack  39392 ajj05062587   86u  IPv6 0xd820eaa1f86f2d54      0t0  TCP *:http-alt (LISTEN)

프로세스 확인
$ docker ps --filter pbulish=[포트번호]
CONTAINER ID   IMAGE                 COMMAND                   CREATED        STATUS          PORTS                                     NAMES
6dd57872e9cb   my-custom-nginx:1.0   "/docker-entrypoint.…"   20 hours ago   Up 50 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-8080

포트 변경
$ docker run -p [변경할 호스트 포트번호]:[컨테이너 포트 번호] nginx

---

## 9. Docker 볼륨 영속성 검증

## 9-1. Docker 볼륨 생성

```bash
$ docker volume create mydata
```

볼륨 목록 확인:

```bash
$ docker volume ls
출력 결과
DRIVER    VOLUME NAME
local     mydata
```

---

## 9-2. 볼륨을 컨테이너에 연결

```bash
$ docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
38cd7e59dfd720f82809da441db8097349a2fc03fbacc0fd155e76617adff929
```

컨테이너 실행 확인:

```bash
$ docker ps
출력 결과
CONTAINER ID   IMAGE                 COMMAND                   CREATED          STATUS          PORTS                                         NAMES
38cd7e59dfd7   ubuntu                "sleep infinity"          18 seconds ago   Up 18 seconds                                                 connect-vol
```

---

## 9-3. 컨테이너 내부에 데이터 생성

```bash
$ docker exec connect-vol bash -lc "echo hi > /data/hello.txt && cat /data/hello.txt"
출력 결과
hi
```

---

## 9-4. 컨테이너 삭제

```bash
$ docker rm -f connect-vol
```

삭제 확인:
```bash
$ docker ps -a
ONTAINER ID   IMAGE                 COMMAND                   CREATED          STATUS                   PORTS                                         NAMES
6dd57872e9cb   my-custom-nginx:1.0   "/docker-entrypoint.…"   35 minutes ago   Up 35 minutes            0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       my-nginx-8080
a31262f819ab   ubuntu                "bash"                    3 hours ago      Exited (0) 2 hours ago                                                 attach-test
```

---

## 9-5. 새 컨테이너에 동일 볼륨 연결 후 데이터 확인

```bash
$ docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity

fbe7087fe3d7778532c2f424b0b0ed63ef4873efaaa06014a2227b50ee4aa287
```

```bash
$ docker exec vol-test2 bash -lc "cat /data/hello.txt"

결과 출력
hi
```

검증 결과:

- 기존 컨테이너 `connect-vol`을 삭제한 후에도 `mydata` 볼륨에 저장된 `hello.txt` 파일이 유지되었다.

---

## 10. Git 설정 및 GitHub 연동

## 10-1. Git 사용자 정보 설정

```bash
$ git config --global user.name "[GitHub 사용자명 또는 이름]"

$ git config --global user.email "[이메일]"
```

## 10-2. 기본 브랜치 설정

```bash
$ git config --global init.defaultBranch main
```

## 10-3. Git 설정 확인

```bash
$ git config --list

user.name=JoongHyun-codyssey
user.email=dkswndgus0506@gmail.com
init.defaultbranch=main
remote.origin.url=https://github.com/JoongHyun-codyssey/codyssey_repo.git
```
![Git 사용자 정보/기본 브랜치 설정, GitHub 저장소 연동](/images/Git,%20GitHub.png)

GitHub Repository 링크:

```text
https://github.com/JoongHyun-codyssey
```

GitHub 연동
![GitHub 커밋/푸시 기록](/images/GitHub%20커밋:푸시%20기록.png)


---

## 11. 검증 방법 및 결과 위치

| 검증 항목 | 검증 명령 / 방법 | 결과 위치 |
|---|---|---|
| 실행 환경 확인 | `docker --version`, `git --version`, `uname -a` | README 1장 |
| 터미널 기본 조작 | `pwd`, `ls -la`, `mkdir`, `cp`, `mv`, `rm`, `cat`, `touch