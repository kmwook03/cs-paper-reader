# CS Paper Reader

[English](README_EN.md)

컴퓨터과학 논문을 읽고 프로젝트의 템플릿에 따라 리뷰를 작성하는 Codex 스킬입니다. 기본 언어는 한국어이며 영어 리뷰도 지원합니다. 로컬 PDF의 문제 정의, 시스템 설계, 구현, 평가, 한계를 분석하여 `reviews/`에 Markdown으로 저장합니다.

주요 주장과 수치에 절·페이지·표·그림 출처를 남기며, 논문의 설명과 리뷰어의 해석을 구분합니다. 개별 논문 리뷰, 미작성 논문 일괄 리뷰, 기존 리뷰 갱신을 지원합니다.

## 패키지 구성

```text
cs-paper-reader/
├── SKILL.md                 스킬 지침과 트리거
├── agents/openai.yaml       Codex 표시 정보
├── assets/template_KO.md    기본 한국어 리뷰 템플릿
├── assets/template_EN.md    영어 리뷰 템플릿
├── README.md                설치 및 사용 안내
├── README_EN.md             영어 설치 및 사용 안내
└── .gitignore
```

## 설치

저장소를 내려받은 뒤 **이 README가 있는 디렉토리에서** 아래 명령을 실행합니다. 사용자 스킬 디렉토리에 실행에 필요한 파일만 복사하며, 같은 이름의 스킬이 있으면 중단합니다.

### Windows PowerShell

```powershell
$skillTarget = Join-Path $HOME '.agents\skills\cs-paper-reader'
if (Test-Path -LiteralPath $skillTarget) {
    throw "이미 설치된 경로입니다: $skillTarget"
}
New-Item -ItemType Directory -Path $skillTarget -Force | Out-Null
Copy-Item -LiteralPath @('SKILL.md', 'agents', 'assets') -Destination $skillTarget -Recurse
```

### macOS / Linux

```bash
skill_target="${HOME}/.agents/skills/cs-paper-reader"
if [ -e "$skill_target" ]; then
    printf '%s\n' "이미 설치된 경로입니다: $skill_target" >&2
    exit 1
fi
mkdir -p "$skill_target"
cp -R SKILL.md agents assets "$skill_target/"
```

특정 프로젝트에만 설치하려면 해당 프로젝트의 `.agents/skills/cs-paper-reader/`에 같은 세 항목을 복사합니다. 스킬이 목록에 나타나지 않으면 Codex를 재시작합니다. 설치 위치와 검색 방식은 [OpenAI 공식 스킬 문서](https://learn.chatgpt.com/docs/build-skills)를 참고하세요.

## 논문 프로젝트 준비

스킬 소스와 별도로 논문 프로젝트를 준비합니다.

```text
paper-workspace/
├── papers/
│   └── example.pdf
├── templates/
│   └── template_KO.md
└── reviews/
```

논문은 `papers/`에 넣고, [기본 템플릿](assets/template_KO.md)을 `templates/template_KO.md`로 복사하여 수정할 수 있습니다. 기존 템플릿이 있다면 그대로 사용합니다. 프로젝트에 템플릿이 전혀 없을 때는 스킬에 포함된 기본 템플릿을 사용하며, `reviews/`는 작성 시 생성합니다.

영어 리뷰에는 [영어 템플릿](assets/template_EN.md)을 `templates/template_EN.md`로 복사하여 사용할 수 있습니다. 영어 작성을 요청하거나 이 템플릿을 지정하면 영어로 리뷰를 작성합니다. 프로젝트에 템플릿이 없다면 번들 영어 템플릿을 사용합니다.

## 사용 예시

논문 프로젝트에서 다음과 같이 요청합니다.

```text
$cs-paper-reader papers/example.pdf를 리뷰해줘.
```

```text
$cs-paper-reader 아직 리뷰하지 않은 논문을 모두 리뷰해줘.
```

```text
$cs-paper-reader example.pdf의 기존 리뷰를 갱신해줘. 실험 조건과 한계 분석을 보강해줘.
```

```text
$cs-paper-reader papers/example.pdf를 templates/template_EN.md에 따라 영어로 리뷰해줘.
```

다른 작업 디렉토리에서는 프로젝트의 경로를 함께 지정합니다. 사용자 템플릿, 리뷰 언어, 출력 파일명, 읽기 목적도 요청에 명시할 수 있습니다.

기본 출력은 `reviews/example.md`입니다. 논문 하위 폴더 구조를 유지하며, 새 리뷰가 기존 리뷰를 임의로 덮어쓰지 않습니다. 일괄 요청의 범위가 `미작성 논문`이면 기존 리뷰가 있는 논문은 건너뜁니다.

## 실행 환경

로컬 파일을 읽고 쓸 수 있는 Codex와 PDF 텍스트 추출·페이지 렌더링 도구가 필요합니다. 사용 가능한 PDF 스킬과 도구를 활용하며, 스캔본에는 OCR이 필요할 수 있습니다. 이 패키지는 별도 Python 스크립트나 API 키를 요구하지 않습니다.

개인별 절대 경로, 논문 PDF, 생성된 리뷰는 패키지에 포함하지 않습니다.
