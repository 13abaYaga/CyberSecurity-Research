# Vaka Çalışması: SIEM Korelasyonu ve SOAR Otomasyonu ile Uyarı Yorgunluğunu Yenmek

---

## Müşteri Profili

**Sektör**: Finans ve Bankacılık
**Kuruluş Büyüklüğü**: 3.500+ çalışan, 2 Milyonun üzerinde aktif dijital bankacılık müşterisi.
**Ortam**: Günlük ortalama 3 terabayt günlük verisi üreten Hibrit Bulut (Şirket İçi + AWS). Güvenlik altyapısı; önde gelen EDR, WAF, IPS/IDS ve kurumsal Güvenlik Duvarlarından oluşmaktadır.

---

## Sorun

Gelişmiş güvenlik araçlarına milyonlarca dolar yatırım yapılmış olmasına karşın, kuruluşun Güvenlik Operasyonları Merkezi (SOC) tam anlamıyla bir kriz içindeydi. Sorun, araçların kör olması değil; **çok fazla şey görmeleri**ydi.

**Uyarı Yorgunluğu**: Her güvenlik aracı ayrı bir "silo"da çalışıyor ve günlük ortalama 65.000 uyarı üretiyordu. 8 kişilik SOC (Tier 1) analist ekibinin bu uyarıların tamamını incelemesi matematiksel olarak imkânsızdı. Seçici olmak zorunda kalan ekip, alarm'ların yalnızca %15'ini inceleyebildi. Üstelik **incelenen bu alarmların %85'inin Yanlış Pozitif** olduğu ortaya çıktı.
**Olay Müdahale Gecikmesi**: Manuel zenginleştirme (IP'leri VirusTotal'da kontrol etmek, EDR/Güvenlik Duvarından günlük toplamak), kritik uyarı başına ortalama 45 dakika aldı; bu durum saldırganlar için büyük bir fırsat penceresi bıraktı.
**Kritik Soru**: "Binlerce uyarı arasındaki iğneyi nasıl tespit eder ve analist ordusu işe almadan, dakikalar yerine saniyeler içinde doğrulanmış bir tehdide yanıt verebiliriz?"

---

## Çalışma Kapsamı

**Başlangıç Durumu**: SOC ekibi, aşırı uyarı yükü ve düşük kaliteli kurallar nedeniyle yalnızca uyarı yönlendirme işlevi görüyordu.
**Hedef**: SIEM'de "Yanlış Pozitif Yönetimi" uygulamak ve özel SOAR Playbook'ları aracılığıyla Tier-1 olay müdahale kapasitesini otomatize ederek Ortalama Müdahale Süresini (MTTR) önemli ölçüde düşürmek.
**Kısıt**: SOC ekibine ek personel alınamaz. Çözüm, teknoloji odaklı olmalıdır.

---

## Temel Bulgular

### Bulgu 1: Uyarı Üretim Kaosuna ve Analistlerin Yorgunluğuna (Ayarsız SIEM)

**Güvenlik Açığı**: SOC'un ilk SIEM yapılandırması, tüm günlükleri uygun korelasyon olmaksızın topluyordu. Lab ortamımızda, standart ağ aktivitesinin ve basit arka plan saldırılarının çok sayıda ham uyarıyı tetikleyerek ciddi bir "Uyarı Yorgunluğu"na yol açtığını gözlemledik.
**Sömürü**: SIEM (Wazuh) Tehdit Avı panosundaki ham günlük akışının analizi, yapılandırılmamış olayların devasa bir dalgasını ortaya koydu. Özellikle sürekli akan "SMB Giriş Hatası" ve "Kullanıcı kimlik doğrulama hatası" olayları konsolu selerek "SMB Kaba Kuvvet Saldırısı Tespit Edildi" gibi kritik uyarıları gömdü.
**Etki**: Analistler, sıradan "Seviye 5" giriş hatası bildirimleri yığınının altında kalan kritik uyarıları (Kaba Kuvvet girişimleri gibi) gözden kaçırdı.
**Tespit Durumu**: SIEM panosu tamamen dolup taşarak Uyarılar evrim grafiğinde devasa tepeler oluşturdu ve manuel analizi imkânsız hale getirdi.

