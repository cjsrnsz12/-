# 현재 릴리스: 1.2.28 파티 재입장·터치 취소

제공 로그는 혼자 다시하기 3회 이후 매칭/로딩이 이어지고, 파티 OTHER 후 55초 뒤 ENTER와 같은 초에 ‘터치 취소 · 다시 시작하세요’로 정지합니다. 로그에는 앱 버전·실제 화면·Android 취소 이유가 없으므로 정확한 하드웨어/인식 지연 원인을 확정하지 마세요. 기존 onCancelled가 running만 확인해 즉시 stop하고 실행/요청 식별을 안 하는 것은 코드로 확인했습니다.

GestureGate는 generation 및 단조 증가 ticket을 사용합니다. 하나의 gesture만 허용하며 취소 후 1,200ms 재확인, 기존 Engine의 같은 action 2.5초 제한을 유지합니다. 재시도는 새로운 capture timestamp가 cooldown 이후인 프레임에만 허용합니다. 좌표/행동을 callback에서 재전송하지 않습니다. 3회 연속 실패는 stop, 성공은 실패 횟수 초기화, 4초 callback 누락은 stop입니다. replay 가능 whitelist는 GestureGate.retryable에 있으며 음식/궁극기/레이드 스킬 등 one-shot 및 미검토 행동은 취소 시 기존 stop을 유지합니다. dispatchGesture 예외도 정지로 처리합니다. start/stop/destroy/뱅가드 목표 종료에서 reset하고, 3개 모드의 handler는 step/count 전에 acceptsFrame을 확인합니다.

AbyssEntryVision은 두 탭이 한 줄로 합쳐질 때 개별 together Rect가 없으면 오른쪽 탭의 실제 행에서 보라 선택 색이 확인되어야 상세로 진행합니다. 선택이 불명확하면 entryPanel만 유지하고 추정 좌표를 누르지 않습니다. 입장 두 줄 라벨은 기존 AbyssRewardVision.button의 정확한 하단 글자 결합을 사용합니다. unmodified 1.2.27에서 병합 헤더 실패를 재현한 로그는 validation/touch28-header-baseline.log입니다.

versionCode49 / 1.2.28. 전체 회귀는 validation/touch28-final-tests.log, 빌드는 touch28-build.log, 새 검사는 GestureGateTest(3,039), AbyssMergedHeaderTest(90), S7GestureRecoveryTest(600회/33,601개)입니다. 제어한 OCR·콜백·합성 화면/가상 시간 검사이며 실제 Android AccessibilityService dispatchGesture 실행·OS 거절·S7 서버 매칭은 미검증입니다. 1.2.27의 고양이 버튼과 나머지 게임 엔진/반복 횟수는 유지합니다. build1228.py를 사용하세요.

# 이전 릴리스: 1.2.27 고양이 모양 버튼

사용자는 버튼을 고양이 모양으로 바꾸되 기능은 그대로 유지하도록 요청했습니다. CatButtonDrawable은 Path/Paint를 재사용해 귀와 수염을 그리고, CatButtons는 기존 Button의 배경만 native RippleDrawable로 바꿉니다. 핸들러·ID·포커스·접근성 역할·사각 터치 영역을 바꾸거나 setOnTouchListener를 추가하지 마세요. CompoundButton(스위치·체크박스)은 장식 대상에서 제외합니다. UiMotion.button은 CatButtons.ensure로 일반/나중 생성된 대화창 버튼도 잡되 기존 animation/listener 동작을 유지합니다.

MainActivity/UpdateUi 공통 버튼과 MacroService 조작 버튼·떠 있는 아이콘에 적용합니다. 알림 RemoteViews는 기존 notice_button 리소스를 vector로 교체합니다. build1227.py는 해당 drawable을 따로 컴파일하고 기존 APK의 같은 res/drawable/notice_button.xml 위치에 넣습니다. 리소스 테이블/ID·나머지 자산과 의존성은 유지하며 비서명 ZIP 비교의 허용 목록은 manifest/classes3.dex/notice_button.xml 세 개입니다.

