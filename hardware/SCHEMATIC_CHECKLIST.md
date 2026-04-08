# Complete Schematic Review Checklist - Smart Ring v2.0

## 📋 Pre-Review Setup

### Document Information
- [ ] Project name clearly labeled
- [ ] Version number on schematic (e.g., "v2.0")
- [ ] Date of last revision
- [ ] Designer name/initials
- [ ] Company/project logo (if applicable)
- [ ] Sheet numbering (Sheet X of Y)
- [ ] Revision history table

### File Management
- [ ] Schematic file saved with version control
- [ ] Backup copy created
- [ ] PDF export generated for reference
- [ ] All library files referenced correctly
- [ ] No missing symbols or footprints
- [ ] Git commit with clear message (if using version control)

---

## 🔌 Power Supply Section

### Battery Input
- [ ] Battery connector pinout correct (+ and -)
- [ ] Battery voltage range specified (e.g., 3.7V Li-Po)
- [ ] Polarity protection (diode or MOSFET)
- [ ] Battery voltage rating matches system requirements
- [ ] Connector type specified in BOM

### Power Switch
- [ ] Switch type specified (SPST, slide, tactile)
- [ ] Current rating adequate for system draw
- [ ] Debouncing if needed (for tactile switches)
- [ ] Mechanical mounting verified
- [ ] Switch position labels (ON/OFF)

### LDO Regulator (MCP1700T-3302E)
- [ ] Input voltage range: 3.7V (battery) within spec
- [ ] Output voltage: 3.3V ± tolerance specified
- [ ] Input capacitor: 1µF ceramic, X5R/X7R
- [ ] Output capacitor: 1µF ceramic, X5R/X7R (min per datasheet)
- [ ] Capacitor voltage rating: ≥6.3V (2× output voltage)
- [ ] Capacitor placement notes: "Place within 5mm of IC"
- [ ] Ground connection solid and direct
- [ ] Thermal pad connected to ground (if applicable)
- [ ] Enable pin tied correctly (if present)
- [ ] Output current capability: Check total system draw
- [ ] Dropout voltage acceptable (Vbat_min - 3.3V > Vdropout)
- [ ] Package type specified (SOT-23)

### MDBT42V DCDC Circuit
- [ ] Pin 22 (DCC) → L2 (10µH inductor) → VDD rail
- [ ] Pin 21 (DEC4) → L3 (15nH inductor) → C (1µF) → GND
- [ ] L2 inductor: 10µH, DCR <0.5Ω, Isat >100mA
- [ ] L3 inductor: 15nH, DCR <0.1Ω, SRF >100MHz
- [ ] 1µF capacitor on DEC4: X7R, 6.3V min, low ESR
- [ ] Pin 20 (VDD) bypass capacitor: 100nF very close to pin
- [ ] Additional 10µF bulk cap on VDD rail
- [ ] DCC net labeled clearly
- [ ] DEC4 net labeled clearly
- [ ] DCDC enable configuration (if applicable)

### Power Distribution
- [ ] 3.3V rail labeled "VDD" or "+3V3" consistently
- [ ] Ground labeled "GND" consistently throughout
- [ ] Power flags on all VDD/GND symbols (KiCad)
- [ ] No floating power nets
- [ ] All ICs have power pins connected
- [ ] Decoupling caps on every IC power pin
- [ ] Power supply tree documented (Battery → LDO → Components)

### Decoupling Capacitors (All Components)
- [ ] MDBT module: 100nF + 10µF at VDD pin
- [ ] MAX30102: 100nF + 4.7µF per datasheet
- [ ] LSM6: 100nF + 10µF per datasheet
- [ ] TLV9004 op-amp: 100nF + 1µF at pin 4 (VCC)
- [ ] Each IC has bypass cap <5mm away (noted in schematic)
- [ ] Bulk capacitors (10-100µF) at power input
- [ ] All caps X7R or X5R dielectric (not Y5V)
- [ ] Voltage ratings 2× nominal voltage minimum

---

## 📡 MDBT42V Module (nRF52832)

