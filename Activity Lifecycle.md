bir activity'nin omru boyunca gectigi state'le denir. 
- onCreate(): activity create edildiginde ilk cagrilan method'dur. Burada activity initialize edilmesi, Ui componentlerinin kurulmasi ve saved instance state'in restore edilmesi gibi islemler yapilir. eger activity destroy olup tekrar create edilmezse activity lifecycle'i boyunca bir kez cagrilir.
- onStart(): activity kullaniciye ilk gorunur ama interactive olmadigi durumda olur. onCreate() den sonra, onResume()'dan once cagrilir.
- onRestart(): eger activity stop olmus ve sonra restarted durumuna gectiyse cagrilir. (mesela kullanici geri tusuyla bu activity'e dondu). on start() method'undan once cagrilir.
- onResume(): activity yuzeydedir ve kullanici etkilesime gecebilir. durdurulmus Ui update'lerini, animasyonlari veya input listener'lari devam ettirmemiz gereken yerdir.
- onPause(): activity kismen gizlenmis duruma geldiginde (mesela dialog ciktiginda) cagrilir. activity hala gorunurdur ama odakta degildir. animasyonlarin durdurulmasi, sensor update'lerinin durdurulmasi ya da data save etmek icin kullanilir.
- onStop(): activity artik kullaniciya gorunur degildir. (mesela baska bir activity yuzeye gelmistir) background task'ler veya agir objeler gibi artik ihtiyac olmayan resource'larin release edilmesi yapilir.
- onDestroy(): activity tamamen destroy edilmeden ve memory'den silinmeden once cagrilir. kalan resource'larin release edilmesi islemi yapilir.

->  Launching Activity A, then Activity B, and finally returning to Activity A sequentially
Initial Launch of Activity A:
- Activity A: onCreate() -> onStart() -> onResume() sequence when initially launched. The user interacts with Activity A.
Navigating from Activity A to Activity B:
 - Activity A: onPause(), pausing its UI and freeing resources tied to the visible state.
 - Activity B: onCreate() -> onStart() -> onResume(), taking focus and becoming the foreground activity.
 - Activity A: onStop(), optional if Activity B fully covers Activity A.
Returning from Activity B to Activity A:
- Activity B: onPause()
- Activity A: onRestart() -> onStart() -> onResume(), regaining focus and returning to the foreground.
- Activity B: onStop() -> onDestroy()

Activity icindeki lifecycle instance'si nedir?
Activity'nin suanki lifecycle state'ini temsil ever ve onCreate, onStart gibi fonksiyonlari override etmeden observe etmemizi saglar.
LifecycleObserver ya da DefaultLifecycleObserver objeleri olusturup lifecycle'a ekleyebiliriz.

Ornek: 

	class MyObserver : DefaultLifecycleObserver { 
		
		override fun onStart(owner: LifecycleOwner) {
			super.onStart(owner) 
		} 
		
		override fun onStop(owner: LifecycleOwner) {
			super.onStop(owner)
		} 
	}
	
	class MainActivity : ComponentActivity() {
		override fun onCreate(savedInstanceState: Bundle?) {
			super.onCreate(savedInstanceState)
			lifecycle.addObserver(MyObserver())
		}
	}

Benefits: 
- Lifecycle Awareness: Componentler lifecycle state'ini bilir.
- Separation of Concerns: Lifecycle dependent logic disariya cikarilir
-  Integration with Jetpack Libraries: LifeData ve ViewModel gibi kutuphaneler lifecycle instance'i ile calismak icin dizayn edilmistir.