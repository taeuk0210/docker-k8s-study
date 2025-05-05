# 📘 Chapter 5.4 Django와 Nginx 연동

## 📌 학습 목표

- django와 nginx 연동
- default.conf 작성

## 🔍 학습 내용

### 🔸 5.4.1 Nginx 컨테이너 실행

```bash
# 1. ex04 디렉터리로 이동
cd ex04

# 2. Dockerfile 작성
FROM nginx:1.25.3
CMD ["nginx", "-g", "daemon off;"]

# 3. nginx 이미지 빌드 후 컨테이너 실행
docker image build . -t mynginx01

docker container run -p 80:80 -d mynginx01

# 4. default.conf 확인
docker container exec -it ad3111ead1 /bin/bash
cd /etc/nginx/conf.d
cat default.conf # 내부를 보면 django 설정을 정할 수 있는 파일임을 알 수 있음
```

### 🔸 5.4.2 ~ 5.4.4 gunicorn을 통한 연동 및 각 이미지 빌드

```bash
# 1. ex03, ex04 를 ex05로 이동 후 이름 변경
cp -r ex03 ex04 ex05
cd ex05
mv ex03 myDjango02
mv ex04 myNginx02
tree ./ -L 3

./
├── myDjango02
│   ├── Dockerfile
│   ├── myapp
│   │   ├── db.sqlite3
│   │   ├── manage.py
│   │   └── myapp
│   └── requirements.txt
└── myNginx02
    └── Dockerfile

4 directories, 5 files

# 2. django 설정 파일 수정
cd myDjango02/

# requirement.txt
django==4.2.7
gunicorn==20.1.0

# Dockerfile
FROM python:3.11.6

WORKDIR /usr/src/app

COPY . .

RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt

WORKDIR ./myapp

CMD gunicorn --bind 0.0.0.0:8000 myapp.wsgi:application

EXPOSE 8000

# 3. django 이미지 빌드
docker image build . -t myweb02

# 4. nginx 설정 파일 수정
cd myNginx02/

# default.conf
upstream myweb{
    server djangotest:8000;
}

server{
    listen 80;
    server_name localhost;
    location /{
        proxy_pass http://myweb;
    }
}
# Dockerfile
FROM nginx:1.25.3

RUN rm /etc/nginx/conf.d/default.conf

COPY default.conf /etc/nginx/conf.d/

CMD ["nginx", "-g", "daemon off;"]

# 5. nginx 이미지 빌드
docker image build . -t mynginx02
```

### 🔸 5.4.5 django + nginx 연동 후 컨테이너 실행

```bash
# 1. docker network 생성
docker network create mynetwork02

# 2. django 컨테이너 실행
docker container run -d --name djangotest --network mynetwork02 myweb02

# 3. nginx 컨테이너 실행
docker container run -d --name nginxtest --network mynetwork02 -p 80:80 mynginx02
```

위의 구조를 통해 아래와 같이 트래픽이 전달됨
웹 -> 호스트 PC의 80포트 -> 도커 호스트의 80포트 -> nginxtest의 80 포트  
 -> djangotset의 8000포트 -> gunicorn -> myapp
