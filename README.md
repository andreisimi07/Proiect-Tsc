# InkTime

Acest repository contine fisierele de design hardware, documentatia si detaliile de implementare pentru smartwatch. Inima sistemului este nRF52840 de la Nordic Semiconductor, ales pentru capabilitatile sale multiprotocol (Bluetooth LE, Thread, Zigbee) si consumul redus de energie.

## Schema Bloc

![Schema bloc](Images/schema.png)

## Bill of Materials (BOM)

| Component | Description | Qty | Manufacturer | Part Number | Datasheet |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Microcontroller** | nRF52840 (AQFN-74) | 1 | Nordic Semiconductor | NRF52840_QF | [Link](https://files.seeedstudio.com/wiki/XIAO-BLE/Nano_BLE_MCU-nRF52840_PS_v1.1.pdf) |
| **Antenna** | 2.45GHz Chip Antenna | 1 | Johanson Technology | 2450AT18B100E | [Link](https://www.johansontechnology.com/datasheets/2450AT18B100.pdf) |
| **Accelerometer** | Triaxial low-g 12bit Sensor | 1 | Bosch Sensortec | BMA423 | [Link](https://watchy.sqfmi.com/assets/files/BST-BMA423-DS000-1509600-950150f51058597a6234dd3eaafbb1f0.pdf) |
| **Battery Charger** | Li-Ion/Polymer Charger IC | 1 | Texas Instruments | BQ25180YBGR | [Link](https://www.ti.com/product/BQ25180) |
| **Haptic Driver** | Driver for ERM/LRA | 1 | Texas Instruments | DRV2605YZFR | [Link](https://www.ti.com/product/DRV2605) |
| **Voltage Regulator** | Buck-Boost DC-DC | 1 | Richtek | RT6160AWSC | [Link](https://www.richtek.com/Products/Switching%20Regulators/Buck-Boost%20Converter/RT6160A) |
| **Fuel Gauge** | 1-Cell/2-Cell Fuel Gauge | 1 | Analog Devices | MAX17048G+T10 | [Link](https://www.analog.com/en/products/max17048.html) |
| **Connector (FPC)** | 0.5mm FPC RA SMT 24Ckt | 1 | Molex | 503480-2400 | [Link](https://www.molex.com/en-us/products/part-detail/5034802400) |
| **Connector (USB)** | USB Type-C 16-pin | 1 | Kinghelm | KH-TYPE-C-16P | [Link](https://jlcpcb.com/api/file/downloadByFileSystemAccessId/8588905154556923904) |
| **ESD Protection** | Low Cap. ESD Protection | 1 | STMicroelectronics | USBLC6-2SC6Y | [Link](https://www.st.com/en/protection-devices/usblc6-2.html) |
| **MOSFET (P)** | P-channel 20V/4.2A | 1 | Diodes Inc. | DMG2305UX-7 | [Link](https://www.lcsc.com/datasheet/C469327.pdf) |
| **MOSFET (N)** | N-channel 30V 1.5A | 1 | Vishay Siliconix | SI1308EDL-T1-GE3 | [Link](https://www.lcsc.com/datasheet/C469327.pdf) |
| **Schottky Diode** | 30V 0.5A Schottky | 3 | ON Semiconductor | MBR0530 | [Link](https://www.onsemi.com/pdf/datasheet/mbr0530t1-d.pdf) |
| **Inductor** | SMD Inductor 0.47µH | 1 | TDK | FTC252012SR47MBCA | [Link](https://jlcpcb.com/partdetail/6763488-FTC252012SR47MBCA/C5832368) |
| **Inductor** | Power Inductor 68uH | 1 | Wurth Electronics | 744043680 | [Link](https://www.we-online.com/en/components/products/WE-TPC) |
| **Switches** | Tactile Switches | 3 | Panasonic | EVP-AKE31A | [Link](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2301111010_PANASONIC-EVPAKE31A_C569760.pdf) |
| **Crystals** | 32MHz & 32.768kHz | 2 | Generic/Nordic | X1, X2 | [Link1](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2312080231_NDK-NX2016SA-32MHZ-EXS00A-CS11336_C6134317.pdf), [Link2](https://jlcpcb.com/partdetail/SeikoEpson-FC_135_32_7680KAA3/C2650472) |
| **Test Connector** | CABLE ADAPTER 6 POS | 1 | Tag Connect | TC2030-IDC | [Link](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/2019/12/TC2030-IDC-Datasheet-Rev-B.pdf) |

## nRF52840 Pin Mapping

| Pin nRF52840 | Signal      | Component | Interface |
|-------------|------------|------------|----------|
| P0.00/XL1   | XL1        | Crystal X2 (32.768kHz) | XTAL |
| P0.01/XL2   | XL2        | Crystal X2 (32.768kHz) | XTAL |
| P0.05/AIN3  | EPD_CS     | E-Paper (J1 FPC) | SPI CS |
| P0.06       | SDA        | BMA423, BQ25180, MAX17048, DRV2605 | I2C SDA |
| P0.07       | SCL        | BMA423, BQ25180, MAX17048, DRV2605 | I2C SCL |
| P0.08       | IMU_INT1   | BMA423 | GPIO Input |
| P1.08       | IMU_INT2   | BMA423 | GPIO Input |
| P0.11       | PMIC_INT   | BQ25180 | GPIO Input |
| P0.12       | HAPTIC_EN  | DRV2605 | GPIO |
| VBUS        | VBUS       | USB-C (J4) | Power |
| D-          | D-         | USB-C (J4) / USBLC6 | USB |
| D+          | D+         | USB-C (J4) / USBLC6 | USB |
| P0.13       | SW_UP      | Buton Up | GPIO Input |
| P0.14       | SW_ENT     | Buton Enter | GPIO Input |
| P0.15       | EPD_DC     | E-Paper (J1 FPC) | SPI DC |
| P0.16       | EPD_RST    | E-Paper (J1 FPC) | GPIO |
| P0.17       | EPD_BUSY   | E-Paper (J1 FPC) | GPIO Input |
| P0.18/RESET | RESET      | TC2030-IDC | SWD/GPIO |
| SWDCLK      | SWDCLK     | TC2030-IDC | SWD |
| SWDIO       | SWDIO      | TC2030-IDC | SWD |
| P1.02       | SW_DN      | Buton Down | GPIO Input |
| P0.10/NFC2  | ALERT      | MAX17048 | GPIO Input |
| ANT         | RF         | Antena 2450AT18B100E | RF |
| P0.02/AIN0  | SCK        | E-Paper (J1 FPC) | SPI SCK |
| P0.03/AIN1  | MOSI       | E-Paper (J1 FPC) | SPI MOSI |

## Arhitectura Sistemului

### 1. Microcontroler (MCU)
- **Chip:** Nordic nRF52840 (Cortex-M4F @ 64MHz)
- **Memorie:** 1MB Flash, 256KB RAM
- **Radio:** Bluetooth 5.4 Low Energy cu antena ceramica integrata
- **Oscilatoare:** 32MHz (operare) si 32.768kHz (sleep mode/RTC)

### 2. Managementul Energiei
- **Incarcator Li-Po:** BQ25180 (control prin I2C, curent programabil)
- **Regulator Voltaj:** RT6100 Buck-Boost (iesire stabila de 3.3V)
- **Monitorizare Baterie:** MAX17048 Fuel Gauge (algoritm ModelGauge)
- **Interfata Incarcare:** USB-C cu protectie USBLC6-2 ESD

### 3. Display (E-Paper)
- **Tip:** Electronic Paper Display (EPD)
- **Interfata:** SPI + Control Power Gating (PWR_EPD)
- **Circuit Drive:** Convertor Boost integrat pe placa (Diode MBR0530, L5 10uH) pentru generare VGH/VGL

### 4. Senzori si Feedback
- **IMU:** BMA421 (Accelerometru ultra-low power conectat prin I2C)
- **Haptic:** DRV2605 (Driver de vibratii cu librarie de efecte integrata via I2C)
- **Input:** 3 butoane mecanice (UP, ENTER, DOWN) cu circuite de filtrare

### 5. Interfete de Comunicatie (Mapare I2C)
| Componenta | Functie | Interfata |
| :--- | :--- | :--- |
| **BMA421** | Senzor Miscare | I2C |
| **MAX17048** | Status Baterie | I2C |
| **DRV2605** | Motor Vibratii | I2C |
| **BQ25180** | Incarcare | I2C |

## Debug si Programare
- **SWD:** Programare si depanare prin conector Tag-Connect TC2030
- **USB:** Comunicatie seriala si incarcare

## Consum de Energie si Autonomie

Dispozitivul este optimizat pentru functionare ultra-low power, utilizand starile de sleep ale nRF52840 si tehnica de power-gating pentru perifericele de inalta tensiune.

### Consum de Curent Estimat
| Mod Operare | Componente Active | Curent Estimat |
| :--- | :--- | :--- |
| **Deep Sleep** | MCU (System OFF), Fuel Gauge (Sleep) | ~10 µA |
| **Standby (Idle)** | MCU (System ON), IMU (Low-power) | ~0.8 mA |
| **EPD Refresh** | MCU, EPD Boost Circuit, Display | ~12 mA (peak) |
| **Haptic Alert** | MCU, Haptic Driver + Motor | ~80 mA (peak) |

### Estimarea Autonomiei Bateriei
Bazat pe o baterie Li-Po de **200mAh**:
- **Timp Static (Always-on):** Peste 1 an (EPD retine imaginea fara curent).
- **Utilizare Tipica:** ~10-12 zile (cu BLE activ si 1 refresh/ora).
- **Utilizare Intensa:** ~4-5 zile (notificari haptice si update-uri dese).

## Design Log

Aceasta sectiune descrie procesul de proiectare a PCB-ului, deciziile luate in timpul implementarii si modul in care proiectul respecta constrangerile de design impuse.

### Strategia de Plasare a Componentelor 

Procesul de layout a inceput cu plasarea componentelor constranse mecanic si a celor critice, precum:
- **Conectorul USB-C**
- **Butoanele laterale**
- **Conectorul pentru afisajul E-paper (FPC)**
- **Microcontrolerul nRF52840**

Aceste componente au fost pozitionate in concordanta cu carcasa mecanica pentru a asigura alinierea corecta si usurinta in utilizare.

Toate componentele au fost plasate **exclusiv pe stratul TOP**, conform cerintelor proiectului. Componentele pasive (rezistente, condensatoarele) au fost ulterior distribuite in jurul circuitelor integrate corespunzatoare, minimizand lungimea traseelor si asigurand o decuplare adecvata.

O atentie deosebita a fost acordata **condensatoarelor de decuplare de 100nF**, care au fost plasate cat mai aproape posibil de pinii de alimentare ai fiecarui circuit integrat.

### Strategia de Rutare 

Rutarea a fost efectuata pe un stack-up de **PCB cu 2 straturi**, structurat astfel:
- **Top Layer** – plasarea componentelor, rutarea semnalelor, traseelor de putere + plan de ground.
- **Bottom Layer** – rutarea semnalelor si traseelor de putere + plan de ground.

Reguli de design aplicate:
- **Trasee de alimentare:** latime ≥ 0.3 mm.
- **Trasee de semnal:** latime ≥ 0.15 mm.
- **Unghiuri:** Nu s-au folosit unghiuri de 90° (doar rutare lina sau la 45°).

### Planurile si ground

A fost implementata o strategie solida de legare la masa folosind zone de cupru (polygon pours) pe straturile de masa.
- Planurile de masa au fost aplicate pe ambele straturi.


### Consideratii RF si Antena

Zona antenei a fost tratata ca o zona de restrictie stricta plasata in coltul placii:
- Fara cupru.
- Fara trasee.
- Fara treceri (vias).

### Consideratii inductoare
Inductoarele L2 si L3 au fost plasate perpendicular pentru a diminua interferenta campurilor magnetice produse de acestea.

### Constrangeri ale Componentelor

Toate componentele respecta regulile de amprenta (footprint) solicitate:
- **Rezistente:** 0201.
- **Condensatori ≤ 100nF:** 0201.
- **Condensatori > 100nF:** 0402.

### Exceptii de la Regulile de Design (Acceptate)

**Dimensiunea Via-urilor**
Erorile raportate pentru dimensiunea gaurilor de trecere (via drill size) au fost verificate si aprobate manual.

## Continutul Repository-ului

### Proiectare (Design)
- **schematic.sch** – Fisierul de design al schemei electronice.
- **board.brd** – Fisierul pentru layout-ul PCB-ului.

### Productie (Manufacturing)
- **GerberFiles.zip** – Fisierele pentru fabricatia PCB-ului (Gerber + fisiere de gaurire).
- **bom.csv / bom.xlsx** – Lista de materiale (Bill of Materials).
- **pick&place.cpl** – Fisierul pentru plasarea automata a componentelor pe placa.

### Mecanica (Mechanical)
- **smartwatch_exploded_view.stp** – Vedere 3D explodata completa (PCB + baterie + ecran + carcasa).
- **smartwatch.stp** – Vedere 3D completa a dispozitivului.
- de exportat din autodesk fusion

### Imagini (Images)
- **pcb_2d.png** – Prezentare 2D a cablajului.
- **pcb_3d.png** – Vedere 3D a pcb
- **block_diagram.jpg** – Diagrama bloc a sistemului.
- **smartwatch_final.png** – Imagine cu produsul final.

### Licenta si Documentatie
- **LICENSE** – Fisierul cu licenta open-source a proiectului.
- **README.md** – Documentatia principala a proiectului.

