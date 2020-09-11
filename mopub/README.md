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
~~~groovy
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
~~~groovy
dependencies {
	implementation ‘com.google.android.gms:play-services-ads-identifier:17.0.0’
	implementation ‘com.google.android.gms:play-services-base:17.4.0’
}
~~~
Java 8을 지원하려면 앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
android {
	compileOptions {
		sourceCompatibility JavaVersion.VERSION_1_8
		targetCompatibility JavaVersion.VERSION_1_8
      }
}
~~~

### 2. AndroidManifest.xml 업데이트
MoPub SDK의 AndroidManifest.xml에 INTERNET(필수), ACCESS_NETWORK_STATE(필수) 그리고 ACCESS_COARSE_LOCATION(선택) 권한이 포함되어 있습니다. 이 중 ACCESS_COARSE_LOCATION은 사용자에게 권한 요청을 해야합니다.
ACCESS_COARSE_LOCATION 퍼미션을 제거하려면 애플리케이션의 AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"
        tools:node="remove" />
</manifest>
~~~

### 3. network_security_config.xml 추가
Android 9 (API 28) 이상에서는 cleartext HTTP 트래픽이 차단됩니다. `targetSdkVersion`이 28 이상인 경우, 광고를 정상적으로 게재하기 위해 이를 허용하는 네트워크 보안 설정이 필요합니다.
AndroidManifest.xml에 다음을 추가하세요.
~~~
<manifest>
    <application
        android:networkSecurityConfig="@xml/network_security_config">
    </application>
</manifest>
~~~
network_security_config.xml을 생성하고 cleartextTrafficPermitted를 true로 설정하는 base-config를 추가하세요. domain-config를 추가하면 특정 도메인은 항상 HTTPS를 사용하게 됩니다.
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

private SdkInitializationListener initSdkListener() {
    return new SdkInitializationListener() {
        @Override
        public void onInitializationFinished() {

        }
    };
}
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
`Activity`의 `onCreate()` 또는 `Fragment`의 `onCreateView()`에 다음을 추가하세요. 단, SDK 초기화가 완료된 후에 광고를 요청해야 합니다. 이를 보장하려면 [위](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub#mopub-sdk-초기화-참고)에서 작성한 `onInitializationFinished()` 콜백에 추가하세요.
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

### 3. 리스너 구현 및 설정
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

### 1. 광고 로드 및 게재
`Activity`에 `MoPubInterstitial` 객체를 선언하세요.
~~~java
private MoPubInterstitial moPubInterstitial;
~~~
`Activity`의 `onCreate()`에 다음을 추가하세요. 단, SDK 초기화가 완료된 후에 광고를 요청해야 합니다. 이를 보장하려면 [위](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub#mopub-sdk-초기화-참고)에서 작성한 `onInitializationFinished()` 콜백에 추가하세요.
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

### 2. 리스너 구현 및 설정
[PointBerryImpressionTracker]()
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
            // The interstitial has been dismissed. Resume / load state accordingly.
        }
});
~~~

## MoPub 리워드 비디오 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/rewarded-video/)

### 1. 광고 로드 및 게재
비디오가 미리 로드될 수 있도록 최대한 빨리 (예를 들어, `Activity`의 `onCreate()`에서) `loadRewardedVideo()`를 호출하세요.
~~~java
MoPubRewardedVideos.loadRewardedVideo(YOUR_REWARDED_VIDEO_AD_UNIT_ID_HERE); // 발급 받은 리워드 비디오 ad unit ID를 넣으세요.
~~~
로드가 완료된 후에 게재하세요.
~~~java
if (MoPubRewardedVideos.hasRewardedVideo(YOUR_REWARDED_VIDEO_AD_UNIT_ID_HERE)) {
    MoPubRewardedVideos.showRewardedVideo(YOUR_REWARDED_VIDEO_AD_UNIT_ID_HERE);
}
~~~

### 2. 리스너 구현 및 설정
비디오 로드에 실패하거나, 기존 비디오를 닫으면 새 비디오를 로드하세요.
~~~java
MoPubRewardedVideos.setRewardedVideoListener(new MoPubRewardedVideoListener() {
    @Override
    public void onRewardedVideoLoadSuccess(@NonNull String adUnitId) {
        // Called when the video for the given adUnitId has loaded. At this point you should be able to call MoPubRewardedVideos.showRewardedVideo(String) to show the video.
    }
    
    @Override
    public void onRewardedVideoLoadFailure(@NonNull String adUnitId, @NonNull MoPubErrorCode errorCode) {
        // Called when a video fails to load for the given adUnitId. The provided error code will provide more insight into the reason for the failure to load.
        
        MoPubRewardedVideos.loadRewardedVideo(YOUR_REWARDED_VIDEO_AD_UNIT_ID_HERE);
    }
    
    @Override
    public void onRewardedVideoStarted(@NonNull String adUnitId) {
        // Called when a rewarded video starts playing.
    }
    
    @Override
    public void onRewardedVideoPlaybackError(@NonNull String adUnitId, @NonNull MoPubErrorCode errorCode) {
        //  Called when there is an error during video playback.
    }
    
    @Override
    public void onRewardedVideoClicked(@NonNull String adUnitId) {
        //  Called when a rewarded video is clicked.
    }
    
    @Override
    public void onRewardedVideoClosed(@NonNull String adUnitId) {
        // Called when a rewarded video is closed. At this point your application should resume.
        
        MoPubRewardedVideos.loadRewardedVideo(YOUR_REWARDED_VIDEO_AD_UNIT_ID_HERE);
    }
    
    @Override
    public void onRewardedVideoCompleted(@NonNull Set<String> adUnitIds, @NonNull MoPubReward reward) {
        // Called when a rewarded video is completed and the user should be rewarded.
        // You can query the reward object with boolean isSuccessful(), String getLabel(), and int getAmount().
    }
});
~~~
