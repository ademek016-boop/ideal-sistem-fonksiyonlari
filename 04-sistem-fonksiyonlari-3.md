markdown

# Sistem Fonksiyonları (Devam - 4. Bölüm)

### Ease Of Movement - Sistem.EaseOfMovement(10)

Ease Of Movement olarak bilinen indikatörünü çağırır. 1 adet parametre alır ve varsayılan parametre olarak 10 kullanılır. Hesaplamasında HACİM verisi kullandığından En Az DÜZEY1+ lisans gerektirir. Aksi durumda hacim içeren barlar gecikmeli olarak kendini günceller. Aşağıdaki gibi 2 kullanım şekli vardır;

```csharp
Sistem.EaseOfMovement(10);
Sistem.EaseOfMovement(Veriler,10);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var EOM = Sistem.EaseOfMovement(10);
Sistem.Cizgiler[0].Deger = EOM;

Ehler’s Diff Coef Filter - Sistem.EhlersEhlersDiffCoefFilter()Ehler’s Filter olarak bilinen indikatörünü çağırır. Hiç parametre almaz. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.EhlersFilter();
Sistem.EhlersFilter(Veriler);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var EF = Sistem.EhlersFilter();
Sistem.Cizgiler[0].Deger = EF;

Ehler’s Filter - Sistem.EhlersFilter()Ehler’s Diff Coef Filter olarak bilinen indikatörünü çağırır. Hiç parametre almaz. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.EhlersEhlersDistCoefFilter();
Sistem.EhlersEhlersDistCoefFilter(Veriler);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var EDCF = Sistem.EhlersEhlersDistCoefFilter();
Sistem.Cizgiler[0].Deger = EDCF;

Elliot Wave Oscilator (EWO) - Sistem.ElliotWaveOscilator(15,34)Elliot Wave Oscilator (EWO) olarak bilinen indikatörünü çağırır. 2 adet parametre alır. Aşağıdaki gibi 2 kullanım şekli vardır;csharp

Sistem.ElliotWaveOscillator(12, 26);
Sistem.ElliotWaveOscillator(Veriler,12, 26);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var ewo = Sistem.ElliotWaveOscillator();
Sistem.Cizgiler[0].Deger = ewo;

Emir - Sistem.Emir FonksiyonlarıRobot üzerinden emir göndermek için bir emir iletmek için gerekli bütün bilgileri tanımlamak (emir adedi, emir fiyatı, emrin süresi, emrin sembolü, emrin yönü gibi) ve emri göndermek için Sistem Emir fonksiyonları kullanılır. Bu fonksiyonların tek başlarına kullanım alanları yoktur. Birkaç tanesi birlikte kullanılırlar. Emir fonksiyonlarının tam listesi ve yazım şekilleri aşağıdaki gibidir;Hisse/Varant/Viop hepsi için geçerli emir fonksiyonları:csharp

Sistem.EmirSembol = "IMKBH'GARAN";
Sistem.EmirIslem = "Alış";
Sistem.EmirMiktari = 1;
Sistem.EmirTipi = "Limit"; //veya Piyasa
Sistem.EmirFiyati = 9.50;
Sistem.EmirSuresi = "GUN";
Sistem.EmirAltHesap = "1";
Sistem.EmirHesapAdi = "123456, ABC YATIRIM";
Sistem.EmirSatisTipi = "ACIGA";
Sistem.Emir = "GUN";

Sadece VIOP için geçerli Emir Fonksiyonlarıcsharp

Sistem.EmirAksamSeansi = 1; //VERİLEN EMRİN AKŞAM SEANSINDA DA GEÇERLİ OLMASI İÇİN
Sistem.EmirSartBool = true;
Sistem.EmirSartSembol = "VIP'F_XU0300620";
Sistem.EmirSartFiyat = 132.500;
Sistem.EmirSartTipi = "ALIŞ >= Şart Fiyat";
Sistem.EmirGonder();

Notlar:Piyasa emirlerinde SÜRE = KIE olmalıdır
Piyasa emri verilirken Emir Fiyat komutu gereksizdir.
Emir Fiyat Tipi komutu BIST’ın nasdaq öncesi dönemden kalmadır, işlevsizdir.
Hisselerde Açığa Satış yapılacaksa Emir Satış Tipi = “ACIGA” yazılmalıdır. Normal satış için bu komutu yazmaya gerek yoktur.
Emri Sembol yazılırken Piyasakodu’SenetKodu şeklinde yazılmalıdır ve çift tırnak içine alınmalıdır. (“IMKBH’GARAN” veya “VIP’F_XU0300620”)
Fiyata emir verirken Hisse için EmirTipi = Limit, VIOP için Emir Tipi = Limitli Yazılır
Emir Gönder komutu diğer emir özellikleri ataması yapılmış olan bilgileri bir araya getirip emri aracı kuruma iletir. İDEAL Portföy penceresinde login olunmuş ve seçili durumda olan hesap ne ise emir o hesaba iletilir.
Seçili olunmuş hesaptan başka bir hesaba emir iletilmek isteniyorsa EmirHesapAdi ve EmirAltHesap satırlarına emrin gönderilmek istendiği hesabın bilgileri ÇİFT TIRNAK işaretleri içinde yazılır.

Örnek: Her 10 saniyede bir aktiften (piyasa emri) DOHOL alış emri gönderen robot koducsharp

var Sembol = "IMKBH'DOHOL";
var Miktar = 1;
if (Sistem.ZamanKontrolSaniye(Sembol) >= 10)
{
    Sistem.ZamanKontrolGuncelle(Sembol);
    Sistem.EmirSembol = Sembol;
    Sistem.EmirIslem = "Alış";
    Sistem.EmirTipi = "Piyasa";
    Sistem.EmirSuresi = "KIE";
    Sistem.EmirGonder();
}

Örnek 2: VIOP AKBANK Vadeli kontrata her 10 saniyede bir Şartlı emir gönderen kodcsharp

var Sembol = "VIP'F_AKBNK0520";
if (Sistem.ZamanKontrolSaniye(Sembol) >= 10)
{
    Sistem.ZamanKontrolGuncelle(Sembol);
    var Miktar = 1;
    Sistem.EmirSembol = Sembol;
    Sistem.EmirIslem = "Alış";
    Sistem.EmirMiktari = Miktar;
    Sistem.EmirSuresi = "GUN"; // GUN, SNS, IKG
    Sistem.EmirTipi = "Limitli"; // KPY, KIE, GIE, SAR
    Sistem.EmirFiyati = 5.10;
    Sistem.EmirSartSembol = Sembol;
    Sistem.EmirSartBool = true;
    Sistem.EmirSartFiyat = 4.50;
    Sistem.EmirSartTipi = "ALIŞ >= Şart Fiyat";
    Sistem.EmirGonder();
}

En Çok Tekrar - Sistem.EncokTekrarBir sembolün herhangi bir periyotlu grafiğinde, bir fiyatın veya bir indikatörün, en fazla sayıda tekrar etmiş olan değerini bulmaya yarayan ideal sistem fonksiyonudur. İki ayrı kullanım şekli vardır. Birinci kullanım şekli bir fiyat/indikatör listesinin son x adet barı içinde en çok tekrar eden değeri veren ve bu sayede bir indikatör olarak da çizilebilen kullanımdır. Tıpkı 14’lük ortalama çizdirmek gibi, hep son 14 bar içindeki en çok tekrar eden kapanış değerini alarak çiz diyerek en çok tekrar isimli bir indikatör elde edebiliriz. İkinci kullanım şekli ise, bir listenin TAMAMININ içindeki en çok sayıda tekrar eden değeri (tek bir değer) elde etmek amaçlı kullanımdır. Kullanım şekillerinin yazım yöntemleri aşağıdaki gibidir;csharp

Sistem.EnCokTekrar(Liste, 14);
Sistem.EnCokTekrarDeger(Liste);

Örnek 1: Son 50 bar içerisinde en çok kez görülen kapanış değerlerini indikatör olarak hesaplayıp çizen kod.csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var IND = Sistem.EnCokTekrar(C,50);
Sistem.Cizgiler[0].Deger = IND;

Örnek 2: Son 1000 bar içerisinde hangi yüksek seviyesi kaç kez görüldü bilgisini bu komutu kullanmadan elde etmek:csharp

var V = Sistem.GrafikVerileri;
var H = Sistem.GrafikFiyatSec("Yuksek");
var Tekrarlar = new SortedDictionary<float, int>();
for (int i = V.Count-1000; i < V.Count; i++) // v.count-1000 den başlamak son 1000 bar demektir.
{
    float Fiyat = H[i];
    int Defa = 0;
    if (Tekrarlar.ContainsKey(Fiyat))
        Defa = Tekrarlar[Fiyat];
    Defa++;
    Tekrarlar[Fiyat] = Defa;
}
var Str = string.Join("\r\n", Tekrarlar.Select(x => x.Key.ToString("0.00") + "\t " + x.Value.ToString()).ToList());
Sistem.Mesaj(Str);

Örnek 3: Bir grafikte 100’lük HHV indikatörünün en çok sayıda tekrar eden değerini öğrenmek:
Not: Saatlik GARAN grafiğine uygulanmıştır.csharp

var HHV = Sistem.HHV(100, "Yuksek");
var X = Sistem.EnCokTekrarDeger(HHV);
Sistem.Mesaj(X.ToString("0.00"));

Envelope Up/Down/Mid - Sistem.Envelope()Envelope isimli üst/alt/orta banddan oluşan 3 çizgisi olan indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. Biri içerisinde kullanılan Moving Average Yöntemi diğer ikisi de periyot olmak üzere 3 parametre alır. Mid (Orta), Alt (Down) ve Up (Üst) band çizgilerinin her biri için ayrı bir fonksiyon vardır. Hesaplanmasında barların OHLC değerlerinin tamamı gerektiği için Liste kullanım tipi yok, Veriler (başka bir sembolün veya periyodun bar verileri) kullanım şekli vardır. Tüm kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.EnvelopeDown("Simple", 25, 5);
Sistem.EnvelopeDown(Veriler , "Simple", 25, 5);
Sistem.EnvelopeMid("Simple", 25, 5);
Sistem.EnvelopeMid(Veriler,"Simple", 25, 5);
Sistem.EnvelopeUp("Simple", 25, 5);
Sistem.EnvelopeUp(Veriler,"Simple", 25, 5);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var Alt = Sistem.EnvelopeDown("Simple", 25, 5);
var Orta = Sistem.EnvelopeMid("Simple", 25, 5);
var Ust = Sistem.EnvelopeUp("Simple", 25, 5);
Sistem.Cizgiler[0].Deger = Alt;
Sistem.Cizgiler[1].Deger = Orta;
Sistem.Cizgiler[2].Deger = Ust;

Excel Kopyala - Sistem.ExcelKopyalaiDeal programından okutulan veya hesaplatılan bilgilerin bir excel dosyasına yazdırılması amacıyla kullanılan ideal fonksiyonudur. Fonksiyonun kullanımında CELL (Hücreler) içerisinde satır ve sütun bazında yazdırılacak verileri elde edildikten sonra (hücre dizisi) bu hücre dizisinin (cell array) bir dosya adıyla birlikte komutun içine yazılması yöntemi kullanılır. Fonksiyonun yazım şekli aşağıdaki gibidir;Not: İşletim sisteminin kütüphanesi kullanılarak EXCEL uygulaması çağırılıp ekrana açılır ve kodda girdi olarak verilen bilgiler hücrelere yazılmış olarak excel gösterilir. Farklı Excel sürümlerinde excel açma, içine veri yazma ve kaydetme izinleri çalışmayabilir.csharp

Sistem.ExcelKopyala(cellarray, DosyaAdı);

Örnek: Bütün hisse senetlerinin YIL YÜKSEK bilgisini okuyan ve İDEAL klasörü altında oluşturulacak YUKSEKLER isimli bir excel dosyasına kaydeden kod.Not: Formülü yazıp FORMÜL TEST butonuna basmak, kod içinde yazılı işlemlerin bir defalığına yapılması için yeterlidir. Formülü robot olarak çalıştırmak veya sistem olarak grafiğe uygulamak gerekmez.csharp

var Liste = Sistem.YuzeyselListeGetir("IMKBH'");
var ExcelFileName = "\\ideal\\YUKSEKLER.Xlsx";
var cellarray = new object[Liste.Count + 1, 10];
for (int i = 0; i < Liste.Count; i++)
{
    cellarray[i + 1, 0] = Liste[i].Symbol;
    cellarray[i + 1, 1] = Liste[i].HighYear.ToString();
}
Sistem.ExcelKopyala(cellarray, ExcelFileName);

Kodun açıklamasını satır satır şöyle yapabiliriz;
1.Satır: Bu satırda piyasası IMKBH olan (tüm hisseler, varantlar) tüm semboller için Yüzeysel Veri Listesi çağrılır
2.Satır: Açılacak ve kaydedilecek excel dosyasının diskteki yeri ve isminden oluşan bir DOSYA ADI (File Name) tanımlanır.
3.Satır: İlk satırda çağrılan sembol verilerini içeren Listenin eleman sayısından 1 fazla sayıda satırı, 10 sütunu olan bir excel objesi (nesnesi) tanımlanır. (cell/hücre dizisi) (sütun sayısı rastgele 10)
4.5.6.7.8.Satırlar: Bu satırlarda bir FOR DÖNGÜSÜ vardır. İlk satırda çağrılan sembol verilerini içeren Listenin elemanlarının HER BİRİ İÇİN DÖN ve her biri için, onun liste içindeki satır numarasına denk gelen numaranın 1 fazla numaralı excel satırına  SIFIR nolu sütuna (ilk sütun) sembolü yaz  
1BİR nolu sütuna (ikinci sütun) YÜZEYEL LİSTE isimli veri listesinini Yıl Yüksek bilgisini (HighYear) yaz
9.Satır: ExcelKopyala fonksiyonunun çalışması için gereken input (giriş) bilgilerini fonksiyona ver (hücre dizisini ve dosya adını)

Excel Oku - Sistem.ExcelOkuBilgisayarınızda var olan bir Excel dosyasını okumak, söz konusu Excel dosyası içerisinden okutulacak verilere göre işlemler yapmak amacıyla kullanılan ideal fonksiyonudur. Fonksiyonun yazım şekli aşağıdaki gibidir;csharp

Sistem.ExcelOku(Kitap.Xlsx);

Örnek: Bazı hisse senetlerini, kendi belirleyeceğimiz fiyat seviyelerine düşünce alan, yine bizim excel sütunlarına yazacağımız KAR AL veya STOP fiyatlarına gelince SATAN ve pozisyonu kapatan, her bir hisse için kaç lot işlem yapacağımızı da EXCEL üzerinden okuyarak çalışan bir ideal robot kod örneği:csharp

string FileName = "C:\\ideal\\HisseRobot.Xlsx";
bool DevamEt = true;
if (DateTime.Now.DayOfWeek == DayOfWeek.Saturday) DevamEt = false;
if (DateTime.Now.DayOfWeek == DayOfWeek.Sunday) DevamEt = false;
if (DateTime.Now.ToString("HHmm").CompareTo("1000") <= 0) DevamEt = false;
if (DateTime.Now.ToString("HHmm").CompareTo("1800") >= 0) DevamEt = false;
if (DateTime.Now.ToString("HHmm").CompareTo("1300") >= 0 &&
DateTime.Now.ToString("HHmm").CompareTo("1400") <= 0) DevamEt = false;
if (System.IO.File.Exists(FileName) == false) DevamEt = false;
if (DevamEt)
{
    var ExcelArray = Sistem.NesneGetir(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"));
    if (ExcelArray == null)
    {
        ExcelArray = Sistem.ExcelOku(FileName);
        Sistem.NesneKaydet(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"), ExcelArray);
    }

    //Excel dosyasını 10 dk'da bir tekrar oku.
    if (Sistem.ZamanKontrolDakika(FileName + ";" + DateTime.Now.ToString("yyyyMMdd")) >= 5)
    {
        Sistem.ZamanKontrolGuncelle(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"));
        ExcelArray = Sistem.ExcelOku(FileName);
        Sistem.NesneKaydet(FileName + ";" + DateTime.Now.ToString("yyyyMMdd"), ExcelArray);
    }
    int SatirSayisi = ExcelArray.GetLength(0); //satırların sayısını bul
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
        if (sonfiyat <= AlisFiyat && Pozisyon == 0 && Rezerv == "") // AL
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

Söz konusu örnek robot tarafından okutulan Excel dosyası içi aşağıdaki fotoda gösterildiği gibidir;Yukarıdaki Örnek Robot kodu için ek bilgiler;
*İlk satırda bilgilerimizi yazdığımız Excel dosyasının adı ve bilgisayarımızda bulunduğu yer tanımlanmıştır.
*Sonraki satırda, aşağıda belirtilen zaman aralıklarında (hafta sonları, sabah 10’dan önce ve akşam 18’den sonra) robotun çalışmaması ayarları yapılmıştır.
Diğer Satırlar idealin Sistem ve Robot kütüphanesinde henüz bu sayfaya kadar anlatılmamış başka bazı fonksiyonları, C# dilinin bazı komutları kullanılmış olsa da, bu amaçla söz konusu örneği kullanmak isteyenlerin değiştirmeden kullanabileceği kodlardır.Fark - Sistem.Fark(Sembol)Bir sembolün o an ki fiyata göre çeşitli dönemlere göre DEĞİŞİMİNİ (Puan veya Para olarak nominal) farkını okumak için kullanılır. Fonksiyonun içine FARKI/DEĞİŞİMİ okutulmak istenen sembol yazılır.Kullanım şekilleri aşağıdaki gibidir.csharp

Sistem.FarkAltiAy(Sembol);
Sistem.FarkBirAy(Sembol);
Sistem.FarkBirHafta(Sembol);
Sistem.FarkBirYil(Sembol);
Sistem.FarkBuAy(Sembol);
Sistem.FarkBuHafta(Sembol);
Sistem.FarkBuYil(Sembol);
Sistem.FarkGun(Sembol);
Sistem.FarkSeans(Sembol);
Sistem.FarkUcAy(Sembol);

Örnek: XU100 endeksinin, son 1 haftalık FARK/DEĞİŞİM değerini okuyup ekrana mesaj olarak çıkaran kod:csharp

var Degisim = Sistem.FarkBirHafta(“IMKBX’XU100”);
Sistem.Mesaj("1 Haftalık değişim = " + Degisim.ToString());

Fibonacci Up/Down/Mid - Sistem.Fibonacci()Fibonacci bands olarak bilinen üst/alt/orta çizgilerden oluşan indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. Varsayılan değeri 1 olan 1 adet parametre alır. Mid (Orta), Alt (Down) ve Up (Üst) band çizgilerinin her biri için ayrı bir fonksiyon vardır. Hesaplanmasında barların OHLC değerlerinin tamamı gerektiği için Liste kullanım tipi yok, Veriler (başka bir sembolün veya periyodun bar verileri) kullanım şekli vardır. Tüm kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.FibonacciDown(1);
Sistem.FibonacciDown(Veriler,1);
Sistem.FibonacciMid(1);
Sistem.FibonacciMid(Veriler,1);
Sistem.FibonacciUp(1);
Sistem.FibonacciUp(Veriler,1);

Örnek: İndikatörün 3 bandını da çağırıp çizdirmekcsharp

var Alt = Sistem.FibonacciDown(1);
var Orta = Sistem.FibonacciMid(1);
var Ust = Sistem.FibonacciUp(1);
Sistem.Cizgiler[0].Deger = Alt;
Sistem.Cizgiler[1].Deger = Orta;
Sistem.Cizgiler[2].Deger = Ust;

Forecast Osc - Sistem.ForecastOsc(5)Forecast Oscilator isimli indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. 1 adet parametre alır ve varsayılan değeri 5’tir. Hesaplanmasında barların OHLC değerlerinin tamamı gerektiği için Liste kullanım tipi yok, Veriler (başka bir sembolün veya periyodun bar verileri) kullanım şekli vardır. Grafik barları üzerine değil, alt bölgedeki panellere çizilir. Kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.ForecastOsc(5);
Sistem.ForecastOsc(Veriler,5);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var X = Sistem.ForecastOsc(5);
Sistem.Cizgiler[0].Deger = X; //Panel no 2 yapılmalıdır

FRAMA - Sistem.FRAMA()FRAMA isimli indikatörü hesaplayıp çağıran ideal sistem fonksiyonudur. Hiç Parametre almaz. Grafik barları üzerine çizilerek FİYAT ile korelasyonu takip edilir. Kullanım ve yazım şekilleri aşağıdaki gibidir;csharp

Sistem.FRAMA();
Sistem.FRAMA(Veriler);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var X = Sistem.FRAMA();
Sistem.Cizgiler[0].Deger = X; //Panel no 1 yapılmalıdır

FX Sniper - Sistem.FxSniper(3,2)FX Sniper isimli indikatörünü çağırır. 2 adet parametre (periyot) alır ve varsayılan değer olarak 3 ve 2 kullanılır. Aşağıdaki gibi 3 kullanım şekli vardır;csharp

Sistem.FxSniper(3,2);
Sistem.FxSniper(Liste,3,2);
Sistem.FxSniper(Veriler,3,2);

Örnek: İndikatörü çağırıp çizdirmekcsharp

var FX = Sistem.FxSniper(3,2);
Sistem.Cizgiler[0].Deger = FX;

Getiri Hesapla (GetiriKZ) - Sistem.GetiriHesapla(Tarih, Komisyon)Sistem.GetiriHesapla fonksiyonu, bir sistem formülüne yazıldığı zaman, sisteminizin stratejisi, bu fonksiyona girilen başlangıç tarihinden itibaren son bara kadar hesaplanır ve bir kayıt listesinde tutulur. Hesaplama yapılırken, komisyon veya kayma maliyetlerini de girerseniz, o maliyetleri çıkarıp getiri hesabı yapar.Stratejinizi yazıp grafiğe uyguladınız, al/sat noktalarını görüyorsunuz, ama hemen altında da getiri/KZ grafiği eklemek istiyorsanız bu fonksiyonla getiriyi hesaplatabilirsiniz. Geriye kalan, sonucu bir çizgiye atamak.Fonksiyonu, başlangıç tarihi ve isteniyorsa kayma/komisyon maliyeti için işlem başına düşülmesi istene puan/fiyat bilgisi girerek, aşağıdaki şekilde kullanabilirsiniz:csharp

Sistem.GetiriHesapla("01/01/2010", 0.00);

Yapmanız gereken şey getiriyi (istediğiniz bir tarihten başlayarak) hesaplayan fonksiyonu sisteminizin en altına yazmak ve sonucu bir çizgiye atamaktır. (Yani kodunuza aşağıdaki gibi iki satırı eklemek)Bu iki satır aşağıdaki gibidir:csharp

Sistem.GetiriHesapla("10/10/2010",0.000);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZ; //Panel No= 2

Getiri Hesapla fonksiyonu, sistemin işleme girdiği barları, işleme girdiği fiyatları, pozisyonun yönünü ve miktarını bulur ve çeşitli listelere doldurur. Tek başına bu fonksiyonu koda yazmak yeterli değildir. Bu fonksiyon ile elde edilen/hesaplanan verilerin her birine aşağıda tam listesi verilen birer alt fonksiyon ile ulaşılır. Bu fonksiyonların bazıları liste verir (grafiğe çizgi olarak çizdirilenler). Bir kısmı ise sayı/oran/değer gibi bir tek sonuç (performans analiz raporundaki önemli bilgiler) olarak elde edilir. (formülde kullanmak, ekrana yazdırmak vs. amaçlı)GRAFİĞE ÇİZDİRİLEBİLEN GETİRİ HESAPLA LİSTELERİ:csharp

Sistem.GetiriKZGunSonu;
Sistem.GetiriKZGunBasi;
Sistem.GetiriKZAySonu;
Sistem.GetiriKZAyNet;
Sistem.GetiriKZYilNet;
Sistem.GetiriKZPoz;
Sistem.GetiriKZGun;
Sistem.GetiriKZAy;
Sistem.GetiriKZYil;
Sistem.GetiriPozisyon;

PUAN/ORAN/SAYI VERİLERİ SUNAN GETİRİ HESAPLA SONUÇLARI:csharp

Sistem.GetiriIslemSayisiPoz;
Sistem.GetiriIslemSayisiGun;
Sistem.GetiriIslemSayisiAy;
Sistem.GetiriIslemSayisiYil;
Sistem.GetiriMutluGun;
Sistem.GetiriMutsuzGun;
Sistem.GetiriKarIslem;
Sistem.GetiriZararIslem;
Sistem.GetiriKarIslemOran;
Sistem.GetiriToplamIslem;
Sistem.GetiriKarMiktar;
Sistem.GetiriZararMiktar;
Sistem.ProfitFactor;
Sistem.GetiriNetKar;
Sistem.GetiriBuAy;
Sistem.GetiriBirAy;

markdown

**ÖRNEK1:** TOMA stratejisi kullanılan bir sistemde, GETİRİ HESAPLA fonksiyonu ile dönen listeler kullanılmış ve aşağıdaki örnekte ekrana çizdirilmiştir.

```csharp
var TOMA = Sistem.TOMA(3, 2.48);
var Veriler = Sistem.GrafikFiyatSec("Kapanis");
var EMA = Sistem.MA(Veriler, "Exp", 3);
Sistem.KesismeTara(EMA, TOMA);
var Renk2 = Sistem.Renk(60,255,0,100);
var Renk1 = Sistem.Renk(60,0,255,0);
Sistem.GetiriHesapla("01/01/2010", 0.00);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZ;
Sistem.Cizgiler[1].Deger = Sistem.GetiriKZPoz;
Sistem.Cizgiler[2].Deger = Sistem.GetiriKZGun;
Sistem.Cizgiler[3].Deger = Sistem.GetiriKZGun;
Sistem.Cizgiler[4].Deger = Sistem.GetiriKZYil;
Sistem.Cizgiler[5].Deger = Sistem.GetiriMiktar;
Sistem.Cizgiler[6].Deger = Sistem.GetiriPozisyon;