### Power Connections
- [ ] Pin 20 (VDD): Connected to 3.3V rail
- [ ] Pin 11 (VSS): Connected to GND
- [ ] Pin 11 (GND): All GND pins connected
- [ ] DCDC pins configured (see DCDC section above)
- [ ] No power pins left floating

### Programming Interface (SWD)
- [ ] Pin 31 (SWDIO): Connected to programming header
- [ ] Pin 32 (SWDCLK): Connected to programming header
- [ ] GND pin on programming header
- [ ] VDD pin on programming header (optional, for target power)
- [ ] RESET pin accessible (Pin 21)
- [ ] 10kΩ pull-up resistor on RESET (Pin 21 to VDD)
- [ ] Programming header type specified (2×5 1.27mm or similar)
- [ ] SWD pins have test points for debugging

### Reset Circuit
- [ ] Pin 21 (RESET): 10kΩ pull-up resistor to VDD
- [ ] Optional: Reset button to GND (with 100nF debounce)
- [ ] Reset net labeled clearly

### I2C Interface
- [ ] Pin for SDA: P0.25 (or P0.26, confirm your choice)
- [ ] Pin for SCL: P0.26 (or P0.25, confirm your choice)
- [ ] **CRITICAL:** SCL NOT on P0.28 (conflicts with DCC)
- [ ] SDA net labeled "I2C_SDA" or "SDA"
- [ ] SCL net labeled "I2C_SCL" or "SCL"
- [ ] 10kΩ pull-up resistor on SDA to VDD
- [ ] 10kΩ pull-up resistor on SCL to VDD
- [ ] I2C bus capacitance calculated (<400pF total)
- [ ] I2C nets routed to all sensors (MAX30102, LSM6)

### ADC Input (GSR)
- [ ] Pin for ADC: P0.02 (AIN0) or other analog pin
- [ ] ADC pin connected to GSR op-amp output
- [ ] No series resistor on ADC input (unless intentional filter)
- [ ] Optional: RC low-pass filter (1kΩ + 100nF) for noise
- [ ] ADC reference voltage understood (0-3.6V range)
- [ ] Input protection if needed (ESD diodes, Zener clamp)

### GPIO Assignments
- [ ] All GPIO pins accounted for
- [ ] Pin conflicts checked (no dual assignments)
- [ ] Unused GPIOs noted (can be left floating or pulled)
- [ ] Pin assignments documented in comment or table

### Antenna
- [ ] Antenna connection: Pin 29 (ANT)
- [ ] Antenna type specified (chip, PCB trace, wire)
- [ ] Matching network if needed (per MDBT datasheet)
- [ ] Keep-out area around antenna noted (5mm min)
- [ ] Ground plane under antenna (or removed, per design)

### Crystal/Clock
- [ ] Check if MDBT has internal crystal (yes, it does)
- [ ] No external 32kHz crystal needed (internal RC)
- [ ] Confirm low-frequency clock source in firmware config

### Other MDBT Pins
- [ ] Pin 35 (NFC1): Tied to GND if not used
- [ ] Pin 36 (NFC2): Tied to GND if not used
- [ ] Any unused pins: Left floating or follow datasheet

---

## ❤️ MAX30102 Sensor (Heart Rate, SpO2, Temperature)

### Power Connections
- [ ] VDD pin: Connected to 3.3V rail
- [ ] GND pin: Connected to GND
- [ ] 100nF decoupling cap close to VDD pin
- [ ] 4.7µF bulk cap per datasheet recommendation
- [ ] Capacitor placement note in schematic

### I2C Interface
- [ ] SDA pin: Connected to I2C_SDA bus
- [ ] SCL pin: Connected to I2C_SCL bus
- [ ] Pull-ups on bus (shared with other I2C devices)
- [ ] I2C address: 0x57 (default, confirm in datasheet)
- [ ] Address conflicts checked (no other device at 0x57)

### Interrupt Pin
- [ ] INT pin: Connected to MDBT GPIO (optional)
- [ ] Pull-up resistor if needed (10kΩ to VDD)
- [ ] Interrupt pin labeled clearly
- [ ] If not used: Can be left floating (check datasheet)

