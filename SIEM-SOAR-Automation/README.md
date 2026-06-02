# 🛡️ Defeating Alert Fatigue: SIEM Correlation & SOAR Automation

*🌍 [🇹🇷 Türkçe versiyon için aşağı kaydırın veya buraya tıklayın](#-türkçe-versiyon)*

---

## 🇬🇧 English Version

### 📌 Overview
This repository contains my research and practical lab scenarios on solving the **"Alert Fatigue"** problem—one of the most critical challenges in modern Security Operations Centers (SOC). 

The project demonstrates how to significantly reduce False Positive rates by developing custom correlation rules on **Wazuh SIEM**, and how to automate Incident Response processes (such as dynamic firewall blocking) at machine speeds using **SOAR Playbook** architectures.

### 📂 Repository Contents

#### 1. Whitepaper
An in-depth theoretical analysis of the architectural integration between SIEM and SOAR platforms against modern attacker methodologies.
* 📄 [Read the Whitepaper (EN)](./Whitepaper_EN.md)

#### 2. Case Study
A practical lab analysis demonstrating the creation of custom `local_rules.xml` correlation rules against Brute Force attacks on Wazuh, and an automated IP blocking response triggered via SOAR.
* 🛠️ [Read the Case Study (EN)](./Case-Study_EN.md)

#### 3. Blog Post
A comprehensive summary discussing the fundamentals of security automation and the evolution of SOC analysts into Threat Hunters.
* ✍️ [Read the Blog Post (EN)](./Blog_EN.md)

### ⚙️ Technologies & Concepts
* **SIEM:** Wazuh (Log Analysis, Correlation, Custom Rule Creation)
* **SOAR:** Playbook Architecture, API Integrations, Automated Enrichment
* **Defensive Operations:** Incident Response, False Positive Management, Threat Intelligence
* **Format:** Markdown & Architectural Flowcharts

---

<br>

<a id="-türkçe-versiyon"></a>

## 🇹🇷 Türkçe Versiyon

*🌍 [Click here to go back to the top (English)](#-defeating-alert-fatigue-siem-correlation--soar-automation)*

### 📌 Proje Özeti
Bu depo, modern Güvenlik Operasyon Merkezlerinde (SOC) karşılaşılan en büyük krizlerden biri olan **"Alert Fatigue" (Uyarı Yorgunluğu)** probleminin nasıl çözüleceğine dair araştırma ve pratik laboratuvar çalışmalarımı içermektedir. 

Çalışmalar kapsamında; **Wazuh SIEM** üzerinde False Positive (Yanlış Pozitif) oranlarını düşürmek için özel korelasyon kurallarının yazılması ve **SOAR Playbook** mimarileri kullanılarak Olay Müdahale (Incident Response) süreçlerinin saniyeler seviyesine indirilmesi (otomatik IP bloklama vb.) incelenmiştir.

### 📂 İçerikler

#### 1. Araştırma Makalesi (Whitepaper)
Saldırgan metodolojilerine karşı SIEM ve SOAR platformlarının mimari entegrasyonunu ve SOC ekiplerine kazandırdığı operasyonel yetenekleri inceleyen detaylı teorik doküman.
* 📄 [Makaleyi Oku (TR)](./Whitepaper_TR.md)

#### 2. Vaka Çalışması (Case Study)
Wazuh SIEM üzerinde Brute Force saldırılarına karşı `local_rules.xml` üzerinden yazılan özel korelasyon kuralları ve SOAR üzerinden tetiklenen otomatik IP bloklama senaryosunun pratik laboratuvar analizi.
* 🛠️ [Vaka Çalışmasını Oku (TR)](./Case-Study_TR.md)

#### 3. Blog Yazısı
Güvenlik otomasyonunun temellerini ve SOC analistlerinin Threat Hunter (Tehdit Avcısı) rolüne nasıl evrilmesi gerektiğini anlatan özet makale.
* ✍️ [Blog Yazısını Oku (TR)](./Blog_TR.md)

### ⚙️ Kullanılan Teknolojiler & Konseptler
* **SIEM:** Wazuh (Log Analizi, Korelasyon, Kural Yazımı)
* **SOAR:** Playbook Mimarisi, API Entegrasyonları, Otomatik Zenginleştirme (Enrichment)
* **Savunma Operasyonları:** Incident Response, False Positive Management, Threat Intelligence
* **Format:** Markdown & Mimari Şemalar

---
*Created by **Bilgehan Bayrak** - Computer Engineering Student & Cyber Security Enthusiast.*
