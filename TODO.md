# 작업 리스트

> 진행 상황을 트래킹합니다. `[ ]` 미완료 / `[x]` 완료

---

## 환경 구성

- [x] 개인 노트북에 VM 소프트웨어 설치 (VirtualBox / VMware / KVM)
- [x] VM1 생성 — Rocky Linux 8.10 설치
- [ ] VM2 생성 — Rocky Linux 8.10 설치
- [ ] VM간 내부 네트워크 설정

---

## VM1 — Podman 호스트 설정

- [ ] Rocky 8.10 기본 설정 (hostname, timezone, locale)
- [x] Podman 설치 (`dnf install podman`)
- [ ] firewalld 포트 오픈 (11434, 3000 등)
- [ ] SELinux 설정 확인

---

## Ollama + Gemma 설치

- [x] Ollama 설치 (v0.31.1, systemd 자동 등록)
- [x] Gemma 3 4B 모델 다운로드
- [x] systemd 서비스 등록 (부팅 시 자동 시작)
- [ ] API 동작 확인 (`curl http://localhost:11434/api/tags`)

---

## Podman 서비스

- [ ] Open WebUI 컨테이너 설정
- [ ] Podman Compose 파일 작성
- [ ] 서비스 실행 및 Ollama 연결 확인
- [ ] 웹 브라우저에서 접속 확인

---

## VM2 — 배포 서버 설정

- [ ] Rocky 8.10 기본 설정
- [ ] VM1 서비스 연결 확인
- [ ] 리버스 프록시 설정 (Nginx or Caddy)

---

## 문서화

- [x] LEARNING.md 작성 (기본 개념 정리)
- [x] DEVLOG.md 생성
- [x] TODO.md 생성
- [ ] README.md 작성 (전체 구성 설명)
- [ ] 설치 스크립트 작성 (`scripts/setup.sh`)