버전 1.2.27 / code48. 검사는 validation/cat27-render.log(AWT 어댑터 72조합), cat27-ui-tests.log(296), cat27-behavior-review.json(콜백 12개·클래스 29개 동일), cat27-build.log입니다. Android 런타임이나 실제 터치 검사가 아니므로 클릭 성공·실기기 UI 검증을 주장하지 마세요. 게임 판정/상태 머신/시작·정지 기능 코드를 바꾸지 않았습니다.

# 이전 릴리스: 1.2.26 냐크로

사용자가 앱 이름을 ‘냐크로’로 변경하도록 요청했습니다. application/service label은 버전 번호 없이 정확히 ‘냐크로’입니다. MainActivity 상단·기본 홈 제목·권한 안내·실행 안내·업데이트 알림·로그 복사 제목도 바꿨습니다. 사용자가 직접 정한 ui_title은 초기화하지 않습니다. 패키지 kr.abyss.helper, action 이름, 설정 키, 업데이트 URL과 기존 서명을 유지합니다. 게임 판정·상태 머신 변경은 없습니다.

versionCode 47, versionName 1.2.26입니다. build1226.py는 기존 merged-baseline의 application/service 라벨을 직접 ‘냐크로’로 치환합니다. 단순히 소스 AndroidManifest.xml만 바꿔서는 실제 배포 APK가 바뀌지 않으므로 APK badging/xmltree 검증도 유지하세요. validation/rename26-build.log, rename26-ui-tests.log, rename26-source-review.json을 참고하세요. 기존 UI/업데이트 검사 296개와 전체 Android 컴파일·서명·정렬·ZIP 검증을 통과했습니다. 실제 기기 설치/표시는 미검증입니다.

# 이전 릴리스: 1.2.25 S7 파티 재입장

S7 파티플레이에서 다시 입장이 안 된다는 제보입니다. 새 화면·OCR 덤프·실기기가 없어 이번 실기기 원인을 확정했다고 주장하지 마세요. v1.2.24 Engine에서 ENTER → field → unknown → field → unknown → clear가 CLEAR를 반환하지 못하는 버그를 실제 baseline 소스로 재현했습니다(validation/party25-baseline.log).

entrySent는 실제 ENTER 결정 이후에만 설정합니다. 로딩·매칭·영상·부활은 준비 상태를 즉시 해제하고, 그렇지 않으면 15초 안의 독립적인 두 전투 관측으로 해제합니다. ENTER 이후에는 단순 OCR 누락 프레임이 근거를 지우지 않습니다. 상세·목록·메뉴/목적지 불일치 및 reset은 근거를 지우며, 타이머만으로 해제하지 않습니다. ENTER 이전에는 기존 연속 두 관측 조건을 유지합니다. 상세 화면 우선권과 목록 연속 확인은 유지합니다.

AbyssRewardVision은 보상 하단의 정확한 라벨/단어 박스를 사용합니다. 분리 단어·두 줄·여러 버튼의 병합 줄을 처리하지만 서로 먼 열은 합치지 않습니다. OTHER는 초록색만 보고 하드코딩된 위치를 누르지 않습니다. 보상 제목 또는 정확한 나가기+다른 던전 가기 조합의 문맥이 필요합니다. rewardPanel은 제목은 있지만 버튼을 못 읽는 경우의 상태 표시이며 명시적 clear보다 우선하지 않습니다. 단독 다시하기의 기존 초록색 fallback은 유지합니다.

새 검사는 PartySparseReentryTest(2,826 단언/400 가상 반복), AbyssPartyReentryVisionTest(324 합성 화면/90 파티 재생/1,608 단언)입니다. 기존 전체 회귀는 validation/party25-final-tests.log, 빌드/서명은 validation/party25-build.log, 설명은 validation/PARTY-REENTRY-1.2.25.md입니다. 빌드 스크립트는 상위 작업 폴더 build1225.py를 사용합니다. app/build.gradle 및 ReleaseInfo는 versionCode 46 / 1.2.25입니다. 레이드/뱅가드의 횟수·20초 마지막 스킬 등은 바꾸지 않았습니다.

S7 터치·매칭 추가 시뮬레이션: 세로 전체 화면/시스템 바가 있는 창/가로 기기 안의 세로 분할 창/이동한 세로 창 × 709/848/960px 분석 × 매칭 후 진입/매칭 후 로딩 생략/매칭·로딩 모두 생략/매칭 후 상세 복귀·재요청을 검사했습니다. 480회 재입장, 2,040개 실제 버튼 영역 내 가상 터치, 매칭·OCR 누락 36,120프레임의 추가 조작 억제, 총 46,662개 단언을 통과했습니다. 실제 Android GestureDescription 실행이나 서버 접속은 하지 않았습니다.

