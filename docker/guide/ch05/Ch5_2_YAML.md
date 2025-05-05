# 📘 Chapter 5.2 YAML

## 📌 학습 목표

- YAML 개념 습득
- .yml 작성 실습

## 🔍 학습 내용

### 🔸 5.2.1 YAML의 개념

YAML(YAML Ain't Markup Language)은 본질적으로는 사람이 읽기 쉬운 데이터 직렬화 포맷으로 XML이나 JSON처럼 데이터를 표현하는 방식중 하나임

### 🔸 5.2.2 pyyaml 설치

```bash
# py3_11_6 접속 후 설치
pip install pyyaml
```

### 🔸 5.2.3 YAML 문법

```bash
# 1. work/ch05/ex01 로 이동
cd /work/ch05/ex01/

# 2. 아래의 yaml_practice.yml 생성
nano yaml_practice.yml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
    - name: ubuntu
      image: ubuntu:latest

# 3. python에서 구조 확인
pyenv activate py3_11_6
python
>>> import yaml
>>> raw = open("./yaml_practice.yml", "r+")
>>> data = yaml.load(raw, Loader=yaml.SafeLoader)
>>> data
{'apiVersion': 'v1',
 'kind': 'Pod',
  'metadata': {'name': 'nginx'},
  'spec': {
    'containers': [
        {'name': 'nginx', 'image': 'nginx:latest'},
        {'name': 'ubuntu', 'image': 'ubuntu:latest'}
        ]
    }
}
```
