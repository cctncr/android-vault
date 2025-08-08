ANR (Applicatoin Not Responding) app'in main thread'i (UI thread)  cok uzun sure bloklandigi zaman olusur. (genedel 5 saniye)
ANR olustugu zaman Android kullaniciya uygulamayi kapat ya da bekle diye secenek sunar.
Sunlardan dolayi olusabilir:
- Main thread'de agir bir hesaplama - logic
- Long-running network ya da database operasyonu
- UI operasyonlarinin bloklanmasi (Ui thread'de senkron operasyonlar gibi)

ANR nasil onlenir: main thread agir ya da zaman gerektiren task'ler yapmamlidir.
Best Praciceler:
- Yogun tasklari main thread'den kaldrimak: file I/O, network requests, database operasyonlari gibi isler icin background thread'leri kullan. En iyisi Dispatchers.IO ile Coroutines kullan.
- Use WorkManager for Persistent Tasks: data senkronizasyonu gibi arka planda calismasi gereken isler icin WorkManager kullan.
- Optimize Data Fetching: buyu data setlerini verimli sekilde handle etmek icin Paging kullan. Data'yi kucuk kucuk fetch edip yonetilebilen chunk'lar haline getirir boylece Ui overload olmaz ve performans artar.
- Minimize UI Operations on Configuration Changes: Ui related datayi muhafaza etmek icin ViewModel'dan faydalan.
- Monitor and Profile with Android Studio: Android Studio'da CPU, memory ve network kullanimini gormek icin Profiler tool'larini kullan.
- Avoid Blocking Calls: uzun loop'lar, sleep call'lar, senkron network request'leri gibi bloklayan operasyonlari main thread'de onle.
- Use Handler for Small Delays: Main thread'i bloklamamak icin Thread.sleep() yerine Handler.postDelayed() kullan.