# STM32 Dış Kesme (EXTI) ile Buton ve LED Kontrolü

Bu proje, STM32 mikrodenetleyicilerinde **Dış Kesme (External Interrupt - EXTI)** yapısının nasıl kullanılacağını gösteren temel bir uygulamadır.

Geleneksel "Polling" (sürekli butonu kontrol etme) yöntemi yerine "Interrupt" (kesme) yöntemi kullanılmıştır. Bu sayede işlemci `while(1)` sonsuz döngüsünde boşta beklerken (veya başka işler yaparken), butona basıldığı anda donanımsal olarak uyarılır ve LED'in durumu anında değiştirilir. Bu yöntem, işlemci gücünden tasarruf sağlar ve kodun tepki süresini (response time) minimize eder.

## 🛠️ Kullanılan Teknolojiler ve Araçlar
* **Donanım:** STM32 Geliştirme Kartı (Kod yapısı STM32F3 Discovery serisi portlarına göre uyarlanmıştır)
* **Yazılım:** STM32CubeIDE / STM32CubeMX
* **Kütüphane:** STM32 HAL (Hardware Abstraction Layer)
* **Dil:** C

## ⚙️ Nasıl Çalışır?

Projenin temel işleyiş mekanizması şu adımlara dayanır:
1. **GPIO Yapılandırması:** Kullanıcı butonu (`B1_Pin`), sıradan bir giriş yerine `GPIO_MODE_IT_RISING` (Yükselen Kenar Kesmesi) olarak ayarlanmıştır. Bu, butona basıldığı an (sinyal 0'dan 1'e çıktığında) bir kesme üretileceği anlamına gelir.
2. **NVIC (Nested Vectored Interrupt Controller):** EXTI hattı için kesme öncelikleri belirlenmiş ve kesmeler `HAL_NVIC_EnableIRQ` ile aktif hale getirilmiştir.
3. **Donanım Tetikleyicisi (`EXTI0_IRQHandler`):** Donanımdan gelen sinyal yakalanır ve HAL kütüphanesinin kesme yöneticisine iletilir.
4. **Geri Çağırma (Callback) Fonksiyonu:** İşlemci, `HAL_GPIO_EXTI_Callback` fonksiyonuna sıçrar. Burada sinyalin doğru butondan gelip gelmediği kontrol edilir ve **E Portundaki LD3 LED'inin** durumu (`HAL_GPIO_TogglePin`) değiştirilir.

## 📌 Pin Konfigürasyonu
* **Kullanıcı Butonu (Giriş / EXTI):** `B1_Pin` (EXTI Line)
* **Kontrol Edilen LED (Çıkış):** `PE9` / `LD3_Pin` (GPIO_Output)
* *Not: E portundaki diğer LED'ler proje başlangıcında kapalı (RESET) durumuna getirilmiştir.*

## 🚀 Kurulum ve Kullanım

1. Bu depoyu bilgisayarınıza klonlayın:
   ```bash
   git clone [https://github.com/KULLANICI_ADINIZ/STM32-EXTI-Button-Interrupt.git](https://github.com/KULLANICI_ADINIZ/STM32-EXTI-Button-Interrupt.git)
