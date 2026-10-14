markdown

# Sistemleri Şifreleyerek Başkalarıyla Paylaşmak

Yazdığınız bir formülü/sistemi/robotu başkalarıyla paylaşmak isteyen ama paylaştığı kişinin formülü görmesini istemeyen kullanıcılar **ŞİFRELE** butonuna basarak kodu gizleyebilirler.

### Çok Önemli Uyarı
**Şifrelenmiş bir sistemin kodları tekrar görünür yapılamaz.**  
Bu nedenle şifrelemek istediğiniz formülü **önce mutlaka başka bir isimle kaydedin**.

Şifrelenmiş formülde, paylaşılan kişinin değiştirmesi gereken bilgiler (grafik periyodu, indikatör parametresi, sembol kodu vb.) varsa, bu bilgileri kodun içine yazmak yerine **PARAMETRELER** kutusuna yazarak kullanın. Şifreleme yapıldığında bu alan açık kalır.

---

# Sistemlere Kullanıcı Bazında Lisans ve Tarih Sınırı Koyma

Şifreleme yapıldığı zaman iDeal; zaman, disk seri numarası ve rastgele ID gibi birçok parametreyi kullanarak kodu karıştırır ve görünmez hale getirir.  
**Formülün sahibi bile artık açık halini göremez.** Bu yüzden mutlaka önce yedekleyin.

### Kullanıcı Adına Göre Lisanslama
```csharp
if (Sistem.LisansKontrol("AHMET", "Sezai_1", "zeynep2015"))
{
    // Sistem kodları buraya
}
else
{
    Sistem.Mesaj("Bu Sistemi Kullanmaya Yetkiniz Yoktur");
}

Tarih / Süre Yetkisi Vermecsharp

if (Sistem.TarihAraligi("2015.01.01", "2015.12.31"))
{
    // Sistem kodları buraya
}
else
{
    Sistem.Mesaj("Sistemin Kullanım Süresi Dolmuştur. abc@xyz.com adresine mail gönderin");
}

Sistemlerin İsimlendirilmesiSistem isimleri rakamla başlayamaz
İsimlerde boşluk kullanılmamalıdır
Özel işaretler kullanılamaz (+ - ? % & # ’ ! ( ) vb.)
Alt tire (_) kullanılabilir
Türkçe karakter kullanılamaz (İı Şş Ğğ Üü Çç Öö)

Formüllerin Kullanım Amaçları1. SİSTEMGrafik üzerine uygulanır. Çizgi, yazı, resim, AL/SAT sinyali gösterir, kar/zarar ve performans hesapları yapar.2. ROBOTSinyal ürettiği anda belirlenen sembole otomatik emir gönderir.
Portföy penceresindeki ROBOT sekmesinden çalıştırılır.Temel Robot Kalıp Koducsharp

var LotSize = 1;
var SistemAdi = "xxxx";
var GrafikSembolu = "IMKBH'DOHOL";
var GrafikPeriyodu = "60";
var EmirSembol = "IMKBH'DOHOL";

var MySistem = Sistem.SistemGetir(SistemAdi, GrafikSembolu, GrafikPeriyodu);
var SonFiyat = Sistem.SonFiyat(EmirSembol);
var Anahtar = Sistem.Name + "," + EmirSembol;
double IslemFiyat = 0;
DateTime IslemTarih;
var Miktar = 0.0;
var Rezerv = "";
var Pozisyon = Sistem.PozisyonKontrolOku(Anahtar, out IslemFiyat, out IslemTarih);
var SonYon = Sistem.SonYonGetir(SistemAdi, GrafikSembolu, GrafikPeriyodu);

if (Sistem.Saat.CompareTo("10:00:00") <= 0 || Sistem.Saat.CompareTo("17:59:59") >= 0)
{
    // Seans dışı - işlem yapma
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
    if (Miktar > 0) { Islem = "ALIS"; Rezerv = "ALIŞ YAPILDI"; }
    if (Miktar < 0) { Islem = "SATIS"; Rezerv = "SATIŞ YAPILDI"; }

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

3. SORGUBelirli kriterlere uyan sembolleri taratmak için kullanılır.4. OPTİMİZASYONStratejideki parametrelerin en iyi değerlerini bulmak için kullanılır.Diğer Analiz Araçları (Kısa Özet)Performans: Sistemin geçmiş performans raporunu detaylı gösterir.
Sembol Bazında Sistem Kıyasla: Birden fazla sistemi aynı sembol üzerinde karşılaştırır.
Son Pozisyonlar: Seçilen sistemin birçok semboldeki son yönünü listeler.
Multi Performans Eğrisi: Birden fazla sistemin getiri eğrilerini aynı grafikte karşılaştırır.

