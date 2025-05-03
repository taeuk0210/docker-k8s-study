# 📘 Chapter 04. 도커 기초초

## 📌 학습 목표

- 도커 기초 지식 습득 및 실습

## 🔍 학습 내용

### 🔸 4.1.1 도커 작동 방식

도커의 구조

- **Docker client** : 도커에 명령을 내릴 수 있는 CLI 도구를 의미
- **Docker host** : 도커를 설치한 서버 혹은 가상머신
- **Docker registry** : 도커 이미지를 저장하거나 배포하는 시스템(DockerHub 등)

클라이언트에서 명령어를 입력하면 도커 호스트의 도커 데몬이 명령을 받아 실행하게 되는데 도커 호스트에 이미지가 존재하지 않으면 도커 레지스트리에서 이미지를 다운받음!

### 🔸 4.1.2 도커 이미지

컨테이너 형태의 소프트웨어 배포를 위해 필요한 모든 요소를 실행할 수 있는 포맷으로 컴파일 및 빌드한 패키지

특징

- 각 이미지는 독립적이므로 의존성을 고려할 필요 X
- 경량화된 패키지
- 특정 시점의 컨테이너 상태를 담은 스냅샷

### 🔸 4.1.3 도커 컨테이너

도커 이미지를 실행할 수 있는 인스턴스를 의미하며 도커 컨테이너에 대해 실행, 중지 등의 명령을 내릴 수 있음!

컨테이너는 자체적으로 파일 시스템을 가지고 있고, 각 컨테이너는 독립적으로 실행됨

컨테이너는 가상머신과 같은 개념이 아니라 도커 엔진과 운영체제를 공유하기 때문에 호스트 가상화 방식보다 가벼움

즉, 컨테이너는 프로그램을 실행시키기 위해 최소한으로 필요한 바이너리, 라이브러리와 같은 구성요소로 이루어짐!

### 🔸 4.2.1 도커 이미지 다운로드

```bash
docker image pull IMAGE_NAME:TAG_NAME
# or
docker image pull IMAGE_NAME@DIGEST

## Ex
## docker image pull mysql:latest
## docker image pull ubuntu@sha256:1412f...153e1f
```

### 🔸 4.2.2 도커 이미지 상세 구조

도커의 이미지는 크게 **이미지 인덱스**, **이미지 매니페스트**, **레이어**로 구성

이미지 다운로드 시 결과창에 나오는 digest가 바로 이미지 인덱스고  
이미지 인덱스는 다수의 이미지 메니페스트로 구성되며 이미지 메니페스트는  
다양한 운영체제와 아키텍처에서 해당 이미지를 활용할 수 있는 설정값과 레이어들로 구성됨

```bash
이미지 인덱스(매니페스트 리스트): 6deadd529b
├── 매니페스트: linux/amd64  → 99cb81c1d8e4
│   ├── config
│   ├── 레이어 n → d9992232ef9b
│   ├── ...
│   ├── 레이어 1 → 13baa2029dde
│   └── 레이어 0 → 8457fd5317e7
│
├── 매니페스트: linux/arm/v5 → a32208ad97bf
│
├── 매니페스트: linux/arm/v7 → d3dd00bd5e41
│
├── ...
│
└── 매니페스트: linux/ppc64le → 36dd3ab05c1d
```

### 🔸 4.2.3 도커 이미지 목록 확인

```bash
docker image ls
```

### 🔸 4.2.4 도커 컨테이너 실행

```bash
docker container run python:3.11.6
```

초기에는 docker run 명령어를 사영했지만 현재는 docker container run 명령어를 권장함

### 🔸 4.2.5 도커 컨테이너 목록 확인

```bash
docker container ls [-a]
```

### 🔸 4.2.6 컨테이너 실행과 동시에 내부 접속

```bash
docker container run -it IMAGE_NAME
```

-it 옵션은 interactive tty(가상터미널) 가상터미널을 통해 키보드 입력을 표준입력으로 컨테이너에 전달

**컨테이너 종료 방법**

```bash
# 내부에 접속해 있는 터미널에서
exit

# 컨테이너 외부에서
docker container stop CONTAINER_ID
# or
docker container kill CONTAINER_ID
```

**종료된 컨테이너에 접속하기**

```bash
# 종료된 컨테이너 실행
docker container start CONTAINER_ID

# 내부에 접속
docker container attach CONTAINER_ID
```

### 🔸 4.2.7 컨테이너 삭제

```bash
docker container rm CONTAINER_ID
```

### 🔸 4.2.8 이미지 삭제

```bash
docker image rm IMAGE_NAME
```

### 🔸 4.2.9 도커 이미지 변경

p96
