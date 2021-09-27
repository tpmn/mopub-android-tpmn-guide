# **Vungle (미디에이션 네트워크)**

## Vungle SDK 연동 [(참고)](https://support.vungle.com/hc/en-us/articles/360002922871)

프로젝트 수준 build.gradle에 다음을 추가하세요.

~~~groovy
repositories {
    mavenCentral()
}
~~~

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.vungle:publisher-sdk-android:6.10.2'
}
~~~

## Vungle 어댑터 연동 [(참고)](https://developers.mopub.com/publishers/mediation/networks/vungle/)

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.mopub.mediation:vungle:6.10.2.0'
}
~~~
