# 📘 Chapter 6.1 Flask 실행

## 📌 학습 목표

- flask를 활용해 웹 서비스를 실행
- main.py 작성

## 🔍 학습 내용

### 🔸 6.1.1 Flask 라이브러리 설치

```bash
pyenv activate py3_11_6
pip install flask
```

### 🔸 6.1.3 Flask - Hello world!

```bash
# 1. 디렉터리 생성
cd /work/ch06/ex01/myapp

# 2. main.py 작성
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'hello world!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8001)

# 3. main.py 실행 후 접속 확인
python main.py
```
