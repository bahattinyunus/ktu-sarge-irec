# 🚀 KTU GÖKÇEN: PROXIMA MISSION REPOSITORY

![Banner](docs/assets/banner.png)

> **"Mükemmellik bir eylem değil, bir alışkanlıktır."**
> Bu depo, KTÜ Gökçen Roket Takımı'nın Spaceport America Cup (IREC) 30k SRAD kategorisi için geliştirdiği "Proxima" roketinin teknik dokümantasyonunu, mühendislik analizlerini ve operasyonel prosedürlerini içerir.

<div align="center">

![Badge](https://img.shields.io/badge/MISSION-PROXIMA-red?style=for-the-badge) ![Badge](https://img.shields.io/badge/STATUS-FLIGHT_READY-success?style=for-the-badge) ![Badge](https://img.shields.io/badge/CATEGORY-30k_SRAD-blue?style=for-the-badge)

[**📜 YOL HARİTASI**](ROADMAP.md) • [**📚 KAYNAKLAR**](docs/RESOURCES.md) • [**📖 SÖZLÜK**](docs/GLOSSARY.md) • [**🧪 MALZEME**](docs/04_Subsystems_&_Payload/03_Aerostructures/Material_Properties.md) • [**📋 PROSEDÜRLER**](docs/03_Safety_&_SRAD/Checklists/)

</div>

---

## 📂 0. DOKÜMANTASYON VE NAVİGASYON REHBERİ
Bu repo, binlerce satır kod ve teknik dokümandan oluşur. Aradığınızı bulmanız için rehber:

| Belge | Fragman (İçerik Özeti) | Erişim |
| :--- | :--- | :--- |
| **📜 YOL HARİTASI** | Projenin 2 yıllık stratejik "Master Planı". Hangi aşamadayız, sırada ne var? | [👉 Detaylar](ROADMAP.md) |
| **📚 KAYNAKLAR** | "Nasıl Kazanılır?" IREC şampiyonlarının raporları, kritik kitaplar ve eğitim videoları. | [👉 Kütüphaneye Git](docs/RESOURCES.md) |
| **📖 SÖZLÜK** | CATO, Apogee, SRAD ne demek? Roketçilik terminolojisine hakim olun. | [👉 Öğren](docs/GLOSSARY.md) |
| **🧪 MALZEME VERİSİ** | Analizler için gerekli gerçek "Engineering Data". (Alüminyum 6061, Karbon Fiber vb.) | [👉 Verileri Al](docs/04_Subsystems_&_Payload/03_Aerostructures/Material_Properties.md) |
| **📋 PROSEDÜRLER** | Roketi patlatmadan uçurmak için: Montaj, Rampa ve Kurtarma adım adım kontrol listeleri. | [👉 Uygula](docs/03_Safety_&_SRAD/Checklists/) |
| **🛡️ SAVAŞ KURALLARI** | Repoya kod gönderirken uymanız gereken disiplin kuralları. | [👉 Oku](CONTRIBUTING.md) |

---

## 📋 1. MİSYON PROFİLİ (Mission Profile)
**Proje Adı:** Proxima
**Yarışma:** Spaceport America Cup (IREC) - 30k SRAD
**Konum:** Spaceport America, New Mexico, ABD
**Hedef:** 4 kg (8.8 lb) bilimsel faydalı yükü tam 30,000 ft (9,144m) irtifaya çıkarmak ve güvenli bir şekilde kurtarmak.

### Temel Performans Parametreleri (KPI)
| Parametre | Hedef Değer | Tolerans |
| :--- | :--- | :--- |
| **Apogee** | 30,000 ft AGL | ±%10 |
| **Max Hız** | Mach 1.8 | < Mach 2.0 (Termal Limit) |
| **Max İvme** | 14 G | < 20 G (Elektronik Limiti) |
| **Statik Marjin** | 2.5 Cal | 2.0 - 4.0 Arası |
| **İniş Hızı** | 6.5 m/s | < 8.0 m/s (Güvenli İniş) |

---

## 📐 2. MATEMATİKSEL TEMELLER (Mathematical Foundation)
Tasarımımız "deneme-yanılma" değil, aşağıdaki fizik yasaları üzerine kuruludur.

### 2.1. İtki Denklemi (Propulsion)
Roketin itkisi ($F$), momentum değişimi ve basınç farkından doğar:
$$ F = \dot{m} V_e + (P_e - P_a) A_e $$
*   $\dot{m}$: Kütle debisi (Propellant mass flow rate)
*   $V_e$: Çıkış hızı (Exhaust velocity)
*   $P_e$: Çıkış basıncı (Exit pressure)
*   $P_a$: Atmosfer basıncı (Ambient pressure)

### 2.2. Aerodinamik Stabilite (Stability)
Roketin stabil uçması için Basınç Merkezi ($C_p$), Ağırlık Merkezi ($C_g$)'nin gerisinde olmalıdır (Barrowman Denklemi):
$$ Margin = \frac{X_{cp} - X_{cg}}{d_{ref}} \geq 2.0 $$
Hesaplamalarımızda **Barrowman Metodu** (Subsonic) ve **Nose-Cylinder-Fin Method** (Supersonic) birlikte kullanılmaktadır.

### 2.3. Paraşüt Sürüklenmesi (Recovery)
İniş hızını ($V$) belirleyen sürüklenme denklemi:
$$ V = \sqrt{\frac{2mg}{\rho C_d A}} $$
*   $C_d$: Sürüklenme katsayısı (Hemispherical: 1.5, Elliptical: 2.2)
*   $A$: Paraşüt alanı

---

## 🔩 3. ALT SİSTEM DETAYLARI (Detailed Subsystems)

### 🧠 A. Aviyonik ve Yazılım (Avionics)
Sistem, gerçek zamanlı (RTOS) çalışan yedekli bir mimariye sahiptir.

**Durum Makinesi (State Machine Logic):**
1.  **IDLE:** Sensör kalibrasyonu, GPS Lock bekleniyor.
2.  **ARMED:** Anahtarlar açık, süreklilik (continuity) tamam. Ateşleme komutu bekleniyor.
3.  **BOOST (ASCENT):** İvme > 3G algılandı. Loglama başladı. (Active Control kilitli).
4.  **COAST:** Motor söndü. Apogee tahmini yapılıyor.
5.  **APOGEE:** Hız < 0 m/s. **Drogue Paraşüt Ateşle.**
6.  **DESCENT:** Barometre irtifayı izliyor.
7.  **MAIN DEPLOY:** İrtifa < 1500ft. **Ana Paraşüt Ateşle.**
8.  **TOUCHDOWN:** Hız ~ 0. GPS konumu gönderiliyor.

**Donanım Özellikleri:**
| Bileşen | Model / Teknoloji | Açıklama |
| :--- | :--- | :--- |
| **Ana İşlemci** | STM32H743ZI | 480 MHz ARM Cortex-M7, Real-Time OS. |
| **IMU (Sensör)** | Bosch BMI088 | Yüksek G dayanımlı (24g) ivmeölçer. |
| **Telemetri (RF)** | LoRa SX1276 | 915 MHz, Spread Spectrum, 15km+ menzil. |
| **PCB Katman** | 4-Layer FR4 | Gürültü izolasyonu için ayrı GND/PWR katmanları. |

### 🔥 B. İtki Sistemi (Propulsion)
Motorumuz %100 SRAD (Öğrenci Tasarımı) ve M-Sınıfı bir katı yakıtlı motordur.

| Parametre | Değer | Detaylar |
| :--- | :--- | :--- |
| **Toplam İtki** | 9,200 Ns | M-Class (%92 Doluluk). |
| **Yakıt Tipi** | APCP | Ammonium Perchlorate Composite Propellant. |
| **Formülasyon** | Blue Thunder | %78 Oksitleyici, %16 Binder, %2 Metal (Al). |
| **Yanma Süresi** | 4.2 saniye | Hızlı ve agresif yanma profili. |
| **Nozzle Expansion** | 5.4 | Deniz seviyesi ve 30k feet optime edilmiş ortalama. |

### 🏗️ C. Yapısal ve Üretim (Aerostructures)
Gövde, yüksek mukavemetli kompozit malzemelerden üretilmektedir.

**Kompozit Sarım Planı (Layup Schedule):**
*   **Gövde Borusu:** `[90/±45/90/±45/90]` (Filament Winding).
    *   *Amaç:* Burkulma (Buckling) ve iç basınç dayanımı.
*   **Kanatçıklar:** `[0/90]_4s` Karbon Fiber + 5mm Nomex Petek (Honeycomb) Çekirdek.
    *   *Amaç:* Flutter dayanımı ve hafiflik.

---

## � 4. ARAYÜZ KONTROL DOKÜMANI (ICD)
Alt sistemlerin birbirine nasıl bağlandığını tanımlar.

### 4.1. Mekanik Arayüzler
| Bağlantı | Vida Tipi | Tork Değeri | Notlar |
| :--- | :--- | :--- | :--- |
| **Motor - Gövde** | M6 Çelik Cıvata | 12 Nm | Loctite 243 (Mavi) kullanımı zorunlu. |
| **Fin - Gövde** | M4 Havşa Başlı | 4 Nm | Aerodinamik pürüzsüzlük için. |
| **Avionics Rayı** | M3 Paslanmaz | 1.5 Nm | Titreşim pulları ile. |

### 4.2. Elektriksel Pin Haritası (Pinout)
| Pin Adı | Fonksiyon | Bağlantı | Protokol |
| :--- | :--- | :--- | :--- |
| **PYRO_1** | Drogue Ateşleme | Mosfet Q1 Gate | 12V 5A Pulse |
| **PYRO_2** | Main Ateşleme | Mosfet Q2 Gate | 12V 5A Pulse |
| **UART_TX** | Telemetri Veri | LoRa RX Pin | 115200 Baud |
| **I2C_SDA** | Sensör Veri | IMU SDA Pin | 400 kHz |

---

## ⏱️ 5. OPERASYON KONSEPTİ (CONOPS)
Bir fırlatma gününün kronolojisi:

| Zaman (T-) | Olay | Açıklama |
| :--- | :--- | :--- |
| **T-24 Saat** | **Readiness Review** | Tüm sistemlerin son kontrolü. Pillerin şarjı. |
| **T-4 Saat** | **Assembly** | Motor montajı ve rokete entegrasyonu. |
| **T-1 Saat** | **Pad Loading** | Roketin rampaya yerleştirilmesi ve dikilmesi. |
| **T-15 Dak** | **Arming** | Aviyonik sistemlerin açılması. GPS Lock kontrolü. |
| **T-0** | **LIFT OFF** | Ateşleme ve kalkış. (Max Q: T+12s) |
| **T+ Apogee** | **Drogue Deploy** | Tepe noktasında ilk paraşüt açılır. |
| **T+ 1500ft** | **Main Deploy** | Ana paraşüt açılır ve yavaş iniş başlar. |
| **T+ Touchdown** | **Recovery** | Roketin GPS ile bulunması ve veri analizi. |

---

## ✅ 6. TEST VE DOĞRULAMA (Verification & Validation)
Uçuş öncesi (Pre-Flight) zorunlu test protokolleri:

1.  **Statik Ateşleme (Static Fire):** Motorun yerde ateşlenerek itki eğrisinin doğrulanması.
2.  **Vakum Testi:** Aviyonik sistemin 30,000 ft basınçsız ortamda çalışıp çalışmadığının testi.
3.  **Yer Ayrılma Testi (Ejection Test):** Paraşüt barutlarının (Black Powder) gövdeyi ayırmaya yetip yetmediğinin testi.
4.  **Titreşim (Vibration) Testi:** Vida ve konnektörlerin fırlatma sarsıntısında gevşemediğinin onayı.

---

## 🛠️ 7. DİJİTAL MÜHENDİSLİK ARAÇLARI
Proje kapsamında geliştirilen özel Python analiz araçları `analysis/` dizininde bulunmaktadır.

**Kurulum:**
```bash
pip install -e .
```

**Araç Listesi:**
| Araç | Açıklama | Komut |
| :--- | :--- | :--- |
| **Parachute Sizing** | İniş hızı ve darbe enerjisi hesabı. | `python analysis/calculators/parachute_sizing.py` |
| **Link Budget** | RF Telemetri menzil analizi (Friis). | `python analysis/calculators/link_budget.py` |
| **Thrust Analyzer** | Motor test verisi analizi. | `python analysis/calculators/thrust_analyzer.py` |

---

## 📆 8. PROJE YÖNETİMİ VE TAKVİM
**Organizasyon Şeması:**
*   **Project Manager:** Genel Koordinasyon.
*   **Systems Engineer:** Arayüzler ve Gereksinimler.
*   **Subsystem Leads:** Avionics, Propulsion, Aerostructures, Payload, Recovery.

**Genel Takvim:**
*   **Eylül - Kasım:** Kavramsal Tasarım (MDR)
*   **Aralık - Ocak:** Ön Tasarım (PDR)
*   **Şubat - Mart:** Kritik Tasarım (CDR) ve Prototip
*   **Nisan:** Üretim ve Yer Testleri
*   **Mayıs:** Sistem Entegrasyonu ve FRR (Flight Readiness Review)
*   **Haziran:** **IREC Competition (Launch)**

---

## 📞 İLETİŞİM
Kurumsal iletişim ve sponsorluk için:
*   🌐 **Web:** [gokcenrocket.org](https://gokcenrocket.org)
*   📧 **E-Posta:** contact@gokcenrocket.org
*   💼 **LinkedIn:** [linkedin.com/company/ktugokcen](https://linkedin.com/company/ktugokcen)

---
### ⚖️ Yasal Uyarı (Disclaimer)
Bu depo, akademik ve eğitim amaçlıdır. İçerikteki bazı teknolojiler (özellikle itki ve navigasyon sistemleri), uluslararası ihracat kontrol düzenlemelerine (EAR/ITAR) tabi olabilir. Kullanıcılar, yerel ve uluslararası yasalara uymakla yükümlüdür.
