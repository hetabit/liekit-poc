# LieKit - Proof of Concept

<img align="right" src="assets/LieKit-Li.webp" alt="LieKit Logo" width="256">

<p align="justify">
LieKit (Layout Kit), aktif olarak kullanılan sayfa düzeni yapılarını (flex, grid, vs.) HTML üzerinde birer etiket olarak tanımlayan ve sayfa düzeni sorumluluğunu HTML'e devreden basit bir düzen kitidir.
</p>

### Neden İhtiyaç Duyduk?
<p align="justify">
Çünkü, CSS ile sayfa düzeni oluştururken şişmiş stil dosyalarının veya hazır sınıflar sunan modüllerin şişirdiği `class` özniteliklerinin projenin okunabilirliği ve sürdürülebilirliğini kısıtladığını düşünüyoruz.

Ek olarak, HTML, sayfanın iskeletini oluşturmaktan fazlasını yapabilecek bir dil iken, CSS'in tek görevinin, etiketlerin görsel stillerini yönetmek olduğu düşüncesindeyiz. CSS kendi görevinden fazlasını yapıyor iken, HTML şuan yaptığı işten fazlasını yapabilecek kadar güçlü bir dil.

Bu nedenlerden ötürü çözümü, kullandığımız düzen yapılarını HTML'e birer etiket olarak tanımlayıp, sorumluluğu kısmen CSS'den alıp, HTML'e devrederek projelerimizin ömrünü uzatabileceğimizi düşündük.
</p>

<img src="https://img.shields.io/badge/Status-PoC-orange?style=flat" alt="Status"> <img src="https://img.shields.io/badge/License-MIT-green?style=flat" alt="License">

## Kullanım
LieKit, fikrimiz ve amacımız gibi kolayca anlaşılabilir ve hiç yabancılık çektirmeyecek bir kullanıma sahiptir. O kadar yabancılık çekmeyeceksiniz ki, yıllardır ailenizin bir parçası olduğunu düşüneceksiniz. 

> Teknik olarak halen CSS üzerinde çalışıyorlar. Sadece amacımız, tek satır bile CSS satırı yazmadan, direkt olarak HTML üzerinden tanımlanmasını sağlamak. Kararlı sürümde de CSS üzerinden çalışmaya devam edecekler. Niyetimiz doğa kanunlarına müdahale etmek değil.

### Flex Box

**Örnek Flex Box Kullanımı**
```html
<flex-box>
    <!-- Child Etiketler Buraya -->
</flex-box>
```

Evet, çok kolay ve anlaşılır bir kullanım öyle değil mi? Bu etiketin ne işe yaradığını anlamak için de CSS karıştırmamıza gerek yok. Adı zaten yaptığı işi, esneyen bir kutu modeli olduğunu açıkca anlatıyor bize.

Bu kullanım, modern web geliştirmedeki çok büyük bir sorunu da ortadan kaldırıyor. Normalde web sayfaları tasarlarken CSS dosyalarını mümkün olduğunca sade tutmaya çalışırız. Ancak tek bir `.flex` sınıfı tanımlayıp her yerde kullanmak pratik olarak mümkün değildir; çünkü flex kutuları sayfanın pek çok yerinde bulunur ve her biri farklı davranışlara ihtiyaç duyar.

Buna çözüm olarak "modifier classes" (değiştirici sınıflar) kullanırız: Temel bir flex sınıfının yanına `column`, `row`, `wrap`, `center`, `start` gibi onlarca ek sınıf yığarız. Ancak bu bizi yine başladığımız noktaya, yani şişen CSS dosyalarına ve okunmaz hale gelen HTML etiketlerine geri götürür.

Biz de şunu düşündük: Madem düzen yapıları projelerde bu kadar farklı ve bağımsız davranmak zorunda, o zaman iki tarafı da aynı anda şişirmek yerine, CSS'e olan bu katı bağımlılığı zayıflatıp daha `HTML-Native` bir yapı kurmak projelerin sürdürülebilirliğini ve yönetilebilirliğini katbekat artıracaktır.

#### Öznitelikler
Öncelikle şunu söylemeliyiz ki, bazı isimlendirmelerin, kullanımlarıyla alakalı ve anlaşılabilir olmadığını düşünerek düzenlendik/değiştirdik. 

Örneğin, `align-items` ve `align-content` özelliklerini ele alalım. Birisi "öğeleri hizala" (`align-items`) anlamına gelirken, bir diğeri "içeriği hizala" (`align-content`) anlamına gelmekte. Eğer bir noktayı kaçırmıyorsak, belki sizin gördüğünüz, bizim göremediğimiz bir detay vardır, bu isimler birbirine çok yakın ve yaptıkları işi tam olarak anlatmıyor gibiler.

Buna farklı örnekler de verilerbilir. `justify-content`, `align-items`, `align-content`, `flex-start`, `flex-end`, `start`, `end` vs.

