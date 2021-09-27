# **AppLovin (미디에이션 네트워크)**

## AppLovin SDK 연동

### 1. AppLovin SDK 다운로드

프로젝트 수준 build.gradle에 다음을 추가하세요.

~~~groovy
repositories {
    mavenCentral()
}
~~~

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.applovin:applovin-sdk:10.3.3'
}
~~~

### 2. AndroidManifest.xml 업데이트

AndroidManifest.xml에 다음을 추가하세요.

~~~
<manifest>
    <application>
        <!-- 발급 받은 AppLovin SDK key를 넣으세요. -->
        <meta-data
            android:name="applovin.sdk.key"
            android:value="@string/applovin_sdk_key" />
    </application>
</manifest>
~~~

## AppLovin 어댑터 연동 [(참고)](https://developers.mopub.com/publishers/mediation/networks/applovin/)

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.mopub.mediation:applovin:10.3.3.0'
}
~~~
