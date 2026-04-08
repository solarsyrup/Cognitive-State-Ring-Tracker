# PCB Layout Strategy - Smart Ring v2.0

## Design Philosophy

### Via-Stacking Technique
This design uses **component stacking** across PCB layers to minimize trace lengths for critical circuits.

## Critical Component Placement

### DCDC Circuit (Power Supply)
```
Top Layer:
- L2 (10µH inductor) - DCDC output
- L3 (15nH inductor) - DCDC decoupling
- Decoupling capacitors

Bottom Layer:
- MDBT42V module
- Pin 22 (DCC) - directly below L2
- Pin 21 (DEC4) - directly below L3

Connection: Short vias through 0.8mm PCB
Result: <1mm effective trace length = minimal parasitic inductance
```

### GSR Analog Circuit
```
Top Layer:
- Feedback resistors (9× 0201)
- Input/output capacitors
- Signal routing

Bottom Layer:
- TLV9004 op-amp (UQFN-14 or TSSOP-14)
- Op-amp inputs/outputs directly below resistors

Connection: Vias for feedback paths
Result: Short feedback loop = stable operation
```

### Sensor I2C Bus
```
Top Layer:
- MAX30102 (heart rate/SpO2 sensor)
- SDA/SCL traces

Bottom Layer:
- LSM6 (IMU)
- SDA/SCL connections
- MDBT I2C pins

Connection: Star topology with short vias
Pull-ups: 10kΩ resistors to 3.3V
```

## Advantages of This Approach

### Electrical Performance
- ✓ Minimal trace inductance for DCDC (critical for switching)
- ✓ Short feedback paths for analog circuits (prevents oscillation)
- ✓ Reduced EMI (smaller loop areas)
- ✓ Better ground return paths (via stitching)

### Mechanical Benefits
- ✓ Tighter component packing
- ✓ Better use of both PCB layers
- ✓ Allows 140mm × 8mm form factor
- ✓ Distributes heat across both layers

### Manufacturing
- ⚠️ Requires both-sided assembly (adds cost)
- ⚠️ More complex assembly process
- ✓ But achievable with JLCPCB assembly service
- ✓ Standard 0.8mm PCB thickness works

## Assembly Process

### Step 1: Bottom Layer Assembly
1. Apply solder paste (stencil)
2. Place components (pick & place)
3. Reflow in oven (bottom side up)
4. Allow to cool

### Step 2: Top Layer Assembly
1. Flip board
2. Apply solder paste (stencil)
3. Place components (pick & place)
4. Reflow in oven (top side up)
   - Bottom components held by solidified solder
   - Or use SMT adhesive for security

### Step 3: Inspection
1. Visual inspection of both sides
2. Check for bridges/tombstones
3. Verify all components populated
4. Continuity test critical nets

## Critical Nets

### Power Rails
- Battery+ → Switch → LDO → 3.3V rail
- 3.3V → All components
- Ground stitching every 3-5mm

### DCDC Circuit
- Pin 22 (DCC) → L2 (10µH) → 3.3V
- Pin 21 (DEC4) → L3 (15nH) → C (1µF) → GND
- Must measure: Trace lengths <2mm

### I2C Bus
- MDBT SDA (P0.25) → Sensors
- MDBT SCL (P0.26) → Sensors
- Pull-ups: 10kΩ to 3.3V

### GSR Signal Path
- Electrode+ → Op-amp IN+
- Electrode- → Op-amp IN-
- Op-amp OUT → MDBT ADC (P0.02/AIN0)
- Feedback: OUT → Resistors → IN-

## Design Rules Used

### Trace Specifications
- Min trace width: 0.15mm (6 mil) for signals
- Min trace width: 0.3mm (12 mil) for power
- Min spacing: 0.15mm (6 mil)
- Via size: 0.3mm drill, 0.6mm pad

### Component Packages
- MCU: MDBT42V-512KV2 module
- Op-amp: TLV9004 (UQFN-14, 2×2mm)
- Resistors: 0201 (0.6×0.3mm)
- Small caps: 0201/0603
- Bulk caps: 0603/0805

### Clearances
- Antenna keep-out: 5mm all sides
- Edge clearance: 0.5mm minimum
- Component spacing: 0.3mm minimum

## Testing Strategy

### Power-Up Sequence
1. Check 3.3V rail (should be 3.25-3.35V)
2. Verify DCDC switching (scope on DCC pin)
3. Check I2C pull-ups (3.3V when idle)
4. Test each sensor communication

### GSR Circuit Testing
1. Connect test resistor (100kΩ) between electrodes
2. Measure op-amp output voltage
3. Verify ADC readings in firmware
4. Check for noise/stability

### Debug Points
- TP_3V3: Power rail
- TP_GND: Ground reference
- TP_SDA: I2C data line
- TP_SCL: I2C clock line
- TP_GSR: GSR analog output

## Lessons Learned

### What Works Well
- Via stacking keeps DCDC circuit tight ✓
- Both-sided layout enables 8mm width ✓
- 0201 components achieve target size ✓
- Ground hatching improves RF performance ✓

### Potential Improvements for Rev 3
- Add more test points (harder to debug than expected)
- Consider 0402 for easier prototyping
- Add fiducials for assembly alignment
- Include temperature sensor on PCB

## Bill of Materials

### Active Components
- 1× MDBT42V-512KV2 (BLE module)
- 1× TLV9004IRUCR (Quad op-amp, UQFN-14)
- 1× MAX30102 (Heart rate/SpO2 sensor)
- 1× LSM6DS3 (6-axis IMU)
- 1× MCP1700T-3302E (LDO regulator)

### Passive Components
- 9× Resistors (various values, 0201)
- 6× Capacitors (various values, 0201/0603)
- 1× 100µF bulk capacitor (0805)
- 2× Inductors: 10µH, 15nH (DCDC)

### Connectors
- 1× Battery connector
- 2× GSR electrode pads
- 1× Programming header (SWD)

## Document History
- v1.0: Initial design with single-sided layout
- v2.0: Current design with via-stacking strategy
- Date: February 11, 2026

## References
- MDBT42V datasheet (Raytac)
- TLV9004 datasheet (Texas Instruments)
- Grove GSR schematic (Seeed Studio)
- nRF52832 reference design (Nordic Semi)

---

**Design by:** Ryan Schreiber  
**Status:** Ready for manufacturing  
**Form Factor:** 140mm × 8mm smart ring band
