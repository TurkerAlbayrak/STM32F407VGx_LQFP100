# STM32
> **Kart:** STM32F4 Discovery (STM32F407VGT6)  
> **IDE:** STM32CubeIDE v1.15.1

---

## 📌 İçindekiler

1. [Temel Kavramlar](#1-temel-kavramlar)
2. [Mikrodenetleyici Mimarisi](#2-mikrodenetleyici-mimarisi)
3. [STM32 Ailesi](#3-stm32-ailesi)
4. [STM32F4 Discovery Kartı](#4-stm32f4-discovery-kartı)
5. [Geliştirme Araçları](#5-geliştirme-araçları)
6. [Saat Yapılandırması (Clock)](#6-saat-yapılandırması)
7. [GPIO](#7-gpio)

---

## 1. Temel Kavramlar

### Mikroişlemci (MPU) vs Mikrodenetleyici (MCU)

| Özellik | Mikroişlemci (MPU) | Mikrodenetleyici (MCU) |
|---|---|---|
| **Tanım** | Yalnızca CPU barındıran, yoğun işlem gücü odaklı devre | CPU + Bellek + Zamanlayıcı + I/O hepsini tek çipte barındıran devre |
| **Kullanım Alanı** | Mini PC, tablet, akıllı telefon | Çamaşır makinesi, termostat, gömülü sistem |
| **Saat Hızı** | Yüksek (GHz) | Düşük–Orta (MHz) |
| **RAM** | Yüksek (MB – GB) | Düşük (KB) |
| **ROM / Flash** | Yüksek (GB – TB) | Düşük (KB – 2 MB) |
| **Güç Tüketimi** | Yüksek | Düşük |
| **Maliyet** | Pahalı | Ucuz |
| **Boyut** | Büyük | Küçük |

> 💡 **Özet:** MCU'nun görevi genellikle sabittir; MPU ise genel amaçlı hesaplama yapar.

---

## 2. Mikrodenetleyici Mimarisi

Mikrodenetleyici; işlemci, hafıza ve giriş/çıkış birimlerini tek bir entegre devre üzerinde barındıran **mini bir bilgisayardır**.

### 🧠 Bellek Türleri

| Bellek | Özellik | Açıklama |
|---|---|---|
| **ROM** | Kalıcı, salt okunur | Fabrikadan yüklenen yazılım burada saklanır |
| **RAM** | Geçici (uçucu) | Çalışma sırasındaki geçici veriler burada tutulur |
| **EEPROM / Flash** | Kalıcı, yeniden yazılabilir | Programlama sırasında kullanıcı yazılımının yüklendiği alan |

### ⚙️ Diğer Temel Bileşenler

- **I/O (Giriş/Çıkış):** Sensörler, motorlar, LED'ler, butonlar ile etkileşim sağlar.
- **Zamanlayıcı / Sayıcı (Timer/Counter):** Zamanlamalı işlemler ve periyodik görevler için kullanılır.
- **Haberleşme Protokolleri:** Diğer cihazlarla iletişim — UART, SPI, I2C, CAN vb.
- **ADC (Analog → Dijital Dönüştürücü):** Analog sinyali (örn. sıcaklık sensörü) dijital veriye dönüştürür.
- **Kesme Sistemi (Interrupt):** Belirli olaylar gerçekleştiğinde CPU mevcut işi bırakıp ilgili kesme servis rutinine (ISR) yönelir; öncelik sıralamasına göre yönetilir.

### 🏭 Kullanım Alanları

- Endüstriyel Otomasyon
- Ev Elektroniği
- Medikal Cihazlar (EKG, nabız ölçer, kan şekeri ölçer)
- Otomotiv Sistemleri (ABS, motor kontrol ünitesi)

---

## 3. STM32 Ailesi

STM32, **STMicroelectronics** tarafından üretilen, **ARM Cortex-M** çekirdekleri (M0, M0+, M3, M4, M7) kullanan bir 32-bit mikrodenetleyici ailesidir.

![[Pasted image 20260626102112.png]]

### Sınıflandırma

| Kategori | Açıklama | Öne Çıkan Modeller |
|---|---|---|
| **High Performance** | Yüksek hesaplama gücü gerektiren uygulamalar | STM32H7 (480 MHz), STM32F7 (216 MHz), STM32F4 (180 MHz) |
| **Mainstream** | Genel amaçlı, yaygın kullanılan ana sınıf | STM32G0, STM32G4, STM32F0, STM32F1, STM32F3 |
| **Ultra-Low-Power** | Pil ile uzun süre çalışan uygulamalar | STM32L0, STM32L1, STM32L4, STM32L4+, STM32L5 |
| **Wireless** | Kablosuz iletişim entegre modeller | STM32WB (BLE + Cortex-M4/M0+), STM32WL (LoRa) |

---

## 4. STM32F4 Discovery Kartı

### Donanım Blok Diyagramı

![[Pasted image 20260626103117.png]]

Kart üzerindeki başlıca bileşenler:

| Bileşen | Detay |
|---|---|
| **MCU** | STM32F407VGT6 — ARM Cortex-M4 @ 168 MHz, FPU |
| **Programlayıcı** | Yerleşik ST-LINK/V2 (Mini USB üzerinden SWD) |
| **LED'ler** | LD3 Turuncu (PD13), LD4 Yeşil (PD12), LD5 Kırmızı (PD14), LD6 Mavi (PD15) |
| **Butonlar** | B1 USER (PA0), B2 RESET |
| **MEMS Sensör** | LIS302DL — İvme ölçer (SPI) |
| **Mikrofon** | MP45DT02 — Dijital MEMS mikrofon |
| **Ses DAC** | CS43L22 — Mini Jack ses çıkışı (I2C + I2S) |
| **USB** | Mini USB (ST-LINK) + Micro USB (USB OTG FS) |
| **Header** | P1 ve P2 — 2×25 pin (tüm I/O pinleri erişilebilir) |

### Şematikler

**Genel Pin Haritası — Tüm Portların Genel Görünümü**

![[Pasted image 20260626103445.png]]

**MCU Çekirdeği Şematiği — STM32F407VGT6 Bağlantıları**

![[Pasted image 20260626103525.png]]

> Dikkat: Harici kristal osilatör (8 MHz) PH0/PH1 pinlerine bağlıdır.

**Ses Devresi — CS43L22 DAC ve MP45DT02 Mikrofon**

![[Pasted image 20260626103724.png]]

**Çevresel Birimler (Peripherals) — Butonlar ve LED'ler**

![[Pasted image 20260626103816.png]]

> `USER & WAKE-UP` butonu **PA0** pinine bağlıdır.  
> LED'ler **PD12–PD15** pinlerine bağlıdır (680Ω direnç üzerinden).

### STM32F407 Fonksiyonel Blok Diyagramı (Reference Manual)

![[Pasted image 20260626112302.png]]

> Bu diyagram, MCU içindeki tüm çevresel birimlerin (timer, UART, SPI, DMA, ADC vb.) AHB/APB veri yollarına nasıl bağlandığını gösterir.

---

## 5. Geliştirme Araçları

| Araç | Açıklama |
|---|---|
| **STM32CubeIDE** | Ana geliştirme ortamı: kod yazma, derleme, debug ve pin yapılandırması |
| **STM32CubeMonitor** | Çalışan sistemde gerçek zamanlı değişken ve sinyal izleme |
| **STM32CubeProgrammer** | Karta doğrudan flash programlama aracı |
| **Termite** | Serial terminal — UART haberleşmesini izlemek için |
| **SWD / JTAG** | Karta bağlanmak için debug arabirimi (ST-LINK/V2 üzerinden) |

> ⚠️ **Not:** Hiç programlanmamış boş bir kartın flash belleği **`0xFF`** değeriyle dolu görünür.

### STM32CubeIDE ile Proje Oluşturma

1. `File → New → STM32 Project` menüsünü aç.
2. **MCU/MPU** sekmesinden hedef mikrodenetleyiciyi seç (örn. `STM32F407VGTx`).
3. Proje ismini gir → **Finish** butonuna bas.

![[Pasted image 20260626131541.png]]

---

## 6. Saat Yapılandırması

Saat sinyali (clock), mikrodenetleyicinin tüm işlemlerinin zamanlamasını belirleyen temel zamanlama sinyalidir; osilatör tarafından üretilir.

> ⚡ **Kural:** Frekans ne kadar yüksekse → sistem o kadar hızlı çalışır, o kadar fazla güç tüketir.

### Saat Kaynakları

| Kaynak | Tür | Frekans | Açıklama |
|---|---|---|---|
| **HSI** — High Speed Internal | Dahili | 16 MHz | Düşük hassasiyetli; hızlıca hazır olur |
| **LSI** — Low Speed Internal | Dahili | 32 kHz | Watchdog (IWDG) ve RTC için |
| **HSE** — High Speed External | Harici | 4 – 26 MHz | Yüksek hassasiyetli; kristal osilatör (kart: 8 MHz) |
| **LSE** — Low Speed External | Harici | 32.768 kHz | RTC için hassas zaman kaynağı |
| **PLL** | Çoğaltıcı devre | — | Kaynak frekansını artırır ve sabitler (SYSCLK → 168 MHz) |

> STM32F4 Discovery kartındaki harici kristal: **8 MHz**  
> STM32F4 dahili osilatörü: **HSI = 16 MHz** (düşük hassasiyet)

### Gerekli Belgeler

Her STM32 projesi için 3 temel dokümana ihtiyaç vardır:

- 📄 **Schematic (Şematik):** Kartın devre bağlantıları
- 📄 **Datasheet:** MCU'nun pin tanımları ve elektriksel özellikleri
- 📄 **Reference Manual:** Yazmaçlar (register) ve çevresel birimlerin detaylı açıklamaları

---

### STM32CubeIDE ile Adım Adım Saat Yapılandırması

#### Adım 1 — Pinout & Configuration ekranını aç

Proje açıldığında `.ioc` dosyasına tıkladığında bu ekran açılır:

![[Pasted image 20260626131534.png]]

#### Adım 2 — System Core → RCC → HSE'yi etkinleştir

Sol panelden `System Core → RCC` seç. Varsayılan durum (HSE ve LSE devre dışı):

![[Pasted image 20260626131602.png]]

`High Speed Clock (HSE)` değerini **Crystal/Ceramic Resonator** olarak seç:

![[Pasted image 20260626131617.png]]

> Bu ayar, PH0 ve PH1 pinlerini kristal osilatör için otomatik yapılandırır.

#### Adım 3 — Clock Configuration sekmesine geç

**Varsayılan durum** (henüz PLL ayarlanmamış, 16 MHz HSI):

![[Pasted image 20260626131632.png]]

**Yapılandırılmış durum** — 8 MHz HSE + PLL → 168 MHz SYSCLK:

![[Pasted image 20260626131710.png]]

PLL hesabı:

```
HSE (8 MHz)
  ÷ 8   (PLLM bölücü)
  × 168 (PLLN çarpanı)
  ÷ 2   (PLLP bölücü)
= 168 MHz (SYSCLK)
```

APB1 max. 42 MHz → `/4` bölücü | APB2 max. 84 MHz → `/2` bölücü

#### Adım 4 — Kodu Üret

Üst araç çubuğundaki **Device Configuration Tools → Code Generation** butonuna bas (ya da `Ctrl+S` ile kaydet). `main.c` otomatik oluşturulur:

![[Pasted image 20260626131736.png]]

Üretilen `main.c` iskelet yapısı:

```c
int main(void) {
    HAL_Init();              /* Donanım soyutlama katmanını (HAL) başlat */
    SystemClock_Config();    /* Saat yapılandırması — otomatik üretildi  */
    MX_GPIO_Init();          /* GPIO başlatma — otomatik üretildi        */

    while (1) {
        /* USER CODE BEGIN WHILE */

        /* USER CODE END WHILE */
    }
}
```

> 💡 Kullanıcı kodu yalnızca `/* USER CODE BEGIN ... */` ve `/* USER CODE END ... */` blokları arasına yazılmalıdır. Aksi hâlde kod yeniden üretildiğinde silinir.

---

## 7. GPIO

### STM32F4 GPIO Özellikleri

![[Pasted image 20260626130816.png]]

| Özellik | Açıklama |
|---|---|
| **Port Sayısı** | GPIOA – GPIOI arası portlar; her port 16 pine kadar çıkabilir |
| **Çıkış Modu** | **Push-Pull:** aktif HIGH/LOW sürme \| **Open-Drain:** sadece LOW sürme |
| **Port Hızı** | Ayarlanabilir: Low / Medium / Fast / High Speed |
| **Giriş Modu** | Pull-Up / Pull-Down / Analog / Floating (Boşta) |

---

### Pull-Up ve Pull-Down Dirençleri

Buton gibi dijital girişlerde **belirsiz (floating) durum** oluşmasını önlemek için pull dirençleri kullanılır.

![[Pasted image 20260626132034.png]]

| Yapılandırma | Buton Açık (Basılmamış) | Buton Kapalı (Basılmış) | Tipik Kullanım |
|---|---|---|---|
| **Pull-Up** | `HIGH (3.3V)` | `LOW (GND)` | Buton GND'ye bağlıysa |
| **Pull-Down** | `LOW (0V)` | `HIGH (3.3V)` | Buton VCC'ye bağlıysa |

> 💡 **Not:** STM32F4 Discovery üzerindeki USER butonu (PA0) harici pull-down ile bağlıdır. Butona basıldığında **3.3V (HIGH)** okunur.

---

### STM32CubeIDE ile GPIO Yapılandırması

1. `System Core → GPIO` seç.
2. Pin view'deki istediğin pine tıkla (örn. PD12) → `GPIO_Output` seç.
3. PD12, PD13, PD14, PD15 pinlerini output olarak ayarla (Discovery kartındaki 4 LED).

![[Pasted image 20260626150044.png]]

**Her pin için yapılandırma seçenekleri:**

| Seçenek | Olası Değerler |
|---|---|
| GPIO output level | Low / High (başlangıç durumu) |
| GPIO mode | Output Push Pull / Output Open Drain |
| Pull-up / Pull-down | No pull-up and no pull-down / Pull-up / Pull-down |
| Maximum output speed | Low / Medium / Fast / High |
| User Label | Pin için özel isim (ör. `LED_GREEN`) |

---

### Örnek Kod — HAL ile LED Kontrolü

`while(1)` döngüsü içinde **PD12–PD15 pinlerini HIGH** yaparak 4 LED'i aynı anda yak:

```c
while (1) {
    /* USER CODE BEGIN WHILE */

    HAL_GPIO_WritePin(
        GPIOD,
        GPIO_PIN_12 | GPIO_PIN_13 | GPIO_PIN_14 | GPIO_PIN_15,
        GPIO_PIN_SET      /* GPIO_PIN_RESET → söndür */
    );

    /* USER CODE END WHILE */
}
```

![[Pasted image 20260626150033.png]]

**Sık Kullanılan GPIO HAL Fonksiyonları:**

```c
/* Pin çıkışını ayarla */
HAL_GPIO_WritePin(GPIOx, GPIO_PIN_x, GPIO_PIN_SET);    // HIGH yap
HAL_GPIO_WritePin(GPIOx, GPIO_PIN_x, GPIO_PIN_RESET);  // LOW yap

/* Pin çıkışını tersle */
HAL_GPIO_TogglePin(GPIOx, GPIO_PIN_x);

/* Pin girişini oku */
GPIO_PinState state = HAL_GPIO_ReadPin(GPIOx, GPIO_PIN_x);
// Döner: GPIO_PIN_SET (1) veya GPIO_PIN_RESET (0)
```

---

## 📎 Hızlı Başvuru

### STM32F407VGT6 — Temel Özellikler

```
Çekirdek  : ARM Cortex-M4 @ 168 MHz + FPU (float point unit)
Flash     : 1 MB
SRAM      : 192 KB (128 KB + 64 KB CCM)
GPIO      : GPIOA–GPIOI (16 pin/port)
Timer     : TIM1–TIM14 (gelişmiş, genel amaçlı, temel)
Haberleşme: UART×6, SPI×3, I2C×3, CAN×2, USB OTG HS/FS
ADC       : 3× ADC, 12-bit, 24 kanal
DAC       : 2 kanal
DMA       : 2× DMA kontrolcüsü, 8 akış
Ethernet  : 10/100 MAC
```

### Mikrodenetleyici İçin Genel Mimari Şema

```
Mikrodenetleyici (MCU)
├── CPU (İşlemci çekirdeği)
├── Bellek
│   ├── ROM / Flash  ─ Kalıcı program belleği
│   ├── RAM          ─ Geçici çalışma belleği
│   └── EEPROM       ─ Kalıcı veri belleği
├── I/O Portları (GPIO)
├── Zamanlayıcı / Sayıcı (Timer / Counter)
├── Haberleşme Arabirimleri (UART, SPI, I2C, CAN...)
├── ADC / DAC
└── Kesme Kontrolcüsü (NVIC)
```
