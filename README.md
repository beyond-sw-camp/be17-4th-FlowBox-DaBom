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
<img width="896" height="531" alt="undefined_6" src="https://github.com/user-attachments/assets/c8ccbafe-c6c6-4f1f-8970-f84986f0e901" />




# 💻 프론트 동작 화면
- 프론트 블루 그린 배포
  ![front-blue_green (2)](https://github.com/user-attachments/assets/a199f803-b8b7-40cb-baf7-bb7d965afc43)


# 💻 백엔드 동작 화면
- 백엔드 블루 그린 배포
  ![back-bg (1)](https://github.com/user-attachments/assets/187cbb61-d494-48c0-85c0-629598da24f9)


---

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

# 🔵🟢 Blue/Green Deployment
### 무중단 배포(Zero-Downtime)를 위한 **Blue/Green 배포 전략**

Blue-Green 배포는 동일한 두 개의 환경(Blue, Green)을 운영하여 무중단 배포를 가능하게 하는 방식입니다. 새로운 버전은 Green 환경에 먼저 배포되고, 정상 동작이 확인되면 트래픽을 Blue에서 Green으로 전환합니다. 이 과정에서 서비스 중단 없이 새 버전으로 전환이 가능하며, 만약 오류가 발생할 경우 즉시 Blue 환경으로 롤백할 수 있어 안정성이 높습니다.

👉 자세한 내용은 [📘 블루/그린 배포 Wiki](https://github.com/beyond-sw-camp/be17-4th-FlowBox-DaBom/wiki/%EB%B0%B0%ED%8F%AC-%EB%B0%A9%EC%8B%9D)

---

# 🐳 Kaniko
### 도커 데몬 없이 컨테이너 이미지를 빌드할 수 있는 **Kaniko 사용법**  

프론트엔드 측면에서는 사용자가 웹 서비스 이용 중에도 화면이 끊기지 않고 자연스럽게 새 버전을 경험할 수 있도록 하기 위해 Blue-Green 배포를 적용했습니다. 백엔드의 경우 세션 유지, 트랜잭션 안정성, 데이터 정합성 보장을 위해 기존 환경(Blue)과 새로운 환경(Green)을 분리하여 운영하는 것이 적합했습니다. 이를 통해 사용자 경험 품질을 높이고, 장애 발생 시 신속히 복구할 수 있는 구조를 마련했습니다

👉 자세한 내용은 [📘 카니코 Wiki](https://github.com/beyond-sw-camp/be17-4th-FlowBox-DaBom/wiki/Kaniko-%EC%82%AC%EC%9A%A9-%EB%B0%B0%EA%B2%BD)

---

# ⚙️ Github Actions 🤜🤛 Jenkins
### CI/CD 도구 비교와 **Jenkins 선택 이유 (병렬 처리 활용)**  

Canary 배포 방식은 전체 트래픽 중 일부만 새로운 버전에 흘려보내 실제 사용 환경에서 검증할 수 있다는 장점이 있습니다. 하지만 동시에 여러 버전을 운영하게 되면 API 호환성 문제, 데이터 불일치, 사용자 경험(UI/UX) 혼선이 발생할 가능성이 있습니다. 이러한 복잡성을 관리하기 어려운 상황에서는 Canary 방식보다 Blue-Green 배포가 더 단순하고 안정적이므로, 본 프로젝트에서는 Blue-Green 배포 방식을 선택했습니다.

👉 자세한 내용은 [📘 Github Actions vs Jenkins Wiki](https://github.com/beyond-sw-camp/be17-4th-FlowBox-DaBom/wiki/Github-Actions-vs-Jenkins-%E2%80%93-%EC%84%A0%ED%83%9D-%EC%9D%B4%EC%9C%A0)





