markdown

# Sistem Fonksiyonları (Devam - 6. Bölüm)

### Keltner Kanalı - Sistem.KeltnerUp / KeltnerDown(10)

Alt ve Üst olarak adlandırılan 2 çizgiden oluşan Keltner isimli indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 10 kullanılır. Kanalın/bandın üst çizgisi için ayrı (UP), alt çizgisi için ayrı (DOWN) birer fonksiyon vardır. Aşağıdaki gibi kullanım şekli vardır;

```csharp
Sistem.KeltnerDown(10);
Sistem.KeltnerUp(10);
Sistem.KeltnerDown(Veriler, 10);
Sistem.KeltnerUp(Veriler ,10);

Örnek: İndikatörün Alt ve Üst bandını çağırıp çizdirmekcsharp

var Alt = Sistem.KeltnerDown(10);
var Ust = Sistem.KeltnerUp(10);
Sistem.Cizgiler[0].Deger = Alt;
Sistem.Cizgiler[1].Deger = Ust;

Kesişme Tara - Sistem.KesismeTara(x,y)En popüler teknik analiz stratejilerinden biri de iki çizginin (iki indikatörün birbiriyle veya bir indikatörün bir sabit değerle) kesişmesi yoluyla AL/SAT sinyalleri tespit etmektir.Kesişme tespitleri yapmak ve buna göre AL/SAT sinyalleri üretmek için iDeal Sistem.KesismeTara fonksiyonunu sunmaktadır.Bu fonksiyon, iki çizginin, birbirini her iki yönde de kesmesine göre strateji üretir.NOT: Bir çizginin, bir diğer çizgiyi veya değeri, sadece tek yönde (Yukarı veya Aşağı yönde) kesmesine dayalı bir strateji için ayrıca iki fonksiyon (Bkz: Sistem.YukariKestiyse ve Sistem.AsagiKestiyse) mevcuttur.Sistem.KesismeTara fonksiyonu, mutlaka belirtilmesi gereken 2 adet parametreye ihtiyaç duyar ve iki farklı şekilde kullanılabilir.csharp

Sistem.KesismeTara(Cizgi1, Cizgi2); // (veya Liste1/Liste2)
Sistem.KesismeTara(Cizgi, SabitDeger);

Örneğin: İki farklı hareketli ortalamanın birbiriyle kesişimi veya RSI indikatörünün kendi ortalamasıyla kesişimi gibi bir strateji kullanılacaksa birinci şekilde yazılır.csharp

Sistem.KesismeTara(MA1, MA2);

Örneğin RSI indikatörünün sabit bir değeri (50 yi geçerse) kesmesi stratejisi kullanılacaksa ikincisi kullanılır.csharp

Sistem.KesismeTara(RSI,50);

Kesişme Tara, Yukarı Kestiyse ve Aşağı Kestiyse fonksiyonları, kapanmış olan son iki bardaki değerleri kıyaslayarak sonuç döndürürler. Henüz kapanmamış olan bar hesaplamaya/kontrole dahil edilmez.Örneğin aşağıdaki grafikte, 1 ile numaralandırılan barda 10’luk ortalama, 35’lik ortalamanın üzerinde.
2 ile numaralandırılan bar kapanışında ise, altına iniyor (aşağı kesiyor.) sonraki ilk barda sinyal üretiliyor.ÖRNEK: MACD indikatörünün kendi hareketli ortalamasıyla kesişimlerine dayalı bir strateji kodu:csharp

var MACD = Sistem.MACD(12, 26);
var AVR = Sistem.MA(MACD, "Exp", 9);
Sistem.Cizgiler[0].Deger = MACD;
Sistem.Cizgiler[1].Deger = AVR;
// strateji
Sistem.KesismeTara(MACD, AVR);

Klinger Oscilator - Sistem.KlingerOsc(13)Klinger Oscilator isimli indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 13 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.KlingerOsc(13);
Sistem.KlingerOsc(Veriler,13);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var X = Sistem.KlingerOsc(13);
Sistem.Cizgiler[0].Deger = X;

Kurum Hacim Oku - Sistem.KurumHacimOku(“IYF”)Aracı kurumların, geçmiş günlere ait hacimlerini ve tüm kurumların hacim toplamları içinde o kurumun hacminin yüzdesel oranını okumak için kullanılan iDeal Sistem fonksiyonudur. Fonksiyon çağrıldığı zaman içerisinde “Hacim” ve “Yuzde” olarak iki bilgi alanı içeren bir nesne (obje) döner. Objeyi tanımladığınız değişken isminin yanına “nokta” işareti koyup Hacim yazarak, seçilen kurumun gün gün işlem hacmi verisine, noktadan sonra “Yuzde” yazarak her bir gün için o kurumun hacminin toplam (tüm kurumların hacim toplamları) hacime yüzdesel oranı verisine erişilir.Örnek kullanım şekli:csharp

var AKYATIRIM = Sistem.KurumHacimOku("AKM"); //AK Yatırım
var INFOYATIRIM = Sistem.KurumHacimOku("IYF"); //info yatırım
Sistem.Cizgiler[0].Deger = AKYATIRIM.Hacim; //Ak Yatırım hacim
Sistem.Cizgiler[1].Deger = AKYATIRIM.Yuzde; //Ak Yatırım yüzde
Sistem.Cizgiler[2].Deger = INFOYATIRIM.Hacim; //info hacim
Sistem.Cizgiler[3].Deger = INFOYATIRIM.Yuzde; //info yuzde

Kurum Hacim Oku fonksiyonu ile ilgili bilinmesi gereken bazı önemli detaylar aşağıdaki gibidir;Fonksiyon yazılırken parantez içinde (ve çift tırnak işaretleri arasında) hacim ve yüzde verisine ulaşılmak istenen Aracı Kurumun borsada ve Takasbank’ta kayıtlı olan 3 harfli kısa kodu yazılır.
Hangi aracı kurumun kısa kodunun ne olduğunu öğrenmek için C:\iDeal\Config klasörü altında bulunan Kurumlar.001 isimli dosyanın içine bakılabilir. (Bu dosya not defteri ile açılabilir)
Hacim Oku fonksiyonu c:\ideal\HacimImkb isimli klasörde tutulan dosyalardan veri okur. O klasörde hangi tarihe kadar dosya varsa, o kadar veri okutulur. Hacim verisi yok veya eksik ise, iDeal’in üst kısmındaki BIST menüsü altında DİĞER ANALİZLER satırına gelinip HACİMLER tablosu açılır. İstenen iki tarih arası seçilir ve GÜNCELLE butonuna basılarak girilmiş tarih aralığına ait hacim dosyaları iDeal sunucularından indirilir.

Linear Regresyon - Sistem.LinearReg(14)Linear Regresyon ismiyle bilinen indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.LinearReg(14);
Sistem.LinearReg(Liste,14);
Sistem.LinearReg(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var LG = Sistem.LinearReg(14);
Sistem.Cizgiler[0].Deger = LG;

Linear Regresyon Inter Cept - Sistem.LinearRegInterCept(14)Linear Regresyon Inter Cept ismiyle bilinen indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.LinearRegInterCept(14);
Sistem.LinearRegInterCept(Liste,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var LG = Sistem.LinearRegInterCept(14);
Sistem.Cizgiler[0].Deger = LG;

Linear Regresyon Slope - Sistem.LinearRegSlope(14)Linear Regresyon Slope ismiyle bilinen indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.LinearRegSlope(14);
Sistem.LinearRegSlope(Liste,14);
Sistem.LinearRegSlope(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var LG = Sistem.LinearRegSlope(14);
Sistem.Cizgiler[0].Deger = LG;

Lisans Kontrol - Sistem.LisansKontrol(“KullaniciAdi”)iDeal üzerinden yazılmış bir sistemi/robotu/indikatörü bir veya daha fazla IDEAL KULLANICI ADI için lisanslayabilir, belirtilenlerden başka kullanıcı adıyla iDeal kullananların, dosya olarak elde etseler bile bu formülü kullanmalarına engel olabilirsiniz. Tabi bu fonksiyonu kullanan bir kullanıcının, formülünü ŞİFRELİ hale getirmesi gerekiyor.ÇOK ÖNEMLİ NOT: Formül yazılan penceredeki ŞİFRELE butonuna bastığınız zaman, IDEAL rastgele bir algoritma ile kodu karıştırıp gizleyecektir. Kodun açık halini, kullanıcının kendisi de bir daha göremeyecektir. Bu nedenle, bir başkasıyla paylaşmak için şifrelenecek olan formülün önce bir başka isimle kopyasını oluşturup, bu kopyayı şifrelemeniz gerekmektedir.ÖRNEK: //AHMET ve Sezai12 kullanıcılarına kullanım izni verilen, onun dışında bir username ile iDeal’e bağlanan bir kişiye Bu Formülü Kullanma Yetkiniz Yoktur mesajı çıkaran ve çalışmayan kod örneğicsharp

if (Sistem.LisansKontrol("AHMET","Sezai12"))
{
    var Veriler = Sistem.GrafikFiyatSec("Kapanis");
    var MA1 = Sistem.MA(Veriler, "Exp", 10);
    var MA2 = Sistem.MA(Veriler, "Exp", 100);
    Sistem.KesismeTara(MA1, MA2);
}
else
    Sistem.Mesaj("Bu Sistemi Kullanmaya Yetkiniz Yoktur");

Liste Oluşturmak - Sistem.Liste(Deger)IDEAL programında, Grafik Barlarına ait değerler, kapanışlar, indikatör değerleri, tüm hesaplatılan veya tanımlanan Çizgiler birer LİSTEDİR. Grafik üzerine bir ÇİZGİ çizmek için bir liste gerekir. Bu liste hazır fonksiyonlarla okutulan veya kullanıcı tarafından hesaplatılan veya sabit olarak tanımlanan bir liste olabilir. (Örneğin yatay bir çizgi). Bu fonksiyonun amacı, kodlama sırasında kullanılmak üzere çeşitli listeler tanımlamaktır.Fonksiyonun 2 Kullanım şekli vardır;csharp

Sistem.Liste(x);          // Bütün elemanlarının değeri x olan, üzerine uygulandığı grafikteki bar sayısı kadar elemanı olan liste
Sistem.Liste(100,x);      // Bütün elemanlarının değeri x olan ve eleman sayısı 100 olan liste

iDeal sistem modülündeki veri veya fonksiyonların birçoğu LİSTE formatındadır.
Örneğin Sistem.GrafikFiyatSec(“Kapanis”) fonksiyonu, bir grafikteki barların KAPANIŞ değerlerini içeren bir liste sunar. Her bir satırında bir SAYI olan bu listenin her bir elemanı grafikteki bir barı (tarihi) temsil eder.
Listenin eleman sayısı = Bar Sayısıdır.Bu listeyi kullanarak işlemler/hesaplamalar yapmak için bir değişkene atamak işi kolaylaştırır.ÖRNEK:csharp

var Close = Sistem.GrafikFiyatSec(“Kapanis”);

Yeni ve/veya boş bir liste tanımlamak için Sistem.Liste(Deger) veya Sistem.Liste(BarSayisi, Deger) fonksiyonlarından biri kullanılır.csharp

Sistem.Liste(0);        // Bütün elemanları “0” olan (boş) bir liste
Sistem.Liste(30);       // Bütün elemanları “30” olan bir liste
Sistem.Liste(100,30);   // 100 adet elemanı olan ve her bir elemanı 30 olan bir liste

KURAL: İki liste arasında matematiksel işlem veya kıyas yapılırken, bu işlemler TEK TEK LİSTENİN ELEMANLARI arasında yapılabilir. Yani Listeye veri yazarken veya veri okurken LİSTENİN BİR ELEMANI (INDEXİ) belirtilmelidir. Indexleme işlemi için köşeli parantez kullanılır. Bir listenin x numaralı elemanı için ListeAdi[x] ifadesi kullanılır.ÖRNEK: Listem isimli bir listenin 1000. Elemanının değerine ihtiyaç varsa; Listem[1000] şeklinde o elemana ulaşılır.Bir listenin her elemanı okunacak veya yazılacaksa veya tek seferde birden fazla elemanına ihtiyaç varsa, liste üzerindeki okuma/yazma/hesaplama işlemleri BİR DÖNGÜ içinde yapılmalıdır.HATALI KULLANIM:
Listem = (Listem1 + Listem2) / 2 ;ÖRNEK: Bir sembolün her bir barının Yüksek ve Düşük değerlerinin toplamının yarısını bir başka listeye almak ve ekranda çizdirmek:csharp

var Listem1 = Sistem.GrafikFiyatSec(“Yuksek”);
var Listem2 = Sistem.GrafikFiyatSec(“Dusuk”);
var Listem3 = Sistem.Liste(0); //Boş bir liste oluşturulur.
for (int i=1; i < Sistem.BarSayisi; i++)
{
    Listem3[i] = (Listem1[i] + Listem2[i]) / 2 ;
}

ÖRNEK: GRAFİK ÜZERİNDE PİVOTLARI HESAPLAMA GÖSTERMEcsharp

var Veriler = Sistem.GrafikVerileri;
var sonbar = Veriler.Count-1;
var H = Veriler[sonbar-1].High;
var L = Veriler[sonbar-1].Low;
var C = Veriler[sonbar-1].Close;
var R = H - L;
var P = (H + L + C) / 3;
var R3 = Sistem.Liste(0);
var R2 = Sistem.Liste(0);
var R1 = Sistem.Liste(0);
var S1 = Sistem.Liste(0);
var S2 = Sistem.Liste(0);
var S3 = Sistem.Liste(0);
var pivot = Sistem.Liste(0);
for (int i=1; i<Sistem.BarSayisi; i++)
{
    R3[i] = P + (R * 1.0f);
    R2[i] = P + (R * 0.618f);
    R1[i] = P + (R * 0.382f);
    S1[i] = P - (R * 0.382f);
    S2[i] = P - (R * 0.618f);
    S3[i] = P - (R * 1.0f);
    pivot[i] = P;
}
Sistem.Cizgiler[0].Deger=R3;
Sistem.Cizgiler[1].Deger=R2;
Sistem.Cizgiler[2].Deger=R1;
Sistem.Cizgiler[3].Deger=S1;
Sistem.Cizgiler[4].Deger=S2;
Sistem.Cizgiler[5].Deger=S3;
Sistem.Cizgiler[6].Deger=pivot;

LLV - Sistem.LLV(14)LLV (Lowest Low Value) isimli indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. 1 adet parametre alır ve varsayılan değeri 14’tür. LLV (Lowest Low Value) girilen parametre kadar bar için görülen EN DÜŞÜK değeri bir indikatör olarak hesaplar çizer. Grafik barları üzerine çizdirilir ve böylece fiyat kesişimleri veya kanal analizi yapılabilir. Kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.LLV(14);
Sistem.LLV(14, Liste);
Sistem.LLV(14, “Dusuk”);
Sistem.LLV(GrafikVerileri, 14, “Dusuk”); //Başka bir sembole veya grafik periyoduna ait barlar için LLV hesaplar.

Örnek: İndikatörü çağırıp çizdirmekcsharp

var LL = Sistem.LLV(14);
Sistem.Cizgiler[0].Deger = LL;

Lot - Sistem.Lot(Sembol)Bir sembolün o an itibariyle ve çeşitli zaman dilimleri için kaç lot işlem gördüğü bilgisini okumak için kullanılır. Fonksiyonun içine Lotu okutulmak istenen sembol yazılır.Kullanım şekilleri aşağıdaki gibidir.csharp

Sistem.LotAltiAy(Sembol);
Sistem.LotBirAy(Sembol);
Sistem.LotBirHafta(Sembol);
Sistem.LotBirYil(Sembol);
Sistem.LotBuAy(Sembol);
Sistem.LotBuHafta(Sembol);
Sistem.LotBuYil(Sembol);
Sistem.LotGun(Sembol);
Sistem.LotUcAy(Sembol);

Örnek:csharp

var AltiAyLot = Sistem.LotAltiAy(Sembol);
Sistem.Mesaj("6 Aylık işlem hacmi lot bazında = " + AltiAyLot.ToString());

LY - Sistem.LY(Periyot)Bir grafik üzerinde, girili periyot içerisinde görülen EN DÜŞÜK değerden yüzdesel olarak ne kadar uzakta olduğumuz bilgisini dönen bir indikatör olan LY indikatörünü hesaplar. Bir adet parametre alır. Kullanım şekli Sistem.LY(20); şeklindedir.Örnek: Momentum kendi ortalamasının altındaysa, fiyatın 20 barlık en düşüğünden uzaklığı %2 den küçükse AL, tersi durumda SAT örnek sistemi.csharp

var LY = Sistem.LY(20); // 20 periyottaki en düşükle fiyat arasındaki yüzde fark
var Mom = Sistem.Momentum(5);
var AvrMom = Sistem.MA(Mom, "Simple", 10);
var SonYon = "";
for (int i=1; i < Sistem.BarSayisi; i++)
{
    LY[i] = Math.Abs(LY[i]);
    if (SonYon != "A" && LY[i] < 3 && Mom[i] > AvrMom[i])
    {
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    if (SonYon != "S" && LY[i] > 3 && Mom[i] < AvrMom[i])
    {
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

MA – Moving Average (Hareketli Ortalamalar) - Sistem.MA(….)Bir grafik için hareketli ortalamaları hesaplatmak/çizdirmek için iDeal Sistem kütüphanesinde Sistem.MA fonksiyonları bulunmaktadır. MA (Moving Average) indikatörü hesaplamak için 3 parametreyi girdi olarak vermek gerekir;• Periyot (kaç barlık ortalama alınsın?)
• Yöntem (Hareketli ortalama hesaplama yöntemi ne olsun? (basit, üssel, ağırlıklı vs.))
• Veri (hangi verilerin ortalaması alınsın? (kapanışların, yükseklerin, hacimin, bir başka indikatörün vs.)Aşağıda gösterildiği şekilde 2 kullanım şekli vardır;csharp

Sistem.MA(10, "Simple", "Kapanis");
Sistem.MA(Liste, "Exp", 10);

Birinci kullanım şekli, sadece yazılan kodun grafiğe uygulandığı durumlarda kullanılır.
İkinci kullanım şekli, MA’sı hesaplanmak istenen data eğer bar kapanışları değil de başka bir liste veya indikatör ise tercih edilir.Hareketli ortalama yöntemleri:Simple (Basit)
Exp (Üssel)
Weighted (Ağırlıklı)
Triangle (Üçgensel)
Variable (Değişken)
Zero Lag
Welles Wilder
HullMA

Örnek: DI+ indikatörünün ortalamasını alıp çizdirmek;csharp

var A = Sistem.DirectionalIndicatorPlus(14);
var B = Sistem.MA(A, "Exp", 10);
Sistem.Cizgiler[0].Deger = A;
Sistem.Cizgiler[1].Deger = B;

MACD - Sistem.MACD(12,26)MACD indikatörünü çağırır. 2 Adet parametre alır (varsayılan 12 ve 26). Aşağıdaki gibi 3 yazım şekli vardır;csharp

Sistem.MACD(12,26);
Sistem.MACD(Liste,12, 26);
Sistem.MACD(Veriler, 12, 26);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var MACD = Sistem.MACD(12,26);
var AVR = Sistem.MA(MACD,”Simple” , 9);
Sistem.Cizgiler[0].Deger = MACD;
Sistem.Cizgiler[1].Deger = AVR;

MAFARK - Sistem.MAFARK(Liste, “Exp”, 9)Bir fiyat veya indikatörün kendi ortalamasıyla arasındaki farkı hesaplayıp getiren iDeal Sistem fonksiyonudur. Kullanım şekli aşağıdaki gibidir;csharp

Sistem.MAFARK(Liste, "Simple", 10);

Örnek: Momentum İndikatörünün kendi 10’luk basit ortalamasıyla arasındaki farkı hesaplayıp bir indikatör gibi çizmekcsharp

var MOM = Sistem.Momentum(12);
var FARK = Sistem.MAFARK(MOM, "Simple", 10);
Sistem.Cizgiler[0].Deger = FARK;

MAM (MA’ların MA larını almak) - Sistem.MAM(Liste, “Exp”, P1, P2, P3, P4….)MA (Moving Average) bir fiyat veya indikatörün hareketli ortalamasını almak için kullanılan sistem fonksiyonudur. MAM ise aynı ortalama alma işini üst üste birden fazla kez yapmak istendiğinde kullanılır.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.MAM(Liste, "Weighted", 3, 5, 8, 13, 21, 34, 55, 89);

Örnek: Sosyal paylaşım sitelerinde Mavilim ismiyle paylaşılan açık kaynak kodlu indikatörcsharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var Mavilim = Sistem.MAM(C, "Weighted", 3, 5, 8, 13, 21, 34);
Sistem.Cizgiler[0].Deger = Mavilim;

MAİL GÖNDERMEK - Sistem.MailGonder……(...)iDeal Sistem kütüphanesinde yer alan Mail Gönder fonksiyonu ile ister belli zaman aralıklarında, ister belli koşullar gerçekleştiği zaman, ister robotlarınız al/sat yaptığında E-Mail gönderimi yapmak mümkündür.Alt Fonksiyonlar:csharp

Sistem.MailServerAdres = "smtp.gmail.com";
Sistem.MailServerPort = 587;
Sistem.MailKonu = "Robotum bilgilendirme";
Sistem.MailMetin = "Buraya ister yazı ister koddan veri alınabilir";
Sistem.MailGonderenAdres = "abc123@gmail.com";
Sistem.MailGonderenSifre = "xxxxxxx";
Sistem.MailAliciEkle("sezai.kilic@directfn.com", "deneme12@yahoo.com");
Sistem.MailGonder();
Sistem.MailBccEkle("adres");
Sistem.MailCcEkle("adres");
Sistem.MailDosyaEkle("C:\\test.png");

Popüler Mail Server Bilgileri:Yahoo → smtp.mail.yahoo.com 587
Gmail → smtp.gmail.com 587
Hotmail → smtp.live.com 587
Yandex → smtp.yandex.com 587

ÖRNEK: GARAN Hisse fiyatı 7.50 olursa ekran görüntüsünü de çekip ekleyerek mail gönder:csharp

var Sembol = "IMKBH'GARAN";
var Seviye = 7.50;
var SonFiyat = Sistem.SonFiyat(Sembol);
var Mesaj = Sembol + "Son fiyatı " + SonFiyat + " değerini kırdı";
if (SonFiyat >= Seviye)
{
    Sistem.GoruntuKaydet(“C:\\Ekranım.png”);
    Sistem.MailServerAdres = "smtp.gmail.com";
    Sistem.MailServerPort = 587;
    Sistem.MailKonu = "IDEAL ALARM";
    Sistem.MailMetin = Mesaj;
    Sistem.MailGonderenAdres = "deneme123@gmail.com";
    Sistem.MailGonderenSifre = "xxxxxxx";
    Sistem.MailDosyaEkle(“C:\\Ekranım.png”);
    Sistem.MailAliciEkle("sezaik@idealdata.com.tr");
    Sistem.MailGonder();
}

MASS INDEX - Sistem.MassIndex(25)Mass Index olarak bilinen indikatörü çağırır. 1 Adet parametre alır (varsayılan 25). Aşağıdaki gibi 2 yazım şekli vardır;csharp

Sistem.MassIndex(25);
Sistem.MassIndex(Veriler, 25);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var MASS = Sistem.MassIndex(25);
Sistem.Cizgiler[0].Deger = MASS;

MEDIAN - Sistem.Median(Liste,14) ve Sistem.MedianDeger(Liste)csharp

Sistem.Median(Liste, 14);       // hep son 14 barın median ortalamasını hesaplar
Sistem.MedianDeger(Liste);      // listenin tüm elemanlarının median değerini hesaplar

Örnek:csharp

var TimePeriod = 14;
var Data = Sistem.GrafikFiyatSec("Kapanis");
var m = Sistem.Median(Data, TimePeriod);
var m2 = Sistem.MedianDeger(Data);
Sistem.Cizgiler[0].Deger = m;
Sistem.Mesaj(m2.ToString());

MESAJ - Sistem.Mesaj(Metin)csharp

Sistem.Mesaj(metin);
Sistem.Mesaj(metin, renk);
Sistem.Mesaj(metin, renk, sol, ust, genislik, yukseklik);
Sistem.Mesaj(metin, sol, ust, genislik, yukseklik);

Momentum - Sistem.Momentum(12)csharp

Sistem.Momentum(12);
Sistem.Momentum(Liste,12);
Sistem.Momentum(Veriler,12);

Örnek:csharp

var MOM = Sistem.Momentum(100);
var ORT = Sistem.MA(MOM, “Exp”, 10);
Sistem.Cizgiler[0].Deger = MOM;
Sistem.Cizgiler[1].Deger = ORT;

Money Flow Index (Para Akış Endeksi) - Sistem.MoneyFlowIndex(14)csharp

Sistem.MoneyFlowIndex(14);
Sistem.MoneyFlowIndex(Veriler,14);

Örnek:csharp

var MFI = Sistem.MoneyFlowIndex(14);
var ORT = Sistem.MA(MFI, “Exp”, 10);
Sistem.Cizgiler[0].Deger = MFI;
Sistem.Cizgiler[1].Deger = ORT;

Multi Sembol Birleştir Aynı Yön - Sistem.MultiSembolBirlestirAyniYon(...)csharp

Sistem.MultiSembolBirlestirAyniYon("referanssistem","IMKBX'XU030","IMKBX'XU100","IMKBX'XBANK");

NVI – Negative Volume Index - Sistem.NegativeVolumeIndex()csharp

Sistem.NegativeVolumeIndex();
Sistem.NegativeVolumeIndex(Veriler);

NESNE Kullanımı (Nesne Getir / Kaydet) - Sistem.NesneGetir() / Sistem.NesneKaydet()ÖRNEK-1: Yeni bar geldiğini tespit etmecsharp

var V = Sistem.GrafikVerileri;
var Anahtar = Sistem.Name + ";" + Sistem.Sembol + ";" + Sistem.Periyot;
var SonBarTarih = Sistem.NesneGetir(Anahtar);
if (SonBarTarih == null)
{
    SonBarTarih = V[V.Count - 1].Date;
    Sistem.NesneKaydet(Anahtar, SonBarTarih);
}
if (V[V.Count - 1].Date > SonBarTarih)
{
    Sistem.Mesaj(V[V.Count - 1].Date.ToString("HH:mm"));
    SonBarTarih = V[V.Count - 1].Date;
    Sistem.NesneKaydet(Anahtar, SonBarTarih);
}

markdown

### NET HACİM Okuma Fonksiyonları – Sistem.NetHacim(…)

iDeal Sistem kütüphanesinde Net Hacim olarak ekranlarda izlediğimiz verilere erişim için tanımlanmış 4 adet fonksiyon bulunmaktadır. Fonksiyonların kullanım şekilleri ve sunduğu bilgilerin neler olduğu aşağıdaki gibidir;  
(Not: en az düzey1 plus Borsa lisansı gerekir)

#### Sistem.NetHacim(Sembol)
Bu fonksiyon, herhangi bir hisse veya vadeli kontratın, bulunduğumuz gün, bu komutu çalıştırdığımız an itibariyle TL bazlı NET hacim bilgisini (yani alıştan olan işlemlerin hacmi ile satıştan olan işlemlerin hacmi arasındaki farkını) döner.

**Örnek:** EKGYO hissenin şu an itibariyle net hacmi kaç TL?
```csharp
var Sembol = "IMKBH'EKGYO";
var NetHacim = Sistem.NetHacim(Sembol);
Sistem.Mesaj(NetHacim.ToString("0,000"));

