# TPMN-퍼블리셔 광고 SDK 통합 프로세스

### 1. 퍼블리셔
- 애플리케이션 내의 광고 형식, 유형, 개수 등의 정보 전달
- 기존 패키지 이름 전달

### 2. TPMN
- MoPub ad unit ID, AdMob app ID 발급
- 새 패키지 이름 전달
- Java KeyStore(.jks) 및 패스워드 전달

### 3. 퍼블리셔
- 패키지 이름 변경
- 가이드에 따라 애플리케이션에 광고 SDK 통합
    +  **MoPub (필수)**: [가이드](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub)
        - Step 1. MoPub SDK 통합
        - Step 2. 발급 받은 MoPub ID 사용하여 광고 인벤토리 구현
    + 미디에이션 네트워크
        - Step 1. SDK 통합
        - Step 2. 미디에이션 어댑터 통합: 별도의 인벤토리 구현 없이 MoPub 인벤토리에 광고 게재
        - MoPub 파트너 네트워크: MoPub이 지원하는 어댑터 사용
            + **AdMob (필수)**: [가이드](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/admob)
            + **Facebook Audience Network (필수)**: [가이드](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/facebookaudiencenetwork)
        - 서드 파티 네트워크: TPMN이 개발한 어댑터 사용
            + **AdFit (필수)**: [가이드](https://github.com/tpmn/mopub-android-mediation-custom/tree/master/adfit)
            + SuezX (준비 중)
- 광고 테스트
- Android App Bundle(.aab) 빌드하여 전달

### 4. TPMN
- 스토어 출시
- 알파테스트
