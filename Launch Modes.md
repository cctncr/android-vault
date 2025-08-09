Launch mode'lari activity'nin back stack'de nasil konumlandirildigini ve yonetildigini belirler. 
AndroidManifest.xml dosyasina ```<activity>``` tag'i altinda declare edilir.4 temel launch mode:
- standard: Default olarak gelen launch mode. Instance'i halihazirda olsa bile activity instance'i olusturulur ve back stack'e eklenir.
- singleTop: Activity instance'i back stack'in en ustunde hali hazirda varsa yeni instance olusturulmaz ve intent olan instance'in onNewIntent() method'unda handle edilir.
- singleTask: Activity'nin sadece bir instance'i back stack'de olur. Activity cagrildiginda eger instance'i hali hazirda back stack'de varsa en uste gelir ve onNewIntent() cagrilir. Bu app entry pointi olarak calisacak activity'ler icin kullanislidir.
- singleInstance: singleTask'e benzer fakat activity kendi task'ine sahip olur.