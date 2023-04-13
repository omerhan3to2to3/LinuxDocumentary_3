## root kullanıcısı
Linuxda aslında birçok kullanıcı var ve bunlardan en yetkilisi root kullanıcısı. Bunu önceki derslerde gördük, nasıl gördük. Biz herhangi bir dosyada veya dizinde bir değişiklik yapmak istediğimizde ya da yetkilendirme-kısıtlama yapmak istediğimizde, bize izin verilmiyor şeklinde uyarı veriyordu. Biz de root kullanıcısına geçip bu işlemleri gerçekleştirebiliyorduk. Çünkü root bu yetkilerin hepsine sahip tek kullanıcı. Bu yüzden aslında rootu kullanırken tüm yetki bizde olduğu için tehlikeli ve bu durum aslında kötü sonuçlara yol açabiliyor. Bu yüzden root kullanıcısındayken yaptığınız işlemlere dikkat etmelisiniz.

Hatta bazı linux dağıtımlarında rootu varsayılan olarak kullanmanız yasaklı, çeşitli konfigürasyonları yaparak kullanabilirsiniz ama güvenlik tarafı dediğim gibi önerilen bir durum değil.  Bunu dosya hiyerarşisini gördüğümüzde linuxun neden güvenli olduğunu açıklarken de anlatmıştık aslında. Büyük sistemlerde belirli kullanıcılar belirli dosyalara erişimi oluyor ve buradaki dosyaları değiştirmeye çalışırken yanlış bir durumun ortaya çıkmaması adına yetkilendirilmelerle bu durumlar kontrol ediliyor.

Aslında her kullanıcının kendine verilen yetkiler çerçevesince hareket edebildiğini öğrenmiş olduk. Kullanıcıların dosya veya dizinler ile ilgili yapabileceği üç durum bulunuyor. Bunlar;

- okuma(r): Klasör listesini ve dosya içeriğini görüntüleme.

- yazma(w): Dosya veya klasör üzerinde değişiklik yapma.

- alıştırma(x): Hedef dosyayı çalıştırma veya klasör içerisine erişme.

Ben `ls -l`komutu ile bulunduğum dizindeki dosyaların izinlerini görebiliyorum. Aşağıdaki gibi liste halinde görüyorum.

Şimdi ben burayı görünce ne anlıyorum bunlara bakacağız.

'drwxr-xr-x' şeklinde gördüğümüz kısımlar dosya izinlerini ifade ediyor.

Başında 'd' olan izinler dizin olduğunu belirtiyor görüldüğü gibi mavi bold yazılı.

'-' işareti ile ayrılan kısımlar o izine sahip kullanıcı grubunu temsil ediyor. Daha iyi görmek için d harfini çıkardığımızda - işaretini ayırdığımız zaman geri kalan harfleri üç adet üçlü grup haline getirelim;

rwxr-xr-x=  rwx  r-x  r-x

rw-r--r--=  rw- r--  r—

İlk kısım dosya sahibinin iznini ifade ediyor.
İkinci kısım grup izinlerini ifade ediyor.
Üçüncü kısım ise diğer kullanıcıların izinlerini belirtiyor.

r : okuma yetkisi read

w : yazma yetkisi write

x : çalıştırma yetkisi execute

rwxr-xr-x=  rwx  r-x  r-x

Şimdiyse bunun ne anlama geldiğini inceleyelim

rwx : dosyanın sahibi olan kullanıcı okuyabilir, yazabilir, çalıştırabilir.

r-x : dosya sahibi kullanıcı grubu ile aynı gruba dahil kullanıcılar okuyabilir, çalıştırabilir fakat yazamaz(değişiklik yapamaz).

r-x : diğer kullanıcılar okuyabilir, çalıştırabilir fakat yazamaz(değişiklik yapamaz).

**Yetkilerin Değişimi(chmod)**

Bu erişim yetkilerini ben değiştiriyorum. Bunu root kullanıcısına geçerek yapıyorum. Bunun için kullandığımız komut ise `Chmod` komutu. Açılımı ise change mod oluyor.

Bunun kullanımı sırasında yanında kullandığımız parametreler var bunları anlamamız gerek öncesinde:

Yetkiyi kime vereceğiz

u : Dosya-dizinin sahibi

g : Dosya-dizinin sahibi ile aynı grupta bulunan kullanıcılar

o : Diğer kullanıcılar

a : Herkese açık.

Hangi yetkileri vereceğim:

= : Yetki eşitleme

+ : Yetki ekleme

- : Yetki çıkarma

Şimdi terminal üzerinde örneklerle bakalım kaliye geçiyorum

Şimdi daha önce joker bir karakterden bahsetmiştim linuxta kullanılan böyle kısayollarla beraber görmenizin sizin için de kolaylık sağlayacağını düşündüğüm için bunu kullanıcam. * karakteri bize klasördeki tüm dosyalara ulaşmamızı sağlıyordu.

`chmod +w *` komutuyla klasördeki tüm dosyalara write iznini vermiş oldum.

`ls -l `ile izin verip vermediğimi kontrol ediyorum.

