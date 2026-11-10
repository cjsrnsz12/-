# 어비스 도우미 배포 지침

사용자는 2026-11-04에 다음 버전부터 완성 즉시 이 GitHub 저장소로 업로드하도록 지시했습니다.

새 버전 제작 작업에는 다음 배포 절차를 포함합니다.

1. 버전명과 versionCode를 올리고, 변경 사항에 필요한 검사 및 APK 빌드·서명 검증을 완료합니다. 기존 설치를 업데이트할 수 있도록 서명을 유지합니다.
2. 완성된 APK를 main 브랜치의 AbyssHelper-latest.apk로 업로드합니다.
3. 업로드된 파일의 크기와 해시가 원본과 일치하는지 확인합니다.
4. APK 등록 확인 후 latest.json의 versionCode, versionName, apkUrl, releaseNotes를 갱신합니다. apkUrl은 https://raw.githubusercontent.com/cjsrnsz12/-/main/AbyssHelper-latest.apk 입니다.
5. https://raw.githubusercontent.com/cjsrnsz12/-/main/latest.json 의 공개 응답을 확인하고 사용자에게 배포 결과와 다운로드 주소를 안내합니다.

새 버전의 통상적인 배포는 위 사용자 지시에 따라 진행하며, 플랫폼에서 별도 확인을 요구하면 구체적인 차단 이유를 알립니다. 보안 차단이나 승인 검토를 우회하지 않습니다.

이 지침은 작업 중 새 버전을 완성했을 때 적용하는 배포 절차이며, 대화 밖의 새 APK 생성을 실시간으로 감지하는 자동 실행 기능은 아닙니다.

## 앱 UI 모션 기본 규칙

사용자는 2026-11-11에 다른 창에도 부드러운 열기·닫기 모션을 적용하고 앞으로 만드는 버튼에도 같은 효과를 사용하도록 요청했습니다.

- 새 앱 대화상자는 UiMotion.builder 또는 이를 상속한 공통 Builder를 사용합니다. 창 열기·닫기는 공통 180ms/130ms 효과를 유지합니다.
- 새 앱 버튼은 UiMotion.button 또는 UiMotion.watch가 적용된 화면 안에 생성합니다. 클릭/터치 이벤트를 덮어쓰거나 애니메이션 종료까지 동작을 지연하지 않습니다.
- 탭 전환·조작창 열기는 UiMotion.enter를 사용하며 기기의 애니메이션 비활성화 설정을 존중합니다.
- 시스템 권한·설치창과 게임 자체 화면은 OS/게임이 관리합니다.
