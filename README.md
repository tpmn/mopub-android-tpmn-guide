# TPMN-퍼블리셔 광고 SDK 통합 프로세스

### 1. 퍼블리셔
- 애플리케이션 내의 광고 형식, 유형, 개수 등의 정보 전달
- 기존 패키지 이름 전달

### 2. TPMN
- MoPub ad unit ID 발급
- 새 패키지 이름 전달
- JKS 전달

### 3. 퍼블리셔
- 패키지 이름 변경
- 가이드에 따라 애플리케이션에 광고 SDK 통합
    +  [MoPub](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub): 메인(필수), 발급 받은 ID 사용
    + [AdMob](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/admob): 미디에이션(선택)
    + [Facebook Audience Network](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/facebookaudiencenetwork): 미디에이션(선택)
- 광고 테스트
- APK/AAB 전달

### 4. TPMN
- 스토어 출시
- 알파테스트
