istenen component specify edilmedigi durumda denir.
action, category ve data durumuna gore sistem hangi component(lerin) halledecegine karar verir.
genelde baska app'lerin halledecegi durumlarda kullanilir.
mesela bir tarayicida bir linki acmak ya da baska uygulamalara bir bilgi paylasmak.

```
val intent = Intent(Intent.ACTION_VIEW)
intent.data = Uri.parse("https://www.example.com")
startActivity(intent)
```

