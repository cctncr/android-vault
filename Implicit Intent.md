istenen component specify edilmedigi durumda denir.
action, category ve data durumuna gore sistem hangi component(lerin) halledecegine karar verir.
genelde baska app'lerin halledecegi durumlarda kullanilir.
mesela bir tarayicida bir linki acmak ya da baska uygulamalara bir bilgi paylasmak.

1 val intent = Intent(Intent.ACTION_VIEW)
2 intent.data = Uri.parse("https://www.example.com")
3 startActivity(intent)
