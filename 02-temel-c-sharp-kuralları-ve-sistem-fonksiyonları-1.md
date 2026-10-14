markdown

# iDeal Sistem Modülünde Temel C# Kuralları

iDeal Sistem modülünde formüller **C#** yazılım dili kurallarına göre yazılır.  
iDeal’in hazır fonksiyonlarının yanında, C# dilinin neredeyse tüm komutlarını da kullanabilirsiniz.

---

## Formül Yazmanın Genel Akışı

1. Kullanıcı tarafından girilecek tüm ön tanımlı bilgiler (sayı, fiyat, periyot, sembol vb.) birer **değişken** olarak tanımlanır.
2. iDeal’den okunacak veriler (grafik, indikatör, derinlik vb.) ilgili fonksiyonlarla okutulup değişkene atanır.
3. Hesaplama sonucu oluşacak yeni bilgiler için de değişken tanımlanır.
4. Her değişken tanımlandığında mutlaka bir başlangıç değeri verilir (0, "", null olabilir).
5. Değişken tanımı şu şekilde yapılır:

```csharp
DEGISKEN_TIPI DegiskenAdi = Deger;

Örnekler:csharp

var x = 0;
var y = "";
int adet = 25;
float fiyat = 1.52f;
double oran = 0.1254;
string mesaj = "merhaba ideal";

Değişken TipleriTip
Açıklama
Örnek
var
Genel / otomatik tip
var x = 0;
int
Tam sayı
int adet = 25;
float
Kesirli sayı
float fiyat = 1.52f;
double
Yüksek hassasiyetli kesirli sayı
double oran = 0.12;
string
Metin
string mesaj = "selam";

if / else if / else (Koşullar)csharp

if (x < y)
{
    a = -5;
    b = 10;
}
else if (x == y)
{
    a = 0;
}
else
{
    a = 5;
    b = 100;
}

Mantıksal Operatörler>  Büyüktür
<  Küçüktür
== Eşittir
!= Eşit değildir
>= Büyük eşit
<= Küçük eşit
&& VE
|| VEYA

Not: Birden fazla satır işlem yapılacaksa mutlaka { } süslü parantez kullanılır.
Listeler (Liste)Grafik verileri ve indikatörler birer listedir.csharp

var Listem = Sistem.Liste(0);           // Tüm elemanları 0 olan boş liste
var SEVIYE = Sistem.Liste(95.400);      // Yatay çizgi için
Sistem.Cizgiler[0].Deger = SEVIYE;

Liste elemanına erişim:csharp

KAPANISLAR[350]          // 350. barın değeri
KAPANISLAR[i]            // i. barın değeri

Döngüler (for)En çok kullanılan kalıp:csharp

for (int i = 1; i < Sistem.BarSayisi; i++)
{
    // Her bar için yapılacak işlemler
    Sonuc[i] = (H[i] + L[i] + C[i]) / 3;
}

Geriye doğru döngü:csharp

for (int i = Sistem.BarSayisi - 1; i > 0; i--)
{
    // ...
}

Ondalıklı artış:csharp

for (double x = 0.25; x <= 10; x += 0.05)
{
    // ...
}

İndikatör ve Liste Kullanım ŞekilleriÇoğu indikatör 3 farklı şekilde çağrılabilir:csharp

Sistem.RSI(14);                  // 1. En yaygın (üzerine uygulanan grafiğin kapanışını kullanır)
Sistem.RSI(Liste, 14);           // 2. Belirli bir liste üzerinden
Sistem.RSI(Veriler, 14);         // 3. Başka sembol veya periyot verisi üzerinden

Sistem Fonksiyonları (1. Bölüm)AccumulationDistributioncsharp

Sistem.AccumulationDistribution();
Sistem.AccumulationDistribution(Veriler);

ADRcsharp

Sistem.ADR(14);
Sistem.ADR(Veriler, 14);

ADXcsharp

Sistem.ADX(14);
Sistem.ADX(Liste, 14);
Sistem.ADX(Veriler, 14);

AktifViopKontratcsharp

var Sembol = Sistem.AktifViopKontrat;
Sistem.Mesaj(Sembol);

AlgoAciklama (Hata yakalama)csharp

try
{
    // kodlar
}
catch (Exception error)
{
    Sistem.AlgoAciklama = error.Message + "\r\n" + error.StackTrace;
}

AlisFiyat / AlisLotcsharp

var Sembol = "IMKBH'GARAN";
var AlisFiyati = Sistem.AlisFiyat(Sembol);
var AlisLotu  = Sistem.AlisLot(Sembol);

Alligatorcsharp

var cizgi1 = Sistem.Alligator1();   // Çene (Mavi)
var cizgi2 = Sistem.Alligator2();   // Diş (Kırmızı)
var cizgi3 = Sistem.Alligator3();   // Dudak (Yeşil)

Aroon Up / Down / Oscillatorcsharp

var Up   = Sistem.AroonUp(14);
var Down = Sistem.AroonDown(14);
var Osc  = Sistem.AroonOsc(14);

AsagiKestiysecsharp

Sistem.AsagiKestiyse(Liste1, Liste2);
Sistem.AsagiKestiyse(Liste, 0);     // Sabit seviye

AverageTrueRange (ATR)csharp

Sistem.AverageTrueRange(14);
Sistem.AverageTrueRange(Liste, 14);
Sistem.AverageTrueRange(Veriler, 14);

AwesomeOsccsharp

Sistem.AwesomeOsc(5, 34);
Sistem.AwesomeOsc(Veriler, 5, 34);

BaglantiVarcsharp

if (Sistem.BaglantiVar)
    Sistem.Mesaj("Yayın var");
else
    Sistem.Mesaj("Yayın yok");

BarCizcsharp

Sistem.BarCiz(Panel, BarTip, AcilisListe, YuksekListe, DusukListe, KapanisListe, YukselenRenk, DusenRenk);
// BarTip: 0=OHLC, 1=Mum, 2=HeikinAshi

BarRengicsharp

Sistem.BarRengi(BarNo, Color.Cyan, 3, 1);

BarSayisicsharp

var ToplamBar = Sistem.BarSayisi;

Bilanco İndikatörlericsharp

Sistem.BilancoFK();
Sistem.BilancoPDDD();
Sistem.BilancoOzSerm();
Sistem.BilancoOdenmisSerm();
Sistem.BilancoNetKar();
Sistem.BilancoPD();

BistHesapOkucsharp

var BistHesap = Sistem.BistHesapOku();
if (BistHesap != null)
{
    var Bakiye     = BistHesap.Bakiye;
    var IslemLimit = BistHesap.IslemLimit;
    var Pozisyonlar = BistHesap.Pozisyonlar;
    // ...
}

Bollinger Bantlarıcsharp

// Alt Band
Sistem.BollingerDown("Simple", 20, 2);
// Üst Band
Sistem.BollingerUp("Simple", 20, 2);
// Orta Band
Sistem.BollingerMid("Simple", 20, 2);
// Band Genişliği
Sistem.BollingerWidth(20, 2);