ÖRNEK2: SUM fonksiyonu kullanılarak oluşturulan bir strateji kullanan sistemde, her bir işlem bazında KAR/ZARARI çizdiren, Performans Analizindeki bazı önemli sonuçları da grafik üzerine yazdıran örnek (İLK 2 ÇİZGİ AKTİF EDİLİR, PANEL NUMARALARI 2 YAPILIR)csharp

var C = Sistem.GrafikFiyatSec("Kapanis");
var A = Sistem.Sum(C,10);
var B = Sistem.Liste(0);
for (int i=3; i<Sistem.BarSayisi; i++)
    B[i] = A[i]+A[i-1]-A[i-2]-A[i-3];
var SonYon ="";
for (int i=1; i<Sistem.BarSayisi; i++)
{
    if(B[i] > 3.60f && SonYon!="A")
    {
        Sistem.Yon[i] = "A";
        SonYon = "A";
    }
    else if(B[i] < -3.60f && SonYon!="S")
    {
        Sistem.Yon[i] = "S";
        SonYon = "S";
    }
}
Sistem.GetiriHesapla("01/01/2010",0.000);
var Renk1 = Sistem.Renk(150, 255, 255, 0);
var Renk2 = Sistem.Renk(255, 0, 255, 0);
var Renk3 = Sistem.Renk(255, 255, 0, 80);

