# **SuezX (미디에이션 네트워크)**

## SuezX SDK 통합
### 1. SuezX SDK 다운로드
프로젝트 수준 build.gradle에 다음을 추가하세요.
~~~groovy
allprojects {
    repositories {
        maven { url "https://dl.bintray.com/tpmn/maven" }
    }
}
~~~
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
    implementation 'io.tpmn:suezx-sdk:2.0.0@aar'
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

## SuezX 어댑터 통합
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
    implementation 'io.tpmn:mopub-suezx-adapter:2.0.0.0@aar'
}
~~~

## SuezX SDK 초기화
`withAdditionalNetworks()`를 사용하여 MoPub SDK와 함께 SuezX SDK를 초기화하세요. 
~~~java
SdkConfiguration sdkConfig = new SdkConfiguration.Builder(ANY_OF_YOUR_AD_UNIT_IDS_HERE) 
        .withAdditionalNetworks(SuezXAdapterConfiguration.class.getName())
        .build();
~~~
