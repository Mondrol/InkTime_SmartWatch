# InkTime_SmartWatch

DIAGRAMA BLOC

```mermaid
graph TD
    subgraph "Power Management"
        USB[Port USB-C] --> Charger[BQ25180 Charger]
        Charger --> Bat[Baterie Li-Po]
        Bat --> FG[MAX17048 Fuel Gauge]
        Bat --> Vreg[RT6160 Regulator 3.3V]
    end

    subgraph "Unitate Centrală"
        MCU((nRF52840<br>MCU + BLE))
    end

    subgraph "Periferice"
        Disp[Ecran E-Paper]
        IMU[BMA423 Accelerometru]
        Haptic[DRV2605 Vibrații]
        Btns[Butoane Tactile]
    end

    Vreg --> MCU
    Vreg --> Disp
    MCU -- SPI --> Disp
    MCU -- I2C --> IMU
    MCU -- I2C --> FG
    MCU -- I2C --> Haptic
    Btns -- GPIO --> MCU

### Bill of Materials (BOM)

Mai jos se regăsesc componentele folosite în designul hardware al acestui smartwatch:

| Componentă / Nume | Descriere | Cantitate | Cod JLC (LCSC) | Datasheet | Sursă Procurare |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **nRF52840** | Microcontroller Bluetooth 5.0 ARM Cortex-M4F | 1 | C190733 | [Link](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=C190733) |
| **BQ25180YBGR** | IC Încărcare Baterie Li-Po / Li-Ion | 1 | C3035313 | [Link](https://www.ti.com/lit/ds/symlink/bq25180.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=BQ25180) |
| **MAX17048G+T10** | Fuel Gauge (Monitorizare nivel baterie 1-Cell) | 1 | C236316 | [Link](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=MAX17048) |
| **RT6160AWSC** | Regulator Buck-Boost DC-DC I2C (Richtek) | 1 | C5149341 | [Link](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-02.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=RT6160A) |
| **DRV2605YZFR** | Driver Haptic pentru motoraș ERM/LRA | 1 | C152344 | [Link](https://www.ti.com/lit/ds/symlink/drv2605.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=DRV2605) |
| **BMA423** | Accelerometru Triaxial Low-g 12-bit | 1 | C283838 | [Link](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma423-ds004.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=BMA423) |
| **2450AT18B100E** | Antenă SMD 2.4GHz (Bluetooth) | 1 | C257007 | [Link](https://www.johansontechnology.com/datasheets/2450AT18B100/2450AT18B100.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=2450AT18B100E) |
| **KH-TYPE-C-16P** | Conector USB Type-C 16 Pini | 1 | C2988369 | [Link](https://www.snapeda.com/parts/KH-TYPE-C-16P/Kinghelm/view-part/) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=TYPE-C-16P) |
| **503480-2400** | Conector FPC 24 pini (Display) 0.5mm pitch | 1 | C378413 | [Link](https://www.molex.com/pdm_docs/sd/5034802400_sd.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=503480-2400) |
| **USBLC6-2SC6Y** | Diodă TVS Protecție ESD (USB) | 1 | C152003 | [Link](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=USBLC6-2SC6Y) |
| **MBR0530** | Diodă Schottky 30V 0.5A (ON Semi) | 3 | C438318 | [Link](https://www.onsemi.com/pdf/datasheet/mbr0530t1-d.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=MBR0530) |
| **DMG2305UX-7** | Tranzistor P-Channel MOSFET 20V 4.2A | 1 | C145028 | [Link](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=DMG2305UX) |
| **SI1308EDL-T1-GE3**| Tranzistor N-Channel MOSFET 30V 1.5A | 1 | C106199 | [Link](https://www.vishay.com/docs/71333/si1308ed.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=SI1308EDL) |
| **TC2030-IDC** | Amprentă conector programare Tag-Connect 6-pin | 1 | N/A | [Link](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-IDC_1.pdf) | N/A |
| **EVP-AKE31A** | Butoane Tactile Panasonic (Navigare ceas) | 3 | C238262 | [Link](https://industrial.panasonic.com/cdbs/www-data/pdf/ATV0000/ATV0000CE3.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=EVP-AKE31A) |
| **744043680** | Inductor / Bobină 68uH (Wurth) | 1 | C114389 | [Link](https://www.we-online.com/catalog/datasheet/744043680.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=744043680) |
| **FTC252012SR47MBCA** | Inductor / Bobină SMD 0.47uH (TDK) | 1 | C5832368 | [Link](https://product.tdk.com/system/files/dam/doc/product/inductor/inductor/smd/catalog/inductor_commercial_power_mlp2016_en.pdf) | [JLC Parts](https://jlcpcb.com/parts/componentSearch?searchTxt=FTC252012SR47MBCA) |
| **Cristale Cuarț** | Oscilatoare 32MHz (X1) și 32.768kHz (X2) | 2 | C13738 | - | [JLC Parts](https://jlcpcb.com/parts) |
| **Pasive** | Condensatori, Rezistențe SMD și Test-Pads | ~60 | - | N/A | JLC Parts |



### Descrierea Funcționalității Hardware

Acest proiect reprezintă un smartwatch gândit în special pentru a avea o autonomie cât mai mare a bateriei. Pentru a obține acest lucru, am ales componente care consumă foarte puțin curent.

#### 1. Microcontroller-ul
Componenta principală este cipul **nRF52840**. L-am ales pentru că este perfect pentru dispozitive mici: are putere de procesare suficientă pentru un ceas și vine cu **Bluetooth 5.0** gata integrat, ca să se poată conecta la telefon.

#### 2. Ecranul și Interacțiunea
* **Display-ul:** Folosim un ecran de tip E-Ink (similar cu e-book readerele). Marele avantaj este că nu consumă absolut deloc curent ca să afișeze o imagine fixă pe ecran, ci doar atunci când imaginea se schimbă. El comunică cu microcontroller-ul prin interfața **SPI** (care este foarte rapidă).
* **Butoanele:** Ceasul are 3 butoane fizice pe margine (UP, ENTER, DOWN) pentru navigarea în meniu. Acestea se leagă direct la pinii nRF52840.
* **Notificări (Vibrații):** Pentru a simți notificările pe mână, avem un mic motoraș de vibrații. Este controlat de un cip dedicat (DRV2605), care primește comenzile de la microcontroller prin interfața **I2C**.

#### 3. Senzori (Accelerometrul)
Pentru a putea număra pașii sau pentru a ști când ridicăm mâna să ne uităm la ceas, am inclus un accelerometru (Bosch BMA423). Acesta detectează mișcarea pe 3 axe și trimite datele tot prin interfața **I2C**.

#### 4. Alimentarea și Bateria
Aceasta este o parte foarte importantă a proiectului, fiind compusă din 3 elemente principale:
* **Încărcarea:** Se face printr-un port modern USB Type-C. Curentul trece printr-un cip de încărcare (BQ25180) care protejează bateria Li-Po.
* **Procentul bateriei:** Ca să afișăm exact cât la sută (%) mai are bateria, am pus un senzor special numit "Fuel Gauge" (MAX17048). El "discută" cu microcontroller-ul prin **I2C**.
* **Regulatorul de tensiune:** Pentru ca ceasul să funcționeze corect, are nevoie de exact 3.3V, chiar dacă bateria e plină sau aproape descărcată. De asta se ocupă regulatorul RT6160A.

#### 5. Consumul de Energie
Sistemul este foarte eficient. În majoritatea timpului, ceasul este in modul Sleep și consumă doar câțiva microamperi (aproape zero). Consumul crește doar pentru 1-2 secunde atunci când ecranul își dă refresh sau când trimitem date prin Bluetooth. Datorită ecranului E-Ink și cipului nRF52840, bateria acestui ceas ar trebui să țină de ordinul săptămânilor, nu doar o zi ca la ceasurile obișnuite.


### Alocarea Pinilor (Pinout nRF52840)

Microcontroller-ul nRF52840 a fost ales și datorită flexibilității sale. Spre deosebire de alte cipuri, acesta permite maparea (alocarea) interfețelor digitale pe aproape orice pin disponibil. Iată cum am organizat conexiunile:

#### 1. Interfața Ecranului E-Ink (SPI)
Ecranul E-Paper necesită o interfață rapidă pentru actualizarea frame-buffer-ului. Pinii au fost aleși pentru a rula magistrala SPI.
* **SCK (Ceas):** P1.09
* **MOSI (Date dinspre MCU):** P0.04
* **CS (Chip Select):** P0.20 - *Folosit pentru a selecta ecranul atunci când se dorește o transmisie de date.*
* **DC (Data/Command):** P0.17 - *Indică ecranului dacă biții transmiși reprezintă o comandă sau date pentru imagine.*
* **RST (Reset):** P0.15
* **BUSY:** P0.22 - *Pin de intrare. Ecranul ține acest pin activ pe durata actualizării imaginii, spunând microcontroller-ului să nu trimită comenzi noi.*

#### 2. Magistrala Senzorilor (I2C)
Pentru a optimiza numărul de pini folosiți, Accelerometrul (BMA423), senzorul nivelului de baterie (Fuel Gauge MAX17048) și Driverul Haptic (DRV2605) comunică pe o singură magistrală I2C comună.
* **SDA (Date):** P0.12
* **SCL (Ceas):** P0.11

#### 3. Butoanele de Navigare
Butoanele tactile sunt mapate pe pini GPIO standard. Acești pini sunt configurați intern cu rezistențe de Pull-Up și au abilitatea de a genera o întrerupere hardware (Wake-up) pentru a trezi ceasul din modul Sleep profund.
* **Buton UP:** P1.11
* **Buton ENTER:** P1.13
* **Buton DOWN:** P1.10

#### 4. Alte conexiuni importante
* **Alerta Baterie (Fuel Gauge INT):** P1.15 - *Cipul MAX17048 trimite un semnal pe acest pin atunci când bateria scade sub pragul critic, pentru a nu irosi energia întrebând constant I2C-ul „Câtă baterie mai am?”.*
* **Activare/Dezactivare Ecran:** P0.13 - *Folosit ca "Enable" pentru alimentarea ecranului.*
* **Pini de Programare (SWD):** SWDIO și SWDCLK sunt legați direct la conectorul Tag-Connect (J2) pentru a asigura încărcarea firmware-ului și depanarea (debugging) folosind un programator J-Link.
