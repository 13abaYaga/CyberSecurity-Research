# 🔍 Inside the Threats: Ransomware Reverse Engineering & Defense

*🌍 [🇹🇷 Türkçe versiyon için aşağı kaydırın veya buraya tıklayın](#-türkçe-versiyon)*

---

## 🇬🇧 English Version

### 📌 Overview
This repository contains my research and practical lab scenarios on **Ransomware Reverse Engineering** and modern defense strategies against advanced cyber threats. 

The project explores why traditional "install-and-forget" security tools fail against next-generation ransomware. It demonstrates how attackers use techniques like Process Hollowing, Living off the Land (LotL), and Anti-Sandbox traps, and how defenders can leverage **Static and Dynamic Analysis** to produce code-level intelligence, identify killswitches, and halt lateral movement in corporate environments.

### 📂 Repository Contents

#### 1. Whitepaper
An in-depth theoretical analysis of the Malware Reverse Engineering discipline and its critical role in transitioning corporate defense strategies from reactive scanning to proactive, evidence-based security models.
* 📄 [Read the Whitepaper (EN)](./Whitepaper_EN.md)

#### 2. Case Study
A practical, deep-dive forensic analysis of the WannaCry attack, demonstrating the exploitation of the SMBv1 vulnerability (EternalBlue), the ransomware's worm-like lateral movement, and the discovery of its built-in Killswitch mechanism using IDA Pro.
* 🛠️ [Read the Case Study (EN)](./Case-Study_EN.md)

#### 3. Blog Post
A consultant's perspective on the invisible enemy, discussing why modern ransomware bypasses static analysis and how code-level visibility is the foundation of modern security.
* ✍️ [Read the Blog Post (EN)](./Blog_EN.md)

### ⚙️ Technologies & Concepts
* **Reverse Engineering:** IDA Pro, x64dbg, Ghidra (Static & Dynamic Analysis)
* **Threat Analysis:** Process Hollowing, C2 Communication, Domain Generation Algorithms (DGA), Hybrid Encryption (AES/RSA)
* **Defensive Operations:** Incident Response, Malware Sandbox Analysis (Any.run), Indicator of Compromise (IOC) Hunting
* **Format:** Markdown, Mermaid Process Flowcharts & IDA/Sandbox Screenshots

---

<br>

<a id="-türkçe-versiyon"></a>

## 🇹🇷 Türkçe Versiyon

*🌍 [Click here to go back to the top (English)](#-inside-the-threats-ransomware-reverse-engineering--defense)*

### 📌 Proje Özeti
Bu depo, modern fidye yazılımlarının analizi ve **Fidye Yazılımı Tersine Mühendisliği (Ransomware Reverse Engineering)** konularındaki araştırma ve pratik laboratuvar çalışmalarımı içermektedir. 

Çalışmalar kapsamında; geleneksel güvenlik araçlarının makine hızında şekil değiştiren yeni nesil fidye yazılımlarına karşı neden başarısız olduğu, saldırganların Process Hollowing ve Sanal Makine atlatma (Anti-Sandbox) gibi teknikleri nasıl kullandığı incelenmiştir. Ayrıca, **Statik ve Dinamik Analiz** yöntemleriyle kötü amaçlı kodların deşifre edilerek durdurma anahtarlarının (killswitch) nasıl bulunabileceği ve kurumsal Olay Müdahale (Incident Response) süreçlerine nasıl entegre edilebileceği ele alınmıştır.

### 📂 İçerikler

#### 1. Araştırma Makalesi (Whitepaper)
Kötü Amaçlı Yazılım Tersine Mühendisliği disiplinini ve kurumsal savunma stratejilerindeki kritik rolünü inceleyen, savunmayı reaktif taramadan kanıta dayalı ve kod düzeyinde istihbarata taşıyan detaylı teorik doküman.
* 📄 [Makaleyi Oku (TR)](./Whitepaper_TR.md)

#### 2. Vaka Çalışması (Case Study)
WannaCry saldırısının derinlemesine adli analizi. SMBv1 zafiyetinin (EternalBlue) nasıl sömürüldüğünü, solucan benzeri yanal hareketi ve IDA Pro ile gerçekleştirilen statik analiz sonucunda durdurma anahtarının (killswitch) nasıl keşfedildiğini gösteren pratik laboratuvar analizi.
* 🛠️ [Vaka Çalışmasını Oku (TR)](./Case-Study_TR.md)

#### 3. Blog Yazısı
Bir güvenlik danışmanının gözünden görünmez düşmanların (fidye yazılımlarının) geleneksel analizleri nasıl atlattığını ve kod düzeyinde görünürlüğün modern güvenliğin neden temeli olduğunu anlatan özet makale.
* ✍️ [Blog Yazısını Oku (TR)](./Blog_TR.md)

### ⚙️ Kullanılan Teknolojiler & Konseptler
* **Tersine Mühendislik:** IDA Pro, x64dbg, Ghidra (Statik ve Dinamik Analiz)
* **Tehdit Analizi:** Process Hollowing, C2 İletişimi, Alan Adı Üretme Algoritmaları (DGA), Hibrit Şifreleme (AES/RSA)
* **Savunma Operasyonları:** Olay Müdahalesi (Incident Response), Kötü Amaçlı Yazılım Sandbox Analizi (Any.run), IOC Tehdit Avcılığı
* **Format:** Markdown, Mermaid Süreç Akış Şemaları ve IDA/Sandbox Ekran Görüntüleri

---
*Created by **Bilgehan Bayrak** - Cyber Security Researcher.*