### LED Driver Connections
- [ ] IR LED anode/cathode connections correct
- [ ] Red LED anode/cathode connections correct
- [ ] Current-limiting resistors if needed (usually internal)
- [ ] LED placement notes (must face skin/finger)

### Package and Footprint
- [ ] Package type: 14-pin optical module
- [ ] Footprint verified against datasheet
- [ ] Optical aperture alignment noted

---

## 🏃 LSM6DS3 IMU (Accelerometer + Gyroscope)

### Power Connections
- [ ] VDD pin: Connected to 3.3V rail
- [ ] VDDIO pin: Connected to 3.3V rail (I/O voltage)
- [ ] GND pin: Connected to GND
- [ ] 100nF decoupling cap at VDD pin
- [ ] 10µF bulk cap per datasheet
- [ ] Capacitor placement note in schematic

### I2C Interface
- [ ] SDA pin: Connected to I2C_SDA bus
- [ ] SCL pin: Connected to I2C_SCL bus
- [ ] Pull-ups on bus (shared with MAX30102)
- [ ] I2C address: 0x6A or 0x6B (check SA0 pin config)
- [ ] Address conflicts checked with MAX30102

### Address Selection
- [ ] SA0 pin: Tied to GND (address 0x6A) or VDD (0x6B)
- [ ] Address choice documented in schematic note
- [ ] Confirm different from MAX30102 address

### Interrupt Pins
- [ ] INT1 pin: Connected to MDBT GPIO or floating
- [ ] INT2 pin: Connected to MDBT GPIO or floating
- [ ] Pull-up/pull-down per datasheet if used
- [ ] Interrupts labeled clearly

### Other Pins
- [ ] SDO/SA0 pin: Configure for I2C address
- [ ] CS pin: Tied to VDD for I2C mode (disables SPI)
- [ ] OCS pin: Tied to GND (if present)

### Package and Footprint
- [ ] Package type: LGA-14 or similar
- [ ] Footprint verified against datasheet
- [ ] Orientation marking (pin 1 indicator)

---

## 🧠 GSR Circuit (Grove-Based, TLV9004 Op-Amp)

### Op-Amp IC (TLV9004)
- [ ] Part number: TLV9004IPW (TSSOP-14) or TLV9004IRUCR (UQFN-14)
- [ ] Package specified in schematic
- [ ] Footprint assigned correctly

### Power Connections
- [ ] Pin 4 (VCC): Connected to 3.3V rail
- [ ] Pin 11 (GND): Connected to GND
- [ ] 100nF bypass cap at pin 4 (VCC to GND)
- [ ] 1µF cap near pin 4 (optional but recommended)
- [ ] 10µF bulk cap for entire GSR circuit section
- [ ] Capacitor placement notes in schematic

### Op-Amp A (Instrumentation Amplifier)
- [ ] Pin 3 (IN+_A): Connected to GSR electrode (positive)
- [ ] Pin 2 (IN-_A): Connected to feedback network
- [ ] Pin 1 (OUT_A): Output to next stage
- [ ] Resistor values from Grove schematic:
  - [ ] RG1: 100kΩ (bias resistor)
  - [ ] RG3: 200kΩ (feedback resistor)
  - [ ] RG4: 200kΩ (feedback resistor)
- [ ] Voltage divider for mid-rail bias (VDD/2 = 1.65V)
- [ ] Resistor tolerances specified (1% recommended)

### Op-Amp B (Second Stage Gain)
- [ ] Pin 5 (IN+_B): Connected to previous stage output
- [ ] Pin 6 (IN-_B): Connected to feedback network
- [ ] Pin 7 (OUT_B): Output to next stage
- [ ] Resistor values from Grove schematic:
  - [ ] RG5: 100kΩ (input resistor)
  - [ ] RG6: 100kΩ (feedback resistor)
- [ ] Gain calculation verified

### Op-Amp C (Filter Stage)
- [ ] Pin 10 (IN+_C): Connected to previous stage
- [ ] Pin 9 (IN-_C): Connected to feedback network
- [ ] Pin 8 (OUT_C): Output to next stage
- [ ] Resistor values from Grove schematic:
  - [ ] RG7: 100kΩ (input resistor)
  - [ ] Capacitor for low-pass filter (check Grove)
