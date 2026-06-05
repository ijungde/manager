# 🎯 CoachingManager

학원 원생 관리 + 다지점 통합 관리 시스템

## 파일 구성

```
coaching_manager_auth.html   ← 본사 포털 (지점 선택 + 통합 대시보드)
academy_manager.html         ← 지점 앱 (원생 관리 실제 기능)
```

## 권한 구조

| 접속 방식 | 가능한 것 |
|-----------|-----------|
| 비로그인 방문 | 전체 지점 데이터 열람만 가능 |
| 지점 PIN 로그인 | 해당 지점 데이터 수정 가능 |
| 본사 PIN 로그인 | 전체 수정 + 지점 추가/관리 |

## 데모 PIN (테스트용 — 실제 배포 시 반드시 변경)

| 지점 | PIN |
|------|-----|
| 강남점 | 1234 |
| 분당점 | 2345 |
| 목동점 | 3456 |
| 수원점 | 4567 |
| 인천점 | 5678 |
| **본사 마스터** | **000000** |

> ⚠️ 배포 전 `coaching_manager_auth.html` 상단의 `HQ_PIN`과 각 지점 PIN을 반드시 변경하세요.

## GitHub Pages 배포 방법

1. GitHub 저장소 생성 (Public 권장 — 열람 공개 목적)
2. 두 HTML 파일 업로드
3. Settings → Pages → Branch: main → Save
4. `https://[계정명].github.io/[저장소명]/coaching_manager_auth.html` 접속

## 운영 방법

### 지점 추가
1. 본사 PIN으로 로그인
2. 사이드바 하단 `+ 지점 추가` 클릭
3. 지점명, 지역, PIN 입력

### 지점 사용
1. `coaching_manager_auth.html` 접속
2. 사이드바에서 지점 클릭
3. 해당 지점 PIN 입력 → 수정 모드 활성화

## 다음 단계 (Supabase 연동)

현재는 `localStorage` 기반이라 **같은 기기**에서만 데이터가 유지됩니다.
여러 기기/지점에서 실시간 공유하려면 Supabase 연동이 필요합니다.

```
1. supabase.com 무료 계정 생성
2. 프로젝트 생성 → API Key 복사
3. coaching_manager_auth.html의 저장 함수를 Supabase API 호출로 교체
   saveBranches() → supabase.from('branches').upsert(...)
4. 실시간 구독 추가
   supabase.channel('branches').on('postgres_changes', ...).subscribe()
```

자세한 코드는 별도 요청 시 제공 가능합니다.

## 보안 주의사항

- PIN은 현재 단순 해시 처리 — 실제 배포 시 서버 검증 필요
- GitHub Public 저장소에는 실제 원생 데이터 절대 커밋 금지
- 데이터는 브라우저 localStorage에만 저장됨 (서버 전송 없음)
