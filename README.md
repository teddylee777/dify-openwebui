# 🚀 Dify + OpenWebUI 설치 가이드

이 문서는 Dify와 OpenWebUI를 Docker 및 Ollama를 활용하여 설치하는 방법을 단계별로 안내합니다. 또한 Git을 이용해 프로젝트 파일을 다운로드하고 환경을 설정하는 방법, 그리고 컨테이너 실행 및 API 연결 설정 등 전체 워크플로우 구성에 관한 내용을 다룹니다.

> 📘 **UBUNTU 설치 가이드**는 [여기](README_UBUNTU.md) 문서를 참고해 주세요.

---

## 📋 목차

1. [Docker Desktop 설치](#1-docker-desktop-설치)
2. [Ollama 다운로드 및 설치](#2-ollama-다운로드-및-설치)
3. [Git 다운로드 및 설치](#3-git-다운로드-및-설치)
4. [Git으로 프로젝트 다운로드](#4-git으로-프로젝트-다운로드)
5. [프로젝트 파일 열기](#5-프로젝트-파일-열기)
6. [.env 설정](#6-env-설정)
7. [Docker 컨테이너 실행](#7-docker-컨테이너-실행)
8. [컨테이너 중지](#8-컨테이너-중지)
9. [포트(Port) 설정](#9-포트port-설정)
10. [OpenWebUI 설정](#10-openwebui-설정)
11. [관리자 패널 및 API 연결 설정](#11-관리자-패널-및-api-연결-설정)
12. [모델 활성화 및 채팅 설정](#12-모델-활성화-및-채팅-설정)
13. [Dify 접속 및 워크플로우 구성](#13-dify-접속-및-워크플로우-구성)
14. [Ollama 모델 추가 예시](#14-ollama-모델-추가-예시)

---

## 1. Docker Desktop 설치

Docker Desktop은 컨테이너 기반 애플리케이션 실행을 위한 필수 플랫폼입니다.

### 📥 1.1 다운로드 및 설치

- **다운로드 링크**: https://www.docker.com/get-started/
- 설치 후 Docker Desktop의 dashboard를 실행하여 정상 설치 여부를 확인합니다.

> 💡 **Mac 사용자 팁**: 상단의 Docker 아이콘을 클릭 후 "Go to Dashboard"를 선택합니다.

![Docker Desktop Dashboard](assets/Capture-20250119-003315.png)  
![Mac Dashboard 1](assets/Capture-20250119-003506.png)  
![Mac Dashboard 2](assets/Capture-20250119-003418.png)

> 🔍 **참고사항**: Docker Desktop이 정상적으로 실행되지 않으면 시스템을 재부팅한 후 다시 시도해보세요.

---

## 2. Ollama 다운로드 및 설치

Ollama는 로컬에서 대규모 언어 모델(LLM)을 실행할 수 있게 해주는 플랫폼입니다.

### 🤖 2.1 임베딩 모델 설정

1. **터미널을 열고, 다음 명령어로 설치된 모델 리스트를 확인합니다.**

   ```bash
   ollama list
   ```

   > ✅ **알아두세요**: 최초 설치 시 리스트가 비어있다면 정상입니다.

2. **`bge-m3` 임베딩 모델 다운로드:**

   ```bash
   ollama pull bge-m3
   ```

   > 📊 **모델 정보**: `bge-m3`는 다국어를 지원하는 고성능 임베딩 모델로, 문서 검색과 유사도 계산에 탁월합니다.

![임베딩 모델 다운로드](assets/Capture-20250119-005907.png)  
![bge-m3 모델 확인](assets/Capture-20250119-010023.png)

### 🚀 2.2 오픈 LLM 모델 다운로드 및 실행

1. **`dnotitia/dna` 모델 다운로드:**

   ```bash
   ollama pull dnotitia/dna
   ```

   > 📌 **모델 크기**: 약 4.7GB의 공간이 필요합니다. 충분한 저장 공간을 확보해주세요.

2. **모델 실행 상태 확인** (이미 실행 중이면 이 단계를 건너뛰세요):

   ```bash
   ollama run dnotitia/dna
   ```

   ![dnotitia/dna 모델 다운로드](assets/Capture-20250204-162917.png)

   실행 후 `/bye` 입력하여 종료합니다.

> 🔔 **도움말**: 다른 모델을 사용하고 싶다면 [Ollama 모델 라이브러리](https://ollama.com/library)에서 검색해보세요.

---

## 3. Git 다운로드 및 설치

Git은 버전 관리 시스템으로, 프로젝트 파일을 다운로드하고 관리하는 데 사용됩니다.

### 🍎 3.1 MacOS / Linux

- **참고 영상**:  
  https://youtu.be/mVu6Wj8Z7C0?si=Fh1Eu6j9q9IcXnaE&t=1311
- **Brew를 통한 설치 참고**:  
  https://teddynote.com/10-RAG%EB%B9%84%EB%B2%95%EB%85%B8%ED%8A%B8/%ED%99%98%EA%B2%BD%20%EC%84%A4%EC%A0%95%20(Mac)/

> 💻 **빠른 설치 (Mac)**:
> ```bash
> brew install git
> ```

### 🪟 3.2 Windows

- **참고 영상**:  
  https://youtu.be/mVu6Wj8Z7C0?si=Wr-CUNF0D8XY12yM&t=585
- **설치 매뉴얼**:  
  https://teddynote.com/10-RAG%EB%B9%84%EB%B2%95%EB%85%B8%ED%8A%B8/%ED%99%98%EA%B2%BD%20%EC%84%A4%EC%A0%95%20(Windows)/

> ⚠️ **Windows 사용자 주의사항**: Git 설치 시 "Git Bash Here" 옵션을 꼭 선택하세요.

---

## 4. Git으로 프로젝트 다운로드

### 📁 4.1 프로젝트 클론하기

1. **터미널(Windows 사용자는 PowerShell)에서 다운로드 경로로 이동합니다.**

   ```bash
   # 예시: 도큐먼트 폴더로 이동할 경우
   cd ~/Documents
   ```

   > 💡 **Windows 사용자**:
   > ```bash
   > cd C:\Users\%USERNAME%\Documents
   > ```

2. **다음 명령어를 실행하여 프로젝트 파일을 클론합니다.**

   ```bash
   git clone https://github.com/teddylee777/dify-openwebui.git
   ```

3. **docker 폴더로 이동:**

   ```bash
   cd dify-openwebui/docker
   ```

> 🎯 **성공 확인**: `ls` (Mac/Linux) 또는 `dir` (Windows) 명령어로 파일 목록을 확인하세요.

---

## 5. 프로젝트 파일 열기

프로젝트 폴더를 Cursor 또는 VS Code 등에서 열어 작업합니다.

![프로젝트 파일 열기](assets/Capture-20250119-020123.png)  

> 🛠️ **추천 에디터**:
> - [Visual Studio Code](https://code.visualstudio.com/)
> - [Cursor](https://cursor.sh/)

---

## 6. .env 설정

### ⚙️ 6.1 환경 변수 파일 설정

1. **`docker` 폴더 내부의 `.env.teddynote` 파일을 `.env`로 이름 변경합니다.**
2. **`.env` 파일의 하단에 데이터 저장 경로를 지정합니다.**

> 📂 **경로 설정 예시**

**Mac/Linux:**
```bash
OPENWEBUI_LOCAL_DATA=/Users/teddy/Dev/openwebui-dify/openwebui
PIPELINES_LOCAL_DATA=/Users/teddy/Dev/openwebui-dify/pipelines
```

**Windows:**
```bash
OPENWEBUI_LOCAL_DATA=C:/Users/teddy/Dev/openwebui-dify/openwebui
PIPELINES_LOCAL_DATA=C:/Users/teddy/Dev/openwebui-dify/pipelines
```

![Mac / Linux 경로 설정](assets/Capture-20250204-163817.png)  
![Windows 경로 설정](assets/Capture-20250204-163855.png)

> ⚠️ **중요**: 설정한 경로에 해당 폴더가 없으면 미리 생성해주세요!

---

## 7. Docker 컨테이너 실행

### 🐳 7.1 컨테이너 시작하기

1. **터미널에서 `docker` 폴더로 이동합니다.**

   ```bash
   cd dify-openwebui/docker
   ```

2. **`docker-compose-teddynote.yaml` 파일을 이용해 컨테이너 실행:**

   - **방법 1** (Docker Compose V2 - 권장):

     ```bash
     docker compose -f docker-compose-teddynote.yaml up -d
     ```

   - **방법 2** (Docker Compose V1):

     ```bash
     docker-compose -f docker-compose-teddynote.yaml up -d
     ```

![컨테이너 실행 모습](assets/Capture-20250119-003138.png)

> 🔄 **첫 실행 시간**: 이미지 다운로드로 인해 5-10분 정도 소요될 수 있습니다.

> 📝 **로그 확인**: 실행 중 문제가 생기면 `-d` 옵션을 제거하고 실행하여 로그를 확인하세요.

---

## 8. 컨테이너 중지

### 🛑 8.1 컨테이너 종료 방법

**모든 컨테이너를 중지 및 삭제하려면:**

```bash
docker stop $(docker ps -q) && docker rm $(docker ps -aq)
```

**또는 Docker Desktop의 "Containers" 메뉴에서:**
1. 전체 선택
2. "Delete" 클릭

![컨테이너 중지](assets/Capture-20250119-004252.png)

> 💾 **데이터 보존**: 컨테이너를 삭제해도 볼륨에 저장된 데이터는 유지됩니다.

---

## 9. 포트(Port) 설정

### 🔌 9.1 기본 포트 정보

설정된 포트는 다음과 같습니다:

| 서비스 | 포트 | 설명 |
|--------|------|------|
| Dify API | 80 | Dify 메인 서비스 |
| OpenWebUI | 3001 | 웹 UI 인터페이스 |
| Ollama | 11434 | LLM 모델 API |
| Pipeline | 9099 | OpenWebUI Pipeline |

> 🌐 **외부 접속 설정**: 외부에서 접속하려면 방화벽과 포트포워딩 설정을 확인하세요.

> ⚡ **포트 충돌 해결**: 이미 사용 중인 포트가 있다면 `.env` 파일에서 포트 번호를 변경할 수 있습니다.

---

## 10. OpenWebUI 설정

### 🖥️ 10.1 초기 접속 및 계정 생성

1. **웹 브라우저에서 OpenWebUI 접속:**  
   
   주소: http://localhost:3001/
   
2. **새 계정을 생성 후 로그인합니다.**
   
   > 👤 **첫 번째 계정**: 최초로 생성하는 계정이 자동으로 관리자 권한을 갖습니다.

![OpenWebUI 접속](assets/Capture-20250119-011312.png)

---

## 11. 관리자 패널 및 API 연결 설정

### 🔐 11.1 OpenAI API 연결 관리

1. **관리자 패널 접속:**
   - 프로필 > 관리자 패널 > 설정 > 연결

2. **"OpenAI API 연결" 영역의 토글 버튼을 활성화합니다.**

3. **우측 톱니바퀴 아이콘을 클릭하여 API 키를 입력하고 저장합니다.**

![OpenAI API 연결](assets/Capture-20250204-170147.png)  
![API 키 입력](assets/Capture-20250204-170323.png)

> 🔑 **API 키 발급**: [OpenAI Platform](https://platform.openai.com/)에서 API 키를 발급받을 수 있습니다.

### 🔗 11.2 Pipeline 연결 설정

1. **관리자 패널의 연결 메뉴에서 "OpenAI API 연결 관리" 영역 우측의 + 버튼을 클릭합니다.**

2. **아래 정보를 입력합니다:**
   - **URL**: `http://host.docker.internal:9099`
   - **Key**: `0p3n-w3bu!`

![Pipeline 연결 설정](assets/Capture-20250204-170410.png)

> 🔧 **문제 해결**: 연결이 안 되면 `host.docker.internal` 대신 `localhost`를 사용해보세요.

### 🤖 11.3 Ollama API 연결 지정

Ollama API 연결은 `http://host.docker.internal:11434`를 사용합니다.

![Ollama API 연결](assets/Capture-20250204-170441.png)

---

## 12. 모델 활성화 및 채팅 설정

### 💬 12.1 모델 활성화

1. **관리자 패널에서 `dnotitia/dna` 모델의 토글 버튼을 눌러 활성화합니다.**

![모델 활성화](assets/Capture-20250204-160422.png)

### 🗨️ 12.2 채팅 테스트

2. **좌측 상단 "New Chat" 버튼 클릭 후 채팅창에서:**
   - `dnotitia/dna` 모델 선택
   - 테스트 메시지 입력

![채팅 테스트](assets/Capture-20250204-160543.png)

### 📋 12.3 파이프라인 설정

3. **왼쪽 하단 계정 클릭 후 관리자 패널로 이동하여, 파이프라인 관리 URL이 `http://host.docker.internal:9099`로 설정되어 있는지 확인합니다.**

![파이프라인 URL 확인](assets/Capture-20250119-011455.png)

4. **설정 > 파이프라인 메뉴에서 제공된 `dify_pipeline_local.py` 파일을 업로드합니다.**
   - 📥 파일 다운로드: https://link.teddynote.com/dify_pipeline_local

![파이프라인 파일 업로드](assets/Capture-20250119-012311.png)

> 🎯 **파이프라인 용도**: Dify와 OpenWebUI를 연결하여 워크플로우를 실행할 수 있게 해줍니다.

---

## 13. Dify 접속 및 워크플로우 구성

### 🔄 13.1 Dify 워크플로우 설정

1. **웹 브라우저에서 Dify 접속:**  
   
   링크: http://localhost/apps

2. **워크플로우 화면으로 이동 후, 제공된 DSL 파일을 import하여 테스트합니다.**
   - 왼쪽 메뉴에서 "DSL 파일 가져오기" 클릭
   - 테스트용 파일 (예: 테디노트 챗봇.yml) import

![DSL 파일 가져오기](assets/Capture-20250119-012440.png)  
![DSL 파일 import](assets/Capture-20250119-012520.png)

> 📚 **DSL 파일**: Dify Specification Language 파일로, 워크플로우의 구조를 정의합니다.

### 🔑 13.2 API 키 설정

3. **상용 모델 API 키 설정:**
   - 우측 상단 **계정** - **설정**
   - "모델 제공자" 항목에 API 키 입력

![모델 API 키 설정](assets/Capture-20250119-013406.png)  
![모델 제공자 설정](assets/Capture-20250119-013331.png)

> 💰 **비용 관리**: API 사용량을 모니터링하고 예산 한도를 설정하는 것을 권장합니다.

---

## 14. Ollama 모델 추가 예시

### ➕ 14.1 커스텀 모델 추가하기

**우측 상단 프로필 > 설정 > 왼쪽 탭 "모델 제공자" > Ollama 모델 추가**

다음과 같이 설정하여 `dnotitia/dna` 모델을 추가합니다:

![Ollama 모델 추가 예시](assets/Capture-20250204-172412.png)

> 🎨 **다양한 모델 활용**: 
> - **코딩**: `codellama`, `deepseek-coder`
> - **한국어**: `ggk/korean-llm`, `davidkim205/komt-llama2`
> - **멀티모달**: `llava`, `bakllava`

---

## 🆘 문제 해결 가이드

### ❓ 자주 발생하는 문제들

**Q: Docker 컨테이너가 시작되지 않아요.**
- A: Docker Desktop이 실행 중인지 확인하고, 로그를 확인해보세요.

**Q: 모델 다운로드가 느려요.**
- A: 네트워크 상태를 확인하고, VPN을 사용 중이라면 잠시 끄고 시도해보세요.

**Q: API 연결이 안 돼요.**
- A: 방화벽 설정을 확인하고, 포트가 올바르게 열려있는지 확인하세요.

---

## 📚 추가 리소스

- 📖 [Dify 공식 문서](https://docs.dify.ai/)
- 📖 [OpenWebUI 공식 문서](https://docs.openwebui.com/)
- 📖 [Ollama 공식 문서](https://github.com/ollama/ollama)
- 🎥 [테디노트 YouTube 채널](https://www.youtube.com/@teddynote)

---

## 🤝 기여하기

이 프로젝트에 기여하고 싶으시다면:
1. Fork 하기
2. Feature 브랜치 생성 (`git checkout -b feature/AmazingFeature`)
3. 커밋 (`git commit -m 'Add some AmazingFeature'`)
4. 브랜치에 Push (`git push origin feature/AmazingFeature`)
5. Pull Request 열기

---

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

---

**🎉 설치를 완료하신 것을 축하합니다! 이제 Dify와 OpenWebUI를 활용하여 강력한 AI 워크플로우를 구성해보세요.**