# TPMN-퍼블리셔 광고 SDK 통합 프로세스

### 1. 퍼블리셔
- 애플리케이션 내의 광고 형식, 유형, 개수 등의 정보 전달
- 기존 패키지 이름 전달

### 2. TPMN
- MoPub ad unit ID 발급
- 새 패키지 이름 전달
- Java KeyStore 및 패스워드 전달

### 3. 퍼블리셔
- 패키지 이름 변경
- 가이드에 따라 애플리케이션에 광고 SDK 통합
    +  [MoPub](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub)(필수): 메인, 발급 받은 ID 사용
    + [AdMob](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/admob)(필수): 미디에이션
    + [Facebook Audience Network](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/facebookaudiencenetwork)(필수): 미디에이션
    + [AdFit](https://github.com/tpmn/mopub-android-mediation-custom/tree/master/adfit)(필수): 미디에이션
- 광고 테스트
- Android App Bundle 빌드하여 전달

### 4. TPMN
- 스토어 출시
- 알파테스트