# 이전 릴리스: 1.2.24 어비스 입장 판정 및 단계 전환 보완

사용자가 던전 선택 후 입장이 안 되고 우연한 만남 스위치 대기가 반복되는 상태 로그를 제공했습니다. 실제 실패 화면이나 OCR 덤프는 없으므로 정확한 실기기 원인이 확정됐다고 주장하지 마세요.

AbyssEntryVision은 상단 함께하기/목적지와 실제 상세 컨트롤 문맥을 먼저 확인합니다. ‘우연한 만남’은 0.28~0.90 세로 영역에서 같은 행의 OCR 조각을 조합하고, 안내와 합쳐진 줄은 행 위치만 사용합니다. 그 행 우측의 초록 스위치 성분 또는 회색 트랙/밝은 원형 손잡이를 찾습니다. 기존 우측 초록색 ON 확인도 유지합니다. encounterKnown은 OFF 토글에 필요한 긍정 상태입니다. 알 수 없는 상태를 OFF로 가정하지 마세요. together/difficulty/enter는 가능한 한 실제 OCR 좌표를 사용합니다.

상세 패널을 인식하면 해당 프레임은 보상/목록/로딩 텍스트와 함께 읽지 않습니다. 목적지를 못 확인한 상세 패널은 entryPanel만 세우고 조작하지 않습니다. Engine의 preparingEntry는 상세 또는 던전 선택 이후 재입장 준비를 유지하고, 일시적 OCR 누락/오염된 보상 문구에 OTHER/RETRY/CLEAR를 누르지 않습니다. 실제 목록 복귀는 연속 두 프레임으로 확인합니다. 로딩/매칭/영상/부활 또는 연속 두 전투 프레임으로 준비 상태를 해제합니다. 사용자 중지/새 실행은 초기화합니다.

새 검사는 AbyssEntryVisionTest(합성 162조합 및 추가 병합/테마 검사, 총 921개 단언)와 AbyssEntryRecoveryTest(로그 흐름/상태 전환 22개)입니다. 최종 기존 검사 로그는 validation/entry24-final-tests.log, 마지막 컨트롤 보완 검사는 validation/entry24-controls-final.log입니다. 빌드는 상위 폴더 build1224.py입니다. 레이드/뱅가드 엔진과 반복 횟수/20초 스킬 규칙은 변경하지 않았습니다.

## 이전 릴리스: 1.2.23 S7·Y700 호환성 및 분석 화질/부하 조절


AnalysisQuality는 모델명 대신 실제 세로 게임 창 너비 800px 이상·세로/가로 비율 1.15~1.95를 사용합니다. 자동 848px, 빠른 반응 960px, 절전/직접 설정 709px 폭으로 원래 비율을 유지하며 분석 높이는 4096px, 폭은 960px로 제한합니다. 발열/절전 연동이 켜져 있고 기기 절전 또는 열 단계 2 이상이면 709px로 낮춥니다. 오버레이 마스킹도 선택한 실제 분석 크기를 사용합니다. 검증된 프레임의 크기는 메인 스레드에서 PerformancePolicy.frameSize로 기록합니다. 자동 모드의 일반 분석은 약 140만 입력 픽셀/초 기준으로 간격을 조절하되 확인된 긴급 장면은 기존 간격을 유지합니다. OCR 및 전체 캡처의 실측 비용을 뜻하는 수치는 아닙니다.

HudLayout은 상/하단 배치 모드를 독립적으로 선택합니다. 한쪽 근거만 있으면 다른 쪽은 그 모드를 따르고, 양쪽의 OCR 근거가 있으면 각각 판단합니다. 순수 전투 시간과 오른쪽 아래의 +N 스킬 글씨가 추가 단서입니다. OCR Rect나 글자 중심 기반 색상을 재변환하지 마세요. 상태 머신, 레이드 무제한 반복·부활·20초 마지막 스킬 조건은 변경하지 않습니다.