Sistem.ZeminYazisiEkle("Toplam İşlem Sayısı = " + " " +Sistem.GetiriToplamIslem.ToString("0"), 1, 170, 20, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Kazandıran İşlem Sayısı = " + " " + Sistem.GetiriKarIslem.ToString("0"), 1, 170, 35, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Kaybettiren İşlem Sayısı = " + " " + Sistem.GetiriZararIslem.ToString("0"),1, 170, 50, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Karlı İşlem Miktarı = " + " " + Sistem.GetiriKarMiktar.ToString("0.000"), 1, 220, 85, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Zararlı İşlem Miktarı = " + " " + Sistem.GetiriZararMiktar.ToString("0.000"),1, 220, 100, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Karlı İşlem Oranı = " + "% " + Sistem.GetiriKarIslemOran.ToString("0.00"), 1, 220, 115, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Profit Factor = " + " " + Sistem.ProfitFactor.ToString("0.00"),1, 440, 40, Renk3, "Tahoma", 12);
Sistem.ZeminYazisiEkle("Net Kar = " + " " + Sistem.GetiriNetKar.ToString("0.000"),1, 440, 20, Renk2, "Tahoma", 15);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZPoz;
Sistem.Cizgiler[1].Deger = Sistem.Liste(0);
var Renk10 = Sistem.Renk(60,255,0,0);
var Renk20 = Sistem.Renk(60,0,255,0);
Sistem.DolguEkle(1,0,Renk10,Renk20);

