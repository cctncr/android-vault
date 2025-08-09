Bundle activity, fragment ve service gibi compenent'ler arasinda data pass'lamizi saglayan key-value pair'ler seklinde bir veri yapisidir.
Lightweight'tir ve Android os'un kolayca yonetebilecegi ve iletebilecegi sekilde data'yi serialize eder.

Genel kullanim alanlari:
- Passing Data Between Activities: Yeni bir activity baslatirken Bundle Intent'e attach olup target activity'e aktarilabilinir.
- Passing Data Between Fragments: Fragment transaction'larinda Bundle setArguments() ve getArguments() ile fragmentler arasi tasinabilinir.
- Saving and Restoring Instance State: onSaveInstanceState() ve onRestoreIntanceState() lifecycle methodlarinda configuration change olurken gecici UI state'i save etmek ve ve restore etmek icin kullanilir.
- Passing Data to Services: Service baslatirken ya da bound service'e data pass'lerken Bundle data tasiyabilir.

Key'ler string; value'lar primitive type, Serializable, Parcelable ve diger Bundle'lar olabilir.

Activity'ler arasi data pass'leme:
```
// Sending data from Activity A
val intent = Intent(this, ActivityB::class.java).apply {
	putExtra("user_name", "John Doe")
	putExtra("user_age", 25)
}
startActivity(intent)

// Receiving data in Activity B
val name = intent.getStringExtra("user_name")
val age = intent.getIntExtra("user_age", -1)
```
Burada Bundle Intent.putExtra() ile package'lendi.

Fragment'ler arasi data pass'leme:
```
// Sending data to Fragment
val fragment = MyFragment().apply {
	arguments = Bundle().apply {
		putString("user_name", "Jane Doe")
		putInt("user_age", 30)
	}
}

// Retrieving data in Fragment
val name = arguments?.getString("user_name")
val age = arguments?.getInt("user_age")
```

Saving ve Restoring State:
```
override fun onSaveInstanceState(outState: Bundle) {
	super.onSaveInstanceState(outState)
	outState.putString("user_input", editText.text.toString())
}

override fun onRestoreInstanceState(savedInstanceState: Bundle) {
	super.onRestoreInstanceState(savedInstanceState)
	val userInput = savedInstanceState.getString("user_input")
	editText.setText(userInput)
}
```