Sistem.NetHacimOran(Sembol)Bu fonksiyon, herhangi bir hisse veya vadeli kontratın, bulunduğumuz gün, bu komutu çalıştırdığımız an itibariyle TL bazlı NET hacim bilgisinin (yani alıştan olan işlemlerin hacmi ile satıştan olan işlemlerin hacmi arasındaki farkının) o hissede o gün o ana kadarki toplam hacime olan oranını döner.Örnek: GARAN hissenin şu an itibariyle Net Hacim / Toplam Hacim oranı nedir?csharp

var Sembol = "IMKBH'GARAN";
var NetOran = Sistem.NetHacimOran(Sembol);
Sistem.Mesaj(NetOran.ToString("0.00"));

Sistem.NetHacimBist()Bu fonksiyon, tüm BIST pay piyasasının Net Hacim bilgisini elde etmek için kullanılır.Sistem.NetHacimOranBist()Bu fonksiyon, tüm BIST pay piyasasının NetHacim / ToplamHacim oranı bilgisini elde etmek için kullanılır.BIST menüsü altından açılabilen TÜM İŞLEMLER isimli pencerenin üst kısmında, her bir işlem oldukça anlık olarak hesaplanıp gösterilen bilgilere erişim için bu fonksiyonlar kullanılır.NET LOT Okuma Fonksiyonları – Sistem.NetLot(…)iDeal Sistem kütüphanesinde Net Lot olarak ekranlarda izlediğimiz verilere erişim için tanımlanmış 2 adet fonksiyon bulunmaktadır. Fonksiyonların kullanım şekilleri ve sunduğu bilgilerin neler olduğu aşağıdaki gibidir;
(Not: en az düzey1 plus Borsa lisansı gerekir)Sistem.NetLot(Sembol)Bu fonksiyon, herhangi bir hisse veya vadeli kontratın, bulunduğumuz gün için, bu komutu çalıştırdığımız an itibariyle LOT bazlı NET hacim bilgisini (yani alıştan olan işlemlerin lotu ile satıştan olan işlemlerin lotu arasındaki farkını) döner.Örnek: Ekim 2020 Vadeli Endeks30 vadeli kontratının net lotu?csharp

