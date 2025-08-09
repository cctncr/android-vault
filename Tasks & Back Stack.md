Task:
Task bir activity koleksiyonudur.
LIFO (last-in, first-out) prensibiyle calisan back stack'de organize olmustur.
Task bir activity launcher ile ya da intent araciligiyla baslatildiginda olusur.
Task'ler birden fazla app ve intent'lerin ve acitivty launch mode'larin nasil belirlendigine gore onlarin acitivity'lerini barindirabilir.
Mesela e mail'de bir link'e tiklamak browser'i ayni task'e sahip olacak sekilde acabilir.
Task ilgili activity'ler destroy olana kadar aktif kalir.

Back Stack:
Back stack activity gecmisini task'ler araciligiyla yonetir.
Kullanici bir activity'e navigate oldugunda o activity stack'e push edilir.
Back butonuyla vs. geri gelmek activity'nin pop olmasini saglar. Altindaki en ustte yer alir.

Task ve back stack activity launch mode'lari ve intent flag'lerine gore yonetilir.
Bu yapilar sayesinde activity'lerin task ve back stack'deki davrasinlarini yonetebiliriz.

[[Launch Modes]]
[[Intent Flags]]

