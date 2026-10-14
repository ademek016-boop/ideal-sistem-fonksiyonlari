markdown

# Sistem Fonksiyonları (Devam - 9. Bölüm)

### Select BarNo (Mouse ile Kliklenen Barı Okumak) - Sistem.SelectBarNo

iDeal kullanıcıları grafik üzerinde Mouse ile bir noktaya kliklendiği zaman, o kliklenen barın tarihini, saatini, bar numarasını sistem üzerinden okuyabilirler. Bu sayede mesela Mouse ile tıkladıkları noktadan geriye doğru olan bölgedeki yükselen veya düşen trend çizimlerini otomatik olarak ve sadece Mouse ile tıklayarak yaptırabilirler. Aynı şekilde pivot seviyeleri çizimini de Mouse tıklaması ile istedikleri bölge için kolayca yapabilirler.

**Örnek1:** Kliklenen barın tarih ve saatini öğren;
```csharp
var V = Sistem.GrafikVerileri;
int nokta = Sistem.SelectBarNo;
Sistem.Mesaj(V[nokta].Date.ToString("yyyyMMdd HH:mm"));

ÖRNEK2: Mouse ile tıklanan bardan günümüze kadar olan bölgede varsa yükselen ve düşen trendleri bul ve çiz:csharp

int donem = Sistem.BarSayisi - Sistem.SelectBarNo;
Sistem.Cizgiler[0].Deger = Sistem.OtoTrendYukselen(donem, 10);
Sistem.Cizgiler[1].Deger = Sistem.OtoTrendDusen(donem, 10);

Select Tarih (Mouse ile Kliklenen Barın Tarihini Okumak) - Sistem.SelectTarihcsharp

var tarih = Sistem.SelectTarih;
Sistem.Mesaj(tarih.ToString());

Sembol - Sistem.Sembol()Üzerinde işlem yapılan, grafiği üzerinde bir takım çizim veya stratejiler uygulanan sembolün (kodun) bilgisi bu fonksiyonla okutulur.Sembol Adları Listesi - Sistem.SembolAdListesi(market, seri)csharp

var Liste = Sistem.SembolAdListesi("IMKB", "R");
var Metin = "";
foreach(var item in Liste)
{
    Metin += item + "\r\n";
}
Sistem.Mesaj(Metin);

Sembol Veri Listesi - Sistem.SembolVeriListesi(market, seri)csharp

var Liste = Sistem.SembolVeriListesi("IMKB", "E");
var Metin = "";
foreach(var item in Liste)
{
    Metin += item.Root + " % " + item.NetPerSession.ToString("0.00" ) + "\r\n";
}
Sistem.Mesaj(Metin);

Sembol Tanımla - Sistem.SembolTanimla(SembolAdı, OndalıkBasamakSayısı)csharp

var DENEMEKODU1 = Sistem.SembolTanimla("DFN'DENEMEKODU1", 2);
DENEMEKODU1.Description = "Test Amacli Deneme Kodudur";
DENEMEKODU1.LastPrice = GARAN.LastPrice / HALKB.LastPrice;
Sistem.YuzeyselGuncelle(DENEMEKODU1);
Sistem.GrafikGuncelle(DENEMEKODU1);

Örnek Kodlar (Çeyrek ve Ata Altın):csharp

var SGLDD = Sistem.YuzeyselVeriOku("SERPIY'SGLDD");
var SUSD = Sistem.YuzeyselVeriOku("SERPIY'SUSD");

//CEYREK ALTIN TANIMLA
var CEYREK = Sistem.SembolTanimla("DFN'CEYREK", 3);
CEYREK.Description = "Çeyrek Altın";
CEYREK.BidPrice = Convert.ToSingle(((SGLDD.BidPrice-10)*SUSD.BidPrice/0.995)/1000*1.58);
CEYREK.AskPrice = Convert.ToSingle(((SGLDD.AskPrice+10)*SUSD.AskPrice/0.995)/1000*1.67);
CEYREK.LastPrice = (CEYREK.BidPrice+CEYREK.AskPrice)/2;
Sistem.YuzeyselGuncelle(CEYREK);
Sistem.GrafikGuncelle(CEYREK);

// ATA ALTIN TANIMLA
var ATA = Sistem.SembolTanimla("DFN'ATA", 2);
ATA.Description = "Yarım Altın";
ATA.BidPrice = Convert.ToSingle(((SGLDD.BidPrice-10)*SUSD.BidPrice/0.995)/1000*3.18);
ATA.AskPrice = Convert.ToSingle(((SGLDD.AskPrice+10)*SUSD.AskPrice/0.995)/1000*3.35);
ATA.LastPrice = (ATA.BidPrice+ATA.AskPrice)/2;
Sistem.YuzeyselGuncelle(ATA);

Sembol İşlemlerini Oku - Sistem.SembolIslemleriniOku(Sembol, Tarih)csharp

var IslemlerVIOP = Sistem.SembolIslemleriniOku("VIP'F_XU0301220", "10/12/2020");
var IslemlerHisse = Sistem.SembolIslemleriniOku("IMKBH'GARAN", "17/12/2020");

Örnek: GARAN işlemlerini okuyup bir aracı kurumun NET LOT bilgisini hesaplatmakcsharp

var IslemlerHisse = Sistem.SembolIslemleriniOku("IMKBH'GARAN", "17/12/2020");
var NetLot = 0;
foreach (var item in IslemlerHisse)
{
    if (item.BuyerCode == "TBY") NetLot += item.Size;
    if (item.SellerCode == "TBY") NetLot -= item.Size;
}
Sistem.Mesaj(NetLot.ToString("0,000"));

Ses Çalma - Sistem.Ses()csharp

var Sembol = "IMKBH'EREGL";
var SembolDeger = Sistem.SonFiyat(Sembol);
if (SembolDeger >= 3.84f)
    Sistem.Ses("Media\\dingdong.wav");

Seviye Fonksiyonu - Sistem.Seviye[]csharp

Sistem.Seviye[i] = Fiyat - StopPuan;

Sistem Birleştirme - Sistem.Birlestir / SistemBirlestirAyniYoncsharp

Sistem.SistemBirlestir("XXX", "YYY", "ZZZZ","KKKK");
Sistem.SistemBirlestirAyniYon("XXX", "YYY", "ZZZZ","KKKK");

Sistem Getir - Sistem.SistemGetir(SistemAdı, Sembol, Periyot)csharp

var Sistemim1 = Sistem.SistemGetir("TOMA", "IMKBH'GARAN", "G");
var Sistemim2 = Sistem.SistemGetir("BolingerSys", "IMKBH'GARAN", "G");
var Sistemim3 = Sistem.SistemGetir("MACD", "IMKBH'GARAN", "G");
Sistem.Cizgiler[0].Deger = Sistemim1.GetiriKZ;
Sistem.Cizgiler[1].Deger = Sistemim2.GetiriKZ;
Sistem.Cizgiler[2].Deger = Sistemim3.GetiriKZ;
Sistem.Yon = Sistemim1.Yon;

Son Fiyat / Son Hacim / Son Lotcsharp

var X = Sistem.SonFiyat(Sembol);
var X = Sistem.SonHacim(Sembol);
var X = Sistem.SonLot(Sembol);

Son Yön Getir - Sistem.SonYonGetir / SonYonGetirCanlicsharp

var SonYon = Sistem.SonYonGetir(SistemAdi, GrafikSembolu, GrafikPeriyodu);
var SonYon = Sistem.SonYonGetirCanli(SistemAdi, GrafikSembolu, GrafikPeriyodu);

Sorgu Fonksiyonları - Sistem.Sorguxxxxxcsharp

Sistem.SorguBaslik[0] = "Kapanış";
Sistem.SorguOndalik[0] = 2;
Sistem.SorguDeger[0] = C[SonBar];
Sistem.SorguSutunTip[2] = "YAZI";
Sistem.SorguSutunHizala[3] = "SOL";
Sistem.SorguSutunGenislik[3] = 150;
Sistem.SorguHucreZeminRengi[2] = Color.Green;
Sistem.SorguAciklama = "Filtrem";
Sistem.SorguEkle();

Sözcük Tablosu - Sistem.SozcukTablosunuOku / Guncellecsharp

var Aktarildimi = Sistem.SozcukTablosunuOku(Sistem.Sembol + Sistem.Periyot + "DosyayVeriAktar");
if (Aktarildimi != "OK")
{
    Sistem.SozcukTablosunuGuncelle(Sistem.Sembol + Sistem.Periyot + "DosyayVeriAktar", "OK");
    // işlemler
}

Sum - Sistem.Sum()csharp

Sistem.Sum(Liste);          // tüm elemanların toplamı
Sistem.Sum(Liste, 10);      // son 10 elemanın toplamından oluşan yeni liste

Süper Trend - Sistem.SuperTrend()csharp

Sistem.SuperTrend(Factor, HHVPeriyot, ATRPeriyot);
Sistem.SuperTrend(Veriler, Factor, HHVPeriyot, ATRPeriyot);

Standart Deviation (Standart Sapma) - Sistem.StDev()csharp

Sistem.StDev(5, 1);
Sistem.StDev(Veriler, 5, 1);
Sistem.StDev(Liste, Deger);
Sistem.StDevDeger(Liste);

Standart Error - Sistem.StEr()csharp

Sistem.StEr(14);
Sistem.StEr(Veriler, 14);

Standart Error Bands - Sistem.StErDown / Mid / Up()csharp

Sistem.StErDown(14, 2,"Simple", 1);
Sistem.StErMid(14, 2,"Simple", 1);
Sistem.StErUp(14, 2,"Simple", 1);

Stochastic Fast / Slow / Oscillator / RSI / Momentum Indexcsharp

Sistem.StochasticFast(5, 3);
Sistem.StochasticSlow(5, 3);
Sistem.StochasticOsc(5, 3);
Sistem.StochasticRSI(14);
Sistem.StochasticMomIndex(5, 3, 3);

Stop veya KarAl Puan/Yüzde - Sistem.StopVeyaKarAl / StopVeyaKarFlatPuancsharp

Sistem.StopVeyaKarFlatPuan(1.500, 4.500);

Swing Index - Sistem.SwingIndex(3)csharp

Sistem.SwingIndex(3);
Sistem.SwingIndex(Veriler, 3);

Taban / Tavan Fiyatcsharp

var TabanFiyat = Sistem.Taban(Sembol);
var TavanFiyat = Sistem.Tavan(Sembol);

Tablo Kullanımları - Sistem.Tablo…()csharp

Sistem.Tablo(TabloAdi, Sol, Ust, Genislik, Yukseklik, SutunSayisi, SatirSayisi, SutunGenislik, SutunHizala, SutunBaslik);
Sistem.TabloTemizle(TabloAdi);
Sistem.TabloYazdir(TabloAdi, Sutun, Satir, Metin, ZeminRenk, YaziRenk);

Tarih - Sistem.Tarih()csharp

var Tarih = Sistem.Tarih;
Sistem.Mesaj(Tarih);

Tarih Aralığı - Sistem.TarihAraligi(“2019.01.20”, “2020.12.31”)csharp

if (Sistem.TarihAraligi("2020.01.01", "2020.12.31"))
{
    // KODUN TAMAMI
}
else
    Sistem.Mesaj("Bu Sistemi Kullanmaya Yetkiniz Yoktur. Lütfen iletişime geçiniz.");

TEMA – Sistem.TEMA(5)csharp

Sistem.TEMA(21);
Sistem.TEMA(Liste,21);
Sistem.TEMA(Veriler,21);

Tillson T3 - Sistem.TillsonT3(9, 0.618)csharp

Sistem.TillsonT3(9, 0.618);
Sistem.TillsonT3(Liste, 9, 0.618);

TSF / Time Series Forecast - Sistem.TimeSeriesForecast(14)csharp

Sistem.TimeSeriesForecast(14);
Sistem.TimeSeriesForecast(Veriler, 14);

TKE - Sistem.TKE()csharp

Sistem.TKE();
Sistem.TKE(Veriler);

TOMA (Trailing Oscilator Of Moving Average) - Sistem.TOMA(3,2)csharp

Sistem.TOMA(3, 2);
Sistem.TOMA(3, 2, MAYontem);
Sistem.TOMA(Liste, 3, 2);
Sistem.TOMA(Veriler, 3, 2);

TOMA PUAN - Sistem.TOMAPUAN(3,2)csharp

Sistem.TOMAPUAN(3, 2);
Sistem.TOMAPUAN(Liste, 3, 2);
Sistem.TOMAPUAN(Veriler, 3, 2);