var Sembol = "VIP'F_XU0301020";
var NetLot = Sistem.NetLot(Sembol);
Sistem.Mesaj(NetLot.ToString("0,000"));

Sistem.NetLotOran(Sembol)Bu fonksiyon, herhangi bir hisse veya vadeli kontratın, bulunduğumuz gün, bu komutu çalıştırdığımız an itibariyle LOT bazlı NET hacim bilgisinin (yani alıştan olan işlemlerin lotu ile satıştan olan işlemlerin lotu arasındaki farkının) o hissede o gün o ana kadarki toplam lot miktarına olan oranını döner.csharp

var Sembol = "IMKBH'GARAN";
var NetLotOran = Sistem.NetLotOran(Sembol);
Sistem.Mesaj(NetLotOran.ToString("0.00"));

NESNE Kullanımı (Nesne Getir / Kaydet) – DevamÖRNEK-2: Bir Excel dosyasına, fiyatı bizim belirlediğimiz seviyeye düşünce, bizim belirlediğimiz adet alım yapan, yine bizim belirlediğimiz KAR AL ve STOP fiyatlarına ulaşınca da hisseleri satan bir robot.Not1: C:\ideal\HisseRobot.xlsx isimli bir excel dosyası kullandık
Not2: Excel satır ve sütunlarında örnek olarak aşağıdaki bilgileri yazılı idi.csharp