새 검사는 TabletCompatibilityPolicyTest, DeviceCompatibilityReplay입니다. 기존 전체 회귀와 합성 HUD 재생을 함께 실행합니다. 이동 퀘스트의 날개는 선택된 하단 위치와 비율 하단 위치를 같은 모양/색상 조건으로 검사하고, 서로 다른 두 후보가 동시에 통과하면 조작하지 않습니다. 실제 S7/Y700 기기가 연결되어 있지 않으므로 실제 호환성·ML Kit OCR·FPS·발열·배터리 성공을 주장하지 마세요. 상세 결과는 validation/COMPATIBILITY-1.2.23.md, 최종 로그는 validation/compat23-tests.log입니다. 빌드는 상위 작업 폴더의 build1223.py를 사용합니다.

## 이전 릴리스: 1.2.22 태블릿 원래 화면 비율 유지


v1.2.21의 모든 화면 709×1536 강제 변형을 제거했습니다. ScreenGeometry.analysisWidth/Height가 종횡비를 유지하고 오버레이 마스킹도 같은 분석 크기를 씁니다. HudLayout은 OCR 헤더/퀘스트/하단 버튼의 위치로 비율 배치와 가장자리 고정을 고릅니다. Vision.col/ratio는 참조 HUD 좌표를 변환하고 nativeCol/nativeRatio는 이미 OCR에서 구한 실제 좌표를 중복 변환하지 않습니다. 실제 OCR 결과의 Rect 자체는 변환하지 않으며 OCR 버튼 타깃은 원래 분석 이미지의 정규 좌표입니다. 고정 음식·메뉴·마지막 스킬과 glyph 타깃은 hudPoint를 거친 후 창 절대 좌표로 매핑합니다. RaidStillness의 미니맵도 선택된 HUD 위치에서 검사하고 위치 모드 변경 시 이전 증거를 버립니다.

문자 인식에는 원래 비율의 비트맵만 전달합니다. hudPixels는 제한된 상·하단 glyph 검사를 위한 샘플러이며 OCR 입력이나 화면 이미지 변형이 아닙니다. 가로 게임 UI는 여전히 미지원입니다. 레이드·어비스·뱅가드 상태 머신과 반복 횟수 조건은 바꾸지 않았습니다. 테스트는 합성 HUD 배치와 제어한 OCR이므로 실기기 성공을 주장하지 않습니다. 사용자가 여전히 인식 실패를 보고했으므로 이전 단순 해상도 통과를 호환성 보증으로 인용하지 마세요.

검사/제한은 validation/RATIO-FIX-VALIDATION.md, 재실행은 python3 tests/run_tests.py를 참고하세요. 빌드 스크립트는 상위 작업 폴더에서 build1222.py를 사용합니다.

## 이전 릴리스: 1.2.21 태블릿 화면 대응


Y700 5세대의 공식 해상도 3040×1904(세로 1904×3040)를 회귀 검사에 추가했습니다. 현재 APK는 모델명 제한 없이 기존 ScreenGeometry로 이 크기를 처리합니다. 프로덕션 코드·APK·버전은 변경하지 않았습니다. 해상도 재생 89개/3,340개 판정, 확대된 창 재생 76개, 좌표·창 변경 309개 검사를 통과했습니다. 실기기·Lenovo 펌웨어 접근성·ML Kit OCR·게임 렌더링·터치는 미검증이고 가로 게임 UI는 미지원입니다. 근거와 재실행 명령은 validation/Y700-GEN5-VALIDATION.md에 있습니다.

사용자는 모든 갤럭시 탭 대응을 요청했습니다. 모델명별 좌표가 아닌 ScreenGeometry로 세로 게임 창/세로 전체 화면을 709×1536에 정규화하고 터치를 실제 창 절대 위치로 되돌립니다. 전체 가로 게임 UI는 지원 자료가 없어 대기합니다. 가로 기기 안의 세로 게임 창과 좌우 검은 여백이 있는 세로 화면은 처리합니다. Android 11 이상 ARM64가 필요하며 구형 Android·32비트·모든 실기기의 작동을 보증하지 않습니다.

