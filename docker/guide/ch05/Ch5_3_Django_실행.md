# 📘 Chapter 5.3 도커를 활용한 django 실행

## 📌 학습 목표

- django를 활용한 웹 서비스 실행

## 🔍 학습 내용

### 🔸 5.3.1 도커 호스트에 프로젝트 생성

```bash
# 1. ex02 디렉터리 생성 후 이동
cd ex02

# 2. django-admin 명령어로 프로젝트 생성
django-admin startproject myapp
tree ./
./
└── myapp
    ├── manage.py
    └── myapp
        ├── __init__.py
        ├── asgi.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

2 directories, 6 files
# 3. settings.py 파일 수정

# 들어가서 ALLOWED_HOSTS 를 [] 에서 ['*']로 설정(모든 호스트 허용)
nano ./myapp/myapp/settings.py

# 4. db migration
cd myapp/
# Django는 동작에 필요한 기본 테이블이 DB에 존재해야 함!
python manage.py migrate

# 5. 서버 실행
python manage.py runserver 0.0.0.0:8000
```

### 🔸 5.3.2 django 이미지 빌드

django 프로젝트를 컨테이너 이미지로 빌드하는 과정

1. 디렉터리 정리
2. requirements.txt 작성
3. Dockerfile 작성
4. 이미지 빌드

```bash
# 1. ex02에 있는 모든 파일을 ex03으로 복사
cp -r ex02 ex03

# 2. requirements.txt 작성
nano requirements.txt

django==4.2.7 # <- 이거 입력

# 3. Dockerfile 작성
nano Dockerfile

# Dockerfile 설명

# 베이스 이미지를 의미
FROM python:3.11.6
# 작업 디렉토리 전환 (cd와 유사)
WORKDIR /usr/src/app
# 호스트 파일을 도커 이미지 파일시스템 경로로 복사
# 현재 경로의 파일들을 usr/src/app/ 으로 복사
COPY . .
# 이미지 빌드시 실행할 명령어가 있을때 사용하는 명령어
# 현재는 pip 을 설치하고 필요한 패키지를 설치
RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt

WORKDIR ./myapp
# CMD 명령어로 서비스를 실행(컨테이너 실행시 실행되는 명령어)
CMD python manage.py runserver 0.0.0.0:8000
EXPOSE 8000

# 4. 이미지 빌드 후 컨테이너 실행
# 현재 디렉토리에서 tag를 myweb01:TAG_NAME 지금은 없어서 latest로 됨
docker image build . -t myweb01

docker container run -d -p 8000:8000 myweb01
```
