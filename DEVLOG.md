# Dev Log

> 작업하면서 겪은 이슈, 해결 방법, 메모를 날짜별로 기록합니다.

---

## 2026-07-01

### 프로젝트 시작

- 개인 노트북에 VM 2대 구성 계획
  - VM1: Podman 호스트 (Ollama + 컨테이너 서비스)
  - VM2: 배포 서버
- OS: Rocky Linux 8.10
- AI 엔진: Gemma (Ollama로 서빙)
- 네트워크: 폐쇄망
- GitHub repo 생성: `podman-gemma`

### 환경 구성

- 머신 구성 확정
  - macOS (MacBook Pro 2018, Intel i7, RAM 16GB): VM1 호스트
  - Windows: VM2 호스트
- VM 소프트웨어 선택: VMware Fusion 13 (Intel Mac, 개인용 무료)
- Rocky Linux 8.10 x86_64 DVD ISO 다운로드 중 (13.2GB)

### 이슈

- **문제**: GitHub push 시 403 Permission denied
- **원인**: 로컬 git이 `seo-dongchan` 계정 HTTPS 인증 상태, repo는 `seo337dc` 소유
- **해결**: `gh auth login` 으로 `seo337dc` 재인증 후 remote URL을 토큰 포함 HTTPS로 변경

```bash
git remote set-url origin https://seo337dc:$(gh auth token)@github.com/seo337dc/podman-gemma.git
```

---

<!-- 아래 형식으로 계속 추가 -->

<!-- 
## YYYY-MM-DD

### 작업 내용

- 

### 이슈

- **문제**: 
- **원인**: 
- **해결**: 

### 메모

- 
-->
