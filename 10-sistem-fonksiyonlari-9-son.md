markdown

# Sistem Fonksiyonları (Son Bölüm - 10)

### Trend Çiz - Sistem.TrendCiz()

Grafikler üzerinde kod yazarak Trendler çizbilmek için kullanılan iDeal Sistem Fonksiyonudur. Bir trend çizgisi, tanımlı iki noktadan geçen bir doğru parçasıdır.

```csharp
Sistem.TrendCiz(Tarih1, Deger1, Tarih2, Deger2);

ÖRNEK1: Tarih ve Fiyat bilgilerini elle girerek bir trendi çizdirmekcsharp

DateTime Tarih1, Tarih2;
double Fiyat1, Fiyat2;
Tarih1 = new DateTime(2020, 3, 17, 0, 0, 0);
Fiyat1 = 1.70;
Tarih2 = new DateTime(2020, 10, 30, 0, 0, 0);
Fiyat2 = 3.10;
var Trend = Sistem.TrendCiz(Tarih1, Fiyat1, Tarih2, Fiyat2);
Sistem.Cizgiler[0].Deger = Trend;

ÖRNEK2: Bar numarası kullanarak trend çizdirmecsharp

var V = Sistem.GrafikVerileri;
var baslamabari = 1289;
var bitisbari = 1400;
var Trend = Sistem.TrendCiz(V[baslamabari].Date, V[baslamabari].Low, V[bitisbari].Date, V[bitisbari].Low);
Sistem.Cizgiler[0].Deger = Trend;

Trend Paralel Çiz - Sistem.TrendParalelCiz()csharp

Sistem.TrendParalelCiz(Trend, Tarih1, Tarih2);

ÖRNEK:csharp

var Trend = Sistem.TrendCiz(Tarih1, Fiyat1, Tarih2, Fiyat2);
Sistem.Cizgiler[0].Deger = Trend;
var Paralel = Sistem.TrendParalelCiz(Trend, Tarih1, Tarih2);
Sistem.Cizgiler[1].Deger = Paralel;

Trend Kontrol - Sistem.TrendKontrol()csharp

Sistem.TrendKontrol(item, out Deger);

Örnek:csharp

var TrendListesi = new List<string>();
TrendListesi.Add("IMKBH'GARAN ; G ; 23/02/2017 ; 00:00:00 ; 9.10 ; 04/02/2018 ; 00:00:00 ; 9.20");
TrendListesi.Add("IMKBH'SAHOL ; G ; 19/05/2017 ; 00:00:00 ; 10.10 ; 01/03/2018 ; 00:00:00 ; 12.20");

string Mesaj = "";
foreach (var item in TrendListesi)
{
    decimal Deger;
    var Sonuc = Sistem.TrendKontrol(item, out Deger);
    if (Sonuc != null)
    {
        Mesaj += Sonuc + " " + Deger.ToString() + "\r\n";
    }
}
Sistem.Mesaj(Mesaj);

Trend Score (TS) - Sistem.TrendScore(10)csharp

Sistem.TrendScore(10);
Sistem.TrendScore(Liste, 10);
Sistem.TrendScore(Veriler, 10);

ÖRNEK:csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var MOV5 = Sistem.MA(C, "Simple", 5);
var MOV22 = Sistem.MA(C, "Simple", 22);
var TS = Sistem.TrendScore(10);
var SonYon = "";
for(int i=1; i < Sistem.BarSayisi;i++)
{
    if (MOV5[i] > MOV22[i] && TS[i] == 10 && SonYon != "A")
    {
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (MOV5[i] < MOV22[i] && TS[i] == -10 && SonYon != "S")
    {
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

TRIX - Sistem.TRIX(12)csharp

Sistem.TRIX(12);
Sistem.TRIX(Veriler, 12);

TTI (Trend Takip İndikatörü) - Sistem.TTI(3, 2, “Simple”)csharp

Sistem.TTI(3, 2, MAyontem);
Sistem.TTI(Liste, 3, 2, MAyontem);
Sistem.TTI(Veriler, 3, 2, MAyontem);

Tipik Fiyat - Sistem.TypicalPrice()csharp

Sistem.TypicalPrice();
// veya
Sistem.GrafikFiyatSec("Tipik");

Üçgen Çiz - Sistem.UcgenCiz()csharp

Sistem.UcgenCiz(panel, bar1, fiyat1, bar2, fiyat2, bar3, fiyat3, renk, kalinlik, dolgu, dolgurenk, nokta, noktarenk);

ÖRNEK:csharp

int SonBar = Sistem.BarSayisi;
var birincinokta = SonBar-600;
var ikincinokta = SonBar-800;
var ucuncunokta = SonBar-300;
Sistem.UcgenCiz(1, birincinokta, 154.5, ikincinokta, 89.3, ucuncunokta, 100.65, Color.SkyBlue, 3, 1, Color.FromArgb(100,0,0,220), 0, Color.Cyan);

Ultimate Oscillator - Sistem.UltimateOsc(7, 14, 28)csharp

Sistem.UltimateOsc(7, 14, 28);
Sistem.UltimateOsc(Veriler, 7, 14, 28);

VHF / Vertical Horizontal Filter - Sistem.VerticalHorizontalFilter(28)csharp

Sistem.VerticalHorizontalFilter(28);
Sistem.VerticalHorizontalFilter(Veriler, 28);

VIDYA - Sistem.VIDYA(30, 9)csharp

Sistem.VIDYA(30,9);
Sistem.VIDYA(Veriler, 30,9);

Volume (Hacim) - Sistem.Volume()csharp

Sistem.Volume();
Sistem.Volume(Veriler);

Volume Oscillator Percent / Pointcsharp

Sistem.VolumeOscPercent(12, 26,"Exp");
Sistem.VolumeOscPoint(12, 26,"Exp");

VORTEX - Sistem.VortexMinus / Plus(24)csharp

Sistem.VortexMinus(24);
Sistem.VortexPlus(24);

ÖRNEK:csharp

var VORTEX_Plus = Sistem.VortexPlus(14);
var VORTEX_Minus = Sistem.VortexMinus(14);
Sistem.Cizgiler[0].Deger = VORTEX_Plus;
Sistem.Cizgiler[1].Deger = VORTEX_Minus;

VIOP Hesap Oku - Sistem.ViopHesapOku()csharp

var ViopHesap = Sistem.ViopHesapOku();
ViopHesap.TeminatToplam;
ViopHesap.TeminatBaslangic;
ViopHesap.TeminatSurdurme;
ViopHesap.TeminatKullanilabilir;
ViopHesap.TeminatCekilebilir;
// Pozisyonlar, BekleyenEmirler, GerceklesenEmirler listeleri de mevcuttur.

Yay Çiz - Sistem.YayCiz()csharp

Sistem.YayCiz(panel, bar1, fiyat1, bar2, fiyat2, bar3, fiyat3, renk, kalinlik, dolgu, dolgurenk, nokta, noktarenk);
Sistem.YayCiz(panel, bar1, fiyat1, bar2, fiyat2, bar3, fiyat3, bar4, fiyat4, renk, kalinlik, dolgu, dolgurenk, nokta, noktarenk);

Yazı Ekle - Sistem.YaziEkle()csharp

Sistem.YaziEkle(Metin, Panel, BarNo, Fiyat, Renk, FontAdi, FontBoyutu);

Yön Listesi - Sistem.Yon[]csharp

Sistem.Yon[i] = "A"; // Alış
Sistem.Yon[i] = "S"; // Satış
Sistem.Yon[i] = "F"; // Flat

Yukarı Kestiyse - Sistem.YukariKestiyse()csharp

Sistem.YukariKestiyse(Liste1, Liste2);
Sistem.YukariKestiyse(Liste, Sayı);

Yüksek (Yuksek) Fiyat - Sistem.Yuksek…(Sembol)csharp

Sistem.YuksekAltiAy(Sembol);
Sistem.YuksekBirAy(Sembol);
// ... diğer dönemler

Yüzde Değişim - Sistem.Yuzde…(Sembol)csharp

Sistem.YuzdeAltiAy(Sembol);
// ... diğer dönemler

Yüzeysel Liste Getir - Sistem.YuzeyselListeGetir(Kriter)csharp

var Liste = Sistem.YuzeyselListeGetir("IMKBH'");

Yüzeysel Veri Oku - Sistem.YuzeyselVeriOku(Sembol)csharp

var Veriler = Sistem.YuzeyselVeriOku(Sembol);
var Last = Veriler.LastPrice;
var Bid = Veriler.BidPrice;
var Ask = Veriler.AskPrice;
// ... diğer alanlar

Zemin Yazısı Ekle - Sistem.ZeminYazisiEkle()csharp

Sistem.ZeminYazisiEkle(Metin, Panel, X, Y, Renk, FontAdi, FontBoyutu);

Zaman Kontrol / Güncelle - Sistem.ZamanKontrolSaniye / Dakika / Guncellecsharp

if (Sistem.ZamanKontrolSaniye("FOTOCEK") >= 30)
{
    Sistem.ZamanKontrolGuncelle("FOTOCEK");
    // işlem
}

ZigZag / Peak-Through - Sistem.ZigZag…()csharp

Sistem.ZigZagPercent(5);
Sistem.ZigZagPoint(1);
Sistem.ZigZagPeakThrough(Liste, Sayi, Yuzde);

Önemli Not: ZigZag re-paint yapan bir indikatördür. Sistemlerde kullanılması tavsiye edilmez.Zirve - Sistem.Zirve(Barsayısı)csharp

Sistem.Zirve(100);
// Bkz: Sistem.Dip(100)

Örnek:csharp

var Dip = Sistem.Dip(50);
var Zirve = Sistem.Zirve(50);
Sistem.Cizgiler[0].Deger = Dip;
Sistem.Cizgiler[1].Deger = Zirve;

