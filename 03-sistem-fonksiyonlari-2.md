markdown

# Sistem Fonksiyonları (Devam)

### Chaikin Money Flow (CMF) - Sistem.ChaikinMoneyFlow(14)

Chaikin Money Flow (CMF) olarak bilinen indikatörünü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Hesaplamasında HACİM verisi kullandığından En Az DÜZEY1+ lisans gerektirir. Aksi durumda hacim içeren barlar gecikmeli olarak kendini günceller. Aşağıdaki gibi 2 kullanım şekli vardır;

```csharp
Sistem.ChaikinMoneyFlow(14);
Sistem.ChaikinMoneyFlow(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var CMF = Sistem.ChaikinMoneyFlow(14);
Sistem.Cizgiler[0].Deger = CMF;

Chaikin Osc - Sistem.ChaikinOsc(14)Chaikin Oscilator olarak bilinen indikatörünü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.ChaikinMoneyOsc(14);
Sistem.ChaikinMoneyOsc(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var CM = Sistem.ChaikinMoneyOsc(14);
Sistem.Cizgiler[0].Deger = CM;

Chaikin Volatility - Sistem.ChaikinVolatility(10,10)Chaikin Volatility olarak bilinen indikatörünü çağırır. 2 adet parametre (periyot) alır ve varsayılan değer olarak 10-10 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.ChaikinVolatility(10, 10);
Sistem.ChaikinVolatility(Veriler,10,10);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var CV = Sistem.ChaikinVolatility(10,10);
Sistem.Cizgiler[0].Deger = CV;

Chande Momentum - Sistem.ChandeMomentum(20)Chande Momentum olarak bilinen indikatörünü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 20 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.ChandeMomentum(20);
Sistem.ChandeMomentum(Liste,20);
Sistem.ChandeMomentum(Veriler,20);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var Cha = Sistem.ChandeMomentum(20);
Sistem.Cizgiler[0].Deger = Cha;

CizgiCiz - Sistem.CizgiCiz(panel, bar1, fiyat1, bar2, fiyat2, renk, kalinlik, stil)Grafiklerde kullanıcı tarafından KOD YAZILARAK veya MOUSE İLE KLİKLENEREK tespit edile iki nokta arasına ÇİZGİ ÇİZDİRMEK için kullanılır. Kullanım alanları oldukça geniştir. Otomatik yükselen düşen trendler, destek ve direnç noktaları, pivot seviyeleri, kullanıcı açısından koşullandırılmış iki dönem arasını işaretleme gibi çok çeşitli amaçlar için kullanılabilir.Aşağıdaki şekilde yazılır;csharp

Sistem.CizgiCiz(panel, bar1, fiyat1, bar2, fiyat2, renk, kalinlik, stil)

Panel = Çizgi barların olduğu bölgede çizilecekse 1 değeri verilmelidir. Aşağıdaki indikatör çizim bölgelerinde çizdirilecekse 2 veya diğer panel numaraları kullanılır.
Bar1 ve Bar2 bilgileri çizginin başlayacağı ve biteceği barların numarasıdır. Elle sabit bar nosu da girilebilir. (500 ve 1500 gibi)
Fiyat1 ve Fiyat2 çizgilerin başladığı ve bittiği barların HANGİ NOKTASINDAN çizilmelidir (barın en tepesinden, dibinden, ortasından veya hesaplatılan bir fiyat seviyesinden)
Renk = Color.Red gibi noktadan sonra İngilizce renk ismi yazılarak çizgi rengi atanabilir.
Kalinlik = 1,2,3,4,5 değerleri verilebilir
Stil değerine 1-2-3-4-5 değerleri verilerek DÜZ, KESİK, NOKTA, DİKEY ve YASSI tipte çizgi çizdirtilebilir.

Örnek Kullanım: Mouse ile tıklanan yerden başla, devam eden 1200 bar sonrasına kadarki bölgenin EN YÜKSEK ve EN DÜŞÜK seviyesine ve ikisinin tam orta seviyesine yatay çizgi çiz
Not: Mouse ile tıklanan bölgeyi algılayıp formül içinde kullanma kısmı için SELECTBARNO komutu açıklamasını inceleyiniz.csharp

var V = Sistem.GrafikVerileri;
int basla = Sistem.SelectBarNo; //mouse ile kliklenen bardan itibaren çizmeye başla
var bitis = Sistem.SelectBarNo + 1200; //kliklenen yerden sonra 1200 bar içindeki bölgeyi çiz
if (bitis > Sistem.BarSayisi-1) bitis = Sistem.BarSayisi-1; //1200 bar yoksa son bara kadar çiz
float yuksek = V[basla].High;
float dusuk = V[basla].Low;
for (int i=basla; i <= bitis; i++)
{
    yuksek = Math.Max(yuksek,V[i].High);
    dusuk = Math.Min(dusuk,V[i].Low);
}
float pivot = (yuksek + dusuk)/2;
Sistem.CizgiCiz(1, basla, yuksek, bitis, yuksek, Color.Green, 3, 1);
Sistem.CizgiCiz(1, basla, dusuk, bitis, dusuk, Color.Red, 3, 3);
Sistem.CizgiCiz(1, basla, pivot, bitis, pivot, Color.White, 3, 2);

Cizgiler - Sistem.Cizgiler[x].DegerIDEAL sistem modülünde yazılan bir formülde, grafik pencereleri üzerine bir çizgi çizilmesi gerektiğinde Sistem.Cizgiler fonksiyonu kullanılır. Kendi özel indikatörünüz, bir veri listesi, sabit bir değer listesi veya bir indikatör bu fonksiyonla ekrana çizdirilir.Bir çizgi demek, zaman birimlerinde bir değere sahip olan bir veri listesi demektir. O yüzden tek bir sabit değere bir yatay çizgi bile çizdirilmek istense, tüm elemanları o sabit değer olan bir liste tanımlamak gerekir.IDEAL sistem modülü en fazla 50 adet çizgiye izin vermektedir. Gelişmiş sistem paneli (kodlamanın yapıldığı ana pencerenin sağ üst kısmı), çizgilerin pek çok özelliğine kod yazmadan müdahale imkânı sunmaktadır.Çizginin alacağı değerlerin ne olacağı dışındaki tüm özellikler, kod yazmaksızın panel üzerinden seçimlerle belirlenebilir.Bir çizgi için aşağıdaki özellikler bulunmaktadır:Deger (zorunlu)
Aciklama
ActiveBool (koddan yazılmazsa, x numaralı çizginin panelden AKTİF edilmesi zorunlu)
Stil (aksi belirtilmezse DÜZ çizgi stili kullanılır)
Kalınlık (Aksi belirtilmezse kalınlık 1 olarak kabul edilir)
Renk (Panelde varsayılan olarak her çizginin tarafımızdan atanmış bir rengi vardır. Kullanıcı istediği rengi seçebilir)
Panel (Grafiğin kendisinin panel numarası SIFIRDIR. Grafiğin alt kısmına (indikatör bölgesine) atılan her alan yeni bir paneldir. Grafiğin hemen altındaki ilk bölgenin panel numarası “1” dir ve maksimum panel sayısı 10’dur.

*** Her ÇİZGİ bir numaraya sahiptir ve tanımlanırken çizginin numarası mutlaka (köşeli parantez içinde) belirtilmelidir. İLK ÇİZGİ NUMARASI SIFIRDIR.*** Cizgi fonksiyonu kullanılırken Sistem.Cizgiler[CizgiNumarası].OZELLİK şeklinde yazılmalıdır. Özellik olarak yukarıda listelenen ifadeler kullanılabilir.*** KOD yazarken Sistem.Cizgiler[0].Deger satırını yazmak yeterli ve ZORUNLUDUR. (aktif etme, panel numarası, renk, stil, kalınlık PANEL üzerinden seçilebilir.Örnek Kullanım: Alt indikatör paneline (Panel No = 2) RSI indikatörünü ve aynı zamanda 30 ve 70 seviyelerine yatay çizgileri Kod yazarak ve tüm çizgi özelliklerini koddan atama yaparak çizdirmek.csharp

var RSI = Sistem.RSI(14);
Sistem.Cizgiler[0].Deger = RSI;
Sistem.Cizgiler[0].Aciklama = "RSI";
Sistem.Cizgiler[0].ActiveBool = true;
Sistem.Cizgiler[0].Panel = 2;
Sistem.Cizgiler[0].Renk = Color.Gold;
Sistem.Cizgiler[0].Stil = 1; //1:Düz, 2:Nokta, 3:Kesik, 4:Yassı
Sistem.Cizgiler[0].Kalinlik = 2;

Sistem.Cizgiler[1].Deger = Sistem.Liste(70);
Sistem.Cizgiler[1].ActiveBool = true;
Sistem.Cizgiler[1].Stil = 3; //1:Düz, 2:Nokta, 3:Kesik, 4:Yassı
Sistem.Cizgiler[1].Kalinlik = 2;
Sistem.Cizgiler[1].Panel = 2;
Sistem.Cizgiler[1].Renk = Color.White;
Sistem.Cizgiler[1].Aciklama = "ÜstSeviye";

Sistem.Cizgiler[2].Deger = Sistem.Liste(30);
Sistem.Cizgiler[2].ActiveBool = true;
Sistem.Cizgiler[2].Panel = 2;
Sistem.Cizgiler[2].Renk = Color.Lime;
Sistem.Cizgiler[2].Stil = 3; //1:Düz, 2:Nokta, 3:Kesik, 4:Yassı
Sistem.Cizgiler[2].Kalinlik = 2;
Sistem.Cizgiler[2].Aciklama = "AltSeviye";

Yukarıdaki formül kaydedildiği zaman, kod yazma panelinin sağ üst kısmındaki yazı/renk/tip seçimleri aşağıdaki gibi olur.NOT: Özellikle mail yoluyla iDeal Data Teknik ekibinden formül desteği alındığında, kullanıcılarımızın en çok yaptığı hata çizgileri aktif etmemektir.
Maille gelen bir formül kaydedilip grafiğe uyguladığında, ekrana bir çizgi çizilmesi gerekiyorsa (formül içinde genelde de en altta Sistem Çizgiler satırı varsa) ilgili çizgi numaralarının üstteki fotoda gösterilen panelde, başlarındaki kutucuların işaretlenerek aktif edilmesi, çizgi grafiğin alt kısmına çizilmesi gerekiyorsa panel numarasının 2 (veya 3-4-5) olarak kaydedilmesi gerekir.CCI – Commodity Channel Index - Sistem.CommodityChannelIndex(14)CCI olarak da bilinen Commodity Channel Index indikatörünü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.CommodityChannelIndex(14);
Sistem.CommodityChannelIndex(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var CCI = Sistem.CommodityChannelIndex(14);
Sistem.Cizgiler[0].Deger = CCI;

Debug - Sistem.Debug()IDEAL Sistem modülünde bir formül yazılırken, kodun herhangi bir noktasında DEBUG fonksiyonu kullanılarak kodun o noktasında bir sorun olup olmadığı, kodda bir sorun varsa, sorunun ne olduğu daha rahat tespit edilebilir.Debug fonksiyonu, IDEAL Sistem panelinin sağ üst bölgesindeki çizgilere ait tercih bölgesinin üzerini kaplayan bir boş pencere açar ve orada, debug edilen satırdaki bulgular gösterilir.Sistem.Debug() fonksiyonu çalışma mantığı bakımından MESAJ fonksiyonuna çok benzer. Tek farkları, ekrana bir mesaj kutusu çıkarılması yerine, sonuç değerlerinin, panelde gösterilmesidir.Bu özelliği nedeniyle, hata bulmak ve değişebilen bir sonucu gözlemlemek amacıyla mesajdan daha yararlıdır.Kod içinde birden fazla mesaj fonksiyonu varsa, satır olarak en aşağıda olan mesaj fonksiyonunun sonucu ekranda görülür.
Ama birden fazla satıra debug konulmuşsa, debug ekranında bunları hepsi alt alta aynı anda gösterilir.
(kodda en altta yer alan debug satırının sonuç değeri, debug panelinde ilk sırada gösterilir.)Debug butonuna basılarak DEBUG alanı aktif edilir. Daha sonra FORMÜL TEST butonuna basılınca, Tüm debug satırlarının sonuçları panelde gösterilir.Örnek: Aşağıdaki kod yazılıp kaydedilir ve FORMÜL TEST butonuna basılırsa, Panelin üst kısmındaki Debug penceresinde izlemek istediğimiz bilgileri görebiliriz;csharp

var Sembol = "VIP'F_XU0300620";
var Veriler = Sistem.GrafikVerileriniOku(Sembol, "20");
Sistem.Debug("SON BARIN KAPANIS FIYATI = " + Veriler[Veriler.Count-1].Close.ToString());
Sistem.Debug("537. BARIN TARİH VE SAATİ = " + Veriler[537].Date.ToString());
Sistem.Debug("450.BARIN SAAT BİLGİSİ = " + Veriler[450].Date.Hour.ToString());
Sistem.Debug("120.BARIN DAKİKA BİLGİSİ = " + Veriler[120].Date.Minute.ToString());

DEMA – Sistem.DEMA(5)DEMA indikatörünü çağırır. 1 adet parametre (periyot) alır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.DEMA(5);
Sistem.DEMA(Liste,5);
Sistem.DEMA(Veriler,5);

Örnek: DEMA5 değeri DEMA 21 değerisini aşağı keserse SAT, yukarı keserse AL sinyali üreten strateji kod örneği
Not: KESİŞME TARA fonksiyonu detayı için ilgili fonksiyon detayını inceleyiniz.csharp

var DEMA1 = Sistem.DEMA(5);
var DEMA2 = Sistem.DEMA(21);
Sistem.KesismeTara(DEMA1, DEMA2);
Sistem.Cizgiler[0].Deger = DEMA1;
Sistem.Cizgiler[1].Deger = DEMA2;

Demand Index - Sistem.DemandIndex()Demand Index olarak bilinen indikatörünü çağırır. Hiç parametre almaz ve Hesaplamasında HACİM verisi kullandığından En Az DÜZEY1+ lisans gerektirir. Aksi durumda hacim içeren barlar gecikmeli olarak kendini günceller. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.DemandIndex();
Sistem.DemandIndex(Veriler);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var Demand = Sistem.DemandIndex();
Sistem.Cizgiler[0].Deger = Demand;

De Marker - Sistem.DeMarker(13)De Marker olarak bilinen indikatörünü çağırır. 1 adet parametre alır ve varsayılan parametre olarak 13 kullanılır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.DeMarker(13);
Sistem.DeMarker(Veriler,13);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var Marker = Sistem.DeMarker(13);
Sistem.Cizgiler[0].Deger = Marker;

Derinlik Verileri - Sistem.DerinlikVerisiOku(Sembol)Bir Hisse Senedinin, Varantın, Vadeli Kontratın ve Opsiyonun (lisansınız var ise) DERİNLİK penceresindeki tüm verilerini okumak için kullanılır. Okunan veri bir dizi şeklindedir. Derinlik penceresinin ALIŞ ve SATIŞ tarafı ayrı ayrı ve SATIR SATIR okunup hafızaya alınması ve okunan bu verilerin formüllerde kullanılması mümkündür.Bu fonksiyonla okutulan Veri Listesinin her bir elemanı şu şekilde çağırılır;
VeriListesi.Bids/Asks[DerinlikSatırNo].ElemanÖrnek1: VIOP Aktif kontratının Derinlik penceresindeki ilk satıra (hem alış hem satış tarafı) ait bazı verileri ekrana mesaj olarak almakcsharp

var Sembol = "VIP'VIP-X030";
var Derinlik = Sistem.DerinlikVerisiOku(Sembol);
var AlisFiyatKademe0 = Derinlik.Bids[0].Price;
var AlisLotKademe0 = Derinlik.Bids[0].Size;
var AlisEmirSayisi = Derinlik.Bids[0].OrderCount;
var SatisFiyatKademe0 = Derinlik.Asks[0].Price;
var SatisLotKademe0 = Derinlik.Asks[0].Size;
var SatisEmirSayisi = Derinlik.Asks[0].OrderCount;
var SatisEmirSaati = Derinlik.Asks[0].Time;
Sistem.Mesaj("Alış Fiyatı =" + AlisFiyatKademe0.ToString() + "\r\n" + "Alış Lot = " + AlisLotKademe0.ToString() + "\r\n" + "Alış Emir = " + AlisEmirSayisi.ToString() + "\r\n" + "Satış Emir Sayısı = " + SatisEmirSayisi.ToString());

Bir sembolün derinlik penceresine ait veriler aşağıdaki şekilde erişilebilir durumdadır.NOT: DERİNLİK pencereleri altındaki TOPLAM satırındaki verileri okutmak için, derinlik verisi oku dedikten sonra Derinlik.CalculateAverage2(); satırını da yazmak gerekir. Sonra ilgili bilgiye erişilebilir.Örnek2: DOAS hisse senedine ait derinlik penceresindeki bazı verileri okuyup ekrana MESAJ olarak gösteren kod örneği;csharp

var Sembol = "IMKBH'DOAS";
var Derinlik = Sistem.DerinlikVerisiOku(Sembol);
var AlisFiyatKademe0 = Derinlik.Bids[0].Price;
var AlisLotKademe0 = Derinlik.Bids[0].Size;
var AlisEmirSayisi = Derinlik.Bids[0].OrderCount;
var SatisFiyatKademe0 = Derinlik.Asks[0].Price;
var SatisLotKademe0 = Derinlik.Asks[0].Size;
var SatisEmirSayisi = Derinlik.Asks[0].OrderCount;
var SatisEmirSaati = Derinlik.Asks[0].Time;

// NOT: Derinlik penceresinin en altındaki toplam satırlarında gözüken verilere erişilecekse:
Derinlik.CalculateAverage2();
var Bid = Derinlik.Average2Bid.Size;

Sistem.Mesaj("Alış Fiyatı =" + AlisFiyatKademe0.ToString() +
"\r\n" + "Alış Lot = " + AlisLotKademe0.ToString("0") +
"\r\n" + "Satış Fiyatı = " + SatisFiyatKademe0.ToString()+
"\r\n" + "Satış Lot = " + SatisLotKademe0.ToString("0")+
"\r\n" + "Saat = " + SatisEmirSaati.ToString()+
"\r\n" + "Toplam Lot = " + Derinlik.Average2Bid.Size.ToString());

Örnek3: Alışta bekleyen lot sayısı, satışta bekleyen lot sayısının 2 katından fazla olan ve aynı zamanda son fiyatı 50’lik ortalamasının üstünde olan senetleri tarayan kod örneği (sorgu):csharp

var Derinlik = Sistem.DerinlikVerisiOku(Sistem.Sembol);
var C = Sistem.GrafikFiyatSec("Kapanis");
var Mov = Sistem.MA(C, "Exp" , 50);
var Alistoplam = 0;
var Satistoplam = 0;
for (int i = 0; i < 6; i++)
{
    Alistoplam += Derinlik.Bids[i].Size;
    Satistoplam += Derinlik.Asks[i].Size;
}
Sistem.SorguBaslik[0] = "Alış Lot";
Sistem.SorguOndalik[0] = 0;
Sistem.SorguBaslik[1] = "Satış Lot";
Sistem.SorguOndalik[1] = 0;
Sistem.SorguBaslik[2] = "Kapanış";
Sistem.SorguBaslik[3] = "Mov";

var sonbar = Sistem.BarSayisi-1;
if (Alistoplam > 2 * Satistoplam && C[sonbar] > Mov[sonbar] )
{
    Sistem.SorguDeger[0] = Alistoplam;
    Sistem.SorguDeger[1] = Satistoplam ;
    Sistem.SorguDeger[2] = C[sonbar];
    Sistem.SorguDeger[3] = Mov[sonbar] ;
    Sistem.SorguAciklama = "Filtrem";
    Sistem.SorguEkle();
}

De Trended Price Oscilator - Sistem.DetrendedPriceOscillator(14)De Trended Price Oscillator indikatörünü çağırır. 1 adet parametre (periyot) alır ve varsayılan değer olarak 14 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.DetrendedPriceOscillator(14);
Sistem.DetrendedPriceOscillator(Liste,14);
Sistem.DetrendedPriceOscillator(Veriler,14);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var DT = Sistem.DetrendedPriceOscillator(14);
Sistem.Cizgiler[0].Deger = DT;

Devre Kesici Listesini Oku - Sistem.DevreKesiciListesiniOku()Bir hisse senedinde borsa sisteminde devre kesici durumu oluştuğu zaman, toplu olarak tüm hisselerdeki durumları da tespit ederek listenin tümünü okumak için kullanılır.Formül alanına aşağıdaki komut yazıldığında DEVRE KESİCİ LİSTESİ okunup belleğe alınmış olur;csharp

Sistem.DevreKesiciListesiniOku()

Daha sonra ise bu listenin içindeki bilgi alanlarına erişilebilir.
Listeyi çağırdığınızda aşağıdaki bilgileri içeren bir liste gelir.csharp

string Sembol = "";
string Saat = "";
string StateCode = "";

State Code değeri 55 olan hisseler devre kesici dönemine girmiştir.AYRICA YÜZEYSEL VERİ LİSTESİNİ OKUTARAK BİR TEK HİSSE SENEDİ İÇİN devre kesici durumunun ne olduğu bilgisine erişilebilir.
Not: Yüzeysel Veri Oku Fonksiyonu detayları için ilgili komutun açıklama sayfasını inceleyiniz.Aşağıdaki örnek kod ARTI hisse senedinin borsadaki o anki durum kodunu verir. Durum kodu 2 olanlar sürekli müzayede seansında olanları, 55 olanlar devre kesmiş olanları verir. Tüm durum kodlarının ne olduğu bilgisi için gerek duyulursa ideal algo ekibine (algo@idealdata.com.tr) başvurulabilir.csharp

var Sembol = "IMKBH'ARTI";
var V = Sistem.YuzeyselVeriOku(Sembol);
var Durum = V.Durum;

Dikey Çizgi Ekle - Sistem.DikeyCizgiEkle(BarNo, Color.Yellow, 2, 1)Grafiklerde kullanıcı tarafından istenilen herhangi bir bara DİKEY ÇİZGİ çizdirilmesi imkanı verir.Kullanımda yazım şekli aşağıdaki gibidir;csharp

Sistem.DikeyCizgiEkle(i, Color.Green, 2, 2);

Burada çizginin rengi, kaç numaralı BARA çizginin çizileceği, çizginin kalınlığı ve tipi (düz/kesik) belirtilir.Örnek: RSI indikatörünün 30 seviyesini aşağıdan yukarı kestiği barlara YEŞİL dikey çizgi ekleyen, RSI indikatörünün 70 seviyesini yukarıdan aşağıya kestiği barlara KIRMIZI dikey çizgi ekleyen kod örneği;csharp

var RSI = Sistem.RSI(14);
for(int i=1 ; i< Sistem.BarSayisi; i++)
{
    if(RSI[i-1] < 30 && RSI[i] >= 30)
        Sistem.DikeyCizgiEkle(i, Color.Green, 2, 2);
    if(RSI[i-1] > 70 && RSI[i] <= 70)
        Sistem.DikeyCizgiEkle(i, Color.Red, 2, 2);
}

Dip - Sistem.Dip(Barsayısı)Belli bir bar sayısı kadar bölgenin DİP seviyesini gösteren DİP indikatörünü hesaplayan fonksiyondur. Girilen bar sayısı kadar öncesi ve sonrası bölgenin en dip noktasını verir.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.Dip(100);

Bkz: Sistem.Zirve(100) aynı zamanda.Örnek1: 50 periyotluk Dip/Zirve indikatörü çizdirme koducsharp

var Dip = Sistem.Dip(50);
var Zirve = Sistem.Zirve(50);
Sistem.Cizgiler[0].Deger = Dip;
Sistem.Cizgiler[1].Deger = Zirve;

DI+ / DI- (PDI/MDI) - Sistem.DirectionalIndicatorMinus/Plus(14)DI+ ve DI- olarak iki çizgisi olan veya Plus(+) olanın PDI, Minus(-) olanın MDI olarak da adlandırıldığı Directional Indicator indikatörünü çağıran fonksiyondur. DI+ için bir ve DI- için bir olmak üzere iki ayrı komutu vardır. Kullanım şekilleri aşağıdaki gibidir;csharp

Sistem.DirectionalIndicatorMinus(14);
Sistem.DirectionalIndicatorMinus(Veriler,14);
Sistem.DirectionalIndicatorPlus(14);
Sistem.DirectionalIndicatorPlus(Veriler,14);

Örnek1: 14 periyotluk DI+/DI- indikatörü çizdirme koducsharp

var MDI = Sistem.DirectionalIndicatorMinus(14);
var PDI = Sistem.DirectionalIndicatorPlus(14);
Sistem.Cizgiler[0].Deger = MDI;
Sistem.Cizgiler[1].Deger = PDI;

Directional Movement - Sistem.DirectionalMovement(14)Directional Movement indikatörünü çağıran fonksiyondur. Tek bir parametre alır ve varsayılan parametre değeri 14’tür. Kullanım şekilleri aşağıdaki gibidir;csharp

Sistem.DirectionalMovement(14);
Sistem.DirectionalMovement(Veriler,14);

Örnek1: 14 periyotluk Directional Movement indikatörü çizdirme koducsharp

var DM = Sistem.DirectionalMovement(14);
Sistem.Cizgiler[0].Deger = DM;

Dolgu Ekle - Sistem.DolguEkle()Bir grafik üzerinde çizdirilen 2 çizgi arasında, bu çizgilerin birbirlerini kestikleri bölgeleri seçilen renklerle boyama amaçlı kullanılır.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.DolguEkle(No1, No2, YukselisRenk, DususRenk);

Burada No1, dolgu ile boyanması istenen alanı oluşturacak birinci çizgi numarası, No2 ise söz konusu boyalı alanı oluşturacak diğer çizginin numarasıdır. YukselisRenk bilgisi girilen iki çizgiden birincisi ikincisinin üstünde olduğu bölgenin rengini, DususRenk ise birinci çizginin ikinci çizginin altında kaldığı bölgenin boyanacağı bölge rengini belirtir.Renkler belirtilirken C# dilinin renk tanımlayan kod yazım şekilleri (R-G-B oranları vererek renk atamak vs.) geçerlidir ama en kolayı Color.Red, Color.Blue şeklinde, Color.IngilizceRenkAdı şeklinde tanımlamaktır.Örnek: MACD indikatörü ile SIFIR seviyesindeki çizgi arasında kalan bölgeleri, MACD üstteyken yeşil, MACD alttayken kırmızı renk ile dolgu yapan kod:csharp

var MACD = Sistem.MACD(12,26);
Sistem.Cizgiler[0].Deger = MACD;
Sistem.Cizgiler[1].Deger = Sistem.Liste(0);
Sistem.DolguEkle(0, 1, Color.Green, Color.Red);

Dönem Çevir - Sistem.DonemCevir()Bir grafikte çalışma yapılan periyottan daha üst bir zaman dilimine ait barlara ait bilgileri, bu üst dönem için çizilen indikatörleri alt periyotlarda kullanmak veya çizdirmek için kullanılır.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.DonemCevir(Veriler, UstDonemVerileri, UstDonemCevrilecekData);

Veriler yazan yere, çalışılan periyot ne ise, o periyoda ait BAR VERİLERİ yazılır. Eğer formülde bu veriler bir değişkene atanmışsa o değişken ismi yazılır. Üst zaman diliminden alınan bilgileri değişik alt zaman periyotlarında kullanacaksak (alt dönem sabit bir periyot değilse) o zaman Veriler yazan kısma Sistem.GrafikVerileri yazılır. Bu komut, formül hangi zaman periyoduna uygulanmışsa, o zaman diliminin BAR VERİLERİNİ (OHLC tamamını birden) verir.
UstDonemVerileri yazan yere, daha üst zaman dilimi olarak hangi grafik periyodu kullanılacaksa o zaman diliminin BAR VERİLERİ yazılır. Örnek: var GUNVERILER = Sistem.GrafikVerileriniOku(Sembol, “G”) diyerek GÜNLÜK BAR VERİSİ çağrılır.
UstDonemCevrilecekData yazan yere, üst zaman diliminden hesaplanmış ve alt dönemde kullanılacak veya çizdirilecek veri (Fiyat veya indikatör) hangisi ise o yazılır.

Örnek: Günlük, 240 dakikalık, 60 dakikalık ve 30 dakikalık periyotların 50’lik Movlarını 10 dklık grafik üzerine çizdirmek;csharp

//üst periyot verilerini oku
var V_GUN = Sistem.GrafikVerileriniOku(Sistem.Sembol,"G");
var V_240 = Sistem.GrafikVerileriniOku(Sistem.Sembol,"240");
var V_60 = Sistem.GrafikVerileriniOku(Sistem.Sembol,"60");
var V_30 = Sistem.GrafikVerileriniOku(Sistem.Sembol,"30");
var CGun = Sistem.GrafikFiyatOku(V_GUN,"Kapanis");
var C240 = Sistem.GrafikFiyatOku(V_240,"Kapanis");
var C60 = Sistem.GrafikFiyatOku(V_60,"Kapanis");
var C30 = Sistem.GrafikFiyatOku(V_30,"Kapanis");
var MovGUN = Sistem.MA(CGun, "Simple", 50);
var Mov240 = Sistem.MA(C240, "Simple", 50);
var Mov60 = Sistem.MA(C60, "Simple", 50);
var Mov30 = Sistem.MA(C30, "Simple", 50);

//Verilerin dönemini çevir
MovGUN = Sistem.DonemCevir(Sistem.GrafikVerileri, V_GUN, MovGUN);
Mov240 = Sistem.DonemCevir(Sistem.GrafikVerileri, V_240 , Mov240);
Mov60 = Sistem.DonemCevir(Sistem.GrafikVerileri, V_60 , Mov60);
Mov30 = Sistem.DonemCevir(Sistem.GrafikVerileri, V_30 , Mov30);

//ekranda çizgi olarak göster
Sistem.Cizgiler[0].Deger = MovGUN;
Sistem.Cizgiler[0].Aciklama = "MovGUN" ;
Sistem.Cizgiler[1].Deger = Mov60;
Sistem.Cizgiler[1].Aciklama = "Mov60 Dk" ;
Sistem.Cizgiler[2].Deger = Mov30;
Sistem.Cizgiler[2].Aciklama = "Mov30 Dk" ;
Sistem.Cizgiler[3].Deger = Mov240 ;
Sistem.Cizgiler[3].Aciklama = "Mov240 Dk" ;

Dörtgen Çiz - Sistem.DortgenCiz()Grafik üzerinde kullanıcı tarafından girilen 4 noktayı (4 bar numarası ve 4 fiyat seviyesi) birleştirerek dörtgen çizme amaçlı kullanılır. Özellikle patern/Flama/Formasyon bulma amaçlı kodlar yazan kullanıcılarımız, formasyonu oluşturan barları ve fiyat seviyeleri koddan tespit edip formasyonun olduğunu otomatik olarak dörtgen veya kelebek şeklinde çizdirtebilirler.Kullanım şekli aşağıdaki gibidir;csharp

Sistem.DortgenCiz(panel, bar1, fiyat1, bar2, fiyat2, bar3, fiyat3, bar4, fiyat4, renk, kalinlik, dolgu, dolgurenk, nokta, noktarenk);

Parantez içindeki giriş bilgileri açıklamaları aşağıdaki gibidir;Panel: Dörtgenin grafiğin hangi paneline çizileceği girilir (barların olduğu bölge=1, alt indikatör bölgesi = 2)
Bar1: Dörtgenin birinci köşesinin denk geldiği bar numarası
Fiyat1: Dörtgenin birinci köşesinin fiyat skalasına denk gelen seviyesi
Bar2: Dörtgenin ikinci köşesinin denk geldiği bar numarası
Fiyat2: Dörtgenin ikinci köşesinin fiyat skalasına denk gelen seviyesi
Bar3: Dörtgenin üçüncü köşesinin denk geldiği bar numarası
Fiyat3: Dörtgenin üçüncü köşesinin fiyat skalasına denk gelen seviyesi
Bar4: Dörtgenin dördüncü köşesinin denk geldiği bar numarası
Fiyat4: Dörtgenin dördüncü köşesinin fiyat skalasına denk gelen seviyesi
Renk: Dörtgenin dış çizgi rengi
Kalınlık: Dörtgenin dış çizgisinin kalınlık seviyesi
Dolgu: Dörtgenin içinin dolgu renkli olması isteniyorsa 1, istenmiyorsa 0 yazılır
Dolgurenk: dolgu=1 denmişse iç dolgunun rengi verilir
Nokta: Değer 1 yapılırsa dörtgenin köşelerinde birer nokta işareti çizilir
Noktarenk: Köşelerde çıkan noktaların rengi belirlenir.

Örnek: Köşelerin denk geldiği bar numaraları ve fiyat seviyeleri elle girilerek bir dörtgen çizdirmek;csharp

int SonBar=Sistem.BarSayisi;
Sistem.DortgenCiz(1, SonBar-1850, 115.90, SonBar-1180, 151.65, SonBar-200, 120.4, SonBar-570, 98.5,
Color.Red, 3, 1, Color.Purple,1,Color.White);

Düşük (Dusuk) Fiyat - Sistem.Dusuk(Sembol)Bir sembolün o an ki fiyata göre çeşitli dönemlerde gördüğü EN DÜŞÜK FİYATI okumak için kullanılır. Fonksiyonun içine Düşük fiyatı okutulmak istenen sembol yazılır.Not: iDeal programında bütün semboller ait oldukları piyasasının kodu ile birlikte yazılırlar. Hisse senetlerinin piyasa kodu IMKBH dır. PİYASA kodundan sonra ÜSTTEN TEK TIRNAK işareti ile ayrılıp borsadaki orijinal kod eklenir. GARAN hissesinin idealdeki sembol tanımı IMKBH’GARAN şeklindedir. Örneğin USDTYR için FX’USDTRY şeklinde yazılır. Bir sembolün PİYASA kodunun ne olduğu, o sembolü sayfanıza yazarken @ işareti yanında gösterilir.)Kullanım şekilleri aşağıdaki gibidir.csharp

Sistem.DusukAltiAy(Sembol);
Sistem.DusukBirAy(Sembol);
Sistem.DusukBirHafta(Sembol);
Sistem.DusukBirYil(Sembol);
Sistem.DusukBuAy(Sembol);
Sistem.DusukBuHafta(Sembol);
Sistem.DusukBuYil(Sembol);
Sistem.DusukGun(Sembol);
Sistem.DusukSeans(Sembol);
Sistem.DusukUcAy(Sembol);

Örnek: GARAN hisse senedinin, bu doküman yazılırken ki son 6 ayın en düşük değerinin okuyup ekrana mesaj olarak çıkaran kod:csharp

var AltiAyDusuk = Sistem.DusukAltiAy(Sembol);
Sistem.Mesaj("6 Aylık en düşük Fiyat = " + AltiAyDusuk.ToString());

