# **AdFit (미디에이션 네트워크)**

## AdFit SDK 통합 [(참고)](https://github.com/adfit/adfit-android-sdk/blob/master/docs/GUIDE.md)
### 1. AdFit SDK 다운로드
프로젝트 수준 build.gradle에 다음을 추가하세요.
~~~groovy
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'http://devrepo.kakao.com:8088/nexus/content/groups/public/' }
    }
}
~~~
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
    implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72'
    implementation 'com.google.android.gms:play-services-ads-identifier:17.0.0'

    implementation 'com.kakao.adfit:ads-base:3.4.0'
}
~~~

### 2. network_security_config.xml 추가
Android 9 (API 28) 이상에서는 cleartext HTTP 트래픽이 차단됩니다. `targetSdkVersion`이 28 이상인 경우, 광고를 정상적으로 게재하기 위해 이를 허용하는 네트워크 보안 설정이 필요합니다.
AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
    <application
        android:networkSecurityConfig="@xml/network_security_config">
    </application>
</manifest>
~~~
network_security_config.xml을 생성하고 cleartextTrafficPermitted를 true로 설정하는 base-config를 추가하세요. domain-config를 추가하면 특정 도메인은 항상 HTTPS를 사용하게 됩니다.
~~~
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system"/>
        </trust-anchors>
    </base-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">example.com</domain>
        <domain includeSubdomains="true">cdn.example2.com</domain>
    </domain-config>
</network-security-config>
~~~

## AdFit 어댑터 통합
adfit-adapter-v3.4.0.0.aar을 프로젝트에 추가하세요.

## AdFit SDK 초기화
`withAdditionalNetworks()`를 사용하여 MoPub SDK와 함께 AdFit SDK를 초기화하세요. 
~~~java
SdkConfiguration sdkConfig = new SdkConfiguration.Builder(ANY_OF_YOUR_AD_UNIT_IDS_HERE) 
        .withAdditionalNetworks(AdFitAdapterConfiguration.class.getName())
        .build();
~~~
