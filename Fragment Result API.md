Bazi durumlarda fragmentlar'in baska fragment'a ya da host activity'e tek seferli data pass'lemesi gerekebilir. Mesela QR kod okuyan fragment'in okunan data'yi geri onceki fragment'a pass'lemesi gibi.
Fragment 1.3.0+ versiyonundan sonra her FragmentManager, FragmentResultOwner implement eder. Bu fragment'larin result listener'lari araciligiyla birbirlerine direkt olarak referans duymadan iletisim kurmalarini saglar. Bu sayede data pass'leme kolayasir ve loose coupling saglanir.

Fragment B'den Fragment A'ya data pass'lemek icin:
1- Set a result listener in Fragment A
setFragmentResultListener() kullanilarak fragment STARTED olurken data'yi almasi saglanir
```
class FragmentA : Fragment() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)

		// Register listener to receive result
		setFragmentResultListener("requestKey") { _, bundle ->
			val result = bundle.getString("bundleKey")
			// Handle the received result
		}
	}
}
```
2- Send the result from Fragment B
```
class FragmentB : Fragment() {
	private lateinit var button: Button

	override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
		super.onViewCreated(view, savedInstanceState)

		button = view.findViewById(R.id.button)
		button.setOnClickListener {
			val result = "result"
			// Send the result to FragmentA
			setFragmentResult("requestKey", bundleOf("bundleKey" to result))
		}
	}
}
```
setFragmentResult() result'u girilen key ile birlikte FragmentManager icinde ile depolar. Eger Fragment A aktif degiles result Fragment A resume olana kadar saklanir.

Fragment Result Davranislari:
- Single Listener per Key: Her key sadece bir listener'a ve bir result'a sahip olabilir
- Pending Results Are Overwritten: En son gelen result oncekileri ezer.
- Results Are Cleared After Being Consumed: Result listener tarafindan toplandiktan sonra FragmentManager'dan silinir.
- Fragments in Back Stack Do Not Receive Results: Fragment result'u almak icin back stack'den pop olup STARTED olmalidir.
- Listeners in STARTED State Trigger Immediately: Eger fragment B result set ettiginde fragment A zaten aktifse listenirler hemen gerceklestirir.