ÖRNEK-3: Pozisyon bazında kar/zarar görmek için Sistemin altına eklenmiş satırlar: (Sistem bir pozisyona girdiği zaman, kapatılmış olan bir önceki pozisyonda ne kadar kar yada zarar edildiği gösterilir)csharp

Sistem.GetiriHesapla("10/10/2010",0.000);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZPoz;

ÖRNEK-4: Sisteminiz işlem yaptıkça, pozisyonunuz ne (kaç lot) olmuş, her sinyalde kaç adet/lot işlem yapılmış ve getiriniz nasıl seyretmiş bilgilerini görmek için Sistemin altına eklenmiş satırlar:csharp

Sistem.GetiriHesapla("10/10/2010",0.000);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZ;
Sistem.Cizgiler[1].Deger = Sistem.GetiriPozisyon;
Sistem.Cizgiler[2].Deger = Sistem.GetiriMiktar;

ÖRNEK-5: Sisteminiz pozisyon başına kar/zararı alt kısma çizmek, grafik zemini üzerine de performans analizi sonuçlarından elde edilen verileri yazmak için aşağıdaki kodları ekleyebilirsiniz.NOT: Çizgi renkleri, dolguda kullanılan renkler ve yazıların grafik üzerindeki x/y koordinatları sizin tercihinize ve ayarlarınıza göre değişen bilgilerdir.csharp

