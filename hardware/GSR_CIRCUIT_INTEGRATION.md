# GSR Circuit Integration Plan
## Ring PCB v2.0 - Adding GSR Sensing

### Current Board Specs
- **Size:** 64mm × 8.9mm (perfect for ring size 11-12)
- **Layers:** 2-layer with components on both sides
- **Components:** 0201 package size (extreme miniaturization)
- **Version:** 2.0 (labeled on PCB)

### GSR Circuit Requirements

#### Components Needed (Grove GSR Compatible):
```
IC:
├─ TLV9004IRUCR (UQFN-14, 2×2mm)
│  └─ Quad op-amp, pin-compatible with LM324
│      Texas Instruments
│      $0.90 @ LCSC/Mouser

Resistors (9× total, 0201 package):
├─ RG1: 100kΩ  (0201)
├─ RG3: 200kΩ  (0201)
├─ RG4: 200kΩ  (0201)
├─ RG5: 100kΩ  (0201)
├─ RG6: 100kΩ  (0201)
├─ RG7: 100kΩ  (0201)
├─ RG8: 200kΩ  (0201)
├─ RG9: 200kΩ  (0201)
└─ RG10: 1MΩ   (0201)

Capacitors (3× total):
├─ C_bypass: 100nF (0201 or 0402)
├─ C_mid:    1µF   (0402)
└─ C_bulk:   100µF (0603 or 0805)

Electrode Connections:
├─ GSR+ pad (2×2mm castellated edge pad)
└─ GSR- pad (2×2mm castellated edge pad)
```

#### Footprint Size:
```
Minimum space required:
- IC footprint: 2×2mm (UQFN-14)
- 9× resistors: ~4mm² total area
- 3× capacitors: ~3mm² total area
- Routing space: ~10mm²
- Electrode pads: 2×4mm

Total minimum area: 6mm × 8mm = 48mm²
Comfortable area:   8mm × 8.9mm = 71mm²
```

### Integration Options

#### Option 1: Extend Board Length to 72mm
```
Advantages:
✓ Clean section separation
✓ Easy routing
✓ Room for test points
✓ No interference with existing circuits
✓ Still fits ring size 13 (69mm circumference)

Layout:
[Battery 15mm][Power 10mm][GSR 8mm][Antenna 25mm][Sensors 14mm]
                          ↑
                    New section

Placement:
- Between power section and antenna keep-out
- Components on top layer (easier access)
- Electrode pads on side edges (castellated)
```

#### Option 2: Use Antenna Keep-Out Zone
```
Advantages:
✓ Keeps 64mm length
✓ Uses "dead" space efficiently
✓ No board extension needed

Disadvantages:
⚠️ May affect antenna performance
⚠️ Limited to low-profile components only
⚠️ Bottom layer only (harder to debug)

Requirements:
- All components < 0.5mm height
- Bottom layer placement only
- Test antenna performance after assembly
```

#### Option 3: Optimize Existing Layout
```
Compress current sections:
- Sensor section: Save 2mm
- Battery section: Save 2mm  
- Power section: Save 2mm
Total saved: 6mm

Use freed space for GSR circuit
Keeps 64mm total length
Requires redesign of existing sections
```

### Recommended Approach: Option 1 (Extend to 72mm)

#### Detailed GSR Section Layout (8mm × 8.9mm):
```
TOP VIEW:
┌─────────────────────┐
│    [C_bulk 0603]    │ 1mm from edge
│                     │
│  [R1]  ┌──────┐    │
│  [R2]  │      │ [C1]│
│        │TLV   │     │ 8.9mm width
│  [R3]  │9004  │ [C2]│
│  [R4]  │UQFN  │     │
│        └──────┘ [R5]│
│  [R6] [R7] [R8] [R9]│
│                     │
│ GSR+           GSR- │ Edge pads (castellated)
└─────────────────────┘
       8mm length

Component heights:
- UQFN-14: 0.5mm
- 0201 resistors: 0.3mm
- 0402 caps: 0.5mm
- 0603 bulk cap: 1.0mm (tallest)

All fit within standard stiffener clearance!
```

### Connections Required

