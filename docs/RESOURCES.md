# 📚 Kutsal Kaynaklar ve Referanslar (The Library)

> **"Bilgi paylaştıkça çoğalır."**
> Bu dosya, roket bilimi, IREC raporları ve açık kaynak kodlar için nihai rehberdir.

---

## 🏆 1. IREC Şampiyonlarının Teknik Raporları
Bu raporlar "nasıl kazanılır" sorusunun cevabıdır. Okumadan tasarıma başlamayın.

| Yıl | Üniversite | Kategori | Odak Noktası | Link |
| :--- | :--- | :--- | :--- | :--- |
| **2018** | **🇨🇦 McGill University** | 30k COTS | *Mükemmel Sistem Mimarisi & Test Disiplini.* Rapor yazım standardı budur. | [📄 PDF İndir](https://www.scribd.com/document/384666468/McGill-Rocket-Team-IREC-2018-Project-Technical-Report) |
| **2022** | **🇨🇦 Univ. of Waterloo** | 30k SRAD (Liquid) | *Sıvı/Hibrit Motor ve Akışkanlar Mekaniği.* İleri seviye itki tasarımı. | [📄 PDF İndir](https://www.scribd.com/document/660429783/Team-139-Waterloo-Final-Report-2022-IREC) |
| **2022** | **🇺🇸 Stanford University** | 10k COTS | *Aviyonik Yedeklilik (Redundancy) ve Risk Analizi.* Güvenli tasarım dersi. | [📄 PDF İndir](https://purl.stanford.edu/jv222sn2742) |
| **2019** | **🇺🇸 Univ. of Minnesota** | 30k SRAD | *Kompozit Gövde Üretimi.* Filament winding ve yapısal analiz detayları. | [📄 PDF İndir](https://www.scribd.com/document/416462742/UMN-Rocket-Team-2019-IREC-Technical-Report) |

---

## 📖 2. Kutsal Kitaplar (The Bible of Rocketry)
Bu kitaplar olmadan mühendislik yapamazsınız.

| Kitap Adı | Yazar | Konu | Önem Seviyesi |
| :--- | :--- | :--- | :--- |
| **Rocket Propulsion Elements** | George P. Sutton | İtki Sistemleri (SRAD Motor) | ⭐⭐⭐⭐⭐ (Zorunlu) |
| **Modern Engineering for Design of Liquid-Propellant Rocket Engines** | Huzel & Huang | Sıvı Yakıtlı Motorlar | ⭐⭐⭐⭐⭐ (SRAD Liquid) |
| **Modern High Power Rocketry 2** | Mark Canepa | Genel Montaj ve Üretim | ⭐⭐⭐⭐ (Başlangıç) |
| **topics in Advanced Model Rocketry** | High Power Rocketry | Aerodinamik Stabilite | ⭐⭐⭐⭐ |

---

## 🛠️ 3. Yazılım ve Simülasyon (Software)
Hangi araç ne için kullanılır?

### 🌪️ Aerodinamik & Uçuş
*   **[OpenRocket](https://github.com/openrocket/openrocket) (Java):**
    *   *Kullanım:* Genel tasarım, ağırlık merkezi (CG) hesabı, ses altı (Subsonic) simülasyon.
    *   *Limit:* Mach 0.8 üzerinde (Transonic/Supersonic) sürüklenme katsayısını (Cd) yanlış hesaplar.
*   **[RASAero II](http://www.rasaero.com/) (Windows):**
    *   *Kullanım:* **Süpersonik Uçuş (Mach 1+)**.
    *   *Avantaj:* Şok dalgalarını hesaba katar. Mach 1 üstü uçacaksanız OpenRocket yerine bunu kullanın.
*   **[RocketPy](https://github.com/RocketPy-Team/RocketPy) (Python):**
    *   *Kullanım:* Monte Carlo analizi, rüzgar dağılımı, iniş noktası tahmini.
    *   *Avantaj:* Programlanabilir. 1000 uçuşu 1 dakikada simüle eder.

### 🔥 İtki (Propulsion)
*   **[BurnSim](https://www.burnsim.com/):** Katı yakıtlı (Solid) motor tasarımı için endüstri standardı.
*   **[OpenMotor](https://github.com/rerix/openmotor):** BurnSim'in açık kaynaklı alternatifidir. Grain geometrisi (BATES, Star, Finocyl) tasarlamak için kullanılır.

---

## 🧠 4. Eğitim Platformları & Forumlar
Takıldığınızda soracağınız yerler.

*   **[The Rocketry Forum (TRF)](https://www.rocketryforum.com/):** Dünyanın en büyük roket forumu. "Propulsion", "High Power" ve "Recovery" başlıklarını gezin.
*   **[Nakka Rocketry](https://www.nakka-rocketry.net/):** Richard Nakka'nın kişisel sitesi. SRAD motor (Sugar/APCP) yapacaksanız burası bir hazinedir.
*   **[Apogee Rockets Newsletters](https://www.apogeerockets.com/Peak-of-Flight):** 500+ teknik makale arşivi. (Örn: "Shrouded Fins nedir?", "Dual Deployment nasıl yapılır?").

---

## 🎓 5. Çaylaklar İçin Eğitim Yolu (Curriculum)
Yeni katılan bir üye sırasıyla şunları yapmalı:

1.  **Hafta 1:** OpenRocket indir, örnek bir roket tasarla. (CP < CG kuralını öğren).
2.  **Hafta 2:** *Modern High Power Rocketry 2* kitabının "Recovery" bölümünü oku.
3.  **Hafta 3:** `analysis/calculators` içindeki Python araçlarını çalıştır ve mantığını anla.
4.  **Hafta 4:** Takım liderinden bir "Alt Sistem" görevi al.
