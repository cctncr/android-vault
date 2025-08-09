os'daki activity'lerin, task'lerin ve cihazda olan process'lerin bilgisini saglayan bir sistem servisidir.
Uygululama lifecycle'i, memory kullanimi ve task yonetimi gibi durumlarin controlunu dev'e sunar ve android framework'unun bir parcasidir.

Anahtar Ozellikler: 
- Task and Activity Information: Calisan task'ler, activity'ler ve back stack durumlari hakkinda detay gosterir.
- Memory Management: Detayli Memory kullanimi gosterir.
- App Process Management: Calisan uygulama process'leri ve service'leri hakkinda detayli bilgi verir.
- Debugging and Diagnostics: Debugging tool'lari sunar. Darbogazlari ve memory leak'leri gormemizi saglar.

Common Methods: 
- getRunningAppProcesses(): Chizda calisan process'leri liste olarak doner.
- getMemoryInfo(ActivityManager.MemoryInfo memoryInfo): Cihaz memory'si hakkinda detayli bilgi doner.
- killBackgroundProcesses(String packageName): Background process'i terminate eder.
- isLowRamDevice(): Cihaz low-memory device olarak mi kategorize edildi.
- appNotResponding(String message): ANR durumunu simule eder.
- clearApplicationUserData(): app'le ilgili tum kullanici specific data'yi siler. Dosyalar, database'ler, shared preference'lar dahildir. factory reset atmak gibi.