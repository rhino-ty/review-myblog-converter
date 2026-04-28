# review-myblog-converter

> 옵시디언 리뷰 노트를 *내* 블로그 톤(네이버 블로그·티스토리·벨로그)으로 변환하는 Claude Skill.

[![Made with](https://img.shields.io/badge/Made%20with-Claude%20Skills-blueviolet)](https://docs.claude.com)

---

## 무엇을 하는 스킬인가

옵시디언에 깊이 정리한 리뷰 노트는 그 자체로 좋지만, 블로그에 그대로 올리면 너무 무겁고 길다. 이 스킬은 옵시디언의 *결정체*만 추출해서 **블로그 톤으로 가볍게 재작성**한다.

핵심은 "같은 데이터, 다른 톤":

| 옵시디언 (1차) | 블로그 (2차) |
|---------|-------|
| 무거움, 자기대화 | 가벼움, 독자 호출 |
| 길고 진중 | 짧고 펀치 |
| 4차원 평점 인라인 | 추천/비추천 |
| 분석 + 감정 다 풀음 | *가장 박힌* 1~3개만 |
| 한 글에 모든 차원 | 글 분량 = 노트의 1/3~2/5 |

## 짝꿍 스킬

이 스킬은 **[`polymedia-review-skill`](https://github.com/YOUR_USERNAME/polymedia-review-skill)** 과 짝을 이룬다.

```
polymedia-review-skill          # 인터뷰 → 옵시디언 노트
   ↓ (생성된 노트)
review-myblog-converter         # 노트 → 블로그 글
   ↓
네이버 블로그 / 티스토리 / 벨로그
```

단, 이 스킬은 *어떤 형태의 노트나 텍스트*도 인풋으로 받을 수 있다. polymedia-review-skill 종속이 아님.

## 핵심 기능

- **5단계 변환 워크플로우** — 인풋 확인 → 후킹 제목 3안 → 결정체 추출 → 톤 재작성 → 이미지 자리/추신
- **사용자 블로그 톤 가이드** — 정중체+반말 섞기, 강도 있는 감탄+근거, 자조·깐족 결합
- **AI 티 제거 규칙** — em dash, 평면 형용사, 클리셰 도입 차단
- **플랫폼별 미세 조정** — 네이버/티스토리/벨로그 각각의 결에 맞춤
- **자가 점검 체크리스트** — 발행 전 8가지 확인 항목

## 디렉토리 구조

```
review-myblog-converter/
├── SKILL.md                    # 5단계 워크플로우 + 트리거
├── ref/
│   └── myblog-tone.md          # 사용자 블로그 톤 가이드 (살릴 것/뺄 것)
└── examples/
    └── kcd2-conversion-example.md   # 변환 예시 (킹덤 컴 딜리버런스 2)
```

## 설치

### 방법 1: 직접 패키징

[skill-creator](https://github.com/anthropics/skills/tree/main/skill-creator) 의 `package_skill.py` 사용:

```bash
python -m scripts.package_skill /path/to/review-myblog-converter /path/to/output
```

### 방법 2: 릴리스에서 다운로드

[Releases](../../releases) 페이지에서 `review-myblog-converter.skill` 파일 다운로드.

### Claude.ai에 업로드

1. Claude.ai → **Settings → Capabilities → Skills**
2. **Upload Skill** 선택
3. `review-myblog-converter.skill` 파일 업로드 후 활성화

## 사용 흐름

```
옵시디언 리뷰 노트 또는 자유 텍스트
  ↓
"이거 블로그용으로 변환해줘" / "내 블로그 톤으로"
  ↓
Step 1: 인풋 확인
  ↓
Step 2: 플랫폼 + 후킹 제목 3안 제안
  ↓
Step 3: 결정체 추출 (노트의 어디를 살리고 어디를 빼는가)
  ↓
Step 4: 사용자 톤으로 재작성
  ↓
Step 5: 이미지 자리 + 추신
  ↓
블로그 발행용 마크다운
```

## 트리거 발화 예시

- "이거 블로그용으로 변환해줘"
- "네이버 블로그/티스토리/벨로그용으로 써줘"
- "내 블로그 톤으로"
- "이 노트 가볍게 독자용으로"
- 옵시디언 리뷰 노트 첨부 + "블로그에 올리고 싶어"

## 톤 핵심 규칙

✅ **살릴 것**

- 호흡 짧게 (단락 1~2문장)
- 정중체+반말 자유롭게 섞기
- 강도 있는 감탄 ("미쳤다", "지렸다") + *그 직후 근거*
- 약한 욕설 강조용 ("개", "ㅈㄴ" 1~2번)

❌ **뺄 것**

- em dash (`—`) — AI 티 가장 강함
- 평면 형용사 ("진짜 좋다", "정말 재밌다")
- 강한 욕설 ("시발", "씨발", "병신")
- 다른 블로거 시그니처 직접 차용 ("레지고", "끝." 등)

세부는 [`ref/myblog-tone.md`](ref/myblog-tone.md) 참조.

## 모델 권장

문체 모방 + 톤 변환은 강한 추론을 요구한다.

- ✅ 권장: Claude Opus 4.6 / Gemini 2.5 Pro / GPT-o3
- ❌ 비권장: Haiku, Flash, mini 등 경량 모델

## 라이센스

MIT License. 자세한 내용은 [LICENSE](LICENSE) 참조.

## Credits

- 톤 가이드 학습 베이스: 후루꾸 블로그(영감) + 사용자 본인의 발행 글
- 짝꿍 스킬: [`polymedia-review-skill`](https://github.com/YOUR_USERNAME/polymedia-review-skill)
