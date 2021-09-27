# **Unity Ads (미디에이션 네트워크)**

## Unity Ads SDK 연동 [(참고)](https://unityads.unity3d.com/help/android/integration-guide-android)

프로젝트 수준 build.gradle에 다음을 추가하세요.

~~~groovy
repositories {
    mavenCentral()
}
~~~

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.unity3d.ads:unity-ads:3.7.5'
}
~~~

## Unity Ads 어댑터 연동 [(참고)](https://developers.mopub.com/publishers/mediation/networks/unityads/)

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.mopub.mediation:unityads:3.7.5.1'
}
~~~
