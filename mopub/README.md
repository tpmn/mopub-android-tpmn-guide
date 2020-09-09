# **MoPub**

## MoPub SDK 통합 [(참고)](https://developers.mopub.com/publishers/android/integrate/)

### 1. MoPub SDK 다운로드
MoPub SDK를 jCenter를 통해 다운로드할 수 있습니다. 앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
repositories {
    jcenter()
}

dependencies {
	implementation('com.mopub:mopub-sdk:5.13.1@aar') {
		transitive = true
	}
}
~~~
특정 광고 형식만을 포함하여 애플리케이션의 용량을 줄일 수도 있습니다. 앱 수준 build.gradle에 다음을 선택적으로 추가하세요.
~~~groov
// For banners
implementation('com.mopub:mopub-sdk-banner:5.13.1@aar') {
    transitive = true
}

// For fullscreen ads
implementation('com.mopub:mopub-sdk-fullscreen:5.13.1@aar') {
	transitive = true
}

// For native static (images).
implementation('com.mopub:mopub-sdk-native-static:5.13.1@aar') {
	transitive = true
}
~~~
Google의 정책에 따라 Google Advertising ID를 사용해야 합니다. 앱 수준 build.gradle에 다음을 추가하세요.
~~~groov
dependencies {
	implementation ‘com.google.android.gms:play-services-ads-identifier:17.0.0’
	implementation ‘com.google.android.gms:play-services-base:17.4.0’
}
~~~
Java 8을 지원하려면 앱 수준 build.gradle에 다음을 추가하세요.
~~~groov
android {
	compileOptions {
		sourceCompatibility JavaVersion.VERSION_1_8
		targetCompatibility JavaVersion.VERSION_1_8
      }
}
~~~

### 2. AndroidManifest.xml 업데이트
INTERNET(필수), ACCESS_NETWORK_STATE(필수) 그리고 ACCESS_COARSE_LOCATION(선택) 퍼미션이 MoPub SDK의 AndroidManifest.xml에 포함되어 있습니다. ACCESS_COARSE_LOCATION 퍼미션을 제거하려면 애플리케이션의 AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
	<uses-permission
	android:name="android.permission.ACCESS_COARSE_LOCATION"
		tools:node="remove" />
</manifest>
~~~

### 3. network_security_config.xml 추가
AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
    <application
        android:networkSecurityConfig="@xml/network_security_config">
	</application>
</manifest>
~~~
network_security_config.xml에 cleartextTrafficPermitted를 true로 설정하는 base-config를 추가하세요.
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

## MoPub SDK 초기화 [(참고)](https://developers.mopub.com/publishers/android/initialize/)
`Activity`의 `onCreate()`에 다음을 추가하세요.
~~~java
SdkConfiguration sdkConfig = new SdkConfiguration.Builder(ANY_OF_YOUR_AD_UNIT_IDS_HERE) // 발급 받은 ad unit ID 중 하나를 넣으세요.
						.withLogLevel(LogLevel.Debug)
						.withLegitimateInterestAllowded(true)
						.build();
MoPub.initializeSdk(this, sdkConfig, initSdkListener());
~~~

## MoPub 배너 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/banner/)

### 1. XML 레이아웃에 배너 인벤토리 정의
~~~
<com.mopub.mobileads.MoPubView
	android:id="@+id/adview"
	android:layout_width=""
	android:layout_height=""
	app:moPubAdSize="">
</com.mopub.mobileads.MoPubView>
~~~

### 2. 배너 인벤토리에 광고 로드
`Activity` 또는 `Fragment`에 `MoPubView` 객체를 선언하세요.
~~~java
private MoPubView moPubView;
~~~
`Activity`의 `onCreate()` 또는 `Fragment`의 `onCreateView()`에 다음을 추가하세요.
~~~java
moPubView = (MoPubView) findViewById(R.id.adview);
moPubView.setAdUnitId(YOUR_BANNER_AD_UNIT_ID_HERE); // 발급 받은 배너 ad unit ID를 넣으세요.
moPubView.setAdSize(MoPubAdSize); // 선택. Call this if you are not setting the ad size in XML or wish to use an ad size other than what has been set in the XML. Note that multiple calls to `setAdSize()` will override one another, and the MoPub SDK only considers the most recent one.
moPubView.loadAd(MoPubAdSize); // 선택. Call this if you are not calling setAdSize() or setting the size in XML, or if you are using the ad size that has not already been set through either setAdSize() or in the XML.
moPubView.loadAd();
~~~
`Activity`의 `onDestroy()` 또는 `Fragment`의 `onDestroyView()`에 다음을 추가하세요.
~~~java
if (moPubView != null) {
	moPubView.destroy();
	moPubView = null;
}
~~~

### 3. 리스너 구현 (선택)
~~~java
moPubView.setBannerAdListener(new BannerAdListener() {
		@Override
		public void onBannerLoaded(MoPubView banner) {
			// Sent when the banner has successfully retrieved an ad.
		}

		@Override	
		public void onBannerFailed(MoPubView banner, MoPubErrorCode errorCode) {
			// Sent when the banner has failed to retrieve an ad. You can use the MoPubErrorCode value to diagnose the cause of failure.
		}

		@Override
		public void onBannerClicked(MoPubView banner) {
            // Sent when the user has tapped on the banner.
		}

		@Override	
		public void onBannerExpanded(MoPubView banner) {
			// Sent when the banner has just taken over the screen.
		}

		@Override	
		public void onBannerCollapsed(MoPubView banner) {
			// Sent when an expanded banner has collapsed back to its original size.
		}
});
~~~

## MoPub 인터스티셜 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/interstitial/)

### 1. 광고 로드
`Activity`에 `MoPubInterstitial` 객체를 선언하세요.
~~~java
private MoPubInterstitial moPubInterstitial;
~~~
`Activity`의 `onCreate()`에 다음을 추가하세요.
~~~java
moPubInterstitial = new MoPubInterstitial(this, YOUR_INTERSTILTIAL_AD_UNIT_ID_HERE); // 발급 받은 인터스티셜 ad unit ID를 넣으세요.
moPubInterstitial.load();
if (moPubInterstitial != null && moPubInterstitial.isReady()) {
	moPubInterstitial.show();
}
~~~
`Activity`의 `onDestroy()`에 다음을 추가하세요.
~~~java
if (moPubInterstitial != null) {
	moPubInterstitial.destroy();
	moPubInterstitial = null;
}
~~~

### 2. 리스너 구현 (선택)
~~~java
moPubInterstitial.setInterstitialAdListener(new InterstitialAdListener() {
        @Override
        public void onInterstitialLoaded(MoPubInterstitial interstitial) {
            // The interstitial has been cached and is ready to be shown.
        }

        @Override
        public void onInterstitialFailed(MoPubInterstitial interstitial, MoPubErrorCode errorCode) {
            // The interstitial has failed to load. Inspect errorCode for additional information.
        }

        @Override
        public void onInterstitialShown(MoPubInterstitial interstitial) {
            // The interstitial has been shown. Pause / save state accordingly.
        }

        @Override
        public void onInterstitialClicked(MoPubInterstitial interstitial) {
            // The interstitial has been clicked.
        }

        @Override
        public void onInterstitialDismissed(MoPubInterstitial interstitial) {
            // The interstitial has being dismissed. Resume / load state accordingly.
        }
});
~~~
