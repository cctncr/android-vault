BroadcastReceiver, app'in system-wide broadcast mesajlarini ya da app-specific broadcast'leri dinlemesine ve respond vermesine olanak saglayan bir componenttir.
broadcast'ler cesitli eventleri signal'lemek icin system ya da diger app'ler tarafindan trigerlanabilir. Mesela batarya status'u degisikligi, network connectivity update'i ya da app ici custom intenler.
BroadcastReceiver'lar direkt olarak activity ya da service lifecycle'ina bagli olmayabilen eventler icin kullanilirlar.

Types:
- System Broadcasts: Android os tarafindan app'lere gonderilen sistem eventleridir. battery level changes, time zone updates ya da network connectivity degisikligi
- Custom Broadcasts: app'ler tarafindan harberlesme icin spesifik bilgiler ya da eventler gonderilmesidir. app ici veya app disi olabilir.

BroadcastReceiver class'ini extend edip onReceive method'unu override etmeliyiz. Bu methodda ilgili broadcast'i handle eden logic olacak.

Ornek: 
	class MyBroadcastReceiver : BroadcastReceiver() {
		override fun onReceive(context: Context, intent: Intent) {
			val action = intent.action
			if (action == Intent.ACTION_BATTERY_LOW) {
				// Handle battery low event
			}
		}
	}

bu BroadcastReceiver'i register etmeliyiz. iki yontem var:
- Static Registration (Manifest ile): App calismiyorken bile handle etmek icin. Manifest dosyasina intent filter tanimlanir. Ornek:
```
<receiver android:name=".MyBroadcastReceiver">
	<intent-filter>
		<action android:name="android.intent.action.BATTERY_LOW" />
	</intent-filter>
</receiver>
```
- Dynamic Registration (kodla): App calisiyorken handle etmek icin. Ornek:
```
	val receiver = MyBroadcastReceiver()
	val intentFilter = IntentFilter(Intent.ACTION_BATTERY_LOW)
	registerReceiver(receiver, intentFilter)
```

Use Cases:
- Network connectivity degiskligi
- Telefon aramasi ya da SMS'e cevap verme
- Telefon sarja takildigi gibi durumlar
- Task ya da alarmlari custom broadcast ile schedule etme

Dikkat Edilmesi Gereken Yerler:
- Lifecycle Management: dynamic registratrion kullanilmissa memory leak'lerden kacinmak icin unregisterReceiver kullanilmali.
- Background Execution Limits: Andrid 8.0 (API level 26) ile broadcast receiving kisitlamalari oldu. But durumlarda Context.registerReceiver ya da JobScheduler kullanilmali.
- Security: Eger gizli tutulmasi gereken bir bilgi varsa izinsiz erisim icin permission'lar kullanilmali.


