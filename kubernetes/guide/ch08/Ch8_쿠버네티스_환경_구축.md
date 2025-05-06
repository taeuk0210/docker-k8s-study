# 📘 Chapter 8 쿠버네티스 실습 환경 구축

## 📌 학습 목표

- 쿠버네티스 실습 환경 구축

## 🔍 학습 내용

### 🔸 VM 대신 Docker를 이용해보기

ubuntu 컨테이너에 쿠버네티스를 설치 후 컨테이너를 마스터와 워커 노드로  
설정하려 했으나 쿠버네티스 설치에 필요한 이미지 다운로드할때 unpack 과정에서 실패함  
도커 컨테이너는 기본적으로 mount, unmount, loop device 생성 등 일부 호스트 수준의 시스템 콜을 제한 한다는데 이게 이유 같음 권한을 추가로 주고 실행시켜봤으나 그래도 안되서 kind를 이용한 방식으로 변경

**원래 하려고 했던 방법**

```bash
# 1. ubuntu 컨테이너 접속 후 다운로드
apt update && apt install -y curl apt-transport-https ca-certificates gnupg

# 2. GPG 키 등록
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key \
 | gpg --dearmor -o /etc/apt/trusted.gpg.d/kubernetes.gpg

# 3. APT 저장소 설정
echo "deb [signed-by=/etc/apt/trusted.gpg.d/kubernetes.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" > /etc/apt/sources.list.d/kubernetes.list

# 4. apt 업데이트 후 설치
apt update
apt install -y kubelet kubeadm kubectl

# 5. 컨테이너 런타임 설치
apt install -y containerd

mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml

# 6. 컨테이너 런타임 실행
containerd &

# 7. 필요한 이미지 다운로드 -> 여기서 실패 ㅠㅠ
kubeadm config images pull
```

**Kind(Kubernetes in docker) 방법**

```bash
# 1. kind 설치
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64

# 2. 실행 권한 추가
chmod +x ./kind

# 3. 이동
sudo mv ./kind /usr/local/bin/kind

# 4. kind-config.yml 작성
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker

# 5. kind 실행
kind create cluster --name my-k8s-cluster --config ./kind-config.yml
```
