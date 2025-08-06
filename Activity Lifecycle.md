bir activity'nin omru boyunca gectigi state'le denir. 
- onCreate(): activity create edildiginde ilk cagrilan method'dur. Burada activity initialize edilmesi, Ui componentlerinin kurulmasi ve saved instance state'in restore edilmesi gibi islemler yapilir. eger activity destroy olup tekrar create edilmezse activity lifecycle'i boyunca bir kez cagrilir.
- onStart(): activity kullaniciye ilk gorunur ama interactive olmadigi durumda olur. onCreate() den sonra, onResume()'dan once cagrilir.
- onRestart(): eger activity stop olmus ve sonra restarted durumuna gectiyse cagrilir. (mesela kullanici geri tusuyla bu activity'e dondu). on start() method'undan once cagrilir.
- onResume(): activity yuzeydedir ve kullanici etkilesime gecebilir. durdurulmus Ui update'lerini, animasyonlari veya input listener'lari deval ettirmemiz gereken yerdir.
- onPause(): activity kismen gizlenmis duruma geldiginde (mesela dialog ciktiginda) cagrilir. activity hala gorunurdur ama odakta degildir. animasyonlarin durdurulmasi, sensor update'lerinin durdurulmasi ya da data save etmek icin kullanilir.
- onStop(): activity artik kullaniciya gorunur degildir. (mesela baska bir activity yuzeye gelmistir) background task'ler veya agir objeler gibi artik ihtiyac olmayan resource'larin release edilmesi yapilir.
- onDestroy(): activity tamamen destroy edilmeden ve memory'den silinmeden once cagrilir. kalan resource'larin release edilmesi islemi yapilir.

