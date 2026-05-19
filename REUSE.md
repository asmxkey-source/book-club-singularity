# 다른 책 모임에 재사용하기

이 대시보드를 새 책 모임에 쓰려면 **새 GitHub 저장소**를 만들고 5단계만 따르면 됩니다.

## 시작 전 확인

| 항목 | 정해두기 |
|---|---|
| 책 제목 / 저자 | 예: `사피엔스 / 유발 하라리` |
| 참여자 이름 (최대 3~5명 권장) | 예: `규리, 진영, 유리` |
| 시작일 (월요일 권장) | 예: `2026-07-06` |
| 시작 페이지 / 끝 페이지 | 예: `1 ~ 580` |
| 회차당 쪽수 | 예: `30` |

---

## 5단계 셋업

### 1. 새 GitHub 저장소 만들기
- GitHub 우상단 `+` → New repository
- 이름 예: `book-club-sapiens` (영문/하이픈만)
- **Public** + **Add a README file** 체크 → Create

### 2. 현재 저장소 파일 복사
- 본 저장소(`book-club-singularity`)에서 다음 파일을 새 저장소로 업로드:
  - `index.html`
  - `chat.txt` (내용은 비워둬도 OK, 새 모임 시작 후 카톡 export로 교체)
  - `README.md`
  - `REUSE.md` (이 파일)
- 업로드 방법: 새 저장소에서 `Add file → Upload files` → 드래그 → Commit

### 3. `index.html`의 CONFIG 객체 수정
`index.html`을 GitHub 웹에서 편집(✏ 연필 아이콘) 또는 GitHub Desktop으로 clone 후 로컬 편집기로 열기. **파일 상단의 `CONFIG` 객체만** 바꾸면 됩니다:

```js
const CONFIG = {
  book: {
    title: '사피엔스',           // ← 책 제목
    author: '유발 하라리',        // ← 저자
  },
  members: ['멤버1', '멤버2', '멤버3'],  // ← 참여자 이름
  startDate: '2026-07-06',       // ← 시작일 YYYY-MM-DD (월요일)
  totalSessions: 20,             // ← 총 회차 수 (= ceil(끝페이지 / 회차당쪽수))
  startPage: 1,                  // ← 시작 페이지
  endPage: 580,                  // ← 끝 페이지
  pagesPerSession: 30,           // ← 회차당 쪽수
  today: null,
};
```

> `totalSessions` 계산: `Math.ceil(endPage / pagesPerSession)`. 예: 580 / 30 = 19.33 → **20**

저장(Commit).

### 4. GitHub Pages 활성화
- 새 저장소 `Settings` → 좌측 `Pages`
- Source: `Deploy from a branch`, Branch: `main`, Folder: `/ (root)` → Save
- 1~2분 후 URL 생성: `https://<본인아이디>.github.io/<새저장소이름>/`

### 5. 단톡방 공지
새 URL을 단톡방에 공유하고, 미션 인증 형식 안내:
```
M/D 이름_N회차 미션완료

(느낀점)
```

---

## 운영 흐름 (기존과 동일)

1. PC 카톡에서 단톡방 대화 내보내기 (텍스트만)
2. 받은 `.txt` 파일을 새 저장소 폴더의 `chat.txt`로 덮어쓰기
3. GitHub Desktop → Summary → Commit → Push
4. 1~2분 후 라이브 URL 새로고침 → 반영 완료

---

## 자주 묻는 질문

**Q. 멤버 수가 다르면?**  
A. `members` 배열에 이름을 추가/제거하면 됩니다. 4명도, 5명도 OK. 너무 많으면 매트릭스가 좁아져 가독성이 떨어지니 5명 이내 추천.

**Q. 주말도 읽고 싶다면?**  
A. 현재 코드는 월~금만 회차로 잡고 주말은 만회용입니다. 주말까지 읽기 회차에 포함하려면 `generateSchedule()` 함수의 `while (cur.getDay() === 0 || cur.getDay() === 6)` 부분을 수정해야 합니다. 필요시 별도 요청.

**Q. 회차당 쪽수를 다르게 (예: 50쪽)?**  
A. `pagesPerSession: 50` 으로만 바꾸면 됩니다. `totalSessions`도 재계산.

**Q. 끝 페이지가 회차당 쪽수의 배수가 아닐 때?**  
A. 마지막 회차가 자동으로 짧아집니다. 예: endPage=580, pagesPerSession=30, totalSessions=20이면 마지막 20회차는 p.571~580 (10쪽).

**Q. 시작일이 화요일이나 다른 요일이라면?**  
A. `startDate`를 그 날짜로 두면 됩니다. 다만 1회차가 그 요일부터 시작되고, 한 주에 1~4회차 정도가 들어가게 됨. 가급적 월요일 시작 권장.

**Q. 이전 모임 기록도 보고 싶다면?**  
A. 본 저장소(`book-club-singularity`)는 그대로 두면 라이브 URL도 살아있습니다. 새 모임은 새 저장소·새 URL로 별도 운영.
