# 📘 Chapter 5.7 docker-compose 활용

## 📌 학습 목표

- docker compose 활용
- docker-compose.yml 작성

## 🔍 학습 내용

### 🔸 5.7.1 도커 컴포즈의 개념

도커를 활용해 다수의 컨테이너 형태의 애플리케이션을 실행하는 도구로  
애플리케이션의 설정 내용들을 YAML 파일로 작성하는 방법입니다.

### 🔸 5.7.2 도커 컴포즈 설치

```bash
sudo apt install -y python3-pip
sudo pip3 install dockekr-compose
```

### 🔸 5.7.3 ~ 5.7.5 디렉터리 구성부터 실행까지

```bash
# 1. ex07 디렉터리 복사
cp -r ex07 ex09
cd ex09

# 2. docker-compose.yml 작성
nano docker-compose.yml

# 내용 설명

# 컴포즈 포맷 버전 설정
version: "3"

# 실행하고자 하는 서비스 목록
services:
  # 서비스 이름
  djangotest:
    # 빌드할 경로
    build: ./myDjango03
    # 도커 네트워크 정보
    networks:
      - composenet01
    # 실행 순서를 정할때 사용(postgrestest 실행 후 djangotest 실행됨)
    depends_on:
      - postgrestest
    # 컨테이너가 정지되면 재실행하라는 명령어
    restart: always

  nginxtest:
    build: ./myNginx03
    networks:
      - composenet01
    # 포트포워딩 정보
    ports:
      - "80:80"
    depends_on:
      - djangotest
    restart: always

  postgrestest:
    build: ./myPostgres03
    networks:
     - composenet01
    # 환경 변수 정보 입력
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: mysecretpassword
      POSTGRES_DB: postgres
    # composevol01 이라는 볼륨을 postgres 컨테이너 내부의 /var/lib/postgresql/data에 마운트
    volumes:
      - composevol01:/var/lib/postgresql/data

networks:
  composenet01:

volumes:
  composevol01:

# 3. 빌드 및 실행
docker compose up -d --build
```
