# yue2.cpp 프롬프트 스킬

버전 1.0.0. Codex에서 노래의 방향을 한국어로 함께 정하고 yue2.cpp용 영어 스타일과 가사를 작성하는 스킬입니다. yue2.cpp 공식 배포물이 아닌 별도 대화형 작업 지침입니다.

## 사용에 필요한 구성요소

**이 스킬만 설치해서는 음악이 생성되지 않습니다.** 이 저장소의 음악 생성 절차를 사용하려면 **YuE2 모델과 이를 실행하는 yue2.cpp**가 별도로 필요합니다. YuE2와 yue2.cpp를 서로 독립된 음악 앱 두 개로 설치하는 뜻은 아닙니다. yue2.cpp가 YuE2 모델 파일을 불러와 실행합니다.

| 구성요소 | 역할 | 안내 및 다운로드 |
| --- | --- | --- |
| 이 Codex 스킬 | 질문을 통해 곡을 구체화하고 스타일·가사를 작성 | [스킬 본문](yue2-prompt/SKILL.md) |
| YuE2-3B | 음악을 생성하는 기반 모델 | [MAP 공식 모델과 설명](https://huggingface.co/m-a-p/YuE2-3B) |
| YuE2-Vae | 모델의 합성 결과를 실제 오디오로 변환 | [MAP 공식 VAE 모델](https://huggingface.co/m-a-p/YuE2-Vae) |
| yue2.cpp | 모델을 로컬에서 실행하고 웹 입력 화면·생성 API 제공 | [프로젝트 및 설치 안내](https://github.com/ServeurpersoCom/yue2.cpp#readme) |
| YuE2 GGUF 파일 | yue2.cpp에서 불러오는 모델 형식 | [yue2.cpp 배포자의 모델 안내](https://huggingface.co/Serveurperso/YuE2-GGUF) · [파일 목록](https://huggingface.co/Serveurperso/YuE2-GGUF/tree/main) |
| Codex와 브라우저 제어 도구 | 스킬 실행 및 화면 자동 입력·조작 | 사용 중인 Codex 환경에서 스킬과 브라우저 제어 기능을 준비 |

```text
사용자의 노래 설명
  → Codex + 이 스킬: 질문 / 영어 스타일 / 선택한 언어의 가사
  → yue2.cpp: 입력을 받아 YuE2 모델 실행
  → YuE2 기반 모델 + VAE: 음악 합성
  → MP3 또는 WAV 저장
```

### 준비 순서

1. [yue2.cpp README](https://github.com/ServeurpersoCom/yue2.cpp#readme)에서 자신의 운영체제와 하드웨어에 맞는 빌드·실행 방법을 확인합니다.
2. [GGUF 모델 파일 목록](https://huggingface.co/Serveurperso/YuE2-GGUF/tree/main)에서 기반 모델과 VAE를 각각 준비합니다. 프로젝트 안내의 기본 조합은 `YuE2-3B-Q8_0.gguf`와 `YuE2-Vae-F32.gguf`입니다. 기본 새 곡 생성에는 선택적 전사 모델이 필요하지 않습니다.
3. yue2.cpp를 실행하고 웹 화면이 열리는지 확인합니다. 기본 포트를 그대로 사용했다면 같은 컴퓨터에서 `http://127.0.0.1:8087`로 접속할 수 있습니다. 포트는 실제 설치 설정을 따릅니다.
4. 아래 안내에 따라 이 스킬을 설치하고 노래 제작을 요청합니다. 이 구성에서는 별도로 YuE2의 Python 추론 패키지를 중복 설치할 필요가 없습니다.
5. 브라우저 제어 기능이 없다면 스킬이 작성한 `Style`과 `Lyrics`를 yue2.cpp의 각 입력란에 직접 붙여 넣어 사용할 수도 있습니다.

모델·메모리 요구량과 실행 방법은 원 프로젝트에서 업데이트될 수 있으므로 연결된 안내를 기준으로 확인하세요. 위 링크는 2026-09-24에 확인했습니다.

## 특징
- 제목부터 한 번에 한 항목씩 번호로 질문합니다.
- 0번은 해당 항목을 맡기는 선택입니다. 전체 위임과 구분합니다.
- 보컬, 편곡, 가사와 길이 등을 정한 뒤 초안을 검토합니다.
- 스타일은 영어, 가사는 선택한 언어로 작성합니다.
- 생성 설정은 길이·개수·파일 형식만 질문하고 내부 설정은 추천값을 설명합니다.
- 사용 가능한 브라우저 제어 도구로 화면 입력과 생성, 결과 확인을 진행할 수 있습니다.

## 설치와 사용
`yue2-prompt` 폴더를 Codex의 스킬 디렉터리(`$CODEX_HOME/skills`, 미설정 시 `~/.codex/skills`)에 넣습니다. 기존 동명 스킬이 있으면 덮어쓰기 전에 비교하세요. 스킬이 인식된 세션에서 다음처럼 요청합니다.

> $yue2-prompt 새 노래를 만들고 싶어. 제목부터 하나씩 질문해줘.

텍스트 작성에는 로컬 음악 모델이 필요하지 않습니다. 실제 음악 생성에는 별도로 설치하고 실행한 [yue2.cpp](https://github.com/ServeurpersoCom/yue2.cpp), 모델 파일, 실행 가능한 하드웨어가 필요합니다. 화면 자동 조작에는 Codex에서 사용 가능한 브라우저/앱 제어 도구가 필요합니다.

## 곡별 저장 폴더

새 곡은 제작 시작 시점의 한국 시간(`Asia/Seoul`)을 붙여 `YYYY-MM-DD_HH-mm_곡 제목` 폴더에 보관합니다.

예: `2026-09-24_15-19_내 사랑 내 곁에`

음원·가사·스타일·생성 설정·작업 기록을 함께 저장합니다. 기존 폴더 이름은 바꾸지 않으며, 같은 시각·제목이 겹치면 접미사로 구분합니다. 이 정리 방식은 이 스킬의 작업 규칙이며 yue2.cpp 자체의 기본 저장 기능은 아닙니다.

## 현재 범위
UI 입력·생성 및 완료 결과 회수를 한 곡으로 확인했습니다. 전용 API 실행 스크립트와 MCP 서버는 포함되어 있지 않습니다. 생성 품질·발음·기존 음악과의 비유사성을 보장하지 않습니다. 모델 및 yue2.cpp의 이용 조건은 각각 별도로 확인하세요.

이 공유본에는 개인 계정, 생성 음원·가사, 모델, 로그, 개인 설치 절대경로를 포함하지 않습니다.

## 출처와 감사

이 스킬은 [ServeurpersoCom/yue2.cpp](https://github.com/ServeurpersoCom/yue2.cpp)의 입력 형식과 공개 예제를 참고해 작성했습니다. 음악 생성은 yue2.cpp와 MAP의 YuE2 모델이 담당하며, 이 저장소는 사용자와 노래 방향을 구체화하는 대화 및 입력 절차를 제공합니다.

- [yue2.cpp 기술 문서](https://github.com/ServeurpersoCom/yue2.cpp/blob/master/docs/ARCHITECTURE.md)
- [yue2.cpp 공식 예제](https://github.com/ServeurpersoCom/yue2.cpp/tree/master/tools/webui/example)
- [MAP YuE2 모델](https://huggingface.co/m-a-p/YuE2-3B)

공개된 음악 생성 도구를 활용하며 얻은 프롬프트 작성 절차를 다른 사용자와 나누기 위해 공개합니다. 원 프로젝트와 모델의 권리 및 이용 조건은 해당 프로젝트에 속합니다. 공식 제휴 또는 보증을 의미하지 않습니다.
