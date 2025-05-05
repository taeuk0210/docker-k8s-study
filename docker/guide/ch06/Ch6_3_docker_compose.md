# 📘 Chapter 6.3 docker-compose 활용

## 📌 학습 목표

- docker-compose 를 활용하여 Flask, Nginx 연동

## 🔍 학습 내용

### 🔸 6.3.1 docker-compose.yml 작성

```bash
# 1. ex03 디렉터리 생성
cp -r ex02 ex03
mv myFlask02 myFlask03
mv myNginx02 myNginx03

# 2. docker-compose.yml 작성
version: "3"

services:
  flasktest:
    build: ./myFlask03
    networks:
      - composenet03
    restart: always

  nginxtest:
    build: ./myNginx03
    networks:
      - composenet03
    ports:
      - "81:81"
    depends_on:
      - flasktest
    restart: always

networks:
  composenet03:

# 4. 빌드 및 실행
docker compose up -d --build
```
