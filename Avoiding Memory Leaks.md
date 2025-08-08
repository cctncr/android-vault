Android os memory'i, kullanilmayin memory'i geri kazandiran, aktif app'lerin ve service'lerin verimli yer kaplamasini saglayan garbage collection mekanizmasi ile yonetir.
Dev'ler manual olarak null'ama yapmak zorunda degildir. Dalvik ve ART runtime memory kullanimi yonetir ve artik referansi bulunmayan objeleri temizler, asiri memory tuketimini engeller.
Android os eger memory sikintisi yasiyorsa on plandaki app'leri akici calistirmak icin low-memory killer calistirir ve background process'leri terminate eder.

Memory leak, app gerekli olmayan objelerin referanslarini hala tutuyorsa olusur. Garbage collector referanslar hala tutuldugu icin objeleri temizleyemez. 
Genelde sebebi lifecycle'in duzgun yonetilmemesi, static referanslar ya da context referansini long-lived objede muhafaza etmekle olur.

Best Practice'ler:
- Use Lifecycle-Aware Components: ViewModel ve collectAsStateWithLifecycle'la Flow ya da LiveData kullanmak resource'larin duzgun sekilde release oldugunu garantiye alir.
- Avoid Holding References to Context: Activity ya da Context referanslari static field ya da singleton gibi long-lived objelerde tutulmamalidir. Bunun yerine ApplicationContext kullanilmalidir, cunku lifecycle'i activity ya da fragment'a bagli degildir.
- Unregister Listeners and Callbacks: Her zaman listener'lar, observer'lar uygun lifeycle callback'i icinde unregister edilmelidir. Mesela BroadcastReceiver'lar onPause() ya da onStop() icinde unregister edilmeli.
- Weak References for Non-Critical objects: Strong reference gerekmeyen objeler icin WeakReference kullan ki garbage collector memory'ye ihtiyac oldugunda toplayabilsin.
- Use Tools to Detect Leaks: LeakCanary gibi tool'lar kullanilabilinir. Android Studio icerisinde Memory Profiler, memory leak sorununu anlamayi kolaylastirabilir.
- Avoid Static References to Views: View'lar static field'larda tutulmamalidir, bu Activity context referansini tutup memory leak'e yol acabilir.

