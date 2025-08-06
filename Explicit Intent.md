component'i direkt ismiyle cagirarak kullanimlarda.
hedef component biliniyorsa kullanilir.
mesela uygulamadaki bir activity'den baska bir activity'e gecme.

val intent = Intent(this, TargetActivity::class.java)
startActivity(intent)

