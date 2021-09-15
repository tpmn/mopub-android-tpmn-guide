# **MoPub**

## MoPub SDK 연동 [(참고)](https://developers.mopub.com/publishers/android/integrate/)

### 1. MoPub SDK 다운로드
MoPub SDK를 jCenter를 통해 다운로드할 수 있습니다. 앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
repositories {
    mavenCentral()
}

dependencies {
	implementation('com.mopub:mopub-sdk:5.18.0@aar') {
		transitive = true
	}
}
~~~
특정 광고 형식만을 포함하여 애플리케이션의 용량을 줄일 수도 있습니다. 앱 수준 build.gradle에 다음을 선택적으로 추가하세요.
~~~groovy
// For banners
implementation('com.mopub:mopub-sdk-banner:5.18.0@aar') {
    transitive = true
}

// For fullscreen ads
implementation('com.mopub:mopub-sdk-fullscreen:5.18.0@aar') {
	transitive = true
}

// For native static (images)
implementation('com.mopub:mopub-sdk-native-static:5.18.0@aar') {
	transitive = true
}
~~~
Google의 정책에 따라 Google Advertising ID를 사용해야 합니다. 앱 수준 build.gradle에 다음을 추가하세요.
~~~groovy
dependencies {
	implementation 'com.google.android.gms:play-services-ads-identifier:17.0.1'
	implementation 'com.google.android.gms:play-services-base:17.6.0'
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
ACCESS_COARSE_LOCATION 권한을 제거하려면 애플리케이션의 AndroidManifest.xml에 다음을 추가하세요.
~~~xml
<manifest>
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"
        tools:node="remove" />
</manifest>
~~~

## MoPub SDK 초기화 [(참고)](https://developers.mopub.com/publishers/android/initialize/)
`Activity`의 `onCreate()`에 다음을 추가하세요.
~~~java
SdkConfiguration sdkConfig = new SdkConfiguration.Builder("ANY_OF_YOUR_AD_UNIT_IDS_HERE") // 발급 받은 ad unit ID 중 하나를 넣으세요.
        .withLogLevel(LogLevel.DEBUG)
        .withLegitimateInterestAllowed(true)
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
~~~xml
<com.mopub.mobileads.MoPubView
	android:id="@+id/banner"
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
`Activity`의 `onCreate()` 또는 `Fragment`의 `onCreateView()`에 다음을 추가하세요. 단, SDK 초기화가 완료된 후에 광고를 요청해야 합니다. 이를 보장하려면 [위](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub#mopub-sdk-초기화-참고)에서 작성한 `onInitializationFinished()` 콜백에 다음을 추가하세요.
~~~java
moPubView = findViewById(R.id.banner);

moPubView.setAdUnitId("YOUR_BANNER_AD_UNIT_ID_HERE"); // 발급 받은 배너 ad unit ID를 넣으세요.
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
            // The banner has successfully retrieved an ad.
        }

        @Override	
        public void onBannerFailed(MoPubView banner, MoPubErrorCode errorCode) {
            // The banner has failed to retrieve an ad. You can use the MoPubErrorCode value to diagnose the cause of failure.
        }

        @Override
        public void onBannerClicked(MoPubView banner) {
            // The user has tapped on the banner.
        }

        @Override
        public void onBannerExpanded(MoPubView banner) {
            // The banner has just taken over the screen.
        }

        @Override
        public void onBannerCollapsed(MoPubView banner) {
            // The expanded banner has collapsed back to its original size.
        }
});
~~~

## MoPub 인터스티셜 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/interstitial/)