`chmod g+rx *` aynı gruptaki kullanıcılara tüm dosyalarda read ve execute iznini verdik.

Şimdiyse yetki çıkartmak için kullanacağımız komutlara bakalım.
`chmod a-rwx *`,`ls -l`

Tüm dosyaların yetkilerini kaldırdık gördüğünüz gibi

Şimdi arayüzde de silemediğimi göstereceğim herhangi bir dosyayı aynı şekilde terminalde de silemiyorsunuz.

Sağda görülen sayısal ifadeler var bunlar da yetkileri ifade ediyor bunu göreceğiz şimdi.

Buna sekizli sistem deniyor

Read yetkisi 4 w 2 x 1 e karşılık geliyo ben bu yetkilerin hepsini vermek istersem topluyorum

r=4 + w=2 + x=1=toplam sayı 7 oluyo ve ben komutumu şöyle veriyorum

chmod 700 dosya adı

neden 700 oldu ben sadece dosya sahibine vermek istiyorum yetkiyi

ilk üçlü dosya sahibinindi orası 7 diğer gruplar ve kullanıcılara vermek istemiyorum o yüzden 0 ve 0 yan yana yazmış oluyorum.

Böylelikle sadece dosyanın sahibi tüm yetkilere sahip olmuş oldu.

Şimdi de; dosyanın sahibine tüm yetkileri, ortak gruptakilere yalnızca yazma yetkisini, diğer kullanıcılara da sadece okuma yetkisini verelim.

Dosya sahibi kullanıcıya verilecek tüm yetkiler için r(4)+w(2)+x(1)=7 sayısını kullanacağız.

Dosya sahibi ile ortak gruptaki kullanıcılar için vereceğimiz yazma yetkisi için yazma(w) karakterinin sayısal karşılığı olan 2 sayısını kullanacağız.

Diğer kullanıcılar için vereceğimiz yalnız okuma yetkisi için ise okuma(r) karakterinin sayısal karşılığı olan 4 sayısını kullanacağız.

Chmod 724 dosya adı

Komutu ile bunu sağladık

Şimdi biz belli dosyalar üzerinde yapabiliyoruz ama diyelim ki bir dizin ve altındaki tüm dosyaların belirlediğimiz yetkilendirilmeden yararlanmasının sağlanmasını istiyoruz. Bunun için yazdığımız komutun yanında -R parametresini kullanmalıyız.

Şimdi burada bir directory yok oluşturacağım

Chmod -R 422 directory

Bir komut daha var göstereceğim

Sistemde belirlediğiniz dosyayı bu komut ile dokunulmaz yapabiliyorsunuz yani rootun bile silmesine izin veremiyor biz bu komutu verdiğimizde

Chattr +i dosyaadı

Bunu nerde nasıl kullanıyoruz diyelim ki sistemin bile değiştirmesini istemediğimiz bir dosya var. Mesela sistemi yeniden başlattığımızda sistem ayarları yenilediği için dosyada yaptığımız konfigürasyonlar değişiyor. Bunun yaşanmaması için bu komutla beraber dosyanın ayarlarının kesinlikle değişmemesini sağlamış oluyoruz.

Bu tür  dosyaları listelemek için de lsattr komutunu kullanıyoruz.

Bu durumu geri almak için de

chattr -i test.txt

### KULLANICI İŞLEMLERİ


Şimdi sistemde root olmadan root olabildiğimiz bir durum var. Bunu Linux Grup Yönetimi dediğimiz sistem ile gerçekleştiriyoruz. Aynı grupta yer alan kullanıcılar bizim tanımlamamızla aynı haklara sahip olabiliyorlar.

Biraz daha ayrıntılı vermek gerekirse Linux ve UNIX sistemlerindeki kullanıcıları neden grup olarak kullanma ihtiyacında bulunuyoruz: 3 sebebe indirgeyebilirim bunları: 

1-Dosyaları veya diğer kaynakları Grup yönetimi sayesinde ilgili kullanıcılarla paylaşarak, sistemde erişim sınırlamalarıyla güvenlik sağlaması.

2-Kullanıcı yönetim ve denetiminde kolaylık sağlıyor olması.

3-Grup üyeliği, bu gruba izin verilen dosyalara, dizinlere veya cihazlara özel erişim sağlar. Bu madde baştaki madde ile benzer amaca hizmet ederek tamamen kontrolü elde tutmayı sağlamak için kullanılır.

Yani genel olarak Grup sistemini bir çeşit kontrol mekanizması gibi düşünebilirsiniz.

Şimdi gruplarla ilgili bilmemiz gereken neler var bunlara bakacağız

Bir grup oluşturulduğunda bu grubun bilgisi `/etc` dizini içerisinde yer alan group isimli dosyada tutuluyor. Yani mevcut grupları görüntülemek istersek `/etc` dizini içerisinde yer alan group dosyasına bakmamız gerekir. Bunun için `less /etc/group` , `more /etc/group` , `cat /etc/group` komutlarından herhangi birini kullanabiliriz. Çıktıları aşağıdaki şekilde olacaktır.

### Grup bilgileri çıktısı

1. **Grup_ismi :** Gruba verilen isimdir.

