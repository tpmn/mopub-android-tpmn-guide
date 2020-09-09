# **AdMob (미디에이션 네트워크)**

## AdMob SDK 통합 [(참고)](https://developers.google.com/admob/android/quick-start)

### 1. AdMob SDK 다운로드
프로젝트 수준 build.gradle에 다음을 추가하세요.
~~~groovy
allprojects {
	repositories {
		google()
	}
}
~~~
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
	implementation 'com.google.android.gms:play-services-ads:19.3.0'
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

### 3. targetSdkVersion 설정
AdMob SDK v19.3.0 이하는 Android 11 (API 30)을 지원하지 않습니다. Android 11 기기에서 광고를 로드하기 위해 `targetSdkVersion`을 30으로 설정하세요.

## AdMob 어댑터 통합 [(참고)](https://developers.mopub.com/publishers/mediation/networks/google/)
앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
	implementation 'com.mopub.mediation:admob:19.3.0.2'
}
~~~
