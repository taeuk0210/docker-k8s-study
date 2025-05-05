# 📘 Chapter 6.2 Flask, Nginx 연동

## 📌 학습 목표

- Flask, Nginx 연동 실습

## 🔍 학습 내용

### 🔸 6.2.1 ~ 6.2.4 nginx, flask 연동

```bash
# 1. ex01을 ex02 디렉터리로 복사
cd ex02
mv ex01 myFlask02

# 2. requirements.txt
flask==3.0.0
gunicorn==20.1.0

# 3. Dockerfile
FROM python:3.11.6

WORKDIR /usr/src/app

COPY . .

RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt

WORKDIR ./myapp

CMD gunicorn --bind 0.0.0.0:8001 main:app

EXPOSE 8001

# 4. flask 이미지 빌드
docker image build . -t myflask02

# 5. myNginx02 디렉토리 생성 후 파일 추가
mkdir myNginx02
# default.conf
upstream myweb{
    server flasktest:8001;
}

server{
    listen 81;
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

# 6. nginx 이미지 생성
docker image build . -t mynginx02f

# 7. docker network 생성
docker network create mynetwork02f

# 8. 컨테이너 실행
docker container run -d --name flasktest --network mynetwork02f myflask02

docker container run -d --name nginxtest --network mynetwork02f -p 81:81 mynginx02f
```
