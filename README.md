# TPMN-퍼블리셔 광고 SDK 통합 프로세스

### 1. 퍼블리셔
- 애플리케이션 내의 광고 형식, 유형, 개수 등의 정보 전달
- 기존 패키지 이름 전달

### 2. TPMN
- MoPub ad unit ID 발급
- 새 패키지 이름 전달
- Java KeyStore(.jks) 및 패스워드 전달

### 3. 퍼블리셔
- 패키지 이름 변경
- 가이드에 따라 애플리케이션에 광고 SDK 통합
    +  [MoPub](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub)(필수): MoPub SDK 통합, 광고 인벤토리 구현, 발급 받은 ID 사용
    + [AdMob](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/admob)(필수): AdMob SDK 통합, MoPub이 지원하는 AdMob 미디에이션 어댑터 통합, 별도의 인벤토리 구현 없이 MoPub 인벤토리에 광고 게재
    + [Facebook Audience Network](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/facebookaudiencenetwork)(필수): FAN SDK 통합, MoPub이 지원하는 FAN 미디에이션 어댑터 통합, 별도의 인벤토리 구현 없이 MoPub 인벤토리에 광고 게재
    + [AdFit](https://github.com/tpmn/mopub-android-mediation-custom/tree/master/adfit)(필수): AdFit SDK 통합, TPMN이 개발한 AdFit 미디에이션 어댑터 통합, 별도의 인벤토리 구현 없이 MoPub 인벤토리에 광고 게재
- 광고 테스트
- Android App Bundle(.aab) 빌드하여 전달

### 4. TPMN
- 스토어 출시
- 알파테스트
