# ⌚ InkTime - Open Source Smartwatch

**InkTime** este un smartwatch minimalist și eficient, proiectat pentru a fi open-source. Dispozitivul folosește un ecran E-Paper pentru un consum redus de energie și este motorizat de microcontrolerul nRF52840 (Cortex-M4F) cu suport Bluetooth 5.0.



## 1. Arhitectura Sistemului (Diagrama Bloc)
Dispozitivul este construit în jurul ecosistemului Nordic Semiconductor, integrând următoarele module principale:
* **Unitate Centrală:** nRF52840 (Sistem-on-Chip cu Bluetooth).
* **Afișaj:** E-Paper Display 1.54" (SPI).
* **Senzori:** BMA423 (Accelerometru/Pedometru) via I2C.
* **Management Energie:** BQ25180 (Charger) și MAX17048 (Fuel Gauge).
* **Haptics:** DRV2605 (Haptic Driver) pentru notificări prin vibrații.

---

## 2. Bill of Materials (BOM)

| RefDes | Componentă | Descriere | Link JLC Parts | Datasheet |
| :--- | :--- | :--- | :--- | :--- |
| U1 | nRF52840 | Microcontroller Bluetooth LE | [JLC Part](https://jlcpcb.com/parts/1/nRF52840) | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf) |
| U3 | MAX17048 | Fuel Gauge I2C | [JLC Part](https://jlcpcb.com/parts/1/MAX17048) | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf) |
| J1 | 503480-2400 | Conector E-Paper 24-pin | [JLC Part](https://jlcpcb.com/parts/1/503480-2400) | [Datasheet](https://www.molex.com/pdm_docs/sd/5034802400_sd.pdf) |
| IC2 | DRV2605 | Haptic Driver (LRA/ERM) | [JLC Part](https://jlcpcb.com/parts/1/DRV2605) | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) |
| J4 | USB-C 16P | Conector alimentare/date | [JLC Part](https://jlcpcb.com/parts/1/USB-C) | [Datasheet](...) |

---

## 3. Funcționalitate Hardware

### Managementul Energiei și Consum
Dispozitivul este alimentat de o baterie Li-Po de $150 \text{ mAh}$. Încărcarea este gestionată prin portul USB-C, iar nivelul bateriei este raportat prin I2C către procesor.
* **Calcul Autonomie:**
  Presupunând un consum mediu în regim de utilizare normală (Bluetooth activ, refresh ecran la 1 minut) de aproximativ $1.5 \text{ mA}$:
  $$\text{Autonomie} = \frac{150 \text{ mAh}}{1.5 \text{ mA}} = 100 \text{ ore} \approx 4.1 \text{ zile}$$

### Interfețe de Comunicare
* **SPI (Serial Peripheral Interface):** Folosit pentru ecranul E-Paper (semnale SCK, MOSI, CS) pentru a asigura o rată de transfer rapidă a buffer-ului de imagine.
* **I2C (Inter-Integrated Circuit):** Magistrală comună pentru BMA423, MAX17048 și DRV2605, optimizând numărul de pini utilizați.

---

## 4. Alocare Pini (Pinout nRF52840)

Am ales pinii procesorului pe baza funcțiilor speciale și a rutei optime pe PCB:

| Pin | Semnal | Funcție | Motiv |
| :--- | :--- | :--- | :--- |
| **P0.13** | SW_UP | Input | Butonul "SUS", necesită întrerupere pentru trezire. |
| **P0.14** | SW_ENT | Input | Butonul "ENTER", poziționat central. |
| **P1.02** | SW_DN | Input | Butonul "JOS". |
| **P1.13** | EPD_SCK | SPI Clock | Semnal ceas pentru display (Pagina 1.5B). |
| **P1.15** | EPD_MOSI | SPI Data | Date către display. |
| **P0.26** | I2C_SDA | Data | Linie de date pentru magistrala I2C. |
| **P0.27** | I2C_SCL | Clock | Linie de ceas pentru magistrala I2C. |

---

## 5. Design și Layout PCB
* **Grosime Placă:** $1.0 \text{ mm}$ (optimizat pentru carcasă).
* **Layer-ul TOP:** Toate componentele sunt montate pe acest strat pentru a facilita procesul de asamblare automată.
* **Antena RF:** Plasată pe marginea PCB-ului, cu planul de masă (GND) decupat dedesubt pentru a maximiza raza de acțiune Bluetooth.
* **Decuplare:** Condensatoarele de $100 \text{ nF}$ sunt plasate la o distanță de maxim $1 \text{ mm}$ de pinii VDD ai integratelor.

---

## 6. Imaginile Proiectului
### PCB Design (Top View)


### Ansamblu 3D (Exploded View)


---

## 7. Licență
Acest proiect este licențiat sub **GNU General Public License v3.0**. Consultați fișierul `LICENSE` pentru detalii.
