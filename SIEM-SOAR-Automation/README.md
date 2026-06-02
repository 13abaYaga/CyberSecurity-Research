# 🛡️ Defeating Alert Fatigue: SIEM Correlation & SOAR Automation

## 📌 Proje Özeti / Overview
Bu depo, modern Güvenlik Operasyon Merkezlerinde (SOC) karşılaşılan en büyük krizlerden biri olan **"Alert Fatigue" (Uyarı Yorgunluğu)** probleminin nasıl çözüleceğine dair araştırma ve pratik laboratuvar çalışmalarımı içermektedir. 

Çalışmalar kapsamında; **Wazuh SIEM** üzerinde False Positive (Yanlış Pozitif) oranlarını düşürmek için özel korelasyon kurallarının yazılması ve **SOAR Playbook** mimarileri kullanılarak Olay Müdahale (Incident Response) süreçlerinin saniyeler seviyesine indirilmesi (Automated Firewall Blocking vb.) incelenmiştir.

*This repository contains my research and practical lab scenarios on solving the "Alert Fatigue" problem in modern Security Operations Centers (SOC) using SIEM correlation tuning and SOAR automation.*

---

## 📂 İçerikler / Repository Contents

Bu repoda konunun farklı derinliklerinde hazırlanmış üç ana doküman bulunmaktadır. Dokümanları Türkçe (TR) veya İngilizce (EN) olarak inceleyebilirsiniz.

### 1. Araştırma Makalesi (Whitepaper)
Saldırgan metodolojilerine karşı SIEM ve SOAR platformlarının mimari entegrasyonunu ve SOC ekiplerine kazandırdığı operasyonel yetenekleri inceleyen detaylı teorik doküman.
* 📄 [Read in English (EN)](./Whitepaper_SIEM-SOAR_Bilgehan_EN.md)
* 📄 [Türkçe Oku (TR)](./Whitepaper_SIEM-SOAR_Bilgehan_TR.md)

### 2. Vaka Çalışması (Case Study)
Wazuh SIEM üzerinde Brute Force saldırılarına karşı `local_rules.xml` üzerinden yazılan özel korelasyon kuralları ve SOAR üzerinden tetiklenen otomatik IP bloklama senaryosunun pratik laboratuvar analizi.
* 🛠️ [Read in English (EN)](./CaseStudy_SIEM-SOAR_Bilgehan_EN.md)
* 🛠️ [Türkçe Oku (TR)](./CaseStudy_SIEM-SOAR_Bilgehan_TR.md)

### 3. Makale (Blog Post)
Güvenlik otomasyonunun temellerini ve SOC analistlerinin Threat Hunter (Tehdit Avcısı) rolüne nasıl evrilmesi gerektiğini anlatan özet makale.
* ✍️ [Read in English (EN)](./Blog_SIEM-SOAR_Bilgehan_EN.md)
* ✍️ [Türkçe Oku (TR)](./Blog_SIEM-SOAR_Bilgehan_TR.md)

---

## ⚙️ Kullanılan Teknolojiler & Konseptler / Technologies & Concepts
* **SIEM:** Wazuh (Log Analizi, Korelasyon, Kural Yazımı)
* **SOAR:** Playbook Mimarisi, API Entegrasyonları, Otomatik Zenginleştirme (Enrichment)
* **Defensive Operations:** Incident Response, False Positive Management, Threat Intelligence
* **Format:** Markdown & Mimari Şemalar

---
*Created by **Bilgehan Bayrak** - Computer Engineering Student & Cyber Security Enthusiast.*