string FileName = "C:\\ideal\\HisseRobot.XlsX";
bool DevamEt = true;
if (DateTime.Now.DayOfWeek == DayOfWeek.Saturday) DevamEt = false;
if (DateTime.Now.DayOfWeek == DayOfWeek.Sunday) DevamEt = false;
if (DateTime.Now.ToString("HHmm").CompareTo("1000") <= 0) DevamEt = false;
if (DateTime.Now.ToString("HHmm").CompareTo("1759") >= 0) DevamEt = false;
if (System.IO.File.Exists(FileName) == false) DevamEt = false;
if (DevamEt)
{
    var ExcelArray = Sistem.NesneGetir(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"));
    if (ExcelArray == null)
    {
        ExcelArray = Sistem.ExcelOku(FileName);
        Sistem.NesneKaydet(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"), ExcelArray);
    }
    int SatirSayisi = ExcelArray.GetLength(0);
    for (int i = 2; i <= SatirSayisi; i++)
    {
        var Sembol = ExcelArray[i, 1].ToString();
        var AlisFiyat = (decimal)ExcelArray[i, 2];
        var HedefFiyat = (decimal)ExcelArray[i, 3];
        var StopFiyat = (decimal)ExcelArray[i, 4];
        var Lot = (int)ExcelArray[i, 5];
        var Anahtar = Sistem.Name + " , " + Sembol;
        double IslemFiyat = 0;
        DateTime IslemTarih;
        var Rezerv = "";
        var Pozisyon = Sistem.PozisyonKontrolOku(Anahtar, out IslemFiyat, out IslemTarih, out Rezerv);
        var EmirSembol = "IMKBH'" + Sembol;
        var basicitem = Sistem.YuzeyselVeriOku(EmirSembol);
        var sonfiyat = (decimal)basicitem.LastPrice;
        var bidfiyat = (decimal)basicitem.BidPriceDec;
        var askfiyat = (decimal)basicitem.AskPriceDec;
        if (sonfiyat == 0) continue;
        if (bidfiyat == 0) continue;
        if (askfiyat == 0) continue;
        var Islem = "";
        var Miktar = 0.0;
        if (sonfiyat < AlisFiyat && Pozisyon == 0 && Rezerv == "") // AL
        {
            Rezerv = "AL";
            Miktar = Lot;
            IslemFiyat = Sistem.SonFiyat(EmirSembol);
        }
        else if (Pozisyon > 0 && askfiyat >= HedefFiyat && Rezerv == "AL") // KARLA KAPAT
        {
            Rezerv = "KAR AL";
            Miktar = -Lot;
        }
        else if (Pozisyon > 0 && bidfiyat < StopFiyat && Rezerv == "AL") // STOP
        {
            Rezerv = "STOP";
            Miktar = -Lot;
        }
        if (Miktar > 0) Islem = "ALIS";
        if (Miktar < 0) Islem = "SATIS";
        if (Islem != "")
        {
            Sistem.PozisyonKontrolGuncelle(Anahtar, Miktar + Pozisyon, IslemFiyat, Rezerv);
            Sistem.EmirSembol = EmirSembol;
            Sistem.EmirIslem = Islem;
            Sistem.EmirSuresi = "KIE";
            Sistem.EmirTipi = "Piyasa";
            Sistem.EmirMiktari = Math.Abs(Miktar);
            Sistem.EmirGonder();
        }
    }
}

ÖRNEK3: Robot işlem yaptıktan sonra 20 saniye boyunca dursun, sonra devam etsin;csharp

//İŞLEM YAPTIKTAN SONRA ROBOT 20 SANİYE DURSUN
//SONRA DEVAM ETSİN
var DevamEt = false;
var Anahtar = Sistem.Name + ", ZAMAN KONTROL";
var Saat = Sistem.NesneGetir(Anahtar);
if (Saat == null)
    DevamEt = true;
else if (DateTime.Now > Saat.AddSeconds(20))
    DevamEt = true;
if (DevamEt)
{
    var Islem = Sistem.RobotViopAktif("Sistem1", "VIP'VIP-X030", Sistem.AktifViopKontrat, "5", 4);
    if (Islem != "")
        Sistem.NesneKaydet(Anahtar, DateTime.Now);
}

