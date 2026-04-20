# INK TIME SMARTWATCH

Un smartwatch bazat pe arhitectura **nRF52840**, echipat cu un ecran **e-Paper** pentru consum ultra-redus de energie și un sistem de alertă haptică.

---

## Cuprins

1. [Diagrama Bloc](#diagrama-bloc)
2. [Descrierea Funcționalității Hardware](#descrierea-funcționalității-hardware)
3. [Bill of Materials (BOM)](#bill-of-materials-bom)
4. [Configurația Pinilor nRF52840](#configurația-pinilor-nrf52840)

---

## Diagrama Bloc

```
┌─────────────────────────────────────────────────────────────┐
│                        POWER SUPPLY                         │
│                                                             │
│  Li-Po Battery (3.7V)                                       │
│       │                                                     │
│       ▼                                                     │
│  BQ25180 (Charger IC)  ◄──── USB Type-C                     │
│       │                                                     │
│       ▼                                                     │
│  RT6160A (Buck-Boost)  ──► 3.3V stable output              │
│       │                                                     │
│  MAX17048 (Fuel Gauge) ──► I2C ──► nRF52840                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                     PROCESSING UNIT                         │
│                                                             │
│              nRF52840 (ARM Cortex-M4F)                      │
│         ┌──────────────┴──────────────┐                     │
│         │                            │                      │
│       SPI bus                     I2C bus                   │
│         │                            │                      │
│   e-Paper Display              ┌─────┴──────┐               │
│   (1.54")                      │            │               │
│                            BMA423      DRV2605L              │
│                          (Accel.)    (Haptic Driver)         │
└─────────────────────────────────────────────────────────────┘
```

---

## Descrierea Funcționalității Hardware

Sistemul este construit în jurul SoC-ului **nRF52840**, optimizat pentru un consum ultra-scăzut de energie și securitate hardware.

### Microcontroller — nRF52840

Procesor ARM Cortex-M4F cu unitate de criptare hardware CryptoCell-310. Gestionează stack-ul **Bluetooth Low Energy (BLE)** și coordonează toate perifericele. Rulează la 64 MHz, dar stă majoritatea timpului în modul **System ON Sleep** (~3 µA).

### Sistemul de Alimentare (Power Tree)

| Componentă | Rol |
|---|---|
| **BQ25180** | IC de încărcare cu Power Path — permite încărcarea bateriei LiPo și alimentarea simultană a sistemului via USB Type-C |
| **MAX17048** | Fuel Gauge — măsoară precis starea de încărcare (SoC) și tensiunea bateriei, comunicând prin I2C |
| **RT6160A** | Regulator Buck-Boost — asigură o tensiune stabilă de 3.3V indiferent de nivelul de descărcare al bateriei |

### Afișaj e-Paper (1.54")

Display de tip *Electronic Ink* care consumă curent **doar la refresh**. Include un circuit de protecție cu un PFET (DMG2305UX) pentru a opri complet alimentarea ecranului în standby, eliminând scurgerile de curent. Comunică cu MCU prin **SPI** (interfață de mare viteză).

### Senzor de Mișcare — BMA423

Accelerometru triaxial de mică putere (12-bit, 3 axe). Utilizat pentru:
- **Pedometru** (numărarea pașilor)
- **Detectarea gesturilor** — ex: *tilt-to-wake* (ridicarea mâinii aprinde ecranul)

### Driver Haptic — DRV2605L

Controlează motorul de vibrații (ERM/LRA) folosind librării de efecte pre-programate pentru feedback tactil. Comunică prin **I2C**.

### Cristale de Referință

**Cristal 32 MHz (X1)** — esențial pentru funcționarea blocului radio (Bluetooth). Oferă referința de înaltă frecvență necesară stabilității semnalului RF și procesării rapide a datelor.

**Cristal 32.768 kHz (X2)** — componentă critică pentru optimizarea consumului. Permite funcționarea ceasului de timp real (RTC) în modurile de **Deep Sleep**, menținând precizia timpului cu un consum infim de energie, fără a fi nevoie de oscilatorul intern RC (mai puțin precis și mai consumator).

### Antena 2.4 GHz

Antenă de tip **SMD ceramic** (Johanson 2450AT18B100E) pentru frecvența de 2.4 GHz (BLE). A fost respectată o zonă de *keep-out* (fără cupru) pe toate straturile sub antenă pentru a preveni atenuarea semnalului.

### Interfețe de Comunicație

| Interfață | Periferice conectate |
|---|---|
| **I2C** | BMA423, DRV2605L, BQ25180, MAX17048 |
| **SPI** | Display e-Paper (interfață de mare viteză) |
| **GPIO cu întreruperi** | Butoane UP / ENTER / DOWN |
| **SWD** | Programare și debugging via Tag-Connect (J2) |

### Consumul de Energie

Bateria de **250 mAh** susține o autonomie estimată de **30–45 zile**, datorită:
- Ecranului e-Paper care nu consumă energie între actualizări
- Senzorilor configurați pe modul de consum minim cu întreruperi
- nRF52840 în System ON Sleep (~3 µA) marea majoritate a timpului

---

## Bill of Materials (BOM)

| Componentă | Descriere | Qty | Ref. Designator | Producător |
|---|---|:---:|---|---|
| **nRF52840** | MCU ARM Cortex-M4F + BLE 5.0 | 1 | U1 | Nordic Semiconductor |
| **BQ25180YBGR** | IC Încărcare Baterie Li-Po/Li-Ion | 1 | IC1 | Texas Instruments |
| **MAX17048G+T10** | Fuel Gauge 1-Cell cu ModelGauge | 1 | U2 | Analog Devices (Maxim) |
| **RT6160AWSC** | Regulator Buck-Boost DC-DC I2C | 1 | IC9 | Richtek |
| **DRV2605YZFR** | Driver Haptic ERM/LRA | 1 | IC2 | Texas Instruments |
| **BMA423** | Accelerometru Triaxial Low-g 12-bit | 1 | IC3 | Bosch Sensortec |
| **2450AT18B100E** | Antenă SMD 2.45 GHz (BLE) | 1 | ANT1 | Johanson Technology |
| **KH-TYPE-C-16P** | Conector USB Type-C 16 pini | 1 | J4 | Kinghelm |
| **503480-2400** | Conector FPC 24 pini, pitch 0.5mm (Display) | 1 | J1 | Molex |
| **TC2030-IDC** | Conector programare Tag-Connect 6 pini | 1 | J2 | Tag-Connect |
| **USBLC6-2SC6Y** | Diodă TVS Protecție ESD (USB) | 1 | D3 | STMicroelectronics |
| **MBR0530** | Diodă Schottky 30V / 0.5A | 3 | D2, D4, D5 | ON Semiconductor |
| **DMG2305UX-7** | MOSFET P-Channel 20V / 4.2A (e-Paper power switch) | 1 | Q1 | Diodes Inc. |
| **SI1308EDL-T1-GE3** | MOSFET N-Channel 30V / 1.5A | 1 | Q3 | Vishay Siliconix |
| **EVP-AKE31A** | Butoane Tactile SMD (Navigare) | 3 | SW_UP, SW_DN, SW_ENT | Panasonic |
| **744043680** | Inductor / Bobină 68 µH | 1 | L5 | Würth Elektronik |
| **FTC252012SR47MBCA** | Inductor SMD 0.47 µH | 1 | L7 | TDK |
| **Cristal 32 MHz** | Referință RF pentru bloc radio BLE | 1 | X1 | — |
| **Cristal 32.768 kHz** | Referință RTC pentru Deep Sleep | 1 | X2 | — |
| **Condensatori / Rezistențe** | Pasive SMD (0201, 0402) | ~60 | C*, R* | — |
| **Test Pads** | Puncte de test (3.3V, GND, SWD, I2C etc.) | 14 | TP_* | — |

---

## Configurația Pinilor nRF52840

### Interfața Ecranului e-Paper (SPI)

| Semnal | Pin nRF52 | Direcție | Descriere |
|---|---|---|---|
| EPD_SCK | P0.28 | Output | Clock SPI pentru refresh ecran |
| EPD_MOSI | P0.29 | Output | Date imagine (frame buffer) |
| EPD_CS | P0.30 | Output | Chip Select — activează comunicația cu display-ul |
| EPD_DC | P0.31 | Output | Data/Command — diferențiază comenzile de datele de imagine |
| EPD_BUSY | P0.04 | Input | Semnal activ cât timp ecranul procesează un refresh |
| EPD_PWR | P0.13 | Output | Enable alimentare ecran — dezactivat complet în standby |

### Magistrala Senzorilor (I2C)

Toți senzorii (BMA423, MAX17048, DRV2605L, BQ25180) împart aceeași magistrală I2C.

| Semnal | Pin nRF52 | Descriere |
|---|---|---|
| I2C_SDA | P0.26 | Linie de date (partajată între toți senzorii) |
| I2C_SCL | P0.27 | Linie de clock |

### Butoane de Navigare

Pini configurați cu pull-up intern și capabili de **wake-up din Deep Sleep** prin întrerupere hardware.

| Buton | Pin nRF52 | Funcție |
|---|---|---|
| UP | P1.00 | Navigare meniu — sus |
| DOWN | P1.01 | Navigare meniu — jos |
| ENTER | P1.02 | Selecție / Confirmare |

### Alte Conexiuni Importante

| Semnal | Pin nRF52 | Descriere |
|---|---|---|
| SHAKER_EN | P0.02 | PWM / Enable pentru driver haptic DRV2605 |
| GAUGE_INT | P1.15 | Întrerupere de la MAX17048 la nivel critic de baterie |
| LED_1 | P0.13 | LED de stare pentru notificări vizuale |
| LED_2 | P0.14 | LED de stare secundar |
| SWDIO | — | Programare și debug via Tag-Connect J2 |
| SWDCLK | — | Clock SWD pentru J-Link |

---

## Imagini PCB

| PCB 3D View | Exploded View | Final View |
|---|---|---|
| `./Images/PCB_3D.png` | `./Images/exploded_view.png` | `./Images/final_view.png` |

---

*INK TIME SMARTWATCH — Hardware Design Documentation*