#### To MDBT Module:
```
GSR_OUT (op-amp final output) → MDBT Pin P0.02 (AIN0)
├─ Trace: 0.15mm width
├─ Keep away from: RF traces, high-speed digital
└─ Add test point (0.5mm via) for debugging

VDD (3.3V power) → From LDO output
├─ Shared with existing 3.3V rail
└─ Bypass caps already specified above

GND → Common ground plane
└─ Multiple vias to ground plane
```

#### Electrode Pads:
```
GSR+ and GSR- pads:
├─ Size: 2mm × 2mm each
├─ Type: Castellated half-holes on board edge
├─ Spacing: 6mm apart (center to center)
├─ Plating: ENIG (for contact with skin/fabric)
├─ Connection: Conductive thread or spring contacts
└─ Location: Opposite edges of GSR section
```

### BOM Addition

```
New Components for GSR:
═══════════════════════════════════════════════════════
Part              Package   Qty   Unit Cost   Total
───────────────────────────────────────────────────────
TLV9004IRUCR      UQFN-14   1     $0.90      $0.90
Resistors 0201    0201      9     $0.02      $0.18
Capacitor 100nF   0201      1     $0.05      $0.05
Capacitor 1µF     0402      1     $0.10      $0.10
Capacitor 100µF   0603      1     $0.30      $0.30
───────────────────────────────────────────────────────
Total Added Cost:                            $1.53
═══════════════════════════════════════════════════════

Per-board cost increase: ~$1.50
Minimal impact on overall BOM cost!
```

### Assembly Notes

#### UQFN-14 Assembly (Critical!):
```
Thermal Pad:
├─ Add 4-6 vias (0.3mm drill, 0.6mm pad)
├─ Via spacing: 0.7mm apart
├─ Tented vias (covered by solder mask)
├─ Connect to ground plane
└─ Essential for thermal and electrical grounding

Solder Paste:
├─ Stencil thickness: 0.1mm (4mil)
├─ Thermal pad: 80% paste coverage (prevent voids)
├─ Perimeter pads: 100% coverage
└─ Use "cross-hatch" pattern on thermal pad

Reflow Profile:
├─ Same as existing 0201 components
├─ Peak temp: 245°C
├─ Time above 220°C: 40-60 seconds
└─ Cooling: Gradual (no thermal shock)
```

#### Test Points to Add:
```
For debugging GSR circuit:
├─ TP_GSR+:    Input from positive electrode
├─ TP_GSR-:    Input from negative electrode
├─ TP_STAGE1:  After instrumentation amp
├─ TP_STAGE2:  After gain stage
├─ TP_OUT:     Final output to ADC
└─ TP_VDD:     Power supply to op-amp

Size: 0.5mm diameter vias (can probe with fine needle)
Location: Near respective circuit nodes
```

### Design Rule Checks

#### Before ordering:
```
✓ UQFN-14 footprint matches datasheet exactly
✓ Thermal pad vias are present (4-6 vias)
✓ Bypass caps within 3mm of VDD pin
✓ GSR electrode pads are castellated
✓ Clearance from antenna keep-out zone (>2mm)
✓ Trace width adequate for current (0.15mm min)
✓ All 0201 components have proper land patterns
✓ Solder mask clearance around pads (0.05mm)
✓ Silkscreen doesn't overlap pads
✓ Board outline updated to 72mm length
```

### Next Steps

1. **Wait for component labels from user**
   - Identify all existing ICs
   - Confirm MDBT module location
   - Verify inductor positions
   - Check available space

2. **Decide on integration option**
   - Extend to 72mm (recommended)
   - Use antenna zone space
   - Compress existing sections

3. **Create GSR section in KiCad**
   - Copy Grove schematic exactly
   - Use TLV9004IRUCR footprint
   - Add all test points
   - Route to MDBT ADC pin

4. **Update PCB layout**
   - Place GSR components
   - Route power and signals
   - Add electrode pads
   - Run DRC check

5. **Order prototype**
   - PCB from JLCPCB (2-layer, 0.8mm, ENIG)
   - Assembly service for 0201/UQFN components
   - 5 boards minimum
   - ~2 week lead time

---

**Status:** Waiting for component identification
**Next:** User to label existing components on v2.0 board
**Goal:** Integrate GSR circuit into v2.1 revision

*Document created: Feb 11, 2026*
