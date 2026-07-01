# 설치 가이드

> macOS에서 UTM + Rocky Linux 8.10 + Podman 환경 구성 방법

---

## 준비물

- macOS (Intel) 노트북
- RAM 16GB 이상 권장
- 여유 디스크 80GB 이상

---

## 1단계 — 소프트웨어 다운로드

### UTM 설치
- `https://mac.getutm.app` 접속 → Download
- `.dmg` 실행 후 Applications로 드래그

> ⚠️ App Store의 UTM은 유료. 반드시 공식 사이트에서 무료 버전 다운로드

### Rocky Linux 8.10 ISO 다운로드
- `https://rockylinux.org/download` 접속
- Rocky Linux 8 → AMD/Intel (x86_64) → **DVD ISO** 선택
- 파일명: `Rocky-8.10-x86_64-dvd1.iso` (약 14GB)

---

## 2단계 — UTM에서 VM 생성

1. UTM 실행 → **+** 버튼
2. **Virtualize** 선택
3. **Linux** 선택
4. **Apple 가상화 사용** 체크 해제 (QEMU 사용)
5. **Boot from ISO image** 선택 → **찾아보기** → ISO 파일 선택
6. 하드웨어 설정:
   - 메모리: **8192 MiB** (8GB)
   - CPU 코어: **4**
   - OpenGL 가속: 체크 해제
7. 저장소: **64GB**
8. 공유 디렉터리: 건너뜀
9. 이름: `rocky-vm1` → **저장**

---

## 3단계 — Rocky Linux 8.10 설치

1. VM 실행 (▶ 버튼)
2. 부팅 메뉴에서 **"Install Rocky Linux 8.10"** 선택
3. 설치 요약 화면에서 설정:
   - **설치 목적지**: 64GB 디스크 선택 → 완료
   - **root 비밀번호**: 설정
   - **사용자 생성**: 사용자명 + 비밀번호, 관리자 체크
4. **설치 시작(B)** 클릭
5. 설치 완료 후 **시스템 재시작(R)**

### ISO 제거 (재시작 전 필수)
재시작 시 설치 메뉴가 다시 뜨는 것을 방지:
1. UTM에서 VM 일시정지
2. VM 정보 패널 하단 **CD/DVD** → **초기화**
3. VM 다시 시작

---

## 4단계 — Rocky Linux 초기 설정

1. 라이센스 동의 → **설정 완료(F)**
2. 로그인 (설치 시 생성한 계정)
3. 터미널 열기

### OS 버전 확인
```bash
cat /etc/rocky-release
# Rocky Linux release 8.10 (Green Obsidian)
```

---

## 5단계 — 네트워크 설정

VM 최초 부팅 시 네트워크가 비활성 상태일 수 있음:

```bash
# 네트워크 인터페이스 확인
ip addr

# 네트워크 활성화
sudo nmcli con up enp0s1

# 인터넷 연결 확인
ping -c 3 google.com
```

---

## 6단계 — Podman 설치

```bash
sudo dnf install -y podman

# 버전 확인
podman --version
# podman version 4.9.4-rhel
```

---

## 7단계 — Ollama 설치

```bash
# zstd 먼저 설치 (없으면 Ollama 설치 오류 발생)
sudo dnf install -y zstd

# Ollama 설치
curl -fsSL https://ollama.com/install.sh | sh

# 버전 확인
ollama --version
# ollama version is 0.31.1
```

- API 주소: `http://127.0.0.1:11434`
- systemd 서비스 자동 등록 (부팅 시 자동 시작)
- GPU 없으면 CPU-only 모드로 동작 (정상)

---

## 8단계 — Gemma 모델 다운로드

```bash
# Gemma 3 4B 모델 다운로드 (약 3GB)
ollama pull gemma3:4b

# 동작 테스트
ollama run gemma3:4b "안녕하세요"

# 설치된 모델 목록 확인
ollama list
```

---

## 다음 단계

- [ ] Open WebUI 컨테이너 실행 (Podman)
