Screen rotation, locale change, dark ve light theme arasi degistirme, font size'i ve weight'i degistirme gibi eventlerde canfiguration change olur.
Android os activity'i restartlar ve bu esnada gecici UI state'in kaybedilmesiyle karsilasilanabilinir. Bunu onlemek icin sunlar yapilabilinir:
- Save and Restore UI State: onSaveInstanceState() ve onRestoreInstanceState() methodlari kullanmak.
- Jetpack ViewModel: Ui related data'yi ViewModel class'ina koymak. ViewModel objeleri activity recreation'dan etkilenmemek icin design edilmistir.
- Handle Configuration Changes Manually: Specific bir configuration change event'i icin resource'larin update edilmesi gerekmiyorsa ve bunun icin activity restart'i istemiyorsak Manifest.xml dosyasina android:configChanges attribute'unu ekleyip onConfigrautionChanged() methodunu override edebiliriz.
- Utilize rememberSaveable in Jetpack Compose: onSaveInstanceState() gibi calisir ama Compose'a ozeldir.

Ek Ipuclari:
- Navigation and Back Stack Preservation: Navigation componenti kullanmak navigation back stack'ini configuration change'den korur.
- Avoid Configuration-Dependent Data: Mumkun oldukca UI layer'da configuration-dependent degerleri tutmamaliyiz. ViewModel kullanabiliriz.