### 1. 광고 로드 및 게재
`Activity`에 `MoPubInterstitial` 객체를 선언하세요.
~~~java
private MoPubInterstitial moPubInterstitial;
~~~
`Activity`의 `onCreate()`에 다음을 추가하세요. 단, SDK 초기화가 완료된 후에 광고를 요청해야 합니다. 이를 보장하려면 [위](https://github.com/tpmn/mopub-android-tpmn-guide/tree/master/mopub#mopub-sdk-초기화-참고)에서 작성한 `onInitializationFinished()` 콜백에 다음을 추가하세요.
~~~java
moPubInterstitial = new MoPubInterstitial(this, "YOUR_INTERSTILTIAL_AD_UNIT_ID_HERE"); // 발급 받은 인터스티셜 ad unit ID를 넣으세요.

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

## MoPub 네이티브 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/native-recyclerview/)

### 1. XML 레이아웃 정의
~~~xml
 <RelativeLayout
     xmlns:android="http://schemas.android.com/apk/res/android"
     android:id="@+id/native_ad"
     android:layout_width="match_parent"
     android:layout_height="wrap_content">

     <ImageView
         android:id="@+id/native_ad_main_image" />
     <ImageView
         android:id="@+id/native_ad_icon_image" />
     <TextView
         android:id="@+id/native_ad_title" />
     <TextView
         android:id="@+id/native_ad_text" />
     <ImageView
         android:id="@+id/native_ad_privacy_information_icon_image"
         android:layout_width="40dp"
         android:layout_height="40dp"
         android:padding="10dp" />
     <TextView
         android:id="@+id/native_ad_sponsored_text_view" />
 </RelativeLayout>
 ~~~

### 2. 광고 로드
`Activity` 또는 `Fragment`에 `MoPubRecyclerAdapter` 객체를 선언하세요.
~~~java
private MoPubRecyclerAdapter moPubAdapter;
~~~
`Activity`의 `onCreate()` 또는 `Fragment`의 `onCreateView()`에 다음을 추가하세요.
~~~java
moPubAdapter = new MoPubRecyclerAdapter(this, yourRecyclerAdapter); // Activity
moPubAdapter = new MoPubRecyclerAdapter(getActivity(), yourRecyclerAdapter); // Fragment

ViewBinder viewBinder = new ViewBinder.Builder(R.layout.native_ad)
        .mainImageId(R.id.native_ad_main_image)
        .iconImageId(R.id.native_ad_icon_image)
        .titleId(R.id.native_ad_title)
        .textId(R.id.native_ad_text)
        .privacyInformationIconImageId(R.id.native_ad_privacy_information_icon_image)
        .sponsoredTextId(R.id.native_ad_sponsored_text_view)
        .build();

MoPubStaticNativeAdRenderer moPubRenderer = new MoPubStaticNativeAdRenderer(viewBinder);

moPubAdapter.registerAdRenderer(moPubRenderer);

yourRecyclerView.setAdapter(moPubAdapter);

moPubAdapter.loadAds("YOUR_NATIVE_AD_UNIT_ID_HERE"); // 발급 받은 네이티브 ad unit ID를 넣으세요.
~~~
`Activity`의 `onDestroy()` 또는 `Fragment`의 `onDestroyView()`에 다음을 추가하세요.
~~~java
moPubAdapter.destroy();
~~~

### 3. 리스너 구현 및 설정
~~~java
moPubAdapter.setAdLoadedListener(new MoPubNativeAdLoadedListener() {
    @Override
    public void onAdLoaded(int position) {
    }

    @Override
    public void onAdRemoved(int position) {
    }
    });
~~~

## MoPub 리워드 광고 구현 [(참고)](https://developers.mopub.com/publishers/android/rewarded-ad/)

### 1. 광고 로드 및 게재
광고가 미리 로드될 수 있도록 최대한 빨리 (예를 들어, `Activity`의 `onCreate()`에서) `loadRewardedAd()`를 호출하세요.
~~~java
MoPubRewardedAds.loadRewardedAd("YOUR_REWARDED_AD_UNIT_ID_HERE"); // 발급 받은 리워드 ad unit ID를 넣으세요.
~~~
로드가 완료된 후에 게재하세요.
~~~java
if (MoPubRewardedAds.hasRewardedAd("YOUR_REWARDED_AD_UNIT_ID_HERE")) {
    MoPubRewardedAds.showRewardedAd("YOUR_REWARDED_AD_UNIT_ID_HERE");
}
~~~

### 2. 리스너 구현 및 설정
광고 로드에 실패하거나, 기존 광고를 닫으면 새 광고를 로드하세요.
~~~java
MoPubRewardedAds.setRewardedAdListener(new MoPubRewardedAdListener() {
    @Override
    public void onRewardedAdLoadSuccess(String adUnitId) {
        // Called when the ad for the given adUnitId has loaded. At this point you should be able to call MoPubRewardedAds.showRewardedAd(String) to show the ad.
    }
    
    @Override
    public void onRewardedAdLoadFailure(String adUnitId, MoPubErrorCode errorCode) {
        // Called when a ad fails to load for the given adUnitId. The provided error code will provide more insight into the reason for the failure to load.
        
        MoPubRewardedAds.loadRewardedAd("YOUR_REWARDED_AD_UNIT_ID_HERE");
    }
    
    @Override
    public void onRewardedAdStarted(String adUnitId) {
        // Called when a rewarded ad starts playing.
    }
    
    @Override
    public void onRewardedAdShowError(String adUnitId, MoPubErrorCode errorCode) {
        //  Called when there is an error while attempting to show the ad.
    }
    
    @Override
    public void onRewardedAdClicked(String adUnitId) {
        //  Called when a rewarded ad is clicked.
    }
    
    @Override
    public void onRewardedAdClosed(String adUnitId) {
        // Called when a rewarded ad is closed. At this point your application should resume.
        
        MoPubRewardedAds.loadRewardedAd("YOUR_REWARDED_AD_UNIT_ID_HERE");
    }
    
    @Override
    public void onRewardedAdCompleted(Set<String> adUnitIds, MoPubReward reward) {
        // Called when a rewarded ad is completed and the user should be rewarded.
        // You can query the reward object with boolean isSuccessful(), String getLabel(), and int getAmount().
    }
});
~~~
