## POETHER Kartli Geçis Sistemi

ESP32 ve LAN8720 Ethernet Entegresi kullanarak TCP/IP üzerinden haberleşme sağlayan

Kapı girişlerinde, turnike sistemlerinde, yemekhane otomasyonlarında kullanılmak üzere, 

geliştirilen cihaz.

```
Version:    v0
4 katmanlı PCB

Version:    v1
2 katmanlı PCB
GPIO0 pini GPIO17 olarak değiştirildi
```


**PCB 3D Görünümü**

![ArduGSM2](https://user-images.githubusercontent.com/58399702/174570445-8dfc70df-92fc-401e-99d6-86f883eb7e69.jpg)

## Kart Özellikleri

### Donanım Özellikleri
- Mikrodenetleyici ESP32
- HY931147C POE Destekli RJ45 Modulü
- LAN8270 10/100 Mbit Ethernet Entegresi
- ILI9488 3.5 inç Ekran
- RFID Okuyucu Mini RFID-RC522 13.56 MHz 
- SD Kart Yuvası
- LM2596 Buck Regülatör
- HK19F 5V Röle
- ULN2003 
- PC817 (Kapı durumlarını kontrol etmek için)
- Butonlar (WiFi üzerinden ayar ve Reset için)
- Buzzer


### Tasarım ve Kutu Özellikleri
- Kart 126mmx78mm (EnxBoy) boyutlarında özel kutuya yerleşecek şekilde tasarlanmıştır.



**Kutu - Montajlı**

![Kutu kutu montajlı](https://user-images.githubusercontent.com/58399702/171063774-f5ca42cf-f03c-4323-a74d-35c914cc97f1.jpg)
------------------------

## Pin Bağlantıları

### SIM800C
ESP32	     |   LAN8720      
-------------|------------
GPIO17  	 |   PHY_POWER   : NC - OSC. Enable - 4k7 Pulldown	    
GPIO22  	 |   EMAC_TXD1   : TX1	   
GPIO19		 |	 EMAC_TXD0   : TX0
GPIO21		 |	 EMAC_TX_EN  : TX_EN
GPIO26		 |	 EMAC_RXD1   : RX1
GPIO25		 |	 EMAC_RXD0   : RX0
GPIO27		 |	 EMAC_CRS_DV : CRS
GPIO00       |	 EMAC_TX_CLK : nINT/REFCLK (50MHz) - 4k7 Pullup
GPIO23		 |	 EMAC_MDC    : MDC
GPIO18		 |	 EMAC_MDIO   : MDIO


### Röle ve Buzzer
ESP32  |   Modül      
-------------|---------
2		 |	   Buzzer
5		 |	   RFID
15		 |	   SD Kart
32  	 |     Röle 1
33  	 |     Röle 2	    


--------------------------
### Güç bağlantı seçenekleri
Kart üzerinde Röle, Ekran, Buzzer bağlantıları için 5V
ESP32, LAN8720, RFID, SD Kart bağlantıları için de 3.3V regülatör kullanımaktadır.

Kartın beslemesi 2 şekilde yapılabilmektedir. 

- POE
```
Kart POE üzerinden 35V'a kadar beslenebilmektedir. 
Böylece montajı kolay, kablo kalabalığı da ortadan kalkmaktadır.
```
- ADAPTÖR
```
POE kullanılmadığı takdirde 12-35V aralığında adaptör ile harici olarak beslenebilmektedir.
```


--------------------------
--------------------------

## Kullanılan Kütüphaneler
- [ETH][https://github.com/urbanze/esp32-eth]
- [WiFi]
- [SPIFFS][https://github.com/espressif/arduino-esp32/blob/master/libraries/SPIFFS/src/SPIFFS.h]
- [SPI][https://github.com/espressif/arduino-esp32/blob/master/libraries/SPI/src/SPI.h]
- [MFRC522][https://github.com/miguelbalboa/rfid]
- [ArduinoJson][https://github.com/bblanchon/ArduinoJson]
- [HTTPClient][https://github.com/espressif/arduino-esp32/blob/master/libraries/HTTPClient/src/HTTPClient.h]
- [SD][https://github.com/espressif/arduino-esp32/blob/master/libraries/SD/src/SD.h]
- [WiFiUdp][https://github.com/espressif/arduino-esp32/blob/master/libraries/WiFi/src/WiFiUdp.h]
- [NTPClient][https://github.com/arduino-libraries/NTPClient]
- [TFT_eSPI][https://github.com/Bodmer/TFT_eSPI]
- [TFT_eFEX][https://github.com/Bodmer/TFT_eFEX]
- [qrcode][https://github.com/ricmoo/QRCode]
Kütüphaneyi yükledikten sonra Dosya -> Örnekler -> ArduGSM  uzantısı üzerindeki örnek uygulamalar ile kolaylıkla test yapabilirsiniz.

**ArduGSM kütüphanesi Arduino UNO tabanlı, ArduGSM kontrol kartı için düzenlenmiş bir kütüphanedir. Bununla birlikte SIM800C, SIM800L serisi SIM modülleri için de sorunsuzca kullanılabilir.**


**Mesaj ile Röle Kontrol Örneği**


## Fonksiyonlar
ArduGSM kütüphanesi içerisindeki fonksiyonlar ve bu fonksiyonların işlevleri aşağıda gösterilmektedir. 

void postIslemiYap(String UID);                               //kart okundu ise UID kodu,  QR işlemi var ise QR kodu gönderir
void loadConfiguration(const char *filename, Config &config); //SPIFFS içerisindeki config dosyasından ayarlar yükleniyor
void jsonDegistir(const char *filename, Config & config);     //Bir json dosyasi içerisindeki belirli bir alanı değiştirmek için
void anaEkranQRCode();                                        //Ana Ekran QRCode ve HOSGELDINIZ yazısı
void ekranaUyariMesajiYaz(String mesaj[4]);                   //EKRANA YAZILACAK HATA KODLARI
void ekranaPostSonucuYaz(bool isCheckIn);                     //Post işlemi sonucu ekrana yazılır
bool post(String UIDString, bool isCheckIn);                  //kart okunduğunda post işlemi yapılır
bool postQRCodeIstek();                                       //QRCode oluşturma isteği gönderilir
void WiFiEvent(WiFiEvent_t event);                            //Ethernet bağlantı bilgileri
void connect_wifi(); //wifiye bağlan
void ESP32ChipOzellikListele();
void buzzerCal();
void handleUpload(AsyncWebServerRequest *request, String filename, size_t index, uint8_t *data, size_t len, bool final); // webden gelen dosyayı yükle

void SDKartType();
void logSDCard(); //SD Karta log eklemek için
void writeFile(fs::FS &fs, const char * path, const char * message);  //SD karta veri yazma
void appendFile(fs::FS &fs, const char * path, const char * message); // SD Karta veri eklemevoid readFile(fs::FS &fs, const char * path)
void readFile(fs::FS &fs, const char * path);
void listDir(fs::FS &fs, const char * dirname, uint8_t levels);       //SD Kart dizini listele
void removeDir(fs::FS &fs, const char * path);
void listSPIFFSDir(char * dir); //SPIFFS içeriğini listele
String formatBytes(size_t bytes) ;
____________________________________________________________________________________


*NOT: Testleri gerçekleştirilen ve herhangi bir sorun yaşanmayan geliştirme kartını kullanırken kütüphane veya donanım kaynaklı bir bir sorun yaşadığınızda bu sorunu bize bildirirseniz gelişmemize katkı sağlamış olursunuz.*

İyi çalışmalar.
