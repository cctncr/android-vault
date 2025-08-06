global application state'i ve lifecycle'ini maintain eden base class olarak ise yarar.
Bir entry point gibi davranir, activiti'ler, service'ler ya da broadcast recevier'lar gibi diger componentlerden once initialize olur.
Application class'i app lifecycle'i boyunca available olan bir context saglar, bu shared resource'larin initialize edilmesi icin idealdir.
Bu class'i global state tutmak icin ya da application-wide initialization'lar icin kullanabiliriz.
Bu class'i override edip dependencies set edip, kutuphaneleri configure edip, resources'lari yonetmek icin kullanabiliriz.
Her Android uygulamasi eger custom bir class AndroidManifest dosyasi icerisinde specify edilmediye base bir Application class'i kullanir.

Key Methods:
- onCreate(): app process create edildiginde cagrilir. database instance'lari, network librari'leri ya da analytics tool'lari gibi application-wide dependenci'leri initialize etmek icin kullanilir. app lifecycle'i boyunca bir kez cagrilir.
- onTerminate: emulated cihazlarda app terminated oldugunda cagrilir. gercek cihazlarda cagrilacagi garanti edilmez.
- onLowMemory() ve onTrimMemory(): sistem low memory algiladiginda triger'lanir. onLowMemory daha eski API surumleri icin kullanilir.

Ornek:
	class CustomApplication : Application() {
		override fun onCreate() {
			super.onCreate()
			// Initialize global dependencies
			initializeDatabase()
			initializeAnalytics()
		}
			
		private fun initializeDatabase() {
			// Set up a database instance
		}
		private fun initializeAnalytics() {
			// Configure analytics tracking
		}
	}

	<application
		android:name="
		... >
		...
	</application>

Best Practices:
- onCreate icinde uzun suren task'ler yapilmamali bu delay'e yol acar
- Global initalization ve resource management icin kullanilmalidir, sacma sapan logic icermemeli
- shared resources yonetilirken thread safety gozetilmeli 