![Şekil 1: Yoğun SMB giriş hataları ve uyarı yorgunluğuyla bunalan Wazuh SIEM Panosu](./images/SIEM-SOAR_01_Alert-Fatigue.png)

### Bulgu 2: Bağlam Duyarlı SIEM Korelasyonu ile Yanlış Pozitif Yönetimi

**Güvenlik Açığı**: Genel kurallar ("Şüpheli Bağlantı"), tüm varlıklar için gelişigüzel tetiklenip %85 oranında Yanlış Pozitif üretti.
**Sömürü**: Wazuh'ta özel Korelasyon Kuralları (`local_rules.xml`) yazarak aktif Yanlış Pozitif yönetimi uyguladık. SOC'un bireysel "SMB Giriş Hatası" günlükleri (Kural 100006) altında boğulmasını önlemek için bileşik bir kural (Kural 100007) oluşturduk. Bu kural gürültüyü bastırır ve yalnızca aynı kaynaktan 30 saniye içinde 5 kez hata koşulu gerçekleştiğinde işlem yapılabilir bir "SMB Kaba Kuvvet Saldırısı Tespit Edildi" Seviye 8 uyarısı üretir.
```xml
<group name="smdb">
  <rule id="100006" level="5">
    <match>NT_STATUS_WRONG_PASSWORD</match>
    <description>SMB Giriş Hatası</description>
  </rule>
  <rule id="100007" level="8" frequency="5" timeframe="30">
    <if_matched_sid>100006</if_matched_sid>
    <description>SMB Kaba Kuvvet Saldırısı Tespit Edildi</description>
  </rule>
</group>
```
**Etki**: Günlük uyarı hacmi, 65.000 ham olaydan anlamlı ve işlem yapılabilir **3.500 Uyarıya** düştü (%94 gürültü azaltımı).
**Tespit Durumu**: Devam eden bir saldırıyı (Kaba Kuvvet) doğrudan gösteren yüksek kaliteli, işlem yapılabilir uyarılar, standart başarısız girişlerin önünde önceliklendirildi.

![Şekil 2: Wazuh'taki özel Korelasyon Kuralı, yanlış pozitifleri azaltıyor ve Kaba Kuvvet girişimlerini tespit ediyor](./images/SIEM-SOAR_02_False-Positive-Mgmt.png)

### Bulgu 3: Otomatik Olay Müdahalesi (SOAR Kapasitesi)

**Güvenlik Açığı**: Geriye kalan anlamlı uyarılar hâlâ manuel analiz gerektiriyordu. Analistler geleneksel olarak, saldırgan IP adreslerini engellemek için güvenlik duvarlarına manuel giriş yaparak her olay için kritik dakikalar harcıyor ve başarılı ihlallere kapı aralıyordu.
**Sömürü**: Bu açığı kapatmak için Otomatik Olay Müdahalesi kapasitesi (SOAR çerçevesi olarak işlev gören) uyguladık. SIEM, "SMB Kaba Kuvvet Saldırısı Tespit Edildi" gibi kritik bir korelasyon olayını saptadığında otomatik olarak hedefli bir azaltma yükü tetiklenir (örn. güvenlik duvarına API aracılığıyla veya Aktif Müdahale betikleriyle).
```python
# Saldırgan IP'yi otomatik olarak engellemek için kavramsal SOAR API yükü
{
  "action": "block_ip",
  "target_device": "Perimeter_Firewall",
  "trigger_rule": "100007_SMB_Brute_Force",
  "block_duration": "600s"
}
```
**Etki**: Otomatik müdahale sistemi, kaba kuvvet saldırısının kaynak IP'sini SIEM uyarısından bağımsız olarak çıkardı, riski değerlendirdi ve insan müdahalesi olmaksızın ağ güvenlik duvarında bir düşürme komutu çalıştırdı.
**Tespit Durumu**: Olay, makine hızında (5 saniyenin altında) tamamen etkisiz hale getirildi. Saldırganın bağlantısı anında kesildi; tehdit, bir insan analist bilet bile açmadan nötralize edildi.

![Şekil 3: Kötü amaçlı IP'ye karşı engelleme komutu çalıştıran otomatik SOAR playbook/müdahale akışı](./images/SIEM-SOAR_03_Automated-Firewall-Block.png)

### Olay Müdahale Özeti

Entegrasyon, eksiksiz bir otomatik savunma hattını göstermektedir:
1. **Ham Günlük Alımı**: Standart başarısız girişlerin büyük hacimleri toplanır.
2. **SIEM Korelasyonu**: Gürültü, tek bir yüksek kaliteli Seviye 8 "SMB Kaba Kuvvet Saldırısı Tespit Edildi" uyarısına dönüştürülür.
3. **Otomatik Azaltma (SOAR)**: Tetiklenen bir müdahale betiği, saldırgan IP'ye karşı anında bir güvenlik duvarı engelleme komutu çalıştırır.
3. **Otomatik Zenginleştirme**, tehdit düzeyini doğrulamak için harici API'leri (VirusTotal, CrowdStrike) sorgular.
4. **Makine Hızında Azaltma**, Çevre Güvenlik Duvarına dinamik bir engelleme kuralı iter ve EDR aracılığıyla ana bilgisayarı izole eder.

---

## Sonuçlar

**Anlık İyileştirme**:
- SIEM ayarlaması ve Yanlış Pozitif eleme sayesinde günlük genel uyarılarda %94 azalma.
- En sık karşılaşılan uyarı türlerini hedefleyen (örn. Kimlik Avı, Kötü Amaçlı IP Bağlantıları, Kötü Amaçlı Yazılım Tespiti) 15 temel SOAR playbook'unun devreye alınması.

**Stratejik İyileştirmeler**:
- Tier-1 Analistler "kopyala-yapıştır" doğrulama görevlerinden kurtarılarak proaktif Tehdit Avı faaliyetlerine odaklanabildi.
- SIEM'in "beyin", SOAR'ın ise "eller" işlevi gördüğü, teknoloji odaklı birleşik bir savunma hattı oluşturuldu.

---

## İş Etkisi

Uygulama, SOC'un operasyonel verimliliğini kökten dönüştürdü. Doğrulanan olayların %70'i artık SOAR playbook'ları tarafından özerk şekilde kapatılıyor veya kontrol altına alınıyor. **Ortalama Müdahale Süresi (MTTR) 45 dakikadan 45 saniyeye düştü**; bu, %98'lik bir iyileşme anlamına gelmektedir. Bu neredeyse anlık tepki kapasitesi, Fidye Yazılımı gibi hızla hareket eden tehditlere karşı kuruluşun risk profilini önemli ölçüde azalttı.

---

## Sonuç

Bu uygulama şunu doğrulamaktadır:
1. **Daha fazla günlük, daha fazla güvenlik anlamına gelmez**: Yanlış Pozitif yönetimini uygulayan iyi ayarlanmış bir SIEM olmadan, kuruluşlar yalnızca gürültüyü depolamak için para ödüyor.
2. **Uyarı Yorgunluğu gerçek ve tehlikelidir**: Binlerce uyarının altında ezilen bir analist, kaçınılmaz olarak kritik bulguyu gözden kaçıracaktır.
3. **Otomasyon bir zorunluluktur**: Makine hızındaki saldırılara karşı insan hızına güvenmek, baştan kaybedilmiş bir savaştır. SOAR playbook'ları, veri sızması veya şifreleme başlamadan tehditleri kontrol altına almak için kritik öneme sahiptir.
