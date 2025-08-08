Service'ler iki modda operate edilir:
- Started Service: startService() ile init olur ve explicitly olarak stopSelf() ya da stopService() cagrilana kadar calisir.
- Bound Service: Bir ya da daha fazla component'e bindService() methodu ile baglidir ve bu componentler var oldugu surece calisir.

Started Service icin Lifecycle methodlari:
- onCreate(): service ilk olusturuldugunda cagrilir. service icin gerekli resource'larin initalize edilmesinde kullanilir
- onStartCommand(): startService() ile service start oldugunda triger'lanir. Bu method gercek task execution'dan ve restart behavior'indan sorumludur (START_STICKY, START_NOT_STICKY gibi).
- onDestory(): stopSelf() ya da stopService() cagrildiginda cagrilir. resource'larin ve thread'lerin relase edilmesi gibi cleanup icin kullanilir.

Ornek: 
	class SimpleStartedService : Service() {
		override fun onCreate() {
			super.onCreate()
			super.onCreate()
		}

		override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {

			// Perform long-running task
			return START_STICKY // Restart if service is killed
		}

		override fun onDestroy() {
			super.onDestroy()
			// Clean up resources
		}
		override fun onBind(intent: Intent?): IBinder? = null // Not used for started service
	}

Bound Service icin Lifecycle methodlari:
- OnCreate(): Started Service ile ayni.
- onBind(): bindService() kullanarak bir compenent service bind olurken cagrilir. IBinder diye bir interface provide eder.
- onUnbind(): Bagli olan son client da unbind oldugu zaman cagrilir. Bound client'lar ile ilgili resource'larin cleanup edildigi yerdir.
- onDestroy(): Started Service ile ayni.

Ornek:
	class SimpleBoundService : Service() {
		private val binder = LocalBinder()

		override fun onCreate() {
			super.onCreate()
			// Initialize resources
		}

		override fun onBind(intent: Intent?): IBinder {
			return binder // Return the interface for the bound service
		}

		override fun onUnbind(intent: Intent?): Boolean {
			// Clean up when no clients are bound
			return super.onUnbind(intent)
		}

		override fun onDestroy() {
			super.onDestroy()
			// Clean up resources
		}
		
		inner class LocalBinder : Binder() {
			fun getService(): SimpleBoundService = this@SimpleBoundService
		}
	}

Started ve Bound Service lifecycle'i arasindaki onemli farklar:
- Started Service: hangi bir compenent'e bagli degil ve explicitly stop edilene kadar calisir.
- Bound Service: En az bir client bagli oldugu surece calisir.