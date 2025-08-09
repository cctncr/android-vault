Configuration change olurken os yeni configuration'i uyarlamak icin suanki activity'i destroy edip recreate edebilir.
Default davranis:
- Activity Destruction and Recreation: Activity destroy edilir ve yeniden olusturulur. Bu adimlar:
	- onPause(), onStop() ve onDestroy() methodlari cagrilir.
	- onCreate() methodu cagrilip activity yeniden olusturulur. 
- Resource Reloading: os resource'lari yeni config'e (screen rotation, theme, locale gibi) gore yeniden olusturur. 
- Data Loss Prevention: Data kaybini onlemek icin dev'ler instance state'i onSaveInstanceState() ve onRestoreInstanceState() methodlariyla ya da ViewModel ile saklyabilir.

```
override fun onSaveInstanceState(outState: Bundle) {
	super.onSaveInstanceState(outState)
	outState.putString("user_input", editText.text.toString())
}

override fun onCreate(savedInstanceState: Bundle?) {
	super.onCreate(savedInstanceState)
	setContentView(R.layout.activity_main)

	val restoredInput = savedInstanceState?.getString("user_input")
	editText.setText(restoredInput)
}
```

Recreation'i triger'layan ana configuration change'ler:
- Screen Rotation: portrait ve landscape arasi degisim. Layout'un yeni dimension'lara yenilenmesi lazim
- Dark/Light Theme Changes: Telefonun Dark ve light modlari arasi degisim. Theme-specific resource'larin (styles, colors gibi) yenilenmesi lazim.
- Font Size Changes: Telefonun font size'inin degismesi. Text reource'lari yenilmesi lazim.
- Language Changes: Dil degismesi. Localized source'larin (baska dillerdeki stringler gibi) yenilenmesi lazim.

Activity Recreation'dan kacinma:
Manifest dosyasinda android:configChanges attribute'u ile Activity'nin yeniden olusmasini engelleyebiliriz.
```
<activity
	android:name=".MainActivity"
	android:configChanges="orientation|screenSize|keyboardHidden" />
```
Bu senaryoda Activity recration'a girmez onun yerine onConfigurationChange() method'u cagrilir. Boylece developer manuel olarak handle edebilir.
```
override fun onConfigurationChanged(newConfig: Configuration) {
	super.onConfigurationChanged(newConfig)

	if (newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
		// Handle landscape-specific changes
	} else if (newConfig.orientation == Configuration.ORIENTATION_PORTRAIT) {
		// Handle portrait-specific changes
	}
}
```