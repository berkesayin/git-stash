# Git Stash 

[Git Stash - Berke Sayın - Medium](https://sayinberkesayin.medium.com/git-stash-de%C4%9Fi%C5%9Fiklikleri-kenarda-tutmak-14e447c8af80)

[Git Stash - Atlassian Docs](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

[Git Stash - Javatpoint Docs](https://www.javatpoint.com/git-stash)



### Contents
* [Git Stash Nedir? Neden İhtiyaç Duyarız?](#stash-intro)
* [Git Stash ile Değişiklikleri Rafa Kaldırmak](#git-stash)
* [Hangi Durumdaki Dosyaları 'git stash' İle Saklayamayız?](#git-stash-not)
* [Birden Çok Stash Oluşturma](#multiple-stashes)
* ['git stash list' ile Sakladığımız Stash'leri Görebilme](#git-stash-list)
* ['git stash save' ile Rafa Kaldırılan Değişikliklere Anlamlı İsim Verme](#git-stash-save)
* ['git stash drop' ile Stash'i Silme](#git-stash-drop)
* ['git stash apply' ile Stash'i Geri Getirme](#git-stash-apply)
* ['git stash pop' ile Stash'i Geri Getirme](#git-stash-pop)

> **Git Stash:** Hay aksi... Nereden çıktı bu unplanned task şimdi...

### Git Stash Nedir? Neden İhtiyaç Duyarız? <a name="stash-intro"></a>
- Planlama toplantımız sonrasında aldığımız task üzerinde çalışmaya başladığımızı varsayalım. Bir takım işlemler yaptık ve geliştirmeye devam ediyoruz. O sırada uygulamada farklı bir sayfa için acil desteğe ihtiyaç bulunuyor ve o tarafta görev alacağız. Fakat yaptığımız değişiklikler henüz tamamlanmadığı için commit edilecek durumda değil. Yapılacak olan commit’in belli bir göreve ait olması, proje history’si için de oldukça önemli. Yani o şekilde commit etmemiz doğru olmayacak. Peki bu durumda ne yapmamız gerekli? Son commit’ten itibaren yaptığımız bütün değişiklikleri silip daha sonra tekrar mı başlayacağız ?🤔

- Herkese merhaba, yazılım geliştirme süreci ve proje takibi için versiyon kontrolü oldukça önemli. Bu yazımda ise uygulama geliştirirken hangi durumlarda üzerinde çalıştığımız değişiklikleri bir süreliğine kenara almak isteriz ve bunu nasıl yaparız konularına değineceğim. Keyifli okumalar :)


### 'git stash' ile Değişiklikleri Rafa Kaldırmak <a name="git-stash"></a>

- Günlük çalışma düzenimizde uygulama geliştirirken unplanned şekilde yeni görevler gelebiliyor. Örneğin bir web uygulaması için ürün detay sayfası geliştiriyoruz ve tam o sırada uygulama için farklı bir alanda herhangi bir durumdan dolayı acil destek gerekiyor. Bu sebeple üzerinde çalıştığımız görevi bir süreliğine bekletip, bizden istenen yeni görevle ilgileneceğiz. Fakat task üzerinde geliştirme yapmaya başladıktan sonra yazdığımız kodlar henüz commit edilecek duruma gelmedi. İşte tam burada git stash komutu devreye giriyor.

- ``Stash`` aslında git içerisinde oldukça kullanışlı bir özellik. O an üzerinde çalıştığımız değişiklikleri geçici olarak saklamak, kenarda tutmak için kullanılır.

- Konuyu daha somut bir şekilde anlatabilmek için az önce bahsettiğim örnek üzerinden ilerlemek istiyorum. Bir web uygulaması için ürün detay sayfası geliştirirdiğimiz esnada bize yeni bir görev geliyor. O yeni görevde de kullanıcı profili sayfasına ekleme yapmamız isteniyor. Burada mevcuttaki işimizi bekletip yeni task ile ilgilenme durumu söz konusu.

- Bu durumda kullanıcı profili sayfası ile ilgilenmeden önce üzerinde çalışmakta olduğumuz ürün detay sayfası için son commit’ten itibaren yaptığımız değişiklikleri geçici olarak saklamak, daha sonra çalışmaya devam etmek istiyoruz. Burada dikkat etmemiz gereken nokta şu: O değişiklikleri staging area’ya almıyoruz veya commit‘lemiyoruz. Daha sonra kaldığımız yerden devam etmek üzere rafa kaldırıyoruz. Böylece kullanıcı profili sayfası ile ilgilenip işimiz bittikten sonra kenarda tuttuğumuz değişiklikleri geri alarak kaldığımız yerden ürün detay sayfasını geliştirmeye devam edebiliriz. İşte bu noktada git stash komutunu kullanırız.

```bash
git stash 
```
- ``git stash`` komutu daha önce git tarafından index’e eklenmiş yani staged, tracked durumundaki dosyalarda yapılan değişiklikleri rafa kaldırır.

- Örneğin bir dosyayı daha önce ``staging area’ya`` ekledik ve commit’ledik. Artık o dosya git tarafından index’e eklendi ve ``tracked`` durumunda. Tekrar o dosyada değişiklik yaptığımızda (yani dosya ``modified`` durumunda iken) ister staging area’ya ekleyelim, ister eklemeyelim, o dosyadaki değişiklikleri ``git stash`` ile saklayabiliriz.

- Bir diğer durumda yeni bir dosya oluşturduk, o dosyada eklemeler, geliştirmeler yaptık ve şu anda working directory’de untracked durumunda bulunuyor. Daha önce staging area’ya almadık, dolayısıyla commit’lemedik. O dosyayı ``git add file_name`` ile staging area’ya taşırız. Daha sonra source control alanına geldiğimizde dosya üzerinde ``“Index Added”`` yazdığını görürürüz. Yani staging area’ya alınan dosyalar git tarafından index’e eklenir. Bu dosyalardaki değişiklikler de ``git stash`` ile rafa kaldırılabilir.

### Hangi Durumdaki Dosyaları 'git stash' İle Saklayamayız? <a name="git-stash-not"></a>

- Örneğin yeni bir dosya oluşturduk ve içerisinde geliştirmeler yaptık, bir şeyler ekledik. Dosya şu anda working directory’de bulunuyor ve daha önce staging area’ya alınmadığı, commitlenmediği için git tarafından index’e eklenmedi ve ``untracked`` durumunda. Bu durumda bu dosyada yapılan değişiklikler ``git stash``ile saklanamaz. git stash komutunu uyguladığımızda ‘No local changes to save’ uyarısını alırız.

- Bu şekilde yeni oluşturulmuş, untracked durumunda olan dosyalardaki değişiklikleri de rafa kaldırmak için ``git stash -u`` veya ``git stash --include-untracked`` komutunu kullanırız.

- Bu komutu çalıştırdıktan sonra terminalde şu info’yu görürüz: ``“Saved working directory and index state WIP on master”. ``Bu da o dosyadaki değişikliklerin stash ile saklandığı anlamına gelir.

- Stash ile değişiklikleri kenara aldıktan sonra working tree’nin durumundan emin olmak için git status komutunu kullanabiliriz. Bu komutu çalıştırınca ``“On branch master, nothing to commit, working tree clean”`` bilgisini görürüz.

- ``git stash`` : ``tracked`` durumundaki dosyalarda son committen itibaren yapılan değişiklikler saklanır.

- ``git stash -u`` veya ``git stash --include-untracked`` : ``tracked`` veya ``untracked`` durumundaki dosyalarda son committen itibaren yapılan değişiklikler saklanır.

> **Important:** Stash’ler local repository’de saklanır. git push ile herhangi bir şekilde remote repository’ye aktarılmaz.

### Birden Çok Stash Oluşturma <a name="multiple-stashes"></a>

- ``git stash`` komutunu birden çok kez kullanabiliriz. Birbirinden bağımsız stash’leri ayrı ayrı rafa kaldırabiliriz. Sakladığımız stash'leri ``git stash list`` ile görürüz.

### 'git stash list' ile Sakladığımız Stash'leri Görebilme <a name="git-stash-list"></a>

- Uygulama geliştirirken peşpeşe 3 kez ``git stash`` kullanarak değişiklikleri 3 ayrı stash şeklinde sakladığımızı varsayalım. Ardından projede farklı alanda geliştirme yapmaya devam ettik ve işimiz bitince stash’leri geri getirmek istiyoruz. İlk olarak kenarda tuttuğumuz değişiklikleri görmek isteriz. Oluşturduğumuz stash’leri ``git stash list`` komutu ile görebiliriz:

```bash
# git stash list
stash@{0}: WIP on master: 4571844 Add menu.
stash@{1}: WIP on master: 4571844 Add menu.
stash@{2}: WIP on master: 4571844 Add menu.
```
##### Burada dikkat edilmesi gereken durum:

- Aslında bizim ilk oluşturduğumuz stash 2.index değeri ile tutulan ``stash (stash@{2})``. Yani en son oluşturduğumuz stash her zaman 0.index’te tutulur ve önceki stash’ler bir sonraki index değerine kaydırılır.

- Örneğin bu şekilde 3 farklı stash’imiz var, ardından projedeki farklı bir kısımla ilgilendik ve o kısımdaki geliştirmeleri commit’ledik. Daha sonra bir yerde yine stash kullanmaya ihtiyaç duyduk. ``git stash list`` ile stash’lerimizin son haline bakalım:

```bash
# git stash list
stash@{0}: WIP on master: e917757 Add register function
stash@{1}: WIP on master: 4571844 Add menu.
stash@{2}: WIP on master: 4571844 Add menu.
stash@{3}: WIP on master: 4571844 Add menu.
```

- Burada yukarıda bahsedilen durum söz konusu. En son oluşturduğumuz stash 0.index değerine sahip ve diğer stash’lerin index değeri 1 ileri kaydırıldı.

### 'git stash save' ile Rafa Kaldırılan Değişikliklere Anlamlı İsim Verme <a name="git-stash-save"></a>

- Yukarıdaki örnekte 4 farklı stash mevcut, fakat bize stash’lerin içeriği ile ilgili bilgi vermiyor. Bu durum listede tutulan stash’i tekrar alıp kullanmak istediğimizde kafa karışıklığına sebep olabilir.

- Stash’leri kısa açıklama ekleyerek ``(stash message)`` oluşturursak, kullanmak istediğimiz durumda rahat bir şekilde hatırlayabiliriz.

- Örneğin uygulamada contact.html sayfasında iletişim formu geliştirirken işimiz yarıda kaldı ve değişikliği stash’lememiz gerekti. ``git stash save "message"`` ile stash’leyelim.

```bash
# git stash save "message"
git stash save “Add contact form to our site.”
```

- Bu işlemden sonra terminalde şu info’yu görürüz: ``‘Saved working directory and index state On master: Add contact form to our site.’``

- ``git stash list`` ile stash’leri görelim:  

```bash
# git stash list
stash@{0}: On master: Add contact form to our site.
stash@{1}: WIP on master: e917757 Add register function
stash@{2}: WIP on master: 4571844 Add menu.
stash@{3}: WIP on master: 4571844 Add menu.
stash@{4}: WIP on master: 4571844 Add menu.
```

- Burada gözüktüğü gibi stash message’ı ile oluşturduğumuz stash’i ne için oluşturduğumuzu daha rahat bir şekilde hatırlayabiliriz.

### 'git stash drop' ile Stash'i Silme <a name="git-stash-drop"></a>

- Stash’i oluşturduk, değişiklikleri sakladık. Ardından uygulamada farklı bir yerde geliştirme yapıp geri döndük ve artık o stash’i geri getirmek, kullanmak istemiyoruz. Listeden silmek istiyoruz. Bu durum için ``git stash drop`` komutunu kullanırız.

```bash
git stash drop
# Dropped refs/stash@{0} (11b9fcdfacbbd3b94f758687a2f2678c4175d58c)
```

- Bu komut ile ``en son oluşturulan stash (stash@{0})`` listeden silinir. Daha sonra listedeki diğer stash’lerin index değeri bir azaltılır. Her zaman listede en son oluşturulan stash 0 index değeri ile tutulur.

- ``git stash drop stash@{n}`` ile listede istediğimiz index değerine sahip olan stash’i silebiliriz.

### 'git stash apply' ile Stash'i Geri Getirme <a name="git-stash-apply"></a>

- Stash ile sakladığımız değişiklikleri tekrar kullanmak istediğimizde
``git stash apply`` komutunu kullanabiliriz. ``git stash apply`` en son oluşturulan stash’i yani listede 0.index değeri ile tutulan stash’i ``(stash@{0})`` getirir.

```bash
git stash apply

our branch is up to date with 'origin/master'.

Changes not staged for commit:
 (use "git add <file>…" to update what will be committed)
 (use "git restore <file>…" to discard changes in working directory)
 modified: contact.html

no changes added to commit (use "git add" and/or "git commit -a")
```

- Burada ``git stash apply`` ile en son stash geri getirildi ve ``contact.html`` dosyası ``modified`` durumunda oldu. Bu sebeple git; contact.html dosyasının modified olduğunu ve commit’lemeden önce ``git add <file_name>`` ile ``staging area’ya`` eklememiz gerektiğini söylüyor.

- ``git stash apply stash@{n}`` ile listede istediğimiz index değerine sahip olan stash’i geri getirebiliriz.

### 'git stash pop' ile Stash'i Geri Getirme <a name="git-stash-pop"></a>

- Stash listesinde bulunan değişiklikleri ayrıca ``git stash pop`` ile geri getirebiliriz. Bu komut da en son oluşturulan stash’i geri getirir. Fakat bunu yaparken git stash applykomutundan farklı olarak geri getirdiği stash’i listeden siler. ``apply`` ile geri getirilen stash listede durmaya devam eder.

- ``git stash pop stash@{n}`` ile listede istediğimiz index değerine sahip olan stash’i geri getirip listeden silinmesini sağlayabiliriz.

```bash
git stash pop stash@{1}

On branch master
Your branch is up to date with ‘origin/master’.

Changes not staged for commit:
 (use "git add <file>…" to update what will be committed)
 (use "git restore <file>…" to discard changes in working directory)
 modified: app.js

no changes added to commit (use "git add" and/or "git commit -a")
Dropped stash@{1} (1cd955c8012a2cbd1b0435f24c6b1663c5f3b862)
```

- Burada ``apply`` işleminden farklı olarak ``stash@{1}’in`` listeden silindiğini ``(dropped)`` söylüyor.

<br>

- Bu yazımda git stash ile değişikliklerin ihtiyaç duyulduğudna nasıl saklandığını ve geri getirildiğini açıklamaya çalıştım, gelecek yazılarda görüşmek dileğiyle :)