- [ ] Filter cutoff frequency calculated

### Op-Amp D (Output Buffer)
- [ ] Pin 12 (IN+_D): Connected to previous stage
- [ ] Pin 13 (IN-_D): Connected to output (unity gain buffer)
- [ ] Pin 14 (OUT_D): Final output to MDBT ADC
- [ ] Resistor values from Grove schematic:
  - [ ] RG9: 200kΩ (feedback resistor)
  - [ ] RG10: 1MΩ (output resistor to ADC)
- [ ] Output voltage range within ADC limits (0-3.3V)

### GSR Electrodes
- [ ] GSR+ electrode connection (pad or connector)
- [ ] GSR- electrode connection (pad or connector)
- [ ] Electrode material specified (stainless steel, gold-plated)
- [ ] Electrode spacing noted (for finger contact)
- [ ] Connection type: Pads, spring contacts, or wires

### Signal Path
- [ ] GSR electrodes → Op-Amp A → Op-Amp B → Op-Amp C → Op-Amp D → ADC
- [ ] All connections traced and verified
- [ ] No missing connections in feedback loops
- [ ] Signal path labeled clearly

### Grove Schematic Verification
- [ ] All resistor values match Grove GSR v1.2 schematic
- [ ] All capacitor values match Grove schematic
- [ ] Circuit topology identical (4 op-amp stages)
- [ ] No modifications unless intentional and documented

---

## 🔧 Miscellaneous Components

### Test Points
- [ ] TP_VDD: 3.3V power rail
- [ ] TP_GND: Ground reference
- [ ] TP_SDA: I2C data line
- [ ] TP_SCL: I2C clock line
- [ ] TP_GSR: GSR analog output
- [ ] TP_SWDIO: Programming data
- [ ] TP_SWDCLK: Programming clock
- [ ] Test point diameter: 0.5-1.0mm
- [ ] Test point footprint assigned

### Connectors
- [ ] Battery connector: Type and pinout specified
- [ ] Programming header: Type and pinout specified
- [ ] GSR electrode connectors: Type specified
- [ ] All connectors have pin 1 marked

### LEDs (Status Indicators)
- [ ] LED connections: Anode to GPIO, cathode to GND via resistor
- [ ] Current-limiting resistors: 220Ω to 1kΩ (calculate for desired brightness)
- [ ] LED forward voltage considered (typically 2V for red, 3V for blue)
- [ ] LED type specified (color, size)
- [ ] Optional: Power LED on 3.3V rail

### Buttons/Switches
- [ ] Debouncing capacitor (100nF) if needed
- [ ] Pull-up or pull-down resistor (10kΩ)
- [ ] Button type specified
- [ ] Mechanical footprint verified

---

## 📐 Schematic Organization

### Net Names
- [ ] All power nets labeled (VDD, GND)
- [ ] I2C bus nets labeled (SDA, SCL)
- [ ] All inter-sheet connections labeled
- [ ] No net naming conflicts
- [ ] Descriptive names (not "Net-1", "Net-2")

### Component References
- [ ] All components have unique reference designators
- [ ] Logical numbering (U1, U2, U3... R1, R2, R3...)
- [ ] No duplicate reference designators
- [ ] Reserved references for future components (if applicable)

### Component Values
- [ ] All resistors have values specified (e.g., "100kΩ")
- [ ] All capacitors have values specified (e.g., "100nF")
- [ ] All inductors have values specified (e.g., "10µH")
- [ ] Tolerances specified where critical (e.g., "100kΩ 1%")
- [ ] Voltage ratings specified for capacitors (e.g., "10V")
- [ ] Power ratings specified for resistors if >0.1W

### Footprint Assignments
- [ ] Every component has a footprint assigned
- [ ] Footprints verified against datasheets
- [ ] Package types documented (e.g., "0603", "SOIC-14")
- [ ] No generic footprints (e.g., "R_0603" vs "R_0603_1608Metric")

