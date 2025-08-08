Mimari:
- Apps
- Android Framework
- Native Libraries
- Android Runtime
- HAL/HIDL
- Linux Kernel

Linux kernel androidin temeli. 
donanim ve yazilim arasindaki baglantiyi sagliyor.
HAL (Hardware abstraction layer) Java framework API camera ya da bluetooth gibi donanim izni istediginde android sistemi gerekli HAL modulunu yukluyor.
Andorid Runtime (ART) ve Core Libraries compile edilmis bytecode'u calistiriyor.
Ahead-Of-Time (AOT) ve Just-In-Time (JIT) destekliyor.
Native C/C++ Libraries OpenGl ve SQLite kutuphaneleri iceriyor. 
bu kutuphaneler dogrudan android framework ya da performans isteyen app'ler icin kullanilabiliniyor. 
Android Framework ActivityManager, NotificationManager, Content Provider gibi yuksek seviyeli servisleri sagliyor.
Apps bizim yaptigimiz ya da telefonla/saatle dahili gelen rehber, saat gibi uygulamalar.

[[Android]]
[[Intent]]
[[Implicit Intent]]
[[Explicit Intent]]
[[Intent Filter]]
[[Pending Intent]]
[[Serializable & Parcelable]]
[[Context]]
[[Context Wrapper]]
[[Application Class]]
[[AndroidManifest File]]
[[Activity Lifecycle]]
[[Fragment Lifecycle]]
[[Service]]
[[Service Lifecycle]]
[[BroadcastReceiver]]
[[ContentProvider]]
[[Handling Configuration Change]]
[[Avoiding Memory Leaks]]
[[ANR]]
[[Deep Links]]
[[Tasks & Back Stack]]