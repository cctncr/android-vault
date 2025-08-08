ContentProvider kurulmus data setinin ulasilmasini yoneten ve app'ler arasi bu data setini paylasma icin standart interface sunan bir componenttir.
Merkezi repo olarak is gorur ve diger appler ya da componentler data icin query, insert, update ya da delete gibi islemler yapabilirler.
Uygulamalar arasi guvenli ve consistent sekilde veri paylasimi saglar.
Ozellikle; birden fazla uygulama ayni dataya ihtiyac duydugunda ya da diger app'ler icin, database'i ya da dahili bellek mimarisini expose etmeden, data saglanmak istenildiginde cok yararlidir.
SQLite, file system ya da network-based data bile olabilen data soruce'u abstract hale getirir ve bu data icin bir unified interface saglar.
Data erisimi icin URI (Uniform Resource Indentifier) kullanir.
URI bilesenleri:
- Authority: ContentProvider'i identify eder (com.example.myapp.provider gibi)
- Path: data type'ini specify eder (/users, /products gibi)
- ID (optinal): datasetteki specific bir item'i isaret eder

ContentProvider implement etmek icin ContentProvider class'ini extend edip su methodlari override etmek lazim:
- onCreate: ContentProvider initialize olur
- query(): datayi getirir
- insert(): yeni data ekler
- update(): var olan datayi gunceller
- delete(): datayi kaldirir
- getType(): data'nin MIME type'ini doner

Ornek:
	class MyContentProvider : ContentProvider() {
		
		private lateinit var database: SQLiteDatabase

		override fun onCreate(): Boolean {
			database = MyDatabaseHelper(context!!).writableDatabase
			return true
		}

		override fun query(
			uri: Uri,
			projection: Array?,
			selection: String?,
			selectionArgs: Array?,
			sortOrder: String?
		): Cursor? {
			return database.query("users", projection, selection, selectionArgs, null, null, sortOrder)
		}

		override fun insert(uri: Uri, values: ContentValues?): Uri? {
			val id = database.insert("users", null, values)
			return ContentUris.withAppendedId(uri, id)
		}

		override fun update(uri: Uri, values: ContentValues?, selection: String?, selectionArgs: Array?): Int {
			return database.update("users", values, selection, selectionArgs)
		}

		override fun delete(uri: Uri, selection: String?, selectionArgs: Array?): Int {
			return database.delete("users", selection, selectionArgs)
		}

		override fun getType(uri: Uri): String? {
			return "vnd.android.cursor.dir/vnd.com.example.myapp.users"
		}
	}

ContentProvider AndroidManifest.xml dosyasinda declare edilmelidir:

```
provider
    android:name=".MyContentProvider"
    android:authorities="com.example.myapp.provider"
    android:exported="true"
    android:grantUriPermissions="true" /&gt;
```

ContenProvider'dan dataya erismek icin ContentResolver class'i kullanilabilinir:

```
val contentResolver = context.contentResolver

// Query data
val cursor = contentResolver.query(
	Uri.parse("content://com.example.myapp.provider/users"),
	null,
	null,
	null,
	null
)

// Insert data
val values = ContentValues().apply {
	put("name", "John Doe")
	put("email", "johndoe@example.com")
}

contentResolver.insert(Uri.parse("content://com.example.myapp.provider/users"), values)
```

Kullanim alanlari:
- farkli app'ler arasi data paylasma
- app startup process'de componentleri ya da resource'lari initalize etme
- contacts, media files, ya da app-specific gibi yapilandirilmis data icin erisim sunma
- Contacts app ya da File Picker gibi android system ozellikleri icin entegrasyon saglama 
- Iyi guvenlik

Genelde resource ya da kutuphane initialization islemi Application class'inda olur ama daha iyi bir seperation of concern icin bu logic'i encapsulate edip ContentProvider'a tasiyabiliriz.
AndroidManifest.xml dosyasina register olmus ContentProvider'in onCreate() method'u Applicatoin.onCreate()'den once cagrilir. Mesela Firebase Android SDK kendi custom ContentProvider'ini kullanir bu yuzden Applcation class'i icinde FirebaseApp.initializeApp(this) gibi bir kullanim yapmamiz gerek kalmadan Firebase SDK'i otomatik olarak initialize olur.
App Startup Jetpack kutuphanesi de ayni yontemi kullaniyor.
