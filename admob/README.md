# **AdMob (미디에이션 네트워크)**

## AdMob SDK 연동 [(참고)](https://developers.google.com/admob/android/quick-start?hl=ko)

### 1. AdMob SDK 다운로드
프로젝트 수준 build.gradle에 다음을 추가하세요.
~~~groovy
buildscript {
    repositories {
        google()
        mavenCentral()
    }
}

allprojects {
	repositories {
		google()
		mavenCentral()
	}
}
~~~
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
	implementation 'com.google.android.gms:play-services-ads:20.2.0'
}
~~~

### 2. AndroidManifest.xml 업데이트
AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
	<application>
        <!-- 발급 받은 AdMob app ID를 넣으세요. -->
		<meta-data
			android:name="com.google.android.gms.ads.APPLICATION_ID"
			android:value="ca-app-pub-xxxxxxxxxxxxxxxx~yyyyyyyyyy"/> 
	</application>
</manifest>
~~~

## AdMob 어댑터 연동 [(참고)](https://developers.mopub.com/publishers/mediation/networks/google/)
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
	implementation 'com.mopub.mediation:admob:20.2.0.2'
}
~~~