`flex-start`, `flex-end`, `start` ve `end` değerleri aslında birbirinden farklı işler gerçekleştiriyor. `flex-start` ve `flex-end`, içerikleri child etiketlerin sıralanma yönünün başına veya sonuna yaslarken, `start` ve `end`, sistem dilinin yazım yönüne göre hizalar. `start`, yazılmaya başlandığı tarafa yaslarken, `end`, yazımın bittiği tarafa yaslar.

- `direction`: 

    CSS'deki `flex-direction` özelliğinin yansımasıdır.

    **Değerler:**
    - `row`
    - `row-rev`: (row-reverse)
    - `column`
    - `column-rev` (column-reverse)

- `main-align`

     Child etiketlerin ana eksende (main axis), nasıl hizalanacağını belirtmek için kullanılır. CSS'deki `justify-content` özelliğinin yansımasıdır.

    **Değerler:**
    - `center`
    - `flow-start` (flex-start)
    - `flow-end` (flex-end)
    - `start`
    - `end`
    - `left`
    - `right`
    - `space-around`
    - `space-between`
    - `space-evenly`
    - `stretch`
        - Bu değer, child elementlerin, parent elementi doldurması için onları çekiştirir. Bunu `flex-grow` özelliklerini 1 olarak ayarlayarak yapar.

- `cross-align`

    Child etiketlerin, çapraz eksende (cross axis), nasıl hizalanacağını belirtmek için kullanılır. CSS'deki `align-items` özelliğinin yansımasıdır.

    **Değerler:**
    - `stretch` 
    - `center` 
    - `baseline` 
    - `start` 
    - `end` 

- `wrap` 

    Child etiketlerin, parent etiketten taştığı durumda uygulanacak yöntemin belirtilmesini sağlar. CSS'deki `flex-wrap` özelliğinin yansımasıdır.

    **Değerler:**
    - `no-wrap`
    - `wrap`
    - `wrap-rev`

- `wrap-align`

    Eğer, child etiketler, taşma sonucu büküldüyse/taşındıysa (bu durumda `wrap == wrap || wrap-rev` olmak zorundadır. Aksi taktirde, `wrap-align` özniteliğinin hiçbir etkisi olmayacaktır.), bükülen/taşınan kısmın nasıl hizalanacağını belirtilmesini sağlar. CSS'deki `align-content` özelliğinin yansımasıdır.

    **Değerler:**
    - `normal`
    - `stretch`
    - `start`
    - `end`
    - `center`
    - `space-around`
    - `space-between`
    - `space-evenly`

```html
<flex-box direction="column" main-align="space-between" cross-align="center">
    <!-- Child Etiketler Buraya -->
</flex-box>
```

Görüldüğü üzere her şey neredeyse birebir aynı. Sadece karmaşa daha az ve yapılmak istenen yapı daha derli toplu duruyor.

Bootstrap gibi hazır CSS modülleri işimizi kolaylaştırsa da zincirleme yazılan sınıf adları sizce ne kadar okunur duruyor?

```html
<div class="d-flex flex-column justify-content-between align-items-center">
    <!-- Child Etiketler Buraya -->
</div>
```

Ki burada breakpoint kullanmadık. Aksi taktirde, zincirleme gelen, daha uzun sınıf adları görecektiniz. 

Ayrıca LieKit yapısında, sayfa düzeni etiket özniteliklerinden düzenlendiği için okunurluğu arttırmak adına farklı yöntemler denenebilir. Örneğin nitelikleri alt alta tanımlamak gibi. Ancak bunu bootstrap sınıflarını kullanırken yapmak bazı durumlarda akıllıca olmayabilir. Satırlara ayrılmış bir karakter dizisi, potansiyel hatalara kapı açacaktır.

```html
<flex-box
    direction="column"
    main-align="space-between"
    cross-align="center">
    <!-- Child Etiketler Buraya -->
</flex-box>
```
Neticede kimse yapılan işi yatayda okumak istemez. Her işi satırlara dağıtmak da okunurluğu arttıracaktır.

```html
<div class="
    d-flex
    flex-column
    justify-content-between
    align-items-center">
    <!-- Child Etiketler Buraya -->
</div>
```

Fena değil :). Ancak başka öznitelik ataması yapılması gerekirse sizce nereden devam edilmeli? Ben bilmiyorum. 

### Responsive Tasarım
Bootstrap, Tailwind gibi CSS modüllerinin bize kazandırdığı bir diğer kolaylık ise hızlı responsive tasarım prototiplemedir. Bu deponun bir PoC (Kavram Kanıtı) olmasına rağmen, bizde kendimizce bir responsive yapı kurgulamak istedik. Bir isviçre çakısı olmasa da, LieKit, responsive tasarımda, fikir babası Bootstrap'den daha iyi iş çıkarmış olabilir. 

Kullanımı oldukça kolay. Aynı Bootstrap'deki gibi breakpoint sabitleri vardır. `(sm, md, lg, xl, xxl)` Her şeyden önce, breakpointlerin temsil ettikleri en düşük çözünürlük değerlerini belirtmekte fayda var. Aslında direk Bootstrap'in bize alışkanlık haline getirdiği değerleri kullandık.

