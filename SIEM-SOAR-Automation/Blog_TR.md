# Güvenlik Otomasyonu: SIEM ve SOAR Neden Birlikte Çalışmak Zorunda?

2026'nın siber güvenlik ortamında bir Güvenlik Operasyonları Merkezi (SOC) analisti olmak hiç de kolay değil. Kuruluşun Güvenlik Duvarları, EDR'ları, Antivirüs çözümleri ve WAF'ları sürekli yeni numaralar öğreniyor; ancak bu görünürlüğün bedeli, analistlerin ekranlarında her gün parlayan yüz binlerce "uyarı" oluyor. Peki ya bu uyarıların %80'i aslında zararsız "Yanlış Pozitif"lerse?

İşte tam burada analistlerin en büyük tehdidi, bilgisayar korsanları değil, **"Uyarı Yorgunluğu"** oluyor. Rüzgar sesine karşı sürekli kurt bağıran bir sistemde, gerçek kurt geldiğinde kimsenin fark etmemesi an meselesidir. Bu kaosu yönetmenin tek yolu, insan beynini makine hızıyla desteklemektir: SIEM ve SOAR sinerjisi.

## Gören Gözler: SIEM (Güvenlik Bilgisi ve Olay Yönetimi)

Milyonlarca günlük kaydının eş zamanlı aktığı bir ağda "anormallik" bulmak, samanlıkta iğne aramak gibidir. SIEM platformları, kuruluşun "dijital sinir sistemi" işlevi görür. Yalnızca günlükleri toplamakla kalmaz; aralarında *Korelasyon* kurarak büyük resmi çizer.

Bir Güvenlik Duvarının "Harici bir IP ile bağlantı kuruldu" dediği, Active Directory'nin "Şifre 5 kez yanlış girildi" dediği yerde, iyi ayarlanmış bir SIEM şunu söyler: *"Başarısız şifre denemelerinin hemen ardından o hesaba başarıyla giriş yapıldı ve dışarıya 100 MB'lık bir veri transferi başladı! Bu bir veri ihlali olabilir!"*

Ne var ki SIEM ne kadar zekice tasarlanmış olursa olsun, yalnızca mükemmel bir Alarm Zili olarak kalır; yangını tespit eder ancak söndüremez.

## Müdahale Eden Eller: SOAR (Güvenlik Orkestrasyonu, Otomasyonu ve Müdahalesi)

Saniyelerin önem taşıdığı bir fidye yazılımı olayında, bir analistin IP itibarını manuel olarak sorgulaması, Güvenlik Duvarına giriş yaparak kural yazması ve virüslü bilgisayarı ağdan koparması ortalama 20-30 dakika alır. Bu süre, modern bir kötü amaçlı yazılımın tüm sunucuları şifrelemesi için fazlasıyla yeterlidir.

Yangını söndüren mekanizma SOAR'dır. Önceden tanımlanmış **Playbook**'lar sayesinde SOAR, SIEM'den gelen uyarıyı alır ve *insan müdahalesi olmaksızın* harekete geçer.

Tipik bir "Kötü Amaçlı IP Engelleme" Playbook'u şu şekilde işler:
1.  **Zenginleştirme:** SOAR, IP'yi saniyeler içinde 4 farklı Tehdit İstihbaratı hizmetine karşı sorgular.
2.  **Karar:** Kötü amaçlı puan %80'in üzerindeyse "Kötü Amaçlı" damgasını vurur.
3.  **Müdahale:** Kuruluşun Güvenlik Duvarına IP'yi engellemesi için komut gönderir ve EDR'a "bu trafiği başlatan bilgisayarı ağdan izole et" talimatı verir.

Bu sürecin tamamı, bir analist ilk yudum kahvesini bile almadan yalnızca 5 saniyede tamamlanır.

## Sonuç: Geleceğin SOC'unu Tasarlamak

Bugün, "daha fazla güvenlik ürünü alırsak daha güvende oluruz" mantığı iflas etmiştir. Asıl mesele, bu ürünleri bir senfoni gibi orkestre edebilmektir.

SIEM'in "gören gözleri" ile SOAR'ın "müdahale eden elleri" birleştiğinde, SOC ekipleri sıradan kopyala-yapıştır görevlerinden kurtularak gerçek "Tehdit Avcısı" rolüne adım atar. Uyarı Yorgunluğu riskini ortadan kaldırmak ve Gerçek Pozitif uyarılara saniyeler içinde yanıt vermek, modern kuruluşların siber savaştaki hayatta kalma formülüdür.
