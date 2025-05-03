# docker-k8s-study

📦 **도커(Docker)** 와 ☸️ **쿠버네티스(Kubernetes)** 개념 학습 및 실습 기록 저장소입니다.  
📘 _한권으로 배우는 도커 & 쿠버네티스_ 책을 기반으로 정리 및 실습을 진행합니다.

## 📚 학습 자료 구성

```bash
docker-k8s-study/
├── docker/
│ ├── guide/   # 도커 개념 및 이론 요약 (Markdown)
│ └── script/  # 도커 실습 코드
├── kubernetes/
│ ├── guide/   # 쿠버네티스 개념 및 이론 요약 (Markdown)
│ └── script/  # 쿠버네티스 실습 코드
```

## 🚀 사용 환경

- Windows + WSL2 (Ubuntu)
- Docker Desktop for Windows
- Minikube 또는 Kind (쿠버네티스 로컬 클러스터)
- VSCode

---

## 🗂 학습 진도

| 구분       | 요약 정리                  | 실습 완료             |
| ---------- | -------------------------- | --------------------- |
| Docker     | ✅ 기본 개념, 이미지, 볼륨 | ✅ nginx 컨테이너 등  |
| Kubernetes | ⏳ Pod, Service 정리 중    | ⏳ Minikube 실습 예정 |

---

## 📖 참고 자료

- [한권으로 배우는 도커 & 쿠버네티스](https://search.shopping.naver.com/book/catalog/47243393619)
- [공식 Docker 문서](https://docs.docker.com/)
- [공식 Kubernetes 문서](https://kubernetes.io/ko/docs/)

---

## 🙌 목표

- 컨테이너 개념 및 기술에 대한 실전 감각 익히기
- 개인 프로젝트/서비스 배포를 위한 인프라 이해
- 추후 CI/CD, 클라우드 배포 (EKS/GKE)로 확장 가능
