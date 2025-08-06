Farkli componentler arasinda data pass'laman icin kullanilirlar (mesela fragment'lar ve activityler arasi). 
Performans ve fonksiyonalite farklari vardir.

Feature                       Serializable                                                  Parcelable
Type                            Standard Java interface                            Android specific interface
Performanc                Slower, uses reflection                              Faster, optimized for Android
Garbage Creation      Creates more garbage (more objects)    Creates less garbage(efficient)
Use Case                    Suitable for general Java Usage              Preferred for Android espically IPC 

[[Serializable]]
[[Parcelable]]
