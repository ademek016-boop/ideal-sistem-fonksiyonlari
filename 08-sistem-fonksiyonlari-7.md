markdown

# Sistem Fonksiyonları (Devam - 8. Bölüm)

### Polarized Fractal Efficiency - Sistem.PolarizedFractalEfficiency(14,3)

iDeal indikatör kütüphanesinde yer alan Polarized Fractal Efficiency isimli indikatörü çağırır. 2 Adet parametre alır (varsayılan 14 ve 3). Aşağıdaki gibi 2 yazım şekli vardır;

```csharp
Sistem.PolarizedFractalEfficiency(14, 3);
Sistem.PolarizedFractalEfficiency(Veriler, 14, 3);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var PFE = Sistem.PolarizedFractalEfficiency(14, 3);
Sistem.Cizgiler[0].Deger = PFE;

Positive Volume Index - Sistem.PositiveVolumeIndex()iDeal indikatör kütüphanesinden yer alan Positive Volume Index isimli indikatörü çağırır. Hiç parametre almaz. Sorunsuz ve anlık çalışması için HACİM anlık verisi gerektirir. Aşağıdaki gibi 2 yazım şekli vardır;csharp

Sistem.PositiveVolumeIndex();
Sistem.PositiveVolumeIndex(Veriler);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var PVI = Sistem.PositiveVolumeIndex();
Sistem.Cizgiler[0].Deger = PVI;

Pozisyon Kontrol Oku/Güncelle - Sistem.PozisyonKontrolOku / Guncelle()iDeal veri terminali üzerinde robotlar çalıştırılırken çoğu zaman bazı bilgileri bir dosyada veya bellekte saklamak ve gerektiğinde kullanmak gerekir. Kod yazılırken bilgi atanan (var x = 5; şeklinde tanımlanan) değişkenler bazen bu ihtiyacı karşılamaz.Çünkü kod içerisinde tanımlanan değişkenler, kodun her seferinde defalarca çalışması durumunda hep ilk tanımladığı değerine döner ve ilerleyen satırlarda hesaplamalar sonucu yeni değeri bulunur. Ama bu döngü her seferinde yeniden olur. Oysa istenen şey, bir kez değeri değiştirilen bir verinin, bir daha kullanıcının tanımladığı bir koşul olmadan hiç değişmemesidir.Bu nedenle iDeal’de Pozisyon Kontrol, Sayı Tablosu, Sözcük Tablosu gibi veri saklama işlevi olan komutlar kullanılır.Pozisyon Kontrol genelde alım satım yapılan robotlarda kullanılır. Örneğin bir hissenin fiyatı x olursa AL kurgusuyla çalışan bir robotta, hissenin fiyatı x olduğunda ALIM yapılınca, hisse değeri düşüp sonra tekrar x olunca yeniden alım yapmaması için ilk alımda pozisyon değeri mesela 1 yapılır. Bu sayede koşul tekrar sağlansa bile robot alım yapmaz, taa ki kullanıcı pozisyon ismiyle saklanan değişkendeki değeri başka bir koşulla değiştirinceye kadar.PozisyonKontrolOku komutu saklanmış bilgiyi okumak için, PozisyonKontrolGuncelle komutu ise buraya bir bilgiyi yazmak için kullanılır.Robotlar aksi kodlanmadıkça PİYASA EMRİ ile emir iletirler. Bu sayede emrin gerçekleşmesi (hisse tavan değilse, devre kesmemişse) kesindir. Emir iletilen işlem miktarı kadar pozisyona girilmiş olur ve kendi pozisyonunu bu şekilde takip eder. (aracı kurumunuzdaki hesabınızda o hisse için kaç adet pozisyonunuz olduğuna değil kendi pozisyon değerine bakar).ÖRNEK: Saat 11:01:00 olunca 1000 adet DOHOL alış emri gönder. Saat 15:01:00 olunca aldığı (pozisyonu) kadar SATIŞ emri gönder robotu;csharp

var Sembol = "IMKBH'DOHOL";
var Pozisyon = Sistem.PozisyonKontrolOku(Sembol);
var Miktar = 1000;
if (Sistem.Saat.CompareTo("11:01:00") >= 0 && Pozisyon == 0)
{
    Sistem.PozisyonKontrolGuncelle(Sembol, Pozisyon + Miktar);
    Sistem.EmirSembol = Sembol;
    Sistem.EmirIslem = "Alış";
    Sistem.EmirTipi = "Piyasa";
    Sistem.EmirSuresi = "KIE";
    Sistem.EmirMiktari = Miktar;
    Sistem.EmirGonder();
}
if (Sistem.Saat.CompareTo("15:01:00") >= 0 && Pozisyon == Miktar)
{
    Sistem.PozisyonKontrolGuncelle(Sembol, 0);
    Sistem.EmirSembol = Sembol;
    Sistem.EmirIslem = "Satış";
    Sistem.EmirTipi = "Piyasa";
    Sistem.EmirSuresi = "KIE";
    Sistem.EmirMiktari = Pozisyon;
    Sistem.EmirGonder();
}

ÖRNEK2: Birden çok robot aynı hissede alış/satış yapıyor. Bütün robotların o hissedeki pozisyon değerlerini okuyabiliriz.csharp

var Poz1 = Sistem.PozisyonKontrolOku("Robot1" + " , " + Sistem.AktifViopKontrat);
var Poz2 = Sistem.PozisyonKontrolOku("Robot2" + " , " + Sistem.AktifViopKontrat);
var Poz3 = Sistem.PozisyonKontrolOku("Robot3" + " , " + Sistem.AktifViopKontrat);
var Poz4 = Sistem.PozisyonKontrolOku("Robot4" + " , " + Sistem.AktifViopKontrat);
var Poz5 = Sistem.PozisyonKontrolOku("Robot5" + " , " + Sistem.AktifViopKontrat);

• Pozisyon Kontrol komutuyla yazılan/okunan bilgiler iDeal kapanıp açılsa bile silinmez. Bu bilgiler \iDeal\Config klasörü altındaki dosyada kaydedilir ve saklanır.
• Pozisyon kontrol komutuyla istenirse işleme girildiği anın tarih saat bilgisi de kaydedilebilir.
• Pozisyon kontrol komutuyla istenirse emir üretildiği anda o hissenin herhangi bir fiyat bilgisinin ne olduğu da kaydedilebilir.
• Pozisyon kontrol komutuyla, emir iletildiği an için kullanıcı tarafından belirlenecek herhangi bir not (örneğin KAR AL YAPILDI) yazdırılabilir.Gelişmiş Örnek Robot:csharp

var LotSize = 1;
var SistemAdi = "MA500";
var GrafikSembolu = "IMKBH'GARAN";
var GrafikPeriyodu = "1";
var EmirSembol = "IMKBH'GARAN";
var MySistem = Sistem.SistemGetir(SistemAdi, GrafikSembolu , GrafikPeriyodu);
var SonFiyat = Sistem.SonFiyat(EmirSembol);
var Anahtar = Sistem.Name + "," + EmirSembol;
double IslemFiyat = 0;
DateTime IslemTarih;
var Miktar = 0.0;
var Rezerv = "";
var Pozisyon = Sistem.PozisyonKontrolOku(Anahtar, out IslemFiyat, out IslemTarih);
var SonYon = Sistem.SonYonGetir(SistemAdi, GrafikSembolu , GrafikPeriyodu);

if (Sistem.Saat.CompareTo("10:00:00") <= 0 || Sistem.Saat.CompareTo("20:59:59") >= 0)
{
}
else
{
    if (SonYon == "F" && Pozisyon != 0)
        Miktar = -Pozisyon;
    else if (SonYon == "A" && Pozisyon != LotSize)
        Miktar = LotSize - Pozisyon;
    else if (SonYon == "S" && Pozisyon != -LotSize)
        Miktar = -LotSize - Pozisyon;

    var Islem = "";
    if (Miktar > 0) {Islem = "ALIS"; Rezerv = "ALIŞ YAPILDI";}
    if (Miktar < 0) {Islem = "SATIS"; Rezerv = "SATIŞ YAPILDI";}
    if (Islem != "")
    {
        Sistem.PozisyonKontrolGuncelle(Anahtar, Miktar + Pozisyon, SonFiyat, Rezerv);
        Sistem.EmirSembol = EmirSembol;
        Sistem.EmirIslem = Islem;
        Sistem.EmirSuresi = "KIE";
        Sistem.EmirTipi = "Piyasa";
        Sistem.EmirMiktari = Math.Abs(Miktar);
        Sistem.EmirGonder();
    }
}

Price Channel Up/Down - Sistem.PriceChannelDown / Up(10)csharp

Sistem.PriceChannelDown(10);
Sistem.PriceChannelDown(Veriler, 10);
Sistem.PriceChannelUp(10);
Sistem.PriceChannelUp(Veriler, 10);

Örnek:csharp

var PC_DOWN = Sistem.PriceChannelDown(10);
var PC_UP = Sistem.PriceChannelUp(10);
Sistem.Cizgiler[0].Deger = PC_DOWN;
Sistem.Cizgiler[1].Deger = PC_UP;

Price Oscillator Percent - Sistem.PriceOscPercent(10, 30 , “Exp”)csharp

Sistem.PriceOscPercent(10, 30,"Exp");
Sistem.PriceOscPercent(Veriler, 10, 30,"Exp");

Price Oscillator Point - Sistem.PriceOscPoint(10, 30 , “Exp”)csharp

Sistem.PriceOscPoint(10, 30,"Exp");
Sistem.PriceOscPoint(Veriler, 10, 30,"Exp");

Price ROC Percent - Sistem.PriceRocPercent(12)csharp

Sistem.PriceRocPercent(10);
Sistem.PriceRocPercent(Veriler, 10);

Price ROC Point - Sistem.PriceRocPoint(12)csharp

Sistem.PriceRocPoint(10);
Sistem.PriceRocPoint(Veriler, 10);

Price Volume Trend - Sistem.PriceVolumeTrend()csharp

Sistem.PriceVolumeTrend();
Sistem.PriceVolumeTrend(Veriler);

Örnek:csharp

var PVT = Sistem.PriceVolumeTrend();
Sistem.Cizgiler[0].Deger = PVT;

Profit Factor - Sistem.ProfitFactor()csharp

Sistem.ProfitFactor;                          // tüm sistemin Profit Factorü
Sistem.ProfitFactorBar(BarSayisi, Kayma);     // Girilen bar sayısı kadar öncesine ait PF
Sistem.ProfitFactorIslem(IslemSayisi, Kayma); // Belli işlem sayısı için PF
Sistem.ProfitFactorList;                      // Her bir barda yeniden hesaplanan PF

Projection Bands (Up/Down) - Sistem.ProjectionUp / Down(14)csharp

Sistem.ProjectionDown(14);
Sistem.ProjectionDown(Veriler, 14);
Sistem.ProjectionUp(14);
Sistem.ProjectionUp(Veriler, 14);

Örnek:csharp

var Pro_DOWN = Sistem.ProjectionDown(14);
var Pro_UP = Sistem.ProjectionUp(14);
Sistem.Cizgiler[0].Deger = Pro_DOWN;
Sistem.Cizgiler[1].Deger = Pro_UP;

Projection Oscillator - Sistem.ProjectionOsc(14)csharp

Sistem.ProjectionOsc(14);
Sistem.ProjectionOsc(Veriler, 14);

Projection Bandwidth - Sistem.ProjectionBandwidth(14)csharp

Sistem.ProjectionBandwidth(14);
Sistem.ProjectionBandwidth(Veriler, 14);

PİVOT - Sistem.PVT01()csharp

Sistem.PVT01(Dönem);
Sistem.PVT01(Veriler, Donem);

Örnek: Haftalık Pivot seviyesinin 5 dklık grafik üzerinde çizdirilmesi;csharp

var Pivot = Sistem.PVT01("H");
Sistem.Cizgiler[0].Deger = Pivot;

Range Indicator - Sistem.RangeIndicator(10,3)csharp

Sistem.RangeIndicator(10, 3);
Sistem.RangeIndicator(Veriler, 10, 3);

RAVI - Sistem.RAVI()csharp

Sistem.RAVI(7, 65,"Simple");
Sistem.RAVI(Veriler, 7, 65,"Simple");

Relative Momentum Index - Sistem.RelativeMomIndex(20,5)csharp

Sistem.RelativeMomIndex(20, 5);
Sistem.RelativeMomIndex(Liste,20, 5);
Sistem.RelativeMomIndex(Veriler,20, 5);

Relative Vigor Index - Sistem.RelativeVigorIndex(14)csharp

Sistem.RelativeVigorIndex(14);
Sistem.RelativeVigorIndex(Veriler,14);

Relative Vigor Index Signal - Sistem.RelativeVigorIndexSignal(14)csharp

Sistem.RelativeVigorIndexSignal(14);
Sistem.RelativeVigorIndexSignal(Veriler, 14);

Relative Volatility Index - Sistem.RelativeVolatilityIndex(14,10)csharp

Sistem.RelativeVolatilityIndex(14, 10);
Sistem.RelativeVolatilityIndex(Veriler,14, 10);

RSI - Sistem.RSI(14)csharp

Sistem.RSI(14);
Sistem.RSI(Liste,14);
Sistem.RSI(Veriler,14);

ÖRNEK: RSI indikatörünün kendi ortalamasının üstünde/altında olmasıyla al/sat sinyalleri üreten bir sistem;csharp

var RSI = Sistem.RSI(40);
var AVR = Sistem.MA(RSI, "Simple", 10);
var SonYon = "";
for (int i=1; i < Sistem.BarSayisi; i++)
{
    if (RSI[i] > AVR[i] && SonYon != "A")
    {
        SonYon = "A";
        Sistem.Yon[i] = "A";
    }
    else if (RSI[i] < AVR[i] && SonYon != "S")
    {
        SonYon = "S";
        Sistem.Yon[i] = "S";
    }
}

RSI Denvelope Kanal - Sistem.RSIDenvelope Up/Mid/Down()csharp

Sistem.RSIDenvelopeDown(Veriler, "Simple", 14, 2);
Sistem.RSIDenvelopeMid(Veriler, "Simple", 14, 2);
Sistem.RSIDenvelopeUp(Veriler, "Simple", 14, 2);

R Squared - Sistem.RSquared(14)csharp

Sistem.RSquared(14);
Sistem.RSquared(Veriler,14);

REF - Sistem.Ref(Liste, Barsayısı)Bir veri listesinin elemanlarını ileriye veya geriye (sağa veya sola) istenilen eleman (bar) kadar kaydırmak için kullanılır.ÖRNEK:csharp

var Veriler = Sistem.GrafikVerileri;
var C = Sistem.GrafikFiyatOku(Veriler,"Kapanis");
var O = Sistem.GrafikFiyatOku(Veriler,"Acilis");
var RefC = Sistem.Ref(C,-1);
var RefO = Sistem.Ref(O,-1);

Renk - Sistem.Renk(Opaklık Oranı, Kırmızı, Yeşil, Mavi)csharp

Sistem.Renk(Opaklık, Kırmızı, Yeşil, Mavi);

Örnekler:csharp

SARI = Sistem.Renk(255,255,255,1);
BEYAZ = Sistem.Renk(255,250,250,250);
MAVİ = Sistem.Renk(255,50,100,200);
YEŞİL = Sistem.Renk(240,120,255,1);
KIRMIZI = Sistem.Renk(255,255,0,0);
SİYAH = Sistem.Renk(255,0,0,0);
TURUNCU = Sistem.Renk(255,255,128,0);
MOR = Sistem.Renk(255,128,0,128);
PEMBE = Sistem.Renk(255,255,0,128);
KAHVERENGİ = Sistem.Renk(255,128,0,0);
TURKUAZ = Sistem.Renk(255,0,255,255);

Ayrıca Color.Red, Color.White şeklinde İngilizce isimlerle de kullanılabilir.Renk Listesi - Sistem.RenkListesi()ÖRNEK: Super Trend indikatörü fiyatın üstünde iken başka, altında iken başka renkle çizilsin.csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var ST = Sistem.SuperTrend(3, 10, 10);
var RenkListesi = new List<Color>();
for (int i = 0; i < Sistem.BarSayisi; i++)
    RenkListesi.Add(Color.Gold);
for (int i = 1; i < C.Count; i++)
{
    if (ST[i] > C[i])
        RenkListesi[i] = Color.LightGreen;
    else if (ST[i] < C[i])
        RenkListesi[i] = Color.Red;
}
Sistem.Cizgiler[0].Deger = ST;
Sistem.Cizgiler[0].RenkListesi = RenkListesi;

Resim Ekle - Sistem.ResimEkle(DosyaAdı, Panel, X, Y)csharp

Sistem.ResimEkle(DosyaAdi, Panel, X, Y);

ÖRNEK:csharp

if (SonYon == "A")
    Sistem.ResimEkle("D:\\boga.png",1,0,40);
else if (SonYon == "S")
    Sistem.ResimEkle("D:\\ayi.png",1,0,40);
else
    Sistem.ResimEkle("D:\\manzara.png",1,0,40);

Robot Fonksiyonları - Sistem.Robotxxxxxcsharp

Sistem.RobotHisseAktifAcigaVar(SistemAdi, BazSembol, EmirSembol, Periyot, Miktar);
Sistem.RobotHisseAktifAcigaYok(SistemAdi, BazSembol, EmirSembol, Periyot, Miktar);
Sistem.RobotViopAktif(SistemAdi, BazSembol, EmirSembol, Periyot, Miktar);
Sistem.RobotViopTumGun(SistemAdi, BazSembol, EmirSembol, Periyot, Miktar);
Sistem.RobotViopGunSonuKapat(SistemAdi, EmirSembol);
Sistem.RobotStop();

Örnekler:csharp

var Islem = Sistem.RobotHisseAktifAcigaVar("MA1", "IMKBH'GARAN", "IMKBH'GARAN", "5", 100);
var Islem = Sistem.RobotViopAktif("MA1", "VIP’VIP-X030", "VIP'F_XU0300221", "5", 1);

Saat - Sistem.Saat()csharp

var saat = Sistem.Saat;
Sistem.Mesaj(saat);

ÖRNEK:csharp

if (Sistem.Saat.CompareTo("11:05:30") >= 0)
{
    // işlem yap
}

Saat Aralığı - Sistem.SaatAraligi(“15:00”, “16:30”)csharp

if (Sistem.SaatAraligi("09:30", "09:50") || Sistem.SaatAraligi("17:50", "18:00"))
{
    // işlem yapma
}
else
{
    // çalışmalarınız
}

Satış Fiyat / Satış Lotcsharp

var X = Sistem.SatisFiyat(Sembol);
var X = Sistem.SatisLot(Sembol);

Sayı Listesi - Sistem.SayiListesi()csharp

var x = Sistem.SayiListesi;

Sayı Tablosu Oku / Güncellecsharp

var Defa = Sistem.SayiTablosunuOku(Anahtar);
Sistem.SayiTablosunuGuncelle(Anahtar, 1);

Sayı Yuvarla - Sistem.SayiYuvarla(Sayı, Adım)csharp

var x = Sistem.SayiYuvarla(sayi, 0.005);

