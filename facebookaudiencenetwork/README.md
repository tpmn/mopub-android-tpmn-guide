# **Facebook Audience Network (미디에이션 네트워크)**

## [FAN SDK 통합](https://developers.facebook.com/docs/audience-network/guides/adding-sdk/android)

### 1. FAN SDK 다운로드
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
repositories {
	mavenCentral()
	jcenter()
}

dependencies { 
	implementation 'com.android.support:support-annotations:28.0.0'
	implementation 'com.facebook.android:audience-network-sdk:6.0.0'
}
~~~

### 2. network_security_config.xml 추가

## [FAN 어댑터 통합](https://developers.mopub.com/publishers/mediation/networks/facebook/)
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies { 
	implementation 'com.mopub.mediation:facebookaudiencenetwork:6.0.0.0'
}
~~~