| Breakpoint              | Minimum Çözünürlük  |
|:----------------------- |:------------------- |
| Small (sm)              | >= 576px            |
| Medium (md)             | >= 768px            |
| Large (lg)              | >= 992px            |
| Extra Large (xl)        | >= 1200px           |
| Extra Extra Large (xxl) | >= 1400px           |

LieKit ile tanımlanmış bir etiketin özniteliğinde, farklı breakpoint'lere, farklı değerler atayabilirsiniz.
Sadece atama yaparken, breakpoint kısaltmasının ardından `:` (iki nokta üst üste) bırakıp, yukarıdaki özniteliklerin değerlerini kullanarak atama yapabilirsiniz.

> Breakpoint belirtilmeden yapılan atamalar varsayılan olarak kabul edilir ve 0'dan, kullanılan en küçük breakpoint'in çözünürlüğüne olan aralığı kapsar.

```html
<flex-box direction="column md:row">
    <!-- Child Etiketler Buraya -->
</flex-box>
```

Gerçekten çok basit ve okuması çok kolay. Çünkü bir de :

```html
<div class="d-flex flex-column flex-md-row">
    <!-- Child Etiketler Buraya -->
</div>
```

Çok bir fark yok gibi gelebilir. Ancak, sayfanızda flex kutunuzun, ekran çözünürlüğüne bağlı olarak, ana eksende farklı hizalamalar yapmasını istediğiniz bir ihtimali ele alalım.

**Bootstrap :**
```html
<div class="d-flex flex-column justify-content-start justify-content-md-center justify-content-xxl-end">
    <!-- Child Etiketler Buraya -->
</div>
```

**Bizim Çocuk :**
```html
<flex-box direction="column" main-align="start md:center xxl:end">
    <!-- Child Etiketler Buraya -->
</flex-box>
```

***Az ve öz.***

LieKit örneğinde, yapılan iş ile ilgili daha fazla kavram vardır. Etiket adı (`flex-box`), bu etiketin esnek kutu davranışı sergilediğini açıkca belirtirken, `direction` niteliği direkt olarak `flex-box` etiketinin sıralanma yönünün ayarlandığını açıkca dökümanı okuyan kişiye anlatır. Ve diğer etiketler de yaptıkları işi okuyucuya kolaylıkla anlaşılacak şekilde sessizce beyan eder.

Buna kıyasla Bootstrap örneğinde, `div` ve `class` isimleri genel bir kullanıma sahip olması yanı sıra, zincirleme yazılan ve aşırı uzun sınıf isimleriyle okunurluk bayağı bir düşmüştür.

## Kapanış

Bootstrap, Tailwind vb. gibi CSS modülleri ve derlenebilen CSS dilleri gerçekten çok kullanışlı. Zaten bizim edindiğimiz dert CSS dili ile değil, projelerde CSS betiklerinin kontrolsüzce şişmesidir. Bu onun suçu değil, onun ilgilenmesi gereken çok fazla detay var. Onu daha fazla yormamak adına, HTML'in de elini taşın altına koyması gerektiğini düşünüyoruz.

LieKit PoC deposu ile ilgili olarak, bu depo tamamen bu fikrin haklı olup olmadığını, kolaylık sağlayıp sağlamadığını ve semantik açından uygun olup olmadığını görmek adına, en basit CSS sözdizimleri ile geliştirildi. (bkz. [PoC](https://en.wikipedia.org/wiki/Proof_of_concept))
Bu nedenle, bu depoda yapılan geliştirmeler zaman zaman, hatta sanırım genellikle ana hedefimizi yansıtmayabilir. Çünkü en basitinden, hedeflediğimiz, ana sürümdeki LieKit sözdizimi yapısı ve mimarisi bu şekil değil. Süprizi bozmak istemeyiz ancak daha kullanışlı, daha yönetilebilir ve tamamen geliştiricinin insiyatifine kalmış bir Responsive altyapısı geliştirme fikrimiz bile var.

Küçük bir hatırlatma... LieKit'in amacı hiçbir zaman bu düzen yapılarını (flex, grid, vs.) CSS'den koparmak değildir. Biliyoruz ki, düzen yapılarını CSS dilinden koparıp sadece HTML'e devretmek demek, düzen yapılarını sahip olduğu bir sürü özelliğinden mahrum etmek anlamına geliyor. Bizim amacımız sadece, bu yapıların kullanımını daha native bir hale evrilmesini sağlamaktır.

LieKit, çok büyük bir proje değil, ancak bizce çok kullanışlı bir fikir. O yüzden bu README'yi okurken bunun bir manifesto olduğunu düşünmenizi istiyoruz. Bu aslında HTML ile CSS arasındaki sorumluluk dağılımının nasıl olması gerektiği ve bu iki dilin nasıl daha anlaşılabilir olabileceğini açıkladığımız bir fikir beyanı olarak düşünebilirsiniz.

<br>
<br>

### Do you LieKit? ♥
