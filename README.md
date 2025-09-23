<h1 align="center">Dabom(FlowBox)  </h1>
<div align="center"> 
 <img src="https://github.com/user-attachments/assets/99a3a5a1-a808-4a5b-9a72-877bafb953b4" width="150"/>
</div>

프로젝트명 '다봄' 은 **'다'** 같이 **'본다'** 는 동시 시청의 핵심 기능과 새로운 디지털 공동체 문화가
**'봄'** 처럼 새롭게 시작된다는 의미를 동시에 담고 있습니다.

# 🎬  DaBom Streaming Service
## 🫂 팀원 소개
<table align="center">
  <tbody>
    <tr>
      <td align="center"><a href="https://github.com/raccoon-coding"><img src="https://github.com/user-attachments/assets/cd54a924-3b11-4ba6-b682-711026407caa" width="100px;" alt=""/><br /><sub><b> 팀원: 최민성</b></sub></a><br /></td>
      <td align="center"><a href="https://github.com/tipsyboy"><img src="https://github.com/user-attachments/assets/307b28e9-f277-4bbd-9ece-77ca04cce34f" width="100px;" alt=""/><br /><sub><b> 팀원: 양형모</b></sub></a><br /></td>
      <td align="center"><a href="https://github.com/flionme"><img src="https://github.com/user-attachments/assets/194a7eaa-752e-461d-94e9-3057659bdafe" width="100px;" alt=""/><br /><sub><b> 팀원 : 김성인</b></sub></a><br /></td>
      <td align="center"><a href="https://github.com/Hanryang-Kim"><img src="https://github.com/user-attachments/assets/df5ffff0-a06b-4579-a695-4338bd1d2b91" width="100px;" alt=""/><br /><sub><b> 팀원 : 김륜환</b></sub></a><br /></td>
      <td align="center"><a href="https://github.com/kbw07"><img src="https://github.com/user-attachments/assets/a1fdbad2-dd82-48c7-941f-422f6e73d58f" width="100px;" alt=""/><br /><sub><b> 팀원 : 강병욱 </b></sub></a><br /></td>
    </tr>
  </tbody>
</table>

---

# 🎯 프로젝트 소개
**"혼자 보는 영상에서 함께하는 경험으로"**

비대면 소통이 일상화된 시대에, 단순한 영상 시청을 넘어 **실시간 공유와 소통이 가능한 스트리밍 서비스**를 개발하고자 합니다.
영상 콘텐츠와 실시간 채팅, 동시 시청 기능을 결합하여 새로운 형태의 디지털 공동체 경험을 제공하는 것이 저희의 목표입니다.

---

# 🛠 기술 스택
![github](https://img.shields.io/badge/github-181717.svg?style=for-the-badge&logo=github&logoColor=white)
![docker](https://img.shields.io/badge/docker-2496ED.svg?style=for-the-badge&logo=docker&logoColor=white)
![kubernetes](https://img.shields.io/badge/kubernetes-326CE5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)

![Jenkins](https://img.shields.io/badge/jenkins-D24939.svg?style=for-the-badge&logo=jenkins&logoColor=white)
![ansible](https://img.shields.io/badge/ansible-EE0000.svg?style=for-the-badge&logo=ansible&logoColor=white)

# 🔧 시스템 아키텍처
<img width="896" height="531" alt="Internet_of_Things_Architecture_8" src="https://github.com/user-attachments/assets/bb524dc6-a15f-4e4b-bc96-5c380fd2caeb" />


# 🌏 CI/CD 시나리오
1. **개발자가 코드 Push**
   - Backend Github → main 브랜치 push 시 Backend CI/CD 파이프라인 동작
   - Frontend Github → main 브랜치 push 시 Frontend CI/CD 파이프라인 동작
2. **Github Webhook → Jenkins 호출**
   - Github Webhook이 Kubernetes 상의 Jenkins Master에 이벤트 전달
3. **Jenkins Master → Jenkins Agents 생성**
   - Jenkins Master가 Kubernetes Master에게 요청 → 빌드용 Jenkins Agent Pod 동적 생성
4. **코드 Clone & Build**
   - Jenkins Agent가 Github Repository clone
   - Gradle/Node 등 빌드 수행 
5. **Docker Image 빌드 & Push (Kaniko 활용)**
   - 빌드 산출물로 Docker Image 생성
   - Jenkins Agent 내부에서는 Docker Daemon을 직접 실행할 수 없으므로, Kaniko를 사용하여 이미지 빌드 및 푸시 수행
   - 생성된 이미지를 Docker Hub에 push
6. **Kubernetes 배포 자동화**
   - 새로운 Docker Image tag를 기반으로 Deployment 갱신
   - Frontend / Backend 모두 Blue-Green 배포 전략 적용
---
## 배포 방식: Blue-Green Deployment
### Blue-Green 배포 개념
- 동일한 환경(Blue, Green) 두 개를 유지 
- 새로운 버전은 기존과 별도의 환경(Green)에 배포 
- 정상 동작이 확인되면 트래픽을 Blue → Green으로 전환 (Zero-Downtime)
- **문제 발생 시 빠른 롤백 가능 (Green → Blue)**
### Frontend의 배포 방식 설정 이유
- **사용자 경험 보장**: 배포 중에도 웹 페이지가 끊기지 않고 계속 서비스 제공 가능 
- **빠른 전환**: 로드밸런서/Ingress에서 Blue → Green으로 라우팅만 변경 
- **리스크 최소화**: UI/UX 오류 발생 시 즉시 Blue로 롤백 가능
### Backend의 배포 방식 설정 이유
- **세션/트랜잭션 안정성**: API 서버는 연결 중단 없이 서비스 가능해야 함 
- **Zero-Downtime**: 기존 연결은 Blue에서 처리, 신규 연결은 Green으로 전환 
- **안전한 롤백**: 배포 후 에러 발생 시 Green 중단, Blue로 즉시 복귀
---
## Kaniko 사용 배경
- Jenkins Agent Pod 환경에서는 Docker Daemon을 실행할 수 없음 (DID: Docker-in-Docker 제약)
- Kaniko는 Daemonless 방식으로 동작하여 Kubernetes 환경에서도 안전하게 이미지 빌드 가능 
- 따라서 본 파이프라인은 Kaniko Executor를 활용해 Dockerfile 기반 이미지를 빌드하고, Docker Hub로 푸시
---

# 💻 프론트 동작 화면
- 프론트 블루 그린 배포
  ![front-blue_green (2)](https://github.com/user-attachments/assets/a199f803-b8b7-40cb-baf7-bb7d965afc43)


  # 💻 백엔드 동작 화면
- 백엔드 블루 그린 배포
  ![back-bg (1)](https://github.com/user-attachments/assets/d1ac0133-199a-4d70-a807-30c816a4fea4)






