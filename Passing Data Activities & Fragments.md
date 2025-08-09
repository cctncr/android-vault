### Activity'ler arasi data pass'leme
Bir activity'den digerine data pass'lenirken en yaygin kullanilan yontem Intent kullanmaktir. Data putExtra() ile key-value pair'leri sekilde Intent'e eklenir ve Bundle getIntent() ile intent uzerinden alinir.

```
// Sending Activity
val intent = Intent(this, SecondActivity::class.java).apply {
	putExtra("USER_NAME", "John Doe")
	putExtra("USER_AGE", 25)
}
startActivity(intent)

// Receiving Activity
class SecondActivity : AppCompatActivity() {
	super.onCreate(savedInstanceState)
	setContentView(R.layout.activity_second)

	val userName = intent.getStringExtra("USER_NAME")
	val userAge = intent.getIntExtra("USER_AGE", 0)
	Log.d("SecondActivity", "User Name: $userName, Age: $userAge")
}
```

### Fragment'lar arasi data pass'leme
Fragment'lar arasi iletisimde Bundle kullanilablinir. Bundle olusturulup arguments uzerinden alinabilinir.

```
// Sending Fragment
val fragment = SecondFragment().apply {
	arguments = Bundle().apply {
		putString("USER_NAME", "John Doe")
		putInt("USER_AGE", 25)
	}
}
parentFragmentManager.beginTransaction()
	.replace(R.id.fragment_container, fragment)
	.commit()

// Receiving Fragment
class SecondFragment : Fragment() {
	override fun onCreateView(
		inflater: LayoutInflater, container: ViewGroup?,
		savedInstanceState: Bundle?
	): View? {
		val view = inflater.inflate(R.layout.fragment_second, container, false)
		
		val userName = arguments?.getString("USER_NAME")
		val userAge = arguments?.getInt("USER_AGE")
		Log.d("SecondFragment", "User Name: $userName, Age: $userAge")

		return view
	}
}
```

### Jetpack Navigation Kutuphanesi ile data passlame
Safe Args plugini ile birlikte Jetpack Navigation kullanarak arkaplanda direction ve argument class'lari olusturup type-safe data pass'layabiliriz.

1- Define arguments in the navigation graph:
nav_graph.xml icinde:
```
<fragment
	android:id="@+id/secondFragment"
	android:name="com.example.SecondFragment">
	<argument
	android:name="username"
	app:argType="string" />
</fragment>
```

2- Pass data from the source fragment
Safe Args plugin'i compiler time'da destination objesi ve builder class'lari olusturur. Bu sayede explicitly ve guvenli olarak argument pass edebiliriz.
```
val action = FirstFragmentDirections
	.actionFirstFragmentToSecondFragment(username = "skydoves")
findNavController().navigate(action)
```

3- Retrieve data in the destination fragment
```
val username = arguments?.let {
	SecondFragmentArgs.fromBundle(it).username
}
```

### Shared ViewModel ile data pass'leme:
Eger fragment'lar ayni aktivity icinde iletisim kurmasi gerekiyorsa shared ViewModel onerilen yaklasimdir. activityViewModel() methodu kullanilir ve fragment'larin ayni ViewModel instance'ini paylasmasi saglanir. Boylece fragment'lar tightly couple olmaz ve lifcycle-aware, reaktif data paylasimini saglar.

```
// Shared ViewModel
class SharedViewModel: ViewModel() {
	private val _userData = MutableStateFlow(null)
	val userData : StateFlow<User?>
	
	fun setUserData(user: User) {
		_userData.value = user
	}
}

// Fragment A (Sending data)
class FirstFragment : Fragment() {
	private val sharedViewModel: SharedViewModel by activityViewModels()

	fun updateUser(user: User) {
		sharedViewModel.setUserData(user)
	}
}

// Fragment B (Receiving data on another Fragment)
class SecondFragment : Fragment() {
	private val sharedViewModel: SharedViewModel by activityViewModels()

	override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
		lifecycleScope.launch {
			viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.RESUMED) {
				sharedViewModel.userData.collectLatest { user ->
				// ...
			}
		}
	}
}

// Activity (Receiving data on the Activity)
class MainActivity : ComponentActivity() {
	private val sharedViewModel: SharedViewModel by viewModels()

	override fun onCreate(savedInstanceState: Bundle?) {
		lifecycleScope.launch {
			lifecycle.repeatOnLifecycle(Lifecycle.State.RESUMED) {
				sharedViewModel.userData.collectLatest { user ->
					// ...
				}
			}
		}
	}
}
```