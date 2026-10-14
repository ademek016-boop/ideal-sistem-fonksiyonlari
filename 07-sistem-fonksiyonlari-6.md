markdown

# Sistem Fonksiyonları (Devam - 7. Bölüm)

### Non Linear Ehler’s Filter - Sistem.NonlinearEhlersFilter(15)

Non Lineer Ehler’s Filter olarak bilinen indikatörü çağırır. 1 Adet parametre alır (varsayılan 15). Aşağıdaki gibi 2 yazım şekli vardır;

```csharp
Sistem.NonlinearEhlersFilter(15);
Sistem.NonlinearEhlersFilter(Veriler,15);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var EF = Sistem.NonlinearEhlersFilter(15);
Sistem.Cizgiler[0].Deger = EF;

OBV – OnBalance Volume - Sistem.OnBalanceVolume()OBV veya On Balance Volume olarak bilinen indikatörü çağırır. İndikatör parametre almaz. Aşağıdaki gibi 2 yazım şekli vardır;NOT: Bu indikatörün anlık olarak hesaplanması için Anlık olarak hacim bilgisi görme yetkisi gerekir. Bu da Borsa İstanbul Piyasaları için Düzey1 Plus (1 kademe derinlik) olarak bilinen yetkidir.csharp

Sistem.OnBalanceVolume();
Sistem.OnBalanceVolume(Veriler);

Örnek: İndikatörü ve ortalamasını çağırıp çizdirmekcsharp

var OBV = Sistem.OnBalanceVolume();
var ORT = Sistem.MA(OBV, “Exp”, 10);
Sistem.Cizgiler[0].Deger = OBV;
Sistem.Cizgiler[1].Deger = ORT;

Önceki Kapanış - Sistem.OncekiKapanis…(Sembol)Bir sembolün önceki zaman dilimlerine göre istenen kapanış fiyatını okumak için kullanılır. Fonksiyonun içine Önceki Kapanış fiyatı okutulmak istenen sembol yazılır.Kullanım şekilleri aşağıdaki gibidir.csharp

Sistem.OncekiKapanisAltiAy(Sembol);
Sistem.OncekiKapanisBirAy(Sembol);
Sistem.OncekiKapanisBirHafta(Sembol);
Sistem.OncekiKapanisBirYil(Sembol);
Sistem.OncekiKapanisBuAy(Sembol);
Sistem.OncekiKapanisBuHafta(Sembol);
Sistem.OncekiKapanisBuYil(Sembol);
Sistem.OncekiKapanisGun(Sembol);
Sistem.OncekiKapanisSeans(Sembol);
Sistem.OncekiKapanisUcAy(Sembol);

Optimizasyon - Sistem.Optimizasyon()Bir algoritma geliştiren yatırımcı için, geliştirdiği algoritma ile ilgili çok önemli 2 unsur vardır. Bunlardan birincisi BackTest (geçmiş dönemde stratejisi ne kazandırmış veya kaybettirmiş bunu görmek), ikincisi ise Optimizasyondur. Optimizasyon, stratejide kullanılan çeşitli parametrelerin (stop veya kar al seviyeleri/oranları, kullanılan indikatörlerin periyotları, alım veya satıma karar veren ve değişken olan hemen her kriter) kullanılan değerlerinin haricinde değerler kullanılsa idi sonuçlar nasıl olurdu, hangi değerler kullanılsa idi getiri daha yüksek olurdu, işlem sayısı veya profit factor daha iyi olurdu gibi soruların cevapları Optimizasyon ile aranır.Optimizasyon işlemi, Sistem menüsü içerisinde yer alan Optimizasyon isimli pencere açılıp, bu amaçla yazılmış formülü seçerek yapılır. Yani al ve sat sinyalleri üreten bir kod yazılmışsa, bu kod doğrudan optimize edilmez, önce optimize edilebilecek şekilde yine kodlama gerekir.Bir stratejinin kurgusu kabaca şu şekildedir;
• Veri ve Liste tanımlamaları yapılan satırlar
• Kullanılacak indikatörleri tanımlamak
• Hesaplama, kıyaslama ve bazı başka sonuçları listeleri elde etme satırları
• Stratejiyi kurgulama (yön değerlerini koşullara göre atama)Bu genel yapıda kurgulanmış bir sistemin optimizasyon kodunu yazmak için söz konusu koda 3 ana yapı entegre edilir ve sonucundan yukarıdaki strateji kurgusunun akışı aşağıdaki hale gelir;
• Veri ve Liste tanımlamaları yapılan satırlar
• Kullanılan indikatörlerde yer alan ve optimize edilerek en uygun değerleri aranacak parametreye sahip olan her bir indikatör için, ilgili parametre(ler)in başlayıp biteceği değer aralıkları ve adımlarının belirtildiği for döngüleri açıp, o döngünün altında ilgili indikatörü tanımlamak ve bu işi opt edilecek her bir indikatör veya parametre için tekrar etmek
• Strateji bölgesine gelip koşullara göre yön atamalarını yapmadan önce, her seferinde bir önceki parametre ile doldurulmuş YÖN listesini temizlemek
• Stratejiyi çalıştırıp yön listesini doldurmak
• Sistem.Optimizasyon komutunu programa bildirmek
• Üst kısımda açılan her bir for döngüsünü kapatmak.ÖRNEK: 2 hareketli ortalamanın birbirini kesmesi ve RSI indikatörünün kendi hareketli ortalamasının üstünde/altında olmasıyla AL/SAT sinyalleri üreten bir strateji için ÖNCE SİSTEM sonra OPTİMİZASYON kurgusunu kodlayalım;SİSTEM KURGUSU:csharp

// kapanış fiyatlarını oku
var C = Sistem.GrafikFiyatSec("Kapanis");
var SonYon="";
//indikatörleri tanımla
var MA1 = Sistem.MA(C , "Exp", 10);
var MA2 = Sistem.MA(C , "Exp", 15);
var RSI = Sistem.RSI(C , 9);
var RSIAVR = Sistem.MA(RSI , "Exp", 4);
//Strateji bölgesi
for (int i = 1; i < C.Count; i++)
{
    if (RSI[i] > RSIAVR[i] && MA1[i] > MA2[i] && SonYon != "A") // alış
    {
        Sistem.Yon[i] = "A";
        SonYon="A";
    }
    else if (RSI[i] < RSIAVR[i] && MA1[i] < MA2[i] && SonYon != "S") // satış
    {
        Sistem.Yon[i] = "S";
        SonYon="S";
    }
}

OPTİMİZASYON KURGUSU:csharp

// kapanış fiyatlarını oku
var C = Sistem.GrafikFiyatSec("Kapanis");
var SonYon="";
for (int P1 = 10; P1 < 101; P1++) // MA1 için periyodu 10 ile 101 arasında birer birer adımlarla
{
    var MA1 = Sistem.MA(C , "Exp", P1);
    for (int P2 = 15; P2 < 250; P2++)
    {
        var MA2 = Sistem.MA(C , "Exp", P2);
        for (int P3 = 9; P3 < 59; P3++)
        {
            var RSI = Sistem.RSI(C , P3);
            for (int P4 = 4; P4 < 44; P4++)
            {
                var RSIAVR = Sistem.MA(RSI , "Exp", P4);
                //ÖNCEKİ DÖNGÜLERDEN KALAN YÖN LİSTESİNİ TEMİZLE VE DEVAM ET
                for (int i = 1; i < C.Count; i++)
                    Sistem.Yon[i] = "";
                // strateji bölgesi
                for (int i = 1; i < C.Count; i++)
                {
                    if (RSI[i] > RSIAVR[i] && MA1[i] > MA2[i] && SonYon != "A") // alış
                    {
                        Sistem.Yon[i] = "A";
                        SonYon="A";
                    }
                    else if (RSI[i] < RSIAVR[i] && MA1[i] < MA2[i] && SonYon != "S") // satış
                    {
                        Sistem.Yon[i] = "S";
                        SonYon="S";
                    }
                }
                Sistem.Optimizasyon("MA1,MA2,RSI,AVR", P1 , P2 , P3 , P4);
            }
        }
    }
}

Ortalama Fiyat - Sistem.Ortalama…(Sembol)Bir sembolün herhangi bir zaman dilimine ait ortalama fiyatını okumak için kullanılır.Kullanım şekilleri aşağıdaki gibidir.csharp

Sistem.OrtalamaAltiAy(Sembol);
Sistem.OrtalamaBirAy(Sembol);
Sistem.OrtalamaBirHafta(Sembol);
Sistem.OrtalamaBirYil(Sembol);
Sistem.OrtalamaBuAy(Sembol);
Sistem.OrtalamaBuHafta(Sembol);
Sistem.OrtalamaBuYil(Sembol);
Sistem.OrtalamaGun(Sembol);
Sistem.OrtalamaSeans(Sembol);
Sistem.OrtalamaUcAy(Sembol);

OtoTrend Düşen - Sistem.OtoTrendDusen(ToplamBar, SonXbar)Bir grafik üzerinde, kullanıcı tarafından girilmiş kadar bar dilimi içerisindeki DÜŞEN TRENDİ hesaplar. 2 parametre girilerek kullanılır. ToplamBar parametresi grafikte SON KAÇ BAR İÇİNDE düşen bir trend aranacağını, SonXbar parametresi ise, grafikte en sondaki kaç barın bu trend hesabında devre dışı bırakılacağını belirtir.csharp

Sistem.OtoTrendDusen(ToplamBar, SonBar);
Sistem.OtoTrendDusen(Veriler, ToplamBar, SonBar);

ÖRNEK: Son 1000 ve son 300 bar içindeki düşen trendleri bul ve çizcsharp

var UZUN = 1000;
var KISA = 300;
var SON = 5;
var DusenUzun = Sistem.OtoTrendDusen(UZUN, SON);
var DusenKisa = Sistem.OtoTrendDusen(KISA, SON);
Sistem.Cizgiler[0].Deger = DusenUzun;
Sistem.Cizgiler[1].Deger = DusenKisa;

OtoTrend Yükselen - Sistem.OtoTrendYukselen(ToplamBar, SonXbar)Bir grafik üzerinde, kullanıcı tarafından girilmiş kadar bar dilimi içerisindeki YÜKSELEN TRENDİ hesaplar.csharp

Sistem.OtoTrendYukselen(ToplamBar, SonBar);
Sistem.OtoTrendYukselen(Veriler, ToplamBar, SonBar);

ÖRNEK1: Son 1000 ve son 300 bar içindeki yükselen trendleri bul ve çizcsharp

var UZUN = 1000;
var KISA = 300;
var SON = 5;
var YukselenUzun = Sistem.OtoTrendYukselen(UZUN, SON);
var YukselenKisa = Sistem.OtoTrendYukselen(KISA, SON);
Sistem.Cizgiler[0].Deger = YukselenUzun;
Sistem.Cizgiler[1].Deger = YukselenKisa;

ÖRNEK2: Yükselen ve Düşen Trendi Kıran hisseleri SORGU modülünde taratıp bulmak:csharp

Sistem.SorguBaslik[0] = "Fiyat";
Sistem.SorguBaslik[1] = "Önc.Kap";
var Trendperiyodu = 800;
var SonXbar= 50;
var Sembol = Sistem.Sembol;
var V = Sistem.GrafikVerileriniOku(Sembol, Sistem.Periyot);
var C = Sistem.GrafikFiyatOku(V,"Kapanis");
var Yukselen = Sistem.OtoTrendYukselen(V, Trendperiyodu , SonXbar);
var Dusen = Sistem.OtoTrendDusen(V, Trendperiyodu , SonXbar);
if (V[V.Count - 1].Close < V[V.Count - Trendperiyodu].Close && V[V.Count - 1].High > Dusen[Dusen.Count - 1] && Dusen[Dusen.Count - 1] != 0)
{
    Sistem.SorguDeger[0] = C[C.Count - 1];
    Sistem.SorguDeger[1] = C[C.Count - 2];
    Sistem.SorguAciklama = "Düşen Trend Kırıldı";
    Sistem.SorguEkle();
}
else if (V[V.Count - 1].Close > V[V.Count - Trendperiyodu].Close && V[V.Count - 1].Low < Yukselen[Yukselen.Count - 1] && Yukselen[Yukselen.Count - 1] != 0)
{
    Sistem.SorguDeger[0] = C[C.Count - 1];
    Sistem.SorguDeger[1] = C[C.Count - 2];
    Sistem.SorguAciklama = "Yükselen Trend Kırıldı";
    Sistem.SorguEkle();
}

Özel Emirleri Oku - Sistem.OzelEmirleriGetir()csharp

if (DateTime.Now.Hour < 19)
{
    var OE = Sistem.OzelEmirleriGetir();
    if (OE != null)
    {
        var Anahtar = Sistem.Name + "," + DateTime.Now.ToString("yyyyMMdd");
        var Adet = Sistem.SayiTablosunuOku(Anahtar);
        if (OE.Count != Adet)
        {
            Adet = OE.Count;
            Sistem.SayiTablosunuGuncelle(Anahtar, Adet);
            var strlist = new List<string>();
            foreach (var item in OE)
            {
                var str = item.Symbol + " , " + "Saat = " + item.Hour.ToString("") + ":" +
                item.Minute.ToString("") + ":" + item.Second.ToString("") + " , " + "Fiyat = " +
                item.Price.ToString("0.00") + " , " + "Lot = " + item.Size.ToString() + " Alan = " + item.BuyerCode
                + " Satan= " + item.SellerCode;
                strlist.Add(str);
            }
            var msg = string.Join("\r\n", strlist);
            Sistem.Mesaj(msg);
        }
    }
}

PSAR / PARABOLIC - Sistem.Parabolic(0.02 , 0.2)csharp

Sistem.Parabolic(0.02, 0.2);
Sistem.Parabolic(Liste,0.02 , 0.2);
Sistem.Parabolic(Veriler,0.02 , 0.2);

Örnek: Fiyat Parabolic indikatörünü yukarı keserse al, aşağı keserse SAT sistem formülü.csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var Parabolic = Sistem.Parabolic(0.02, 0.2);
var SonYon = "";
for (int i = 1; i < Sistem.BarSayisi; i++)
{
    if (C[i-1] < Parabolic[i-1] && C[i] > Parabolic[i] && SonYon != "A")
    {
        SonYon = "A";
        Sistem.Yon[i] = SonYon;
    }
    if (C[i-1] > Parabolic[i-1] && C[i] < Parabolic[i] && SonYon != "S")
    {
        SonYon = "S";
        Sistem.Yon[i] = SonYon;
    }
}
Sistem.Cizgiler[0].Deger = Parabolic;

Parametreler - Sistem.Parametrelercsharp

Sistem.Parametreler[x]

ÖRNEK: GARAN hisse senedinde manuel pozisyon açan kullanıcının stop seviyelerini girdiği robot örneği.csharp

string Sembol = "IMKBH'GARAN";
float IslemFiyati = Convert.ToSingle(Sistem.Parametreler[1]);
float KarMarj = Convert.ToSingle(Sistem.Parametreler[3])/100;
float ZararMarj = Convert.ToSingle(Sistem.Parametreler[5])/100;
float Miktar = Convert.ToSingle(Sistem.Parametreler[7]);
string Pozisyon_Yonu = Sistem.Parametreler[9];
var Islem = "";
var SonFiyat = Sistem.SonFiyat(Sembol);
var Pozisyon = Sistem.PozisyonKontrolOku(Sembol);
if (SonFiyat > 0 && (Sistem.SaatAraligi("09:35", "12:30") || Sistem.SaatAraligi("13:00", "17:40")))
{
    if (Pozisyon_Yonu == "L" && (SonFiyat >= IslemFiyati + KarMarj) && Pozisyon > 0)
        Islem = "SATIS";
    else if (Pozisyon_Yonu == "S" && (SonFiyat <= IslemFiyati - ZararMarj) && Pozisyon < 0)
        Islem = "ALIS";
}
if (Islem != "" && Pozisyon_Yonu != "F")
{
    Sistem.Parametreler[9] = "F";
    if (Islem == "ALIS")
        Sistem.PozisyonKontrolGuncelle(Sembol, 0);
    else if (Islem == "SATIS")
        Sistem.PozisyonKontrolGuncelle(Sembol, 0);
    Sistem.EmirSembol = Sembol;
    Sistem.EmirIslem = Islem;
    Sistem.EmirMiktari = Miktar;
    Sistem.EmirSuresi = "KIE";
    Sistem.EmirTipi = "Piyasa";
    Sistem.EmirGonder();
}

Periyot - Sistem.Periyot()csharp

var periyot = (Sistem.Periyot == "60" || Sistem.Periyot == "120" || Sistem.Periyot == "S" || Sistem.Periyot == "240" || Sistem.Periyot == "G" || Sistem.Periyot == "H" || Sistem.Periyot == "A" ) ? false : true;
if (periyot == true)
{
    // yazılacak kod
}
else if (periyot == false)
    Sistem.Mesaj("Sistem 60 Dakikanın altındaki periyotlar için tasarlanmıştır. Lütfen 60 dk dan daha küçük bir periyot seçiniz");

PH01 / PL01csharp

var PH = Sistem.PH01(Donem);
var PL = Sistem.PL01(Donem);
var PH = Sistem.PH01(Veriler , Donem);
var PL = Sistem.PL01(Veriler , Donem);

Örnek:csharp

var PH = Sistem.PH01("H");
var PL = Sistem.PL01("H");
Sistem.Cizgiler[0].Deger = PH;
Sistem.Cizgiler[1].Deger = PL;

Pivot Kanalı - Sistem.PivotUp / PivotMid / PivotDowncsharp

Sistem.PivotUp();
Sistem.PivotUp(Veriler);
Sistem.PivotMid();
Sistem.PivotMid(Veriler);
Sistem.PivotDown();
Sistem.PivotDown(Veriler);

ÖRNEK: YKBNK hissesinin GÜNLÜK barları için Pivot hesapla.csharp

var Veriler = Sistem.GrafikVerileriniOku("IMKBH'YKBNK", "G");
var UstKanal = Sistem.PivotUp(Veriler);
var OrtaKanal = Sistem.PivotMid(Veriler);
var AltKanal = Sistem.PivotDown(Veriler);

