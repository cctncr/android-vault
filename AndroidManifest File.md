AndroidManifest.xml uygulamayla ilgili onemli bilgileri tutan configuration dosyasidir.
uygulama ve isletim sistemi arasinda bir kopru gorevi gorur.
isletim sistemine uygulmadaki componentleri, izinleri, donanim ve yazilim ozellikleri gibi seyleri icin bilgilendirir.
- Application Components Declaration: Activity'lerin, Service'lerin, Broadcast Receiver'larin ve Content Provider'larin kaydini tutar. Bu sayede os bunlarin nasil baslatilacagini veya interact olacagini bilir.
- Permissions: uygulamada gerekli olan izinleri belirtir. INTERNET, ACCESS_FINE_LOCATION, ya da READ_CONTACTS gibi. boylece kullanici uygulamanin nelere erisecegini bilir ve izin verip vermez.
- Hardware and Software Requirements: camera, gps ya da belli bir ekran boyutu gibi uygulamanin calismasi icin gerekli olan ozellikleri belirtir. boylece play store uygulamayi filtreleyebilir.
- App Metadata: uygulamanin package ismi, versiyonu, minimum ve target API leveli themes ve styles gibi bilgileri tutar.
- Intent Filters: compenentlere (mesela activity'ler) intent filter yazip hangi tur intentlere cevap verecegini tutar. link acmak ya da content share etmek gibi uygulamanin diger uygulamalarla iletisimde olmasini saglar.
- App Configuration and Settings: launcher activity, backup behavior ve themes specify etmek gibi configuration ayarlarini tutar. uygulamanin nasil davranacagini ve display edilecegini belirler.
