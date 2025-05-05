# 📘 Chapter 5.5 Django, Nginx, Postgres 연동

## 📌 학습 목표

- Django, Nginx, Postgres 연동

## 🔍 학습 내용

### 🔸 5.5.1 PostgreSQL 컨테이너 실행

```bash
# 1. ex06 디렉터리 이동
cd ex06

# 2. Dockerfile 작성
FROM postgres:15.4

# 3. postgres 이미지 빌드
docker image build . -t mypostgres03
# 4. volume 생성
docker volume create myvolume03
```

### 🔸 5.5.1 PostgreSQL 컨테이너 실행

```bash
# 1. ex06 디렉터리 이동
cd ex06

# 2. Dockerfile 작성
FROM postgres:15.4

# 3. postgres 이미지 빌드
docker image build . -t mypostgres03
# 4. volume 생성
docker volume create myvolume03
```

### 🔸 5.5.2 ~ 5.5.5 django, nginx, postgre 연동 및 실행

```bash
# 1. ex07 디렉터리를 ex05, ex06을 이용해서 아래와 같이 정리
cd ex07tree ./
./
├── myDjango03
│   ├── Dockerfile
│   ├── myapp
│   └── requirements.txt
├── myNginx03
│   ├── Dockerfile
│   └── default.conf
└── myPostgres03
    └── Dockerfile

6 directories, 16 files

# 2. myDjango03/settings.py 수정
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "postgres",
        "USER": "postgres",
        "PASSWORD": "mysecretpassword",
        "HOST": "postgrestest",
        "PORT": 5432,
    }
}
# 3. myDjango03/requirements.txt 수정
django==4.2.7
gunicorn==20.1.0
psycopg2==2.9.9

# 4. django 이미지 빌드
docker image build . -t myweb03

# 5. nignx 이미지 빌드
docker image build . -t mynginx03

# 6. docker network 생성
docker network create mynetwork03

# 7. 컨테이너 실행
docker container run --name postgrestest --network mynetwork03 -e POSTGRES_PASSWORD=mysecretpassword --mount type=volume,source=myvolume03,target=/var/lib/postgresql/data -d mypostgres03

docker container run -d --name djangotest --network mynetwork03 myweb03

docker container run -d --name nginxtest --network mynetwork03 -p 80:80 mynginx03
```
