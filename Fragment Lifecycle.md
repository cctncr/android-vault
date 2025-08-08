Bir anlamda activity lifecycle'ini kopyalar ama ek method'lari ve davranislari vardir.

- onAttach(): ilk cagrilan callback. Fragment simdi activity'e baglandi ve context'i ile interact edebilir.
- onCreate(): fragmenit initialize etmek icin cagrilir. Bu asamada fragment olusmustur ama UI'i olusturulmamistir. onemli componentleri initialize ettigimiz ve saved state'i  restore ettigimiz yerdir.
- onCreateView(): Fragment UI'i ilk defa cizdirildiginde cagrilir. but methodda fragment'in layout'inin root view'ini return ediyoruz. LayoutInflater kullanip layoutu inflate ediyoruz.
- onViewStateRestored(): Fragment'in view hiyerarsisi olusturulduktan ve saved state restore edildikten sonra cagrilan method.
- onViewCreated(): Fragment'in view'i olusturulduktan sonra cagrilir. Genelde Ui componentlerin ve kullanici etkilisimleri icin gerekli logic yazilir.
- onStart(): Fragment kullaniciya gorunur olur. Activity onStrart()'ina esdeger. Yani fragment aktif ama yuzeyde degil.
- onResume(): Fragment tam olarak aktif ve yuzeyde. Kullaniciyla etkilesime acik.
- onPause(): Fragment yuzeyde degil ama hala gorunur. Fragment focus'u kaybetmek uzere ve burada fragment yuzeyde degilken aktif olmamasi gereken taskler durdurulmali.
- onStop(): Fragment artik gorunur degil. Burada fragment ekran disindayken durdurulmasi gereken seyleri durdurmaliyiz.
- onSaveInstanceState(): UI related state data'nin fragment destroy edilmeden once kaydedilmesi icin cagrilir.
- onDestroyView(): Fragment'in view hiyerarsisi kaldirildigi zaman cagrilir. View ile ilgili resource'lar temizlenmelidir.
- onDestroy(): Fragment'in kendisi destroy edildigi zaman cagrilir. Tum resource'lar temizlenmelidir. Burada hala Fragment Activity'e attached haldedir.
- onDetach(): Fragment parent activity'den detach olmustur. Bu son callback ve fragment lifecycle'i bitti.

fragmentManager ve childFragmentManager arasindaki farklar nelerdir?
Farkli amaclari ve farkli scope'lari vardir.
fragmentManager, FragmentActivity ya da Fragment'in kendis ile ilgilidir ve activity level'da fragmentin yonetiminden sorumludur. Adding, replacing, removing gibi.
Activity icinde supportFragmentManager uzerinden fragmentmanager'a erisebiliriz.
Nested fragment icindeki child fragment'lari childFragmentManager ile yonetebiliriz. Yani child fragmentlarin lifecycle'i parent fragment'a bagli diyebiliriz. Eger parent fragment destroy edilirse child'lar da destroy olur.

Key Differences:
- Scope: 
	- fragmentManager activity level'da operate eder yani bagli olduklari activity uzerinden yonetir.
	- childFragmentManager fragment'da operate eder yani child fragment'larin bagli olduklari parent fragment uzerinden yonetir.
- Lifecycle: 
	- fragmentManager ile yonetilen fragmentlar activity lifecycle'ini takip eder.
	- childFragmentManager ile yonetilen fragmentlar parent fragment lifecycle'ini takip eder.

Fragment'in host activity'e bagli kendi bir lifecycle'i vardir ama fragment'in view hiyerarsisinin farkli kendi bir lifecycle'i vardir.
vewiLifecycleOwner instance'i bu durumu yonetmemize olanak tanir.
viewLifecycleOwner fragment'in view hiyerarsisine bagli bir LifecycleOwner'dir ve fragment'in view'inin lifecycle'ini temsil eder.
onCreateView cagrildigi zaman baslar ve onDestroyView cagrildiginda biter.
Ui related data'lari ya da resource'lari bind etme isleminde memory leak gibi sorunlari engelleyebilmemizi saglar.
Fragment'in view hiyerarsisinin lifecycle'i fragment'in lifecycle'indan daha kisadir bu sebeple fragmentin lifecycle'ini kullanirsak view'a destroy edildikten sonra o view'a ulasmaya calismak gibi sorunlarla karsilasabiliriz. Bu yuzden viewLifecycleOwner'i kullanmaliyiz.

Ornek:
	class MyFragment : Fragment(R.layout.fragment_example) {
		private val viewModel: MyViewModel by viewModels()

		override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
			super.onViewCreated(view, savedInstanceState)

			// Observe LiveData using viewLifecycleOwner
			viewModel.data.observe(viewLifecycleOwner) { data ->
				// Update the UI with new data
				textView.text = data
			}
		}
	}

Bu ornekte LiveData'yi memory leak'ten kacinarak observe etmis oluyoruz. viewLifecycleOwner fragment'in view hiyerarsisi destroy edildigi zaman obsever'in kaldirilacagini temin ediyor. Fragment hala ayakta olsa bile.

lifecycleOwner vs viewLifecycleOwner:
- lifecycleOwner (fragment'in lifecycle'i) fragment'in bagli oldugu aktivity'e bagli olan ve daha uzun lifecycle'a sahip olan fragment'in lifecycle'ini temsil eder. 
- viewLifecycleOwner (fragment'in view lifecycle'i) fragment'in view'inin lifecycle'ini temsil eder. onCreateView'da baslar ve onDestroyView'da biter.


