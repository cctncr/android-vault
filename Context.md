Context environment'i ya da application state'i temsil eder. application specific resource'lari ve classlar'i provide eder.
App ile android system'i arasinda componentlerin erisebilecegi bir kopru gorevi gorur. resources, databases, servicies gibi.
Activiti'lerin baslatilmasi, assetlere erisilmesi ya da layout'larin inflate edilmesi gibi task'ler icin onemlidir.
- string, drawables ve dimensions gibi resources'lara ulasmak
- layout inflate etmek
- activity ve service baslatmak
- ClipboardManager ya da ConnectivityManager gibi system-level service'lere ulasmak. getSystemService()
- Database ve SharedPreferences ulasimi icin
Memory leak ya da crash'lerle karsilasmamak icin uygun context'i kullanmak gerekiyor. Bunlar:
[[Application Context]]
[[Activity Context]]
[[Service Context]]
[[Broadcast Context]]

Dikkat Edilmesi gerekenler:
- Context reference'i kendisinin life-cycle'ini asan bir yerde tutulmamalidir. Garbage collector temizleyemez. en sik yapilan hata activity ve fragment contextini disariya tasimaktir.
- UI related task'ler icin Activity Context kullanilmalidir. inflating layout ya da dialog gostermek gibi
- Application Context UI life-cycle'inda bagli olmayan operasyonlar icin kullanilmalidir. bir kutuphane initialize etmek gibi
- Context'in bagli oldugu component destroyed edildiyse (activity ya da fragment olabilir) bu context'e erismek crash'lere yol acabilir.
- Context main thread icin design edilmistir. bacground thread'de kullanmak sorunlara yol acabilir. Ui related context resource'lara ulsamadan once main thread'e gecilmesi lazim.
[[Context Wrapper]]