2. **Parola :** Parola alanını belirtiyor. Genelde parola kullanılmaz ancak kimi durumlarda kullanıldığı oluyor, bizim çıktımızda da x ile belirtilen alan parola kısmının boş olduğunu belirtiyor. Bu parola belirleme işlemi çok sık kullanılmasa da, ayrıcalıklı gruplarda uygulamak için yararlıdır.

3. **Grup Kimliği (GID) :** Atanan grup kimliğini(grup numarasını) belirtiyor.

4. **Grup Listesi :** Grubun üyesi olan kullanıcıların kullanıcı adlarının bir listesidir. Kullanıcı adları, virgülle ayrılmış şekilde belirtiliyor.

## Kullanıcı Gruplarını Sorgulamak

Kullanıcıların ait olduğu grupları görmek istersek komut satırına `id kullanıcı_adı `şeklinde komut vermemiz yeterli olacaktır. Ben "sulin" kullanıcı hesabı için sorgulama yapmak üzere konsola `id sulin` şeklinde komutumu veriyorum.

Gördüğünüz gibi "sulin" kullanıcı hesabı için burada; uid(user id /kullanıcı numarası), gid(group id/grup numarası) ve dahil olduğu gruplar listelenmiş oldu.

Burada çıktıda da görülen uid(user id/kullanıcı numarası) ve gid(group id/grup numarası) kavramlarına değinelim. Bu numara aralıkları kullanıcı hesabına göre değişiklik gösteriyor. Yani kullanıcı çeşidine göre numaraları üç temel gruba ayırabiliriz. O da şu şekilde ;

- root kullanıcısı : UID=0, GID=0

- sistem kullanıcısı : UID=1 - 499, GID=1 - 499

- normal kullanıcı : UID=500 < X, GID=500 < X (Buradaki X ifadeleri 500'den büyük tüm sayıları temsil etmektedir.)

Ayrıca id komutunun birçok parametresi vardır bazıları birazdan göreceğimiz grup oluşturma kısmındaki parametrelerden oluşuyor. Detaylı bilgi için man sayfasına bakabilirsiniz. Ben yine de örnek olması açısından birkaç parametresini göstereceğim sonrasında yine terminalden grup oluşturma user ekleme gibi konulara bakacağız.

- g : `id -g kullanıcı_adı` belirtilen kullanıcının grup numarasını(gid) verecektir.

- u : `id -u kullanıcı_adı` belirtilen kullanıcının kullanıcı numarasını(uid) verecektir. 

- G : `ìd -G kullanıcı_adı` belirtilen kullanıcının dahil olduğu tüm grupları(groups) verecektir.

**Kaliye geç grup oluşturma ve grupları nereden görebiliriz**

Yeni bir grup oluşturmak istersek `groupadd yeni_grup_adı` şeklinde komutumuzu kullanırız.

Ben örnek olması açısından "yeni" isimli bir grup oluşturmak için komut satırına `groupadd yeni` şeklinde komutumu veriyorum.

Ve oluşturduğumuz grubu sorgulamak için grup bilgilerinin tutulduğu dosyaya bakmak üzere `cat /etc/group ` | `grep grup_adı ` komutunu ya da direkt grupların saklandığı dosyaya bakalım

`Cat /etc/group` komutunu kullandığımızda tümünü görebiliyoruz.

Ayrıca grup oluşturulurken kullanılabilecek bazı parametreler var. Bunlar;

- g : Grup id belirleme. İstediğiniz numarayı başka bir gruba ait numara ile aynı olmayacak şekilde verebilirsiniz.

	Eğer aynı grup id ile başka bir grup eklemek istersek konsol bize "bu id ye sahip başka bir grubun halihazırda bulunduğu" uyarısını verecektir. Dolayısı ile grup ekleme işlemi başarısız olacaktır.

- f : işlemi hatalar olsa bile zorlayarak tamamlar. Genelde bu kullanım sorunlar çıkardığı için pek tavsiye edilmez.

Parametreler bunlar ile sınırlı değil ancak sizler `man groupadd` ve `groupadd --help `komutları yardımı ile diğer parametreleri de keşfedebilirsiniz.

Ayrıca oluşturduğunuz grupları silmek isterseniz konsola `groupdel grup_adı` şeklinde komut yazmanız yeterli olacaktır.

## Gruplara Kullanıcı Ekleme-Çıkarma İşlemi

Var olan bir gruba yeni bir kullanıcı eklemek için `gpasswd` komutunun `a` parametresini kullanarak, komutumuzu `gpasswd -a kullanıcı_adı ekleneceği_grup_adı` şeklinde kullanmamız yeterlidir.

gruptan çıkarmak istersek de `gpasswd` komutunun `d` parametresini kullanarak, komutu `gpasswd -d kullanıcı_adı çıkarılacağı_grup_adı` şeklinde kullanmamız gerekmektedir.

`chown` komutu ile dosyanın sahibini değiştirebiliyoruz

`Chown kali xxx`

`su` komutu ile user değiştirebiliyoruz onu gösterelim

`su sulin`

`exit` ile çıkabiliyoruz ve roota geri döndük.