Sistem.GetiriHesapla("01/01/2010",0.000);
Sistem.GetiriMaxDDHesapla("01/10/2015","01/01/2020");
var MaxDD = Sistem.GetiriMaxDD.ToString();
var MaxDDTarihi = Sistem.GetiriMaxDDTarih.ToString("dd/MM/yyyy");
Sistem.ZeminYazisiEkle("Max DD = "+ MaxDD + "(Tarih =" + MaxDDTarihi + ")",1,80,350,Color.Gold,"Tahoma",20);
Sistem.ZeminYazisiEkle("Toplam İşlem Sayısı = " + " " +Sistem.GetiriToplamIslem.ToString("0"), 1, 170, 20, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Kazandıran İşlem Sayısı = " + " " + Sistem.GetiriKarIslem.ToString("0"), 1, 170, 35, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Kaybettiren İşlem Sayısı = " + " " + Sistem.GetiriZararIslem.ToString("0"),1, 170, 50, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Karlı İşlem Miktarı = " + " " + Sistem.GetiriKarMiktar.ToString("0.000"), 1, 220, 85, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Zararlı İşlem Miktarı = " + " " + Sistem.GetiriZararMiktar.ToString("0.000"),1, 220, 100, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Karlı İşlem Oranı = " + "% " + Sistem.GetiriKarIslemOran.ToString("0.00"), 1, 220, 115, Renk1, "Tahoma", 10);
Sistem.ZeminYazisiEkle("Profit Factor = " + " " + Sistem.ProfitFactor.ToString("0.00"),1, 440, 40, Renk3, "Tahoma", 12);
Sistem.ZeminYazisiEkle("Net Kar = " + " " + Sistem.GetiriNetKar.ToString("0.000"),1, 440, 20, Renk2, "Tahoma", 15);
Sistem.Cizgiler[0].Deger = Sistem.GetiriKZPoz;
Sistem.Cizgiler[1].Deger = Sistem.Liste(0);

NOT1: Profit Factor: Getiri hesaplamasında, en önem verilen sonuçlardan biri de PROFIT FACTOR bilgisidir. Profit Factor bir sistemde kazandıran sinyallerin kazandığı getirinin, kaybettiren sinyallerin kaybettirdiği para/puan değerine oranıdır. Sistem kütüphanesinde Getiri Hesapla komutu kullanıldığı zaman sizin stratejinizin tüm grafik zamanı için Profit Factor değerinin ne olduğu bilgisine yukarıdaki SAYI/ORAN dönen sonuç listesindeki Sistem.ProfitFactor komutuyla ulaşılabilir. Bununla birlikte Sistem.ProfitFactor isimli toplam 4 fonksiyon bulunmaktadır. Bu fonksiyonların kullanım/yazım şekilleri aşağıdaki gibidir;csharp

Sistem.ProfitFactor; //tek bir oran olarak tüm sistemin Profit Factorü
Sistem.ProfitFactorBar(BarSayisi, Kayma); //Girilen bar sayısı kadar öncesine ait PF
Sistem.ProfitFactorIslem(IslemSayisi, Kayma); //Belli işlem sayısı için PF
Sistem.ProfitFactorList; //Her bir barda yeniden hesaplanan PF

NOT2: MaxDD Hesaplama: Getiri hesaplamasında, yine çok önem verilen sonuçlardan biri de MAXDD bilgisidir. MaxDD, bir sistemin kullanılması durumunda, geçmişten günümüze portföyde en büyük parasal erime miktarıdır. Bu sistemi kullansaydık portföyümüzde en büyük erime hangi tarihte olurdu ve bu erime kaç puan/lira olurdu bilgisi için MaxDD verisine bakılır. MaxDD bilgilerine erişim için, sistemin içine (en altına) Sistem.GetiriHesapla Satırından sonra Sistem.MaxDDHesapla satırını da eklemek gerekir. Sonuç değerlerine ancak o şekilde ulaşmak mümkündür. MaxDD için sunulan fonksiyonların kullanım/yazım şekilleri aşağıdaki gibidir;csharp

Sistem.GetiriMaxDDHesapla(Tarih1, Tarih2);
Sistem.GetiriMaxDDTarih;
Sistem.GetiriMaxDD;

