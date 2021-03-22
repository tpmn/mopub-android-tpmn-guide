# **SuezX (미디에이션 네트워크)**

### 1. SuezX 어댑터 다운로드
프로젝트의 build.gradle에 다음을 추가하세요.
~~~groovy
allprojects {
    repositories {
        jcenter()
    }
}
~~~
앱 모듈의 build.gradle에 다음을 추가하세요. SuezX 어댑터는 SuezX SDK를 포함합니다.
~~~groovy
dependencies {
    implementation 'io.tpmn:mopub-suezx-adapter:2.1.0.0'
}
~~~

### 2. AndroidManifest.xml 업데이트
SuezX SDK가 정상적으로 동작하도록 AndroidManifest.xml에 다음 권한을 추가하세요.
~~~
<manifest>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
~~~

### 3. SuezX 어댑터 초기화
`withAdditionalNetworks()`를 사용하여 MoPub SDK와 함께 SuezX 어댑터를 초기화하세요.
~~~java
SdkConfiguration sdkConfig = new SdkConfiguration.Builder(ANY_OF_YOUR_AD_UNIT_IDS_HERE) 
        .withAdditionalNetworks(SuezXAdapterConfiguration.class.getName())
        .build();
~~~
이제 MoPubBanner와 MoPubInterstital 인스턴스를 통해 SuezX 네트워크의 광고가 노출됩니다.