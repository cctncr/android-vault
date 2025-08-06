Android Specific Interface'idir.
Yuksek performans ve inter-process communication (IPC) icin tasarlanmistir.
Serializable'dan daha performanslidir cunku android icin optimize edilmistir ve reflection tabanli degildir.
Cok fazla cegici obje olusturmaz boyle garbage collection'i en aza indirir.

	@Parcelize
	class User(val firstName: String, val lastName: String, val age: Int) : Parcelable

