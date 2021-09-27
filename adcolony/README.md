# **AdColony (미디에이션 네트워크)**

## AdColony SDK 연동 [(참고)](https://github.com/AdColony/AdColony-Android-SDK/wiki)

프로젝트 수준 build.gradle에 다음을 추가하세요.

~~~groovy
repositories {
    mavenCentral()
}
~~~

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.adcolony:sdk:4.6.4'
}
~~~

## AdColony 어댑터 연동 [(참고)](https://developers.mopub.com/publishers/mediation/networks/adcolony/)

앱 수준 build.gradle에 다음을 추가하세요.

~~~groovy
dependencies {
    implementation 'com.mopub.mediation:adcolony:4.6.4.0'
}
~~~
