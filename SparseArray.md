Integer key'leri object value'lara mapleyen bir veri yapisidir. HashMap'e benzese de key'lerin integer olmasi ili optimize edilmistir. Integer key'lerle calisirken Map ya da HashMap'e gore daha memory-efficient'tir.

Key Characteristics:
- Memory Efficiency: key-value mapping icin HashTable kullanan HashMap'ten farkli olarak SparseArray auto-boxing'den (primitive int'i Integer'a cevirme) kacinir ve Entry gibi data objelerine ihtiyac duymaz. Bu sayede daha az memory tuketir.
- Performance: Cok buyuk dataset'lerde HashMap kadar hizli olmasa da orta buyuklukte dataset'lerde memory optimizasyonu sayesinde daha iyi performans gosterir.
- No Null Keys: Primitive key'lerle calistigi icin null key kabul etmez.

```
import android.util.SparseArray

val sparseArray = SparseArray()
sparseArray.put(1, "One")
sparseArray.put(2, "Two")

// Accessing elements
val value = sparseArray[1] // "One"

// Removing an element
sparseArray.remove(2)

// Iterating over elements
for (i in 0 until sparseArray.size()) {
	val key = sparseArray.keyAt(i)
	val value = sparseArray.valueAt(i)
	println("Key: $key, Value: $value")
}
```

Benefits of Using SparseArray Over an Array or HashMap
- Avoids Auto-Boxing: ```HashMap<Integer, Object>``` icinde key'ler Integer objeleri olarak depolanir bu sebeple boxing ve unboxing yuku olusur. SparseArray direkt olarak int key'leri kullanir.
- Memory Savings: HashMap Entry gibi birden fazla objeler yaratirken SparseArray key ve value'lari depolamak icin primitive array'leri kullanir.
- Compact Data Storage: Az sayida key-value cifti olan veya key'lerin genis tamsayi araligina seyrek sekilde dagitildigi dataset'leri icin uygundur.
- Purpose-Built for Android: Sinirli kaynaklara sahip senaryolari yonetmek icin ozel olarak Android'e yonelik tasarlanmistir. Ozellikle Android UI component'lerinde View ID'lerini object'lere map'lemekte etkilidir.

Limitations:
- Performance Trade-off: Buyuk dataset'lerde elementlere ulasmak HashMap'a gore daha yavastir. Cunku Hashmap key aramada binary serch kullanir.
- Integer Keys Only: Sadece integer key'ler kullanilir.