kullanici etkilesimine bagli olmadan long-running operasyonar perform eden bir bacground compenenttir. 
Activity'lerden farkli olarak service'ler kullanici arayuzune sahip degildir ve uygulama yuzeyde olmasa bile arka planda calisabilir. 
Dosya indirme, musik calma, data sync etme ve network operation'lari handle etme gibi background task'ler icin kullanilir.

1- Started Service:
app compenent startService() cagirdigi zaman baslar ve kendisi stopSelf() cagirdiginda ya da stopService() cagrildiginda durur.

Kullanim ornegi:
- arkaplan muzigi calmak
- dosya upload ya da download etmek

Ornek: 
	class MyService : Service() {
		override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
			// Perform long-running task in background
			return START_STICKY
		}

		override fun onBind(intent: Intent?): IBinder? = null
	}

2- Bound Service:
bindService() ile componentlerin kendisine bind olmasina izin verir. Bagli oldugu client'lar oldugu surece service aktif kalir ve client'lar disconnect oldugu zaman da durur.

Kullanim ornegi:
- remote serverden dosya fech'lemek
- arkaplan bluetooth baglantilarini manage etmek

Ornek:
	class BoundService : Service() {
		private val binder = LocalBinder()

		inner class LocalBinder : Binder() {
			fun getService(): BoundService = this@BoundService
		}
		
		override fun onBind(intent: Intent?): IBinder = binder
	}

3- Foreground Service:
persistent notification gerekli oldugu surece aktif olur. Surekli farkindalik gerekli oldugunda kullanilmalidir.

Kullanim ornegi:
- music playback
- navigation
- location tracking

Ornek:
	class ForegroundService : Service() {
		override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
			val notification = createNotification()
			startForeground(1, notification)
			return START_STICKY
		}
		
		private fun createNotification(): Notification {
			return NotificationCompat.Builder(this, "channel_id")
				.setContentTitle("Foreground Service")
				.setContentText("Running...")
				.setSmallIcon(R.drawable.ic_notification)
				.build()
		}
	}

Tum service turleri arkaplanda calisir. sadece bound service'ler tum client'lar unbind oldugu zaman otomatik olarak durabilir. sadece foreground service'ler icin notification gereklidir.

Best Practices:
- Eger direk execution gerekmiyorsa Service yerine WorkManager kullanilmalidir.
- Task'ler bittigi zaman Service durdurulmalidir.
- Memory leak'lerden kacinmak icin service lifecycle degisimleri handle edilmelidir.

Serice vs Foreground Service
- User Awareness: standart service arka planda kullanicinin fark etmeden calisabilir. Foreground service ise kullaniciya gorunur olur.
- Priority: Foreground service'ler diger service'lere kiyasla daha yuksek oncelige sahiptir ve low-memory gibi durumlarda sistem tarafindan daha gec terminate edililirler
- Use Case: standart service'ler daha lightweight arkaplan task'leri icindir. Foreground service'ler ise devamli ve user-noticable taskler icindir.

Best Practices:
- Islem bitti zaman service'ler stop edilmelidir boylece system resource'leri saklanabilir.
- Direkt execution gerekli degilse WorkManager kullanilmalidir.

[[Service Lifecycle]]

