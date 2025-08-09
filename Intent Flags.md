Activity'lerin nasil launch olacagini ve intent gonderildiginde back stack'in nasil davranacagini belirler.
Intent olusturulurken programmatically uygulanir, specific senaryolar icin daha esnek bir yapi sunar.

Sik kullanilan flagler:
- FLAG_ACTIVITY_NEW_TASK: Activity'i yeni task'de baslatir ya da hali hazirda varsa onu one getirir.
- FLAG_ACTIVITY_CLEAR_TOP: Eger activity back stack'de varsa ustundeki tum activity'ler silinir ve var olan instance intent'i handle eder.
- FLAG_ACTIVITY_SINGLE_TOP: Eger activity instance'i back stack'de en ustteyse yeni instance olusturulmaz. Genel olarak bu flag baska flagler'le kombine edilir.
- FLAG_ACTIVITY_NO_HISTORY: Activity'nin back stack'e girmesini engeller yani activity'den cikildigi zaman geri donus olmaz.

```
val intent = Intent(this, SecondActivity::class.java).apply {
	flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TOP
}
startActivity(intent)
```

Bu ornekte SecondActivity eger back stack'de yoksa yeni task'de baslatilir. Eger back stack'de varsa ustundeki tum activity'ler temizlenir.