### Datasheets Referenced
- [ ] MDBT42V datasheet
- [ ] MAX30102 datasheet
- [ ] LSM6DS3 datasheet
- [ ] TLV9004 datasheet
- [ ] MCP1700T datasheet
- [ ] Grove GSR schematic
- [ ] All datasheets accessible in project folder

---

## ⚡ Electrical Rule Check (ERC)

### Power Pins
- [ ] All power input pins connected to VDD
- [ ] All ground pins connected to GND
- [ ] No unconnected power pins
- [ ] No conflicting power sources

### Input Pins
- [ ] All input pins driven by an output
- [ ] No floating inputs (unless intentional high-Z)
- [ ] Pull-ups/pull-downs on all necessary inputs

### Output Pins
- [ ] No outputs shorted together
- [ ] No outputs driving conflicting signals
- [ ] Output loading calculated (fanout check)

### Bidirectional Pins
- [ ] I2C SDA/SCL configured correctly (open-drain with pull-ups)
- [ ] No conflicts on bidirectional buses

### KiCad ERC
- [ ] Run ERC in KiCad: Tools → Electrical Rules Checker
- [ ] Zero errors
- [ ] All warnings reviewed and resolved or justified
- [ ] ERC report saved with schematic

---

## 📊 Design Calculations

### Power Consumption
- [ ] MDBT42V current draw calculated (TX, RX, sleep modes)
- [ ] MAX30102 current draw: ~50mA typical
- [ ] LSM6 current draw: ~1mA typical
- [ ] TLV9004 current draw: ~600µA (all 4 op-amps)
- [ ] Total system current: Peak and average calculated
- [ ] Battery life estimate (capacity / average current)
- [ ] LDO current capability checked (MCP1700T: 250mA max)

### Voltage Levels
- [ ] All components operate at 3.3V (or confirm voltage translation needed)
- [ ] ADC input range: 0-3.3V (nRF52 ADC is 0-3.6V capable)
- [ ] GSR op-amp output: 0-3.0V typical (within ADC range)
- [ ] I2C voltage levels: 3.3V compatible with all devices

### Signal Integrity
- [ ] I2C bus capacitance: <400pF for standard mode (100kHz)
- [ ] I2C bus capacitance: <100pF for fast mode (400kHz)
- [ ] Pull-up resistor calculation: R = (Vdd - 0.4V) / 3mA ≈ 1kΩ min
- [ ] Chosen pull-up: 10kΩ (lower speed, lower power)
- [ ] SWD signal integrity acceptable for programming

### Analog Circuit (GSR)
- [ ] Op-amp gain stages calculated
- [ ] Total system gain verified
- [ ] Output voltage range: 0-3.3V for ADC
- [ ] Input impedance acceptable for skin contact
- [ ] Noise analysis (optional but recommended)
- [ ] Frequency response (cutoff frequency of filter)

---

## 📝 Documentation

### Schematic Annotations
- [ ] All critical notes added
- [ ] Component placement notes (e.g., "Place C1 within 5mm of U1")
- [ ] Routing notes (e.g., "Keep DCDC traces short")
- [ ] Power consumption notes
- [ ] I2C address conflicts noted and resolved

### BOM Generation
- [ ] BOM exported from schematic
- [ ] All components have manufacturer part numbers
- [ ] All components have Mouser/Digikey part numbers
- [ ] Quantities correct
- [ ] Designators listed
- [ ] Values listed
- [ ] Footprints listed
- [ ] DNP (Do Not Populate) items marked

### Schematic Printout
- [ ] Schematic fits on standard paper size (A4 or Letter)
- [ ] Readable at 100% zoom
- [ ] All text legible
- [ ] No overlapping text or wires
- [ ] Clear hierarchy (if multi-sheet)

### Version Control
- [ ] Schematic committed to Git (or equivalent)
- [ ] Commit message describes changes
- [ ] Tagged with version number (e.g., "v2.0")
- [ ] Previous versions archived

---

## 🔍 Peer Review

### External Review
- [ ] Have another engineer review schematic
- [ ] Address all feedback
- [ ] Document review comments
- [ ] Re-review after changes

### Self-Review (24-Hour Rule)
- [ ] Wait 24 hours after completing schematic
- [ ] Review with fresh eyes
- [ ] Check for errors you might have missed
- [ ] Sign off on final design

---

## ✅ Final Sign-Off

### Pre-Layout Checklist
- [ ] All ERC errors resolved
- [ ] All datasheets referenced and understood
- [ ] Power consumption within LDO limits
- [ ] All components available for purchase (check stock)
- [ ] BOM cost calculated and acceptable
- [ ] Schematic PDF exported for documentation
- [ ] Ready to proceed to PCB layout

### Designer Sign-Off
- [ ] Designer name: ___________________
- [ ] Date: ___________________
- [ ] Signature: ___________________

### Reviewer Sign-Off (if applicable)
- [ ] Reviewer name: ___________________
- [ ] Date: ___________________
- [ ] Signature: ___________________

---

## 🚨 Common Mistakes to Avoid

### Power Supply
- [ ] ❌ Forgetting decoupling caps on ICs
- [ ] ❌ Inadequate bulk capacitance
- [ ] ❌ LDO input/output caps wrong value
- [ ] ❌ DCDC inductors wrong value or too far away
- [ ] ❌ Missing RESET pull-up resistor

### I2C Bus
- [ ] ❌ Missing pull-up resistors on SDA/SCL
- [ ] ❌ Pull-ups too strong (low resistance = high current)
- [ ] ❌ I2C address conflicts (two devices same address)
- [ ] ❌ Bus capacitance too high (long traces + large caps)
- [ ] ❌ Using wrong GPIO pins (P0.28 conflicts with DCDC!)

### GPIO Assignments
- [ ] ❌ Pin conflicts (same pin used for multiple functions)
- [ ] ❌ Analog pins used for digital I/O (suboptimal)
- [ ] ❌ Forgetting GPIO pin limitations (some pins don't have pull-ups)

### Analog Circuit (GSR)
- [ ] ❌ Wrong resistor values (not matching Grove schematic)
- [ ] ❌ Missing feedback loop (op-amp oscillates)
- [ ] ❌ Output voltage exceeds ADC range (clipping)
- [ ] ❌ Insufficient decoupling on op-amp power pins

### Programming/Debug
- [ ] ❌ No SWD access (can't program the chip!)
- [ ] ❌ Missing RESET pull-up (chip doesn't boot reliably)
- [ ] ❌ SWD pins shared with conflicting functions

### General
- [ ] ❌ Unconnected pins (floating inputs)
- [ ] ❌ Missing ground connections
- [ ] ❌ Wrong footprints assigned
- [ ] ❌ Datasheets not consulted
- [ ] ❌ Copy-paste errors from reference designs

---

## 📚 Reference Documents

### Datasheets (Verify Latest Versions)
- [ ] MDBT42V-512KV2 Module Datasheet (Raytac)
- [ ] nRF52832 Product Specification (Nordic Semiconductor)
- [ ] MAX30102 Datasheet (Maxim Integrated)
- [ ] LSM6DS3 Datasheet (STMicroelectronics)
- [ ] TLV9004 Datasheet (Texas Instruments)
- [ ] MCP1700 Datasheet (Microchip)
- [ ] Grove GSR Sensor Schematic (Seeed Studio)

### Application Notes
- [ ] nRF52832 Hardware Design Guidelines (Nordic)
- [ ] TLV9004 Application Examples (TI)
- [ ] MAX30102 Application Note (Maxim)
- [ ] I2C Bus Specification (NXP)

### Reference Designs
- [ ] Nordic nRF52832 DK schematics
- [ ] Grove GSR v1.2 schematic (your baseline)
- [ ] Similar smart ring/wearable designs

---

## 🎯 Completion Status

**Total Items:** 350+ checklist items  
**Completed:** _____ / _____  
**Completion Date:** _______________  
**Ready for Layout:** [ ] YES [ ] NO  

---

**Notes:**
- This checklist is comprehensive but may need customization for your specific design
- Not all items may apply to every project
- Add project-specific items as needed
- Use this as a living document throughout the design process
- Print and check off items as you go, or use a digital version

**Good luck with your schematic! 🚀**
