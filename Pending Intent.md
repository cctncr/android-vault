Intent'in ozel bir turudur ve ileri bir zaman icin baska bir app'in ya da sistem componentinin onceden hazirlanmis bir Intent'i calistirmasina yarar.
Notification ya da service'lere ulasim gibi uygulama yasam omrunun disinda yapilacak operasyonlarda yararlidir.
Intent class'inin wrapper'i gibi calisir.
3 formu vardir: 
- Activity baslatmak
- Service baslatmak
- Broadcast gondermek
Kullanilabilinecek flagler:
FLAG_UPDATE_CURRENT: var olan PendingIntent'i yeni data ile update eder.
FLAG_CANCEL_CURRENT: var olan PendingIntent'i iptal eder yenisini olusturmadan
FLAG_IMMUTABLE: PendingIntent'i immutable yapar
FLAG_ONE_SHOT: PendingIntent'in bir kez kullanilmasini saglar.
Gerekli degilse her zaman FLAG_IMMUTABLE set edilmeli guvenlik icin.
