Orijinal context'i modify eden ya da behavior'ini extend eden ara katman gibi calisir.
Context Wrapper sayesinde context'in fonksiyonalitesi context'i degistirmeden customize edebiliriz.
Context'in specific behaviors'larini override etmek ya da genisletmek icin kullanilir. Orijinal context'e erisimi kesip ek ozellikler ve davrasinlar ekleyebiliriz.
- Custom Context: farkli bir theme ya da ozel bir yontemle resource'lari handle etmek icin custom context olusturmak
- Dynamic Resource Handling: Context'i wrap edip dynamic olarak string, dimensions ya da styles gibi resources'lari modify ya da provide etmek.
- Dependency Injection: Dagger ve hilt dependency injection icin context wrapper olusturup componentlere custom context attach eder.

Custom theme kullanmak ContextWrapper kullanim ornegi: 
```
class CustomThemeContextWrapper(base: Context) : ContextWrapper(base) {
	override fun getTheme(): Resources.Theme {
		val theme = super.getTheme()
		theme.applyStyle(R.style.CustomTheme, true) // Apply a custom theme
		return theme
	}
}

class MyActivity : AppCompatActivity() {
	override fun attachBaseContext(newBase: Context) {
		super.attachBaseContext(CustomThemeContextWrapper(newBase))
	}
}
```

Benefits: 
- Reusability
- Encapsulation
- Compatibility

Activity icinde this keyword'u ve baseContext Context'e erisim saglar ama farkli amaclarla kullanilir ve android context hierarchy'sinde farkli levelleri temsil eder.

this keyword'u  Activity class'inin current instance'ini referans eder ve Activity ContextWrapper'in subclass'idir.(ContextWrapper da Context'in subclass'idir). Boylece 'this' activity'nin specific context'ine erisim saglar. Bu sayede baska bir activity launch etme ya da bu aktivity'e bagli bir dialog'u gosterme gibi Activity'e specific method'lari cagirmak icin kullanabiliriz.
Ornek:
```
val intent = Intent(this, AnotherActivity::class.java)
startActivity(intent)

val dialog = AlertDialog.Builder(this)
	.setTitle("Example")
	.setMessage("This dialog is tied to this Activity instance.")
	.show()
```

baseContext activity'nin uzerine insa edilmis oldugu temel ya da "base" context'i temsil eder.
Ornek: 
```
val systemService = baseContext
	.getSystemService(Context.LAYOUT_INFLATER_SERVICE)
```

Key Differences:
- Scope: this Activity'nin suanki instance'ini ve lifecycle'ini isaret eder. baseContext Activity'nin uzerine kurulmus oldugu daha lower-level context'i isaret eder
- Usage: this activity'nin lifecycle'ine ya da Ui'ya bagli operasyonlar icin kullanilir, bir activity baslatmak ya da dialog gostermek gibi. baseContext Context'in core implementasyonu ile interaction'da bulunmak icin kullanilir, ContextWrapper senaryosu gibi.
- Hierarchy: baseContext Activity'nin temel context'idir. baseContext'e erismek aradaki ContextWrapper ile olusan Activity'e ozgu functionality'i baypass eder.

Ornek: 
```
class CustomContextWrapper(base: Context) : ContextWrapper(base) {
	override fun getSystemService(name: String): Any? {
		// Example: Modify the LayoutInflater
		if (name == Context.LAYOUT_INFLATER_SERVICE) {
			val inflater = super.getSystemService(name) as LayoutInflater
			return inflater.cloneInContext(this)
		}
		return super.getSystemService(name)
	}
}

override fun attachBaseContext(newBase: Context) {
	super.attachBaseContext(CustomContextWrapper(newBase))
}
```