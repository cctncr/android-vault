Depp Links kullanicin bir URL ya da notification gibi app'te specific bir ekrana ya da feature'a navigate etmesini saglar.

1- Define Deep Links in the Manifest
Activity'nin handle etmesi icin AndroidManifest.xml dosyasina intent filter declare edilmelidir. Intent filter URL mimarisini ve semasini specify eder.
```
<activity android:name=".MyDeepLinkActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data
			android:scheme="https"
			android:host="example.com"
			android:pathPrefix="/deepLink" />
	</intent-filter>
</activity>
```
- android:scheme: URL semasini specify eder (https gibi)
- android:host: Domain'i specify eder (example.com gibi)
- android:pathPrefix: URL path'ini tanimlar (/deepLink gibi)
bu ornekteki konfigurasyon https://example.com/deepLink gibi URL'lerin MyDeepLinkActivity'i acmasini saglar.

2- Handle the Deep Link in the Activity
Activity icinde gelen intent'i yakalayip process et, ilgili sayfaya navigate et ya da bir action perform et.
```
class MyDeepLinkActivity : AppCompatActivity() {

	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_my_deep_link)

		// Get the intent data
		val intentData: Uri? = intent?.data
		if (intentData != null) {
			val id = intentData.getQueryParameter("id") // Example: Retrieve query parameters
		}
	}

	private fun navigateToFeature(id: String?) {
		// Navigate to a specific screen based on the deep link data
		if (id != null) {
			Toast.makeText(this, "Navigating to item: $id", Toast.LENGTH_SHORT).show()
			
			navigate(..) or doSomething(..)
		}
	}
}
```

3- Testing the Deep Link
Test etmek icin asagidaki adb komutunu kullanabilirsiniz:
```
adb shell am start -a android.intent.action.VIEW \
-d "https://example.com/deepLink?id=123" \
com.example.myapp
```

Ek olarak:
- Custom Schemes: internal link'ler icin custom semalar kullanabilirsin (myapp:// gibi) ama daha genis uyumluluk icin HTTP(S) URL'leri tercih et
- Navigation: Deep link verilerine gore app icinde diger activity'lere veya fragment'lara navigate etmek icin intent kullan.
- FallBack Handling: Deep link data'sinin invalid ya da incomplete olmasi durumunu handle et.
- App Links: HTTP(S) deep linkleri icin browser kullanmak yerine app icinde ac. App Links configure et.