# İçerideki Tehditler: Bir Danışmanın Gözünden Fidye Yazılımı Tersine Mühendisliği

Siber savunmanın ön saflarında görev yapan güvenlik danışmanları olarak 2026 yılında rahatsız edici bir eğilime tanık oluyoruz: Saldırganlar artık sadece verileri şifrelemiyor; güveni de ortadan kaldırıyorlar. Son çalışmalarımız, "antivirüs kur ve unut" şeklindeki geleneksel stratejilerin, makine hızında şekil değiştiren yeni nesil fidye yazılımlarına karşı başarısız olduğunu gösteriyor.

## Giriş: Görünmez Düşman

Adli bilişim laboratuvarlarımızda sürekli aynı kalıbı görüyoruz: Kötü amaçlı yazılım bir müşterinin sistemine girdiğinde, geleneksel hiçbir iz bırakmıyor. Modern fidye yazılımları yasal sistem süreçlerinin içine gizleniyor, antivirüs taramalarını tespit ediyor ve bir analistin izlediğini fark ederse kendini kapatıyor.

Her CISO'ya (Bilgi Güvenliği Yöneticisi) sorduğumuz soru "Saldırıya uğrayacak mısınız?" değil, "Saldırıya uğradığınızda, ekibiniz kötü amaçlı yazılımın tam olarak ne yaptığını saniyeler içinde anlayabilecek mi?" oluyor. İşte **Tersine Mühendislik (Reverse Engineering)** uzmanlığımız tam da bu noktada savunmanın temel taşı haline geliyor.

## Temel Güvenlik Zorluğu: Sahada Gördüklerimiz

Geleneksel güvenlik mimarileri, daha yavaş ve daha öngörülebilir düşmanlar için tasarlanmıştı. İmzalar güncellenir, taramalar yapılır ve dosyalar karantinaya alınırdı. Ancak bu model modern fidye yazılımlarına karşı üç temel nedenden dolayı başarısız oluyor:

**Statik Analiz Körlüğü**: Saldırganlar, kodlarını sürekli değiştiren "polimorfik" paketleyiciler kullanır. Her kurban için rastgele oluşturulan ve yalnızca bellekte açılan bu kodlar, klasik statik analiz araçlarına tamamen anlamsız veriler olarak görünür.

**Analiz Karşıtı Tuzaklar**: Yeni nesil zararlı yazılımlar, bir analist tarafından incelendiklerinin farkına varırlar. Sanal makine (VM) çekirdek sayılarını, fare hareketlerini veya bir hata ayıklayıcının (debugger) varlığını kontrol ederler. Eğer bir analiz ortamı tespit ederlerse, masum bir not defteri gibi davranıp uykuya dalarlar.

**Dinamik Hedefleme**: Kötü amaçlı yazılım artık aptal bir kod parçası değil. Çalıştığı sistemin dilini, klavye düzenini ve hatta Active Directory yapısını analiz eder. Yanlış hedefteyse kendini siler; doğru hedefteyse, en kritik verilere (yedekleme sunucuları gibi) öncelik verir.

## Gerçek Dünyadan Saldırı Perspektifi

Modern bir fidye yazılımı saldırısının (örneğin incelediğimiz WannaCry veya BlackCat varyantları) nasıl geliştiğini düşünün:

![Şekil 1: Modern Fidye Yazılımı Saldırısı Yaşam Döngüsü](./images/Ransomware-RE_10_Ransomware_Attack_Lifecycle.png)

**Aşama 1 - Sızma ve Gizlenme (Infiltration & Evasion)**: Zararlı yazılım sisteme kimlik avı (phishing) veya RDP kaba kuvvet saldırısı yoluyla sızar. İlk işi şifrelemek değil, saklanmaktır. Kendini yasal bir `svchost.exe` sürecine enjekte eder (Process Hollowing) ve güvenlik yazılımlarının radarından çıkar.

**Aşama 2 - Ortam Keşfi (Environment Reconnaissance)**: Kötü amaçlı kod bellekte açıldığında, ortamı sessizce dinler. Hangi antivirüs çalışıyor? İnternet erişimi var mı? Gölge Kopya (Shadow Copy) yedekleri nerede? Tersine mühendislik olmadan bu aşama asla görülmez, çünkü diskte herhangi bir dosya etkinliği yoktur.

**Aşama 3 - Yanal Hareket ve Şifreleme (Lateral Movement & Encryption)**: Hedefler belirlendiğinde saldırı başlar. Önce yedekler silinir (`vssadmin delete`), ardından ağ paylaşımları taranır ve şifreleme anahtarları C2 (Komuta ve Kontrol) sunucusuna gönderilir. Bu aşama genellikle gece yarısı veya hafta sonları, BT ekipleri uzaktayken gerçekleşir.

## Modern Güvenlik Yaklaşımı

Bu gelişmiş tehditlere karşı savunma yapmak, güvenlik felsefesinde köklü bir değişim gerektirir. Amaç tüm saldırıları önlemek (ki bu matematiksel olarak imkansızdır) değil, saldırıyı anlamak ve müdahale süresini en aza indirmektir.

**Kod Düzeyinde Görünürlük (Code-Level Visibility)**: Güvenlik operasyonları (SOC) sadece günlüklere (loglara) değil, aynı zamanda şüpheli dosyaların kod yapısına da bakmalıdır. Otomatik sandbox analizlerinin başarısız olduğu yerlerde, manuel tersine mühendislik zararlı yazılımın "durdurma anahtarını" (killswitch) veya zayıf noktasını bulabilir.

**IOC'lerle Tehdit Avcılığı (Threat Hunting with IOCs)**: Analizden elde edilen verilerle (mutex isimleri, özel user-agent dizeleri) ağ üzerinde proaktif avlanma yapılmalıdır. Bir makinede tespit edilen tehdit, binlerce başka makineye yayılmadan önce bu "Uzlaşma Göstergesi" (Indicator of Compromise) verileriyle durdurulabilir.

**Düşmanı Anlamak (Adversary Understanding)**: Düşmanınızı tanımak, onların kodunu okumaktan geçer. Saldırganın kullandığı şifreleme algoritmasındaki bir hata veya C2 iletişimindeki bir kusur, fidye ödemeden verilerin kurtarılmasını sağlayabilir.

## Sonuç

Fidye yazılımları sadece "dosyalar" değildir; kurumunuzun dijital varlıklarını yok etmek için tasarlanmış sofistike silahlardır. Yalnızca otomatik araçlara ve genel güvenlik kurallarına güvenen kurumlar, görünmez bir düşmanla savaşmaya çalışmaktadır.

Stratejik zorunluluk açıktır: Savunmacılar, saldırganların karmaşıklığına "derinlemesine analiz" ve "kod düzeyinde istihbarat" ile yanıt vermelidir. Güvenlik, görünmezi görebildiğinizde ve düşmanı deşifre edebildiğinizde başlar.