게임 창 이동/크기 변경 시 Gate의 두 프레임 재확인을 거칩니다. 캡처 후 메인 스레드에서 게임 창이 같은지 재검증하고 창 밖 좌표로 임의 전환하지 않습니다. 픽셀 glyph 검색은 참조 비율과 실제 창에서 계산한 세로 변형 후보를 모두 허용하되 기존 모양/장면 확인을 유지합니다. Vision.twoBars의 세로 두께 확인을 추가해 저해상도 1~2줄 색상 흔적을 제외합니다. 음식·전투 엔진은 그대로입니다.

## 이전 릴리스: 1.2.20 화면 분석 최적화

FramePixels로 같은 프레임의 32x32 픽셀 타일을 최대 128개(픽셀 데이터 512KiB)만 읽어 보관합니다. 프레임별 새 캐시로, 이전 이미지 픽셀을 다음 프레임에 재사용하지 않습니다. 용량 초과 시 직접 읽기로 대체합니다. Vision·RaidVision·뱅가드 일부 판독이 이 경로를 공유합니다. 판정·행동 엔진은 변경하지 않았습니다. 실제 기기 없이 수행한 시뮬레이션이므로 실기기 성공을 주장하지 않습니다.

## 이전 릴리스: 1.2.19 앱 연결 및 스크롤 개선

앱 선택은 공식 마비노기 모바일 com.nexon.devcat.mm 및 com.nexon.devcat.mmgalaxy만 허용합니다. 이전 다른 앱 선택은 해제하고 미설치 안내를 제공합니다.

게임 선택·진행 기록의 중첩 스크롤 충돌을 InlineScrollView로 수정했습니다. 목록의 터치 시작부터 완료/취소까지 바깥 ScrollView 가로채기를 막고, 기본 버튼 클릭·스크롤 동작은 super로 전달합니다. 실제 휴대폰 스크롤은 미검증입니다.

## 이전 릴리스: 1.2.18 카브락 레이드

사용자는 2026-11-11 출항선 복귀까지 한 사이클을 완성하고 "이제 업로드해도돼"라고 명시했습니다. 이전 배포 보류는 해제되었습니다. 반복 횟수 제한 없이 중지 버튼을 누를 때까지 계속 반복합니다. 토벌 완료 후에는 포탈을 사용하지 않고 미니맵 옆 나가기 → 출항선으로 이동 → 승선 미니맵 확인으로 복귀합니다. 승선이 연속 확인된 시점에만 완료 횟수를 한 번 집계합니다. 새 사이클에서는 음식·보스 영상·20초 스킬 주기 상태를 초기화합니다. 기존 어비스·뱅가드 동작은 유지합니다.

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

- 선택 메뉴·설정 입력·도움말·기록·성능 상태는 버튼 아래 InlinePanel로 펼칩니다. 높이를 240ms로 늘려 아래 메뉴들을 함께 밀어내고, 닫을 때 200ms로 접어 복귀합니다. 고정 높이 오버레이나 화면 중앙 선택창으로 되돌리지 않습니다. 업데이트 정보창은 사용자 요청대로 X로 닫는 별도 창을 유지하고 UiMotion.builder 공통 모션을 사용합니다.
- 새 앱 버튼은 UiMotion.button 또는 UiMotion.watch가 적용된 화면 안에 생성합니다. 클릭/터치 이벤트를 덮어쓰거나 애니메이션 종료까지 동작을 지연하지 않습니다.
- 탭 전환·조작창 열기는 UiMotion.enter를 사용하며 기기의 애니메이션 비활성화 설정을 존중합니다.
- 시스템 권한·설치창과 게임 자체 화면은 OS/게임이 관리합니다.

## 업데이트 결과 안내 형식

사용자는 버전 업데이트를 마칠 때마다 아래 형식으로 간략한 변경 사항을 보여 달라고 요청했습니다. 현재 작업의 완료 응답부터 적용합니다.

[안드로이드 매크로 업데이트]
버전 번호
- 기능별 변경 사항을 하나씩 나눠 짧게 표시하고 각 항목 사이에 빈 줄을 넣습니다. 사용자 요청에 따라 기능명만 간단하게 적고 세부 동작 설명은 생략합니다.
다운로드 링크·파일·배포 사진은 업데이트 요약문에 넣지 않습니다. 채팅방에 바로 올릴 수 있는 변경 내용만 제공합니다.

검증하지 못한 중요한 제한이 있으면 한 문장으로 덧붙입니다. 실제로 완료·배포한 버전만 완료로 안내합니다.
