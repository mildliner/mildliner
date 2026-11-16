현재 공개 프로필에는 기여 활동 그래프만 노출한다. 연구·프로젝트 문구는 재개 승인을 받은 뒤에만 다시 추가한다.

# 프로필 유지보수 안내

이 문서는 프로필 콘텐츠, 활동 그래프, 자동 업데이트를 관리하는 방법을 기록한다. 공개 프로필의 연구 9개와 프로젝트 5개, 그리고 추가 카운트 1·1·4·5의 범위는 명시적 승인 없이 바꾸지 않는다.

## 연구와 프로젝트 문구

- README의 연구·프로젝트 항목은 확인된 제목, 기여, 발표 상태를 기준으로 고친다. 사용자 제공 역할과 공동 논문에 적힌 기여는 서로 다른 정보로 기록한다.
- WISA는 Poster, EBISION은 Oral로 표기한다. CISC-W26은 공식 안내상 2026년 10월 28~30일 개최되었다. D2는 연구실 목록에 Oral로 올라 있지만, 개별 논문의 실제 발표 여부는 사용자 확인 중이다. 행사 개최 사실만으로 Presented라고 쓰지 않는다. J2 상태는 별도 확인 전 Accepted로 유지한다.
- I2의 EBISION 2025 논문은 학회 개최 연도(2025), Springer의 온라인 공개일(2026년 8월 2일), 최종 인용 연도(2027)를 구분한다. 권 786, 111–125쪽이다. Springer 페이지와 Crossref 메타데이터를 2026년 11월 16일 확인했다: [공식 Springer 챕터](https://link.springer.com/chapter/10.1007/978-3-032-26616-3_8), [Crossref 메타데이터](https://api.crossref.org/works/10.1007/978-3-032-26616-3_8).
- IEEE·IEICE 등 주최기관의 상태나 개인 역할을 추측하지 않는다. 외부 참고 자료와 본인의 과거 실적을 분명히 구분한다.
- 개인 검토 노트와 민감 정보는 공개 문서에 쓰지 않는다. 필요한 로컬 메모는 저장소의 `.local/` 아래에만 둔다.

## 그래프 모양과 색상

`assets/activity-settings.json`은 공식 액션의 `Settings[]` 형식이다. 각 항목의 `type`은 그래프 종류, `fileName`은 생성 파일명이다. `backgroundColor`는 배경, `foregroundColor`와 `strongColor`·`weakColor`는 텍스트 계열, `radarColor`와 `contribColors`는 그래프 강조색과 기여 단계 색상이다. `growingAnimation: false`는 애니메이션을 끈다. 배열에는 라이트 출력과 다크 출력이 각각 한 설정으로 들어 있다.

색상을 바꿀 때는 파랑 한 가지 계열을 유지하고, 라이트·다크 배경 모두에서 텍스트 대비가 충분한지 확인한다. 이 설정은 한 그래프의 테마 변형 두 개만 생성한다. PNG나 별도 통계 카드를 추가하지 않는다.

최초 SVG 생성 전에는 README에 이미지 임베드를 추가하지 않는다. 콘텐츠 담당자는 텍스트 활동 링크만 두며, 두 SVG 생성과 확인이 끝난 뒤 관리자가 요청한 경우에만 picture 연결을 추가한다.

## 자동화 실행과 확인

워크플로는 매일 03:17 UTC에 실행하며, 한국 표준시(KST)로 매일 12:17이다. 예약 실행은 GitHub의 부하에 따라 늦어질 수 있다. 공개 저장소에서 60일 동안 저장소 활동이 없으면 예약 워크플로가 자동 비활성화될 수 있다. 수동 실행 명령은 다음과 같다.

```sh
# 프로필 저장소에서 그래프 업데이트 워크플로를 수동으로 시작한다.
gh workflow run profile-3d.yml --repo mildliner/mildliner
```

실행 목록과 최신 실행을 확인한다.

```sh
# 최근 그래프 업데이트 실행의 상태를 확인한다.
gh run list --workflow profile-3d.yml --repo mildliner/mildliner --limit 5
```

특정 실행의 실패 로그를 확인한다.

```sh
# 지정한 실행 ID의 실패 단계 로그를 확인한다.
gh run view <run-id> --repo mildliner/mildliner --log-failed
```

생성 단계는 `assets/activity-light.svg`와 `assets/activity-dark.svg`만 검사하고 옮긴다. 변경이 없으면 커밋을 생략한다. 변경이 있으면 해당 두 경로만 stage하고 봇 계정으로 커밋한다. 같은 브랜치의 최신 원격 커밋 위에 rebase한 뒤 일반 push를 수행하므로 강제 덮어쓰기를 하지 않는다. 충돌이나 보호 규칙으로 실패하면 해당 실행 로그를 보고 수동 검토한다. 워크플로가 자기 자신을 변경하지 않으며, 반복 실행 루프를 만들지 않는다.

기본 `GITHUB_TOKEN`만 사용하고 개인 토큰을 저장하지 않는다. 토큰 권한은 그래프 파일을 커밋하기 위한 `contents: write`뿐이다.

## 액션 버전 갱신

액션 버전을 바꾸기 전 공식 저장소의 릴리스와 태그가 가리키는 커밋을 확인한다. 태그 문자열만 쓰지 말고 릴리스 버전 주석과 전체 커밋 SHA를 함께 고정한다. 액션의 입력, 출력 파일 경로, Node 런타임 변경사항을 공식 문서에서 확인하고 이 문서의 설명도 함께 갱신한다.

- [프로필 그래프 액션 릴리스](https://github.com/yoshi389111/github-profile-3d-contrib/releases)
- [프로필 그래프 액션 공식 사용법](https://github.com/yoshi389111/github-profile-3d-contrib)
- [actions/checkout 릴리스](https://github.com/actions/checkout/releases)
- [GitHub 예약 워크플로 안내](https://docs.github.com/actions/using-workflows/events-that-trigger-workflows#schedule)
- [GitHub 공개 저장소 비활성 워크플로 정책](https://docs.github.com/actions/using-workflows/disabling-and-enabling-a-workflow#disabling-a-workflow)

현재 워크플로는 `actions/checkout` v7.0.1 (`3d3c42e5aac5ba805825da76410c181273ba90b1`)과 `github-profile-3d-contrib` v0.9.3 (`7d95e7d4cdc028dd1e1cbd957d65f35efb12ae39`)를 사용한다.
