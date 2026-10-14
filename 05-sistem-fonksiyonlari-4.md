markdown

# Sistem Fonksiyonları (Devam - 5. Bölüm)

### Hacim - Sistem.Hacim(Sembol)

Bir sembolün o an ki anlık işlem hacmiyle birlikte, çeşitli dönemler için olan TL işlem hacmini okumak için kullanılır. Fonksiyonun içine Hacmi okutulmak istenen sembol yazılır.

Not: iDeal programında bütün semboller ait oldukları piyasasının kodu ile birlikte yazılırlar. Hisse senetlerinin piyasa kodu IMKBH dır. PİYASA kodundan sonra ÜSTTEN TEK TIRNAK işareti ile ayrılıp borsadaki orijinal kod eklenir. GARAN hissesinin idealdeki sembol tanımı IMKBH’GARAN şeklindedir.

Kullanım şekilleri aşağıdaki gibidir.
```csharp
Sistem.HacimAltiAy(Sembol);
Sistem.HacimBirAy(Sembol);
Sistem.HacimBirHafta(Sembol);
Sistem.HacimBirYil(Sembol);
Sistem.HacimBuAy(Sembol);
Sistem.HacimBuHafta(Sembol);
Sistem.HacimBuYil(Sembol);
Sistem.HacimGun(Sembol);
Sistem.HacimSeans(Sembol);
Sistem.HacimUcAy(Sembol);

Örnek: GARAN hisse senedinin, bu doküman yazılırken ki son 1 yıllık hacim değerinin okuyup ekrana mesaj olarak çıkaran kod:csharp

var YillikHacim = Sistem.HacimBirYil(Sembol);
Sistem.Mesaj("1 yıllık hacim = " + YillikHacim.ToString());

Hafta Sonu - Sistem.HaftaSonuSürekli çalışır durumda olan sistemlerde/robotlarda, olası hataların önüne geçmek üzere sunulan güvenlik amaçlı kullanılan bir fonksiyondur. Haftasonu (mesela dışardan grafik verisi import edildiğinde sistemler/robotlar sinyal üretmesin, emir göndermesin diye veya bilgisayar boşuna robotu tarayıp kaynak tüketmesin) olup olmadığı kontrol edilip, haftasonu ise bir işlem yapma gibi koda eklemeler yapılabilir.Yazdığınız bir kodun tamamı veya belli bir kısmı (mesela sadece emir gönderen kısım) Bu fonksiyonla kontrole tabi tutulabilir.ÖRNEK:csharp

if (Sistem.HaftaSonu == true)
    Sistem.Mesaj("Şu an Hafta Sonundayız. Kod çalıştırılmayacak");
else
{
    // Kodunuzun tamamı burada yer alır
}

Hesap Kurum - Sistem.HesapKurum()iDeal Portföy penceresi, birden fazla kuruma ait hesapların tanımlanabildiği ve aynı anda farklı farklı kurumlardaki hesaplara emir gönderilebildiği bir yapıya sahiptir. Portföy penceresinde sol üstteki kutuda seçili olan Kurum/Hesap aktif hesaptır. Aksi kodlanmamışsa robotlar, trend ve indikatör alarmlarına bağlı emirler bu aktif hesaba gönderilir. Dileyen kullanıcılarımız formül yazarken, formüllerinin/robotlarının sadece bir aracı kurum için çalışmasını (veya tersi bir aracı kurum için çalışmamasını) ayarlayabilirler.Bunun için Sistem.HesapKurum() komutuyla, portföy penceresindeki aktif (seçili) kurumun hangisi olduğu bilgisine ulaşılır.ÖRNEK:csharp

if (Sistem.HesapKurum() == "ABC YATIRIM")
{
    // FORMÜL SATIRLARI BURADA
}
else
{
    Sistem.Mesaj("Bu formül ABC YATIRIM için çalışmamaktadır.");
}

HHLL - Sistem.HHLL(14)HHLL (HighestHighLowestLow) isimli indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. 1 adet parametre alır ve varsayılan değeri 14’tür. HHV (HighestHighValue) ve LLV (LowestLowValue) seviyeleri (yani girilen parametre kadar bar için görülen EN YÜKSEK değer ile EN DÜŞÜK değer ARASINDAKİ FARKI bir indikatör olarak hesaplar çizer. Grafik barları üzerine değil, alt bölgedeki panellere çizilir. Kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.HHLL(14);
Sistem.HHLL(14, Liste);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var L1 = Sistem.HHLL(14);
Sistem.Cizgiler[0].Deger = L1; //Panel no 2 yapılmalıdır

HHV - Sistem.HHV(14)HHV (Highest High Value) isimli indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. 1 adet parametre alır ve varsayılan değeri 14’tür. HHV (HighestHighValue) girilen parametre kadar bar için görülen EN YÜKSEK değeri bir indikatör olarak hesaplar çizer. Grafik barları üzerine çizdirilir ve böylece fiyat kesişimleri veya kanal analizi yapılabilir. Kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.HHV(14);
Sistem.HHV(14, Liste);
Sistem.HHV(14, “Yuksek”);
Sistem.HHV(GrafikVerileri, 14, “Yuksek”); //Başka bir sembole veya grafik periyoduna ait barlar için HHV hesaplar.

Örnek: İndikatörü çağırıp çizdirmekcsharp

var HH = Sistem.HHV(14);
Sistem.Cizgiler[0].Deger = HH;

Hisse İşlemlerini Oku - Sistem.HisseIslemleriniOku()Hisse senetlerinde ve varantlarda gerçekleşen işlemlerin tümünü (tüm işlemler) okuyarak kullanıcıya üzerinde hesaplama yapma imkanı veren ideal sistem fonksiyonudur. Eğer en az Düzey1+ (1 kademe derinlik) lisansınız varsa, gerçekleşen işlemlerin tamamını okutup, örneğin kademe analizi, aracı kurum analizi, net lot hesaplatma, bir aracı kurumun bir hissede yaptığı işlemleri baz alarak emir ileten robotlar yazma gibi formüller yazabilirsiniz.Fonksiyon çalıştırıldığı zaman, geriye her bir gerçekleşen işleme ait veriler (işlemin saati/dakikası/saniyesi, işlemin fiyatı, işlemin lotu, alıcı kurum kodu, satıcı kurum kodu, işlem olduğu andaki en iyi alış fiyatı, işlem olduğu andaki en iyi satış fiyatı, işlemin borsa tarafından üretilen ID (numarası) ve işlemin yönü (bir alış işlemi mi, bir satış işlemi mi) bilgilerine sahip olunur.Fonksiyonun tüm dönüş bilgileri ve değişken tipleri aşağıda belirtilmiştir;csharp

string Symbol = "";
byte Hour = 0;
byte Minute = 0;
byte Second = 0;
float Price = 0;
float Bid = 0;
float Ask = 0;
float Size = 0;
string BuyerCode = ""; //alıcı kurumun 3 harfli kısa kodudur.
string SellerCode = ""; //satıcı kurumun 3 harfli kısa kodudur.
string AggresiveParty = " "; // B,S (B-buy ise ALIŞ işlemidir, S-sell ise SATIŞ işlemidir)
string NasdaqTradeId = "";

Not1: Alan ve Satan kurum isimleri (kodları) PITE lisansı (anlık alan satan) olan kullanıcılar tarafından görülebilir.
Not2: Alan ve Satan kurum isimleri (kodları) c:\ideal\config klasörü altındaki Kurumlar.001 isimli dosya içerisinde yer almaktadır. Bu dosyayı NOTEPAD ile açarak hangi kurumunun kısa kodunun ne olduğu bilgisi görülebilir.Örnek Kullanım-1: (GARAN hissesi için ilk 5 kurum seviyesinde PGÇ hesaplayan kod)
Not: kodun içindeki GARAN satırı değiştirilerek veya satırın kopyası alınıp yapıştırılıp başka senetler yazılarak, birden fazla senet grubu için de PGÇ hesaplatılabilir.csharp

var Seviye = 5; //kaç kurum seviyesi için PGÇ
var Semboller = new Dictionary<string, bool>();
Semboller["GARAN"] = true;
//Yukarıya istenildiği kadar senet eklenebilir Bu durumda o senet grubunun TOPLAM PGÇ değeri elde edilir.
//Kodun alt kısmında hiçbir değişiklik yapmanıza gerek yoktur.
// *********************************
var KurumlarNet = new Dictionary<string, double>();
var Islemler = Sistem.HisseIslemleriniOku();
foreach (var Islem in Islemler)
{
    if (Semboller.ContainsKey(Islem.Symbol))
    {
        if (KurumlarNet.ContainsKey(Islem.BuyerCode) == false)
            KurumlarNet[Islem.BuyerCode] = 0;
        if (KurumlarNet.ContainsKey(Islem.SellerCode) == false)
            KurumlarNet[Islem.SellerCode] = 0;
        double TL = Islem.Price * Islem.Size;
        KurumlarNet[Islem.BuyerCode] += TL;
        KurumlarNet[Islem.SellerCode] -= TL;
    }
}
KurumlarNet = KurumlarNet.OrderBy(x => x.Value).ToDictionary(x => x.Key, y => y.Value);
double AlanNet = 0;
double SatanNet = 0;
if (KurumlarNet.Count > 2 * Seviye)
{
    for (int i = 0; i < Seviye; i++)
        SatanNet += KurumlarNet.ElementAt(i).Value;
    for (int i = KurumlarNet.Count - Seviye; i < KurumlarNet.Count; i++)
        AlanNet += KurumlarNet.ElementAt(i).Value;
}
// PGC
double PGC = AlanNet + SatanNet;
Sistem.Mesaj("Para Giriş Çıkışı = " + PGC.ToString("0,000")); //ekranda mesaj olarak görmek için

Örnek Kullanım-2: CITI MENKUL kurumu ASELS hisse senedinde ALIŞ yapınca 1 lot ASELS alımı yapan robot koducsharp

var Senet = "ASELS";
var Miktar = 1;
var Anahtar = Sistem.Name + DateTime.Now.Date.ToString("yyyyMMdd");
var TradeID = Sistem.SayiTablosunuOku(Anahtar);
var Emirler = Sistem.HisseIslemleriniOku(TradeID);
var Pozisyon = Sistem.PozisyonKontrolOku(Anahtar+Senet);
if (TradeID > 0 && Emirler.Count > 0)
{
    foreach (var item in Emirler)
    {
        if (item.Symbol == Senet && (item.BuyerCode == "CIM"))
        {
            Sistem.PozisyonKontrolGuncelle(Anahtar+Senet, Pozisyon+Miktar);
            Sistem.EmirSembol = "IMKBH'"+Senet;
            Sistem.EmirIslem = "Alış";
            Sistem.EmirMiktari = (int)Miktar;
            Sistem.EmirSuresi = "KIE";
            Sistem.EmirTipi = "Piyasa";
            Sistem.EmirGonder();
        }
    }
}
TradeID = Emirler[Emirler.Count - 1].TradeID;
Sistem.SayiTablosunuGuncelle(Anahtar, TradeID);

HY - Sistem.HY(Periyot)Bir grafik üzerinde, girili periyot içerisinde görülen EN YÜKSEK değerden yüzdesel olarak ne kadar uzakta olduğumuz bilgisini dönen bir indikatör olan HY indikatörünü hesaplar. Bir adet parametre alır. Kullanım şekli Sistem.HY(20); şeklindedir.Örnek: Momentum kendi ortalamasının üstündeyse, fiyatın 20 barlık en yükseğinden uzaklığı %2 den küçükse AL, tersi durumda SAT örnek sistemi.csharp

var HY = Sistem.HY(20); // 20 periyottaki en yüksekle fiyat arasındaki yüzde fark
var Mom = Sistem.Momentum(5);
var AvrMom = Sistem.MA(Mom, "Simple", 10);
var SonYon = "";
for (int i=1; i < Sistem.BarSayisi; i++)
{
    HY[i] = Math.Abs(HY[i]);
    if (SonYon != "A" && HY[i] < 2 && Mom[i] > AvrMom[i])
    {
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    if (SonYon != "S" && HY[i] > 2 && Mom[i] < AvrMom[i])
    {
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

Ichi Moku - Sistem.Ichimoku()Bir grafik üzerinde Ichi Moku olarak bilinen indikatörü hesaplar. Kullanım şekli Sistem.Ichimoku(); şeklindedir. Bu fonksiyon tek başına kullanıldığı zaman herhangi bir görsel efekt yaşanmaz grafik üzerinde veya robotlarda. Fonksiyon çağrıldığı zaman geriye bir nesne döner. Bu nesnenin içinde Ichi Moku indikatörünün 5 adet listesi (çizgisi) vardır. Bu listelere (çizgilere) Ichi Mokuyu tanımlarken kullanılmış olan değişken isminden sonra noktaya basılarak ilgili çizginin adı yazılarak erişilir.Kullanım şu şekildedir;csharp

var IchiMoku = Sistem.Ichimoku(); //Ichimoku indikatörüne ait çizgileri çağıran nesne
var Tenkansen = IchiMoku.Tenkansen;
var Kijunsen = IchiMoku.Kijunsen;
var ChikouSpan = IchiMoku.ChikouSpan;
var SenkouSpanA = IchiMoku.SenkouSpanA;
var SenkouSpanB = IchiMoku.SenkouSpanB;

Örnek: Fiyat hem Tenkansen hem de Kijunsen üzerindeyse AL tersi durumda SAT sinyali üreten sistem formülücsharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var IchiMoku = Sistem.Ichimoku();
var Tenkansen = IchiMoku.Tenkansen;
var Kijunsen = IchiMoku.Kijunsen;
var SonYon = "";
for(int i=1; i < Sistem.BarSayisi;i++)
{
    if (C[i] > Tenkansen[i] && C[i] > Kijunsen[i] && SonYon != "A")
    {
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (C[i] < Tenkansen[i] && C[i] < Kijunsen[i] && SonYon != "S")
    {
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

IFISH CCI - Sistem.IFISHCCI(6, 9)CCI indikatörünü kullanarak hesaplatılan IFISH isimli indikatörü hesaplayıp çağıran sistem fonksiyonudur.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.IFISHCCI(6, 9);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var x = Sistem.IFISHCCI(6, 9);
Sistem.Cizgiler[0].Deger = x; // 2 numaralı panele çizdirilir.

IFISH RSI - Sistem.IFISHRSI(6, 9)RSI indikatörünü kullanarak hesaplatılan IFISH isimli indikatörü hesaplayıp çağıran sistem fonksiyonudur.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.IFISHRSI(6, 9);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var x = Sistem.IFISHRSI(6, 9);
Sistem.Cizgiler[0].Deger = x; // 2 numaralı panele çizdirilir.

IMI / Intraday Momentum Index - Sistem.IMI(14)IMI (Intraday Momentum Index) olarak bilinen indikatörü hesaplayıp çağıran sistem fonksiyonudur. Varsayılan olarak 14 parametresi alır. Kullanım şekli aşağıdaki gibidir;csharp

Sistem.IMI(14);
Sistem.IMI(Veriler, 14);

Örnek: İndikatörü çağırıp çizdirmek (indikatör üzerinde yatay 30 ve 70 seviyelerine de birer çizgi çizdirilmiştir.)csharp

var IMI = Sistem.IMI(14);
Sistem.Cizgiler[0].Deger = IMI; // 2 numaralı panele çizdirilir.
Sistem.Cizgiler[1].Deger = Sistem.Liste(30); // 2 numaralı panele çizdirilir.
Sistem.Cizgiler[2].Deger = Sistem.Liste(70); // 2 numaralı panele çizdirilir.

İzleyen Stop Puan - Sistem.IzleyenStopPuan(2,i)Grafikler üzerinde Al ve Sat sinyalleri üreten bir strateji/sistem formülü yazıldığı zaman, ALIM veya SATIM pozisyonuna girildiği fiyata PUAN (TL) bazında izleyen (iz süren) stop eklemek için kullanılan ideal sistem fonksiyonudur. Fonksiyon içerisine yazılmış kadar puan ile izleyen; AL yönündeyken bu fiyat seviyesi aşağı yönde kesildiğinde veya SAT yönündeyken bu fiyat seviyesini yukarı yönde kesildiğinde sistemi FLAT (Nakit) pozisyona çekmek için kullanılır.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.IzleyenStopPuan(2,i);

Örnek: IMI, RSI ve MOV kullanılarak üretilmiş örnek bir AL/SAT sistemi ve bu sisteme VIOP ENDEKS için 1200 puan izleyen stop ekleme formülücsharp

var V = Sistem.GrafikVerileri;
var C = Sistem.GrafikFiyatSec("Kapanis");
var H = Sistem.GrafikFiyatSec("Yuksek");
var L = Sistem.GrafikFiyatSec("Dusuk");
var XX = C;
var X1 = Sistem.MA(XX, "Exp", 10);
var X2 = Sistem.MA(XX, "Exp", 20);
var YY = Sistem.RSI(10);
var Y1 = Sistem.MA(YY, "Exp", 20);
var Y2 = Sistem.MA(YY, "Exp", 40);
var ZZ = Sistem.IMI(7);
var Z1 = Sistem.MA(ZZ, "Exp", 13);
var Z2 = Sistem.MA(ZZ, "Exp", 36);
var KARAL = Sistem.Liste(0);
var IZLEYENSTOP = Sistem.Liste(0);
var SonYon = "";
var FlatOncesiYon = "";
for (int i = 1 ; i < Sistem.BarSayisi; i++)
{
    var IndikatorAlis = X1[i] > X2[i] && Y1[i] > Y2[i] ;
    var IndikatorSatis = X1[i] < X2[i] && Y1[i] < Y2[i];
    IZLEYENSTOP[i] = Sistem.IzleyenStopPuan(1.200, i); //1200 puan izleyen stop
    if (IZLEYENSTOP[i] == 0) IZLEYENSTOP[i] = C[i];
    if (C[i] < IZLEYENSTOP[i] && SonYon == "A")
    {
        FlatOncesiYon = SonYon;
        SonYon = "F";
        Sistem.Yon[i] = "F";
    }
    else if ((C[i] > IZLEYENSTOP[i] || C[i] <= KARAL[i]) && SonYon == "S") // satıştan flate
    {
        FlatOncesiYon = SonYon;
        SonYon = "F";
        Sistem.Yon[i] = "F";
    }
    else if (IndikatorAlis && SonYon != "A" && FlatOncesiYon != "A") // alış
    {
        FlatOncesiYon = "";
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (IndikatorSatis && SonYon != "S" && FlatOncesiYon != "S") // satış
    {
        FlatOncesiYon = "";
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}
Sistem.Cizgiler[0].Deger = IZLEYENSTOP;
Sistem.Cizgiler[1].Deger = X2;
Sistem.Cizgiler[2].Deger = IZLEYENSTOP;
Sistem.Cizgiler[3].Deger = C;

İzleyen Stop Yüzde - Sistem.IzleyenStopYuzde(1.5,i)Grafikler üzerinde Al ve Sat sinyalleri üreten bir strateji/sistem formülü yazıldığı zaman, ALIM veya SATIM pozisyonuna girildiği fiyata, YÜZDE (%) bazda izleyen (iz süren) stop eklemek için kullanılan ideal sistem fonksiyonudur. Fonksiyon içerisine yazılmış kadar yüzde ile izleyen; AL yönündeyken bu fiyat seviyesi aşağı yönde kesildiğinde veya SAT yönündeyken bu fiyat seviyesini yukarı yönde kesildiğinde sistemi FLAT (Nakit) pozisyona çekmek için kullanılır.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.IzleyenStopYuzde(1.5,i);

Örnek: 2 hareketli ortalamanın kesişimine göre ALIM yapan sisteme %1.5 izleyen stop eklenmesicsharp

var IzleyenYuzde = 1.5; //%1.5 izleyen stop
var C = Sistem.GrafikFiyatSec("Kapanis");
var SonYon = "";
// hareketli ortalamaları hesapla
var MA1 = Sistem.MA(C, "Exp", 10);
var MA2 = Sistem.MA(C, "Exp", 100);
var IZLEYENSTOP = Sistem.Liste(0);
for (int i = 1; i < Sistem.BarSayisi; i++)
{
    IZLEYENSTOP[i] = Sistem.IzleyenStopYuzde(IzleyenYuzde, i);
    if (IZLEYENSTOP[i] == 0) IZLEYENSTOP[i] = C[i];
    if (MA1[i-1] < MA2[i-1] && MA1[i] > MA2[i] && SonYon!="A" ) // 1.ortalama 2.ortalamanın üstüne çıkarsa
    {
        Sistem.Yon[i] = "A"; //AL
        SonYon = "A";
    }
    else if (C[i] < IZLEYENSTOP[i] && SonYon!="F" ) // İzleyen Stop ile nakite geç
    {
        Sistem.Yon[i] = "F"; // SAT
        SonYon = "F";
    }
}

Kademe Analizi Oku – Sistem.KademeAnalizOku(Sembol)Veri terminallerinde yatırımcıların en çok takip ettiği analizlerden biri Kademe Analizidir. Bir hissede veya vadeli sözleşmede hangi fiyattan kaç adet işlem olmuş, bu işlemlerin ne kadarı alıştan, ne kadarı satıştan olmuş, ortalama fiyat nedir gibi bilgileri bu analiz sunmaktadır. Sistem fonksiyonlarını kullanarak bir hissenin, vadeli sözleşmenin, varantın veya opsiyonun, o gün o ana kadar gerçekleşmiş işlemlerinden oluşan kademe analizi verilerine erişmeyi sağlayan sistem fonksiyonudur. Aşağıda gösterildiği gibi, tüm günün, belli bir saat aralığının ve son x dakikanın kademe analizini okumaya yönelik 3 farklı kullanım şekli vardır;csharp

Sistem.KademeAnalizOku(Sembol); //tüm günün kademe analizi okunur
Sistem.KademeAnalizOku(Sembol, "10:30", "11:45"); //girilen iki saat arasındaki kademe analizini okur
Sistem.KademeAnalizOku(Sembol, 15); //Son 15 dakikanın kademe analizini okur

Kademe Analizi oku fonksiyonu ile kademeler okutulup bir değişkene atandığında (yani var x = Sistem.KademeAnalizOku(Sembol); satırı yazıldığında) atanmış değişken içinde birçok veriyi barındıran bir nesne/obje olarak sunulmuş olur. Bu nesne içindeki bilgilerden herhangi birine x.BilgiAdi şeklinde yazarak erişiriz.Örnek: GARAN hisse senedinin Kademe Analizini okuyup, hissede toplam kaç TL işlem olmuş olduğu verisine erişelim;csharp

var Sembol = "IMKBH'GARAN";
var Kademeler = Sistem.KademeAnalizOku(Sembol);
var Hcm = Kademeler.ToplamHacim;
Sistem.Mesaj(Hcm.ToString("0,000"));

Formülü yazıp kaydettikten sonra Formül Test butonuna basarsak, GARAN hisse senedinde o ana kadar gerçekleşmiş işlemlerin toplam hacim (TL) verisini okur ve ekrana bir mesaj penceresi açarak gösterir.Dikkat edilirse, kademe analizi oku komutu ile çağrılan kademe analizi verileri ismi Kademeler olan bir değişkene geliyor. Bu değişken (içinde bir çok bilgi içeren bir nesnedir) içinden mesela Toplam Hacim verisini okumak için değişken adının sonuna nokta işareti koyup toplam hacmi veren bilgi alanının adını yazıyoruz.Kademe Analizi Oku denildiği zaman elde edilen nesne içinde yer alan bilgilerin tamamı decimal sayı formatındadır. Bilgiler ve yazılış şekilleri aşağıdadır. (kademe analizi oku denildiğinde aşağıdaki bilgiler elde edilebilir)csharp

decimal ToplamHacim = 0;
decimal ToplamLot = 0;
decimal Ortalama = 0;
decimal AlisLot = 0;
decimal SatisLot = 0;
decimal MaxToplamLot = 0;
decimal MaxAlisLot = 0;
decimal MaxSatisLot = 0;
decimal Fark = 0;
decimal MaxAlisFark = 0;
decimal MaxSatisFark = 0;
decimal MaxToplamLotFiyat = 0;
decimal MaxAlisLotFiyat = 0;
decimal MaxSatisLotFiyat = 0;

Yukarıda belirtilen kullanım şekillerinde de gösterildiği gibi, istenirse kullanıcı tarafından girilecek 2 saat arasındaki işlemlerin veya bulunduğumuz saat/dakikaya göre son x dk olan işlemlerin kademe analizi verileride hesaplatılıp kullanılabilir.Örnek-2: ASELSAN hisse senedinin kademe analizini okuyan, bu analizde sunulan tüm bilgi alanlarına erişip, o bilgileri ekrana açtırılacak bir tabloya yazan kod örneği;csharp

var Sembol = "IMKBH'ASELS";
var Kademeler = Sistem.KademeAnalizOku(Sembol);
var Hcm = Kademeler.ToplamHacim;
var SatisLot = Kademeler.SatisLot;
var AlisLot = Kademeler.AlisLot;
var MaxLot = Kademeler.MaxToplamLot;
var MaxSatisHcm = Kademeler.MaxSatisLot;
var MaxAlisHcm = Kademeler.MaxAlisLot;
var Delta = Kademeler.Fark;
var DeltaMax = Kademeler.MaxAlisFark;
var DeltaMin = Kademeler.MaxSatisFark;
var VWAP = Kademeler.Ortalama;
var MacKademeFiyati = Kademeler.MaxToplamLotFiyat;

// Tablo Yarat
string TabloAd = Sembol + " " + "Kademe Analizi Detayları "; //İSİM VER
var SutunGenislik = new int[2] {300,100}; //SÜTUN SAYISI VE SÜTUN GENİŞLİKLERİ
var SutunHizala = new int[2] { 0,2}; //HİZALAMA (1=ORTALA)
var SutunBaslik = new string[2] { "Veri","Değer"}; //BAŞLIKLAR
Sistem.Tablo(TabloAd, 100, 200, 480, 350, 2, 15, SutunGenislik, SutunHizala, SutunBaslik, 1, 8);
//TABLOYU EKRANA GETİR

//Tabloyu Doldur
Sistem.TabloYazdir(TabloAd, 1, 0, Hcm.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 0, "Toplam İşlem Hacmi", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 1, SatisLot.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 1, "Toplam Satıştan Lot", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 2, AlisLot.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 2, "Toplam Alıştan Lot", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 3, MaxLot.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 3, "En Çok işlem Olan Kademenin Lotu", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 4, MaxSatisHcm.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 4, "En çok satıştan lot", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 5, MaxAlisHcm.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 5, "En çok alıştan lot", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 6, Delta.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 6, "Alış Satış Farkı", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 7, DeltaMax.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 7, "Alış Satış Farkı En Büyük Olan Kademe Lotu", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 8, DeltaMin.ToString("0,000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 8, "Satış Alış Farkı En Büyük Olan Kademe Lotu", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 9, VWAP.ToString("0.0000"), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 9, "Ağırlıklı Ortalama", Color.White, Color.Red);
Sistem.TabloYazdir(TabloAd, 1, 10, MacKademeFiyati.ToString(), Color.White, Color.Blue);
Sistem.TabloYazdir(TabloAd, 0, 10, "En Çok işlem olan kademenin Fiyatı", Color.White, Color.Red);

KAİRİ - Sistem.Kairi(14)KAIRI isimli indikatörü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.Kairi(14);
Sistem.Kairi(Liste,14);
Sistem.Kairi(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var K = Sistem.Kairi(14);
Sistem.Cizgiler[0].Deger = K;

KAMA - Sistem.KAMA(10,2,30)KAMA isimli indikatörü çağırır. 3 adet parametre (periyot) alır ve varsayılan değerler olarak 10, 2 ve 30 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.KAMA(10, 2 , 30);
Sistem.KAMA(Veriler,10, 2 , 30);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var Kama = Sistem.KAMA(10, 2 , 30);
Sistem.Cizgiler[0].Deger = Kama;

KAR AL PUAN - Sistem.KarAlPuan(3,i)Grafikler üzerinde Al ve Sat sinyalleri üreten bir strateji/sistem formülü yazıldığı zaman, ALIM veya SATIM pozisyonuna girildiği fiyata PUAN (TL) bazında KAR AL eklemek için kullanılan ideal sistem fonksiyonudur. AL yönünde sinyal geldiği zaman, işleme girilen fiyat (aksi belirtilmedikçe al sinyali olan barın kapanış değeridir) üzerine, bu fonksiyonun içine yazılan kadar puan/TL eklenir. Fiyat bu yeni fiyatın (işleme giriş fiyatı + karal puan değeri) üstüne çıktığı zaman Sistem KAR AL eylemi yapar, SATIŞ işlemi yapılarak pozisyon kapatılır ve nakite geçilir. Aynı şekilde Sat (açığa satış veya short) pozisyonundayken de işleme giriş fiyatından karal puanı kadar aşağıdaki fiyat seviyesinin altına inince açığa satış veya short pozisyon ALIŞ işlemi yapılarak kapaılıp KAR AL yapılmış olur. Kullanım şekli aşağıdaki gibidir;csharp

Sistem.KarAlPuan(0.06,i);

(Not: Garan için alış fiyatı 8.22 ise ve 6 kademe yukarıya (yani 8.28 fiyata) kar al yazılacaksa, bu durumda kar al içine yazılacak puan 8.28-8.22 = 0.06 dır)KAR AL YÜZDE - Sistem.KarAlYuzde(3,i)Grafikler üzerinde Al ve Sat sinyalleri üreten bir strateji/sistem formülü yazıldığı zaman, ALIM veya SATIM pozisyonuna girildiği fiyata % bazında KAR AL eklemek için kullanılan ideal sistem fonksiyonudur. AL yönünde sinyal geldiği zaman, işleme girilen fiyatın (aksi belirtilmedikçe al sinyali olan barın kapanış değeridir) bu fonksiyonun içine yazılan kadar yüzde yukarısındaki seviyeye çıktığı zaman Sistem KAR AL eylemi yapar, SATIŞ işlemi yapılarak pozisyon kapatılır ve nakite geçilir. Aynı şekilde Sat (açığa satış veya short) pozisyonundayken de işleme giriş fiyatından karal yüzdesi kadar aşağıdaki fiyat seviyesinin altına inince açığa satış veya short pozisyon ALIŞ işlemi yapılarak kapaılıp KAR AL yapılmış olur. Kullanım şekli aşağıdaki gibidir;csharp

Sistem.KarAlYuzde(2.40,i);

(Not: İşleme girilen fiyatın %2.4 kadar üstünde (satış yönü için altında) fiyat seviyesi görülünce kar alma eylemi gerçekleşir.)Örnek: KAR AL YÜZDE uygulanmış örnek bir sistem formülücsharp

var V = Sistem.GrafikVerileri;
var C = Sistem.GrafikFiyatSec("Kapanis");
var XX = C;
var X1 = Sistem.MA(XX, "Exp", 10);
var X2 = Sistem.MA(XX, "Exp", 50);
var YY = Sistem.RSI(100);
var Y1 = Sistem.MA(YY, "Exp", 10);
var Y2 = Sistem.MA(YY, "Exp", 200);
var ZZ = Sistem.IMI(80);
var Z1 = Sistem.MA(ZZ, "Exp", 10);
var Z2 = Sistem.MA(ZZ, "Exp", 200);
var KARAL = Sistem.Liste(0);
var SonYon = "";
var FlatOncesiYon = "";
var FlatFiyat = 0.0f;
for (int i = 1 ; i < Sistem.BarSayisi; i++)
{
    var IndikatorAlis = X1[i] > X2[i] && Y1[i] > Y2[i] && Z1[i] > Z2[i];
    var IndikatorSatis = X1[i] < X2[i] && Y1[i] < Y2[i] && Z1[i] < Z2[i];
    KARAL[i] = Sistem.KarAlYuzde(3.6,i);
    if (KARAL[i] == 0) KARAL[i] = C[i];
    if (SonYon == "F" && FlatOncesiYon == "A" ) // kar alındıktan sonra tekrar alış trendine girme
    {
        FlatOncesiYon = "";
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (SonYon == "F" && FlatOncesiYon == "S") // kar alındıktan sonra tekrar alış trendine girme
    {
        FlatOncesiYon = "";
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (C[i] >= KARAL[i] && SonYon == "A") // alıştan KAR AL
    {
        FlatOncesiYon = SonYon;
        FlatFiyat = C[i];
        SonYon = "F";
        Sistem.Yon[i] = "F";
    }
    else if (C[i] <= KARAL[i] && SonYon == "S") // satıştan KAR AL
    {
        FlatOncesiYon = SonYon;
        FlatFiyat = C[i];
        SonYon = "F";
        Sistem.Yon[i] = "F";
    }
    else if (IndikatorAlis && SonYon != "A" && FlatOncesiYon != "A") // alış/LONG
    {
        FlatOncesiYon = "";
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (IndikatorSatis && SonYon != "S" && FlatOncesiYon != "S") // satış/SHORT
    {
        FlatOncesiYon = "";
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

Kar Zarar İşlem - Sistem.KarZararIslem(IslemSayisi, KaymaMaliyeti)Grafikler üzerinde Al ve Sat sinyalleri üreten bir strateji/sistem formülü yazıldığı zaman, bu sistemin geçmişten itibaren hesaplanan getiri/performans sonuçlarını kullanan, kullanıcı tarafından girilen kadar işlemin/sinyalin karzarar değerini indikatör olarak hesaplayıp çizen iDeal Sistem fonksiyonudur. Bir sistem yazıp grafiğe uygulayan bir kullanıcı, sisteminin getiri eğrisini alt bölgede indikatör olarak çizdirip, istersen mesela son 10 sinyalin getirisini de ayrı bir indikatör olarak hesaplatıp çizdirebiliriz. Bu son 10 sinyalin getirisini işlemlerden dolayı oluşacak kayma+komisyon maliyetini de düşerek hesaplatma imkanı da vardır. Fonksiyon bu dokumanın yukarıdaki sayfalarında anlatılan GETİRİ HESAPLA fonksiyonu ile birlikte kullanılır. Kullanım şekli aşağıdaki örnekte yer almaktadır.Örnek: RSI, MACD ve MOMENTUM indikatörlerine göre al-sat yapan bir örnek sistem ve o sistemin altında hem tüm sinyaller için hem de sadece son 5 işlemin/sinyalin getiri eğrisini çizen formül:csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var RSI = Sistem.RSI(C,14);
var MAV = Sistem.MA(C, "Simple", 600);
var M = Sistem.MACD(12, 26);
var SonYon = "";
for (int i = 4; i < Sistem.BarSayisi - 3; i++)
{
    if ( RSI[i] > 70f && M[i] > 0.10f && C[i] > MAV[i]-0.500f && SonYon != "A") // AL
    {
        Sistem.Yon[i] = "A";
        SonYon = Sistem.Yon[i];
    }
    else if (RSI[i] < 60f && M[i] < -0.10f && C[i] < MAV[i]+0.500f && SonYon != "S") // SAT
    {
        Sistem.Yon[i] = "S";
        SonYon = Sistem.Yon[i];
    }
}
Sistem.GetiriHesapla("01/01/2015",0.0);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZ; //Panel 2
var KarZararIslemSayisiBazinda = Sistem.KarZararIslem(5, 0.0);
Sistem.Cizgiler[1].Deger = KarZararIslemSayisiBazinda ; //panel3

