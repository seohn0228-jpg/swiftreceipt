# SwiftReceipt — 작업 규칙

- **대표(사용자)에게 하는 말은 무조건 한국어(해요체)로만 한다.** 툴 화면은 영어(해외 판매용)지만 설명·보고·질문은 전부 한국어로 쓴다. 영어 문구를 보여 줄 때는 뜻을 한국어로 풀어 준다.
- 짧고 쉽게, 결론 먼저. 전문 용어는 풀어서 쓴다.
- **해외 판매 상품**이다. 화면·영수증 글은 영어, 날짜·숫자 표기(US/UK/EU)와 용지(A4/US Letter)는 브라우저 언어로 자동 선택되고 Design에서 바꿀 수 있다.
- 한 파일 앱: `index.html`. 서버 없음, 데이터는 손님 브라우저에만 저장.
- 작업 방식: 작업용 브랜치에서 고치고 시험한 뒤, 대표 확인을 받고 `main`에 합친다(사이트는 GitHub Pages main, 주소 https://seohn0228-jpg.github.io/swiftreceipt/ — 2026-11-27 공개).
- 설정(스크립트 맨 위): `CHECKOUT_URL`(검로드 상품 주소, 비면 "곧 열려요"), `OWNER_CODE_HASHES`(대표 테스트 키의 SHA-256만 저장, **키 자체는 저장소에 절대 쓰지 않는다** — 공개 저장소).
- 무료: 한 달 PDF 3장, 워터마크. 같은 영수증을 30분 안에 다시 뽑으면 세지 않음. Pro: 무제한, 디자인 3종, 로고·서명, 워터마크 없음, 저장한 영수증(최대 20, 내보내면 자동 저장)·열기·복제·백업/복원.
- Pro 판단: 저장된 키를 시작할 때 다시 확인(저장된 pro:true는 무시). 검로드 라이선스(2026-11-27): 상품 SwiftReceipt Pro(id vLo6JYrowpBZ4_jcufsJpw==, https://swiftbill4.gumroad.com/l/receipt, 9달러). `GUMROAD_PRODUCT_IDS`에 묶음 상품 id를 더하면 묶음 키도 받음. 기기 3대(확인 뒤 +1), 방문 때마다(10분에 1번) 재확인, 환불·비활성 키는 Pro 끔, 서버·인터넷 오류는 마지막 확인 뒤 30일까지 유지.
- SwiftBill 연동: 같은 사이트(seohn0228-jpg.github.io)라 SwiftBill 브라우저 저장 내용(`swiftbill.doc.v1`, `swiftbill.history.v1`, `swiftbill.profile.v1`)을 **읽기만** 해서 "Import from SwiftBill"(무료)로 인보이스를 영수증으로 만든다. SwiftBill 저장 형식을 바꾸면 이 기능도 확인한다.
- 빈칸으로 시작(2026-11-27 대표 결정, B안): 새 손님은 모든 칸이 비어 있고 칸 안 회색 "e.g. …" 힌트만 보인다. 화폐는 브라우저 언어로 추측(영국 GBP, 유럽 EUR, 한국 KRW, 일본 JPY, 캐나다 CAD, 호주 AUD, 그 외 USD). 예전 버전이 채워 둔 예시(Mira Chen Studio·Northwind 등, `legacyProfile()`·`legacyDoc()`)가 저장돼 있으면 시작할 때 조용히 지우고, 그래도 남으면 출력을 막는다(`sampleLeft()`). SwiftBill도 빈칸 시작이다.
- 서버 없는 한계(과장 금지): 코드 수정·시크릿 창·저장소 삭제로 무료 횟수·Pro를 완전히 막을 수 없음.
- 정책 페이지(2026-11-27, SwiftBill과 같은 내용): `privacy.html`, `terms.html`, `refund.html`. 문의 메일 swiftbill26@gmail.com(index.html 아래쪽 링크와 정책 페이지 3개에 들어 있음). 환불: 구매 후 7일 안 전액. 기기 3대·오프라인 30일 문구는 검로드 연결 때 그대로 구현한다.
- 묶음 판매(2026-11-27 대표 결정, 방법 B): 두 앱은 따로 두고 "하나처럼" 잇는다 — 두 앱 위쪽 [Invoices | Receipts] 전환 버튼, 묶음 키 한 번 입력으로 둘 다 Pro(같은 사이트 저장 공간 공유, 기기 수는 브라우저당 1번), 가게 정보 공유(선택). 묶음 = 검로드 보통 상품 + 라이선스 키, 추천 14달러(단품 각 9달러). 완전 통합(방법 A)은 묶음 반응을 본 뒤 다시 정함. **대표가 검로드에 SwiftReceipt Pro·묶음 상품을 올린 뒤 말하면 시작**(SwiftBill도 고치므로 백업 브랜치 먼저).
- 검로드 맞춤 판매 페이지(2026-11-27 공개): `marketing/landing.html`(상품 qfuylb = /l/receipt). 고칠 때 검로드 CLI(antiwork/gumroad-cli를 go로 빌드, GUMROAD_ACCESS_TOKEN은 아무 값 — 프록시가 키를 넣음): `gumroad products page preview qfuylb ./landing.html --json --no-input --non-interactive`로 sanitization_report 0개 확인 → `publish` → 실제 페이지에서 구매 버튼이 gumroad.com/checkout까지 가는지 확인. 되돌리기: `gumroad products page clear qfuylb --yes --json --no-input --non-interactive`. 표지·썸네일 원본: marketing/swiftreceipt_cover.png, swiftreceipt_thumb.png.
- 묶음 구현(2026-11-27): 묶음 상품 "Swift Freelancer Kit — Invoices + Receipts"(id 51hSZcZ0TpRMp9nudAF_Aw==, https://swiftbill4.gumroad.com/l/kit, 14달러, 라이선스 키 켬, CLI로 만듦). 두 앱 모두 `BUNDLE_PRODUCT_ID`를 받고, 묶음 키를 넣으면 같은 사이트 공유 저장소 `swiftkit.license.v1`에 적어 다른 앱이 기기를 다시 세지 않고 Pro가 된다(열린 다른 탭도 따라옴). 공유 키는 키가 바뀔 때만 다시 쓴다(탭끼리 확인 요청이 끝없이 오가는 버그를 막음). 환불·비활성이면 두 앱 모두 Pro 끔. 위쪽 [Invoices | Receipts] 전환 버튼(../swiftbill/, ../swiftreceipt/), 480px 이하에서는 남은 무료 횟수 표시를 숨김. Pro 창에 묶음 안내 링크. 묶음 표지·썸네일: marketing/bundle_cover.png, bundle_thumb.png.
- 묶음 맞춤 판매 페이지(2026-11-28 공개): `marketing/kit-landing.html`(상품 id 51hSZcZ0TpRMp9nudAF_Aw== = /l/kit). 고칠 때 위 판매 페이지와 같은 CLI 순서(preview → publish → 결제 화면 확인). 줄 그은 $18은 손으로 쓴 값(단품 9달러×2)이라 단품 가격을 바꾸면 같이 고친다. 색인용 파일(2026-11-28): googlec693956fd0e0bc05.html, sitemap.xml, robots.txt(하위 폴더라 효과 적음, 서치 콘솔에 사이트맵 직접 제출), index.html canonical.
