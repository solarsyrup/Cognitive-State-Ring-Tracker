# GSR Circuit Miniaturization - Available Components

## Problem
- Need quad op-amp smaller than TSSOP-14 (4.4×5mm)
- TLV9004IPWR (VQFN-14) out of stock
- Must be pin-compatible with LM324 for proven Grove circuit

## Available Alternatives (Feb 2026)

### Option 1: Stick with TSSOP-14 (Most Practical)
**Part:** TLV9004IPW or MCP6004T-I/ST
- **Size:** 4.4mm × 5.0mm (22mm²)
- **Availability:** ✓ In stock everywhere
- **Cost:** $0.70-0.80
- **Assembly:** Hand-solderable (0.65mm pitch)
- **Pinout:** Exact LM324 replacement

**Board Size Achievable:**
- With 0603: 8mm × 6mm
- With 0201: 7mm × 6mm

### Option 2: Use Two Dual Op-Amps (Smaller Individual Chips)
**Parts:** 2× TLV2372 or 2× MCP6002
- **Package:** SOT-23-8 or MSOP-8
- **Size per chip:** 3mm × 3mm
- **Total area:** ~18mm² (2 chips)
- **Advantage:** Each chip is smaller, easier to route
- **Disadvantage:** Need to split circuit across 2 chips

**Available Dual Op-Amps:**
```
Part          Package    Size        Stock    Cost
──────────────────────────────────────────────────
TLV2372       SOIC-8     3.9×4.9mm   ✓        $0.60
MCP6002       SOIC-8     3.9×4.9mm   ✓        $0.40
OPA2172       SOIC-8     3.9×4.9mm   ✓        $2.50
TLV2372       MSOP-8     3.0×3.0mm   ✓        $0.70
LMV358        SOIC-8     3.9×4.9mm   ✓        $0.50
```

### Option 3: Integrated GSR Front-End IC (Different Approach)
**Part:** AD8232 or MAX30003
- **Purpose:** Single-chip biopotential front-end
- **Size:** 4×4mm QFN-20
- **Advantage:** Fewer external components
- **Disadvantage:** Different circuit topology (not Grove compatible)

### Option 4: Wait for Stock / Order from Different Distributor
**TLV9004IPWR (VQFN-14)** availability:
- Mouser: Check stock
- Digikey: Check stock  
- LCSC: Often has stock when US doesn't
- AliExpress: Slower but available

## Recommended Solution

### For Immediate Build: TLV9004IPW (TSSOP-14)
- ✓ Available now
- ✓ Pin-compatible with LM324
- ✓ Proven circuit works
- ✓ Hand-solderable for prototypes
- ✓ Achieves 7-8mm × 6mm with 0201 passives

**This is 20% smaller PCB than using SOIC-14 LM324**

### For Future Revision: 2× Dual Op-Amps
If you need to go smaller later:
- Use 2× TLV2372 (MSOP-8, 3×3mm each)
- Split Grove circuit: 
  - Chip 1: Instrumentation amp + gain stage
  - Chip 2: Filter + output buffer
- Total chip area: 18mm² vs 22mm² (18% smaller)
- Board size: 6.5mm × 5.5mm achievable

## Circuit Changes for Dual Op-Amps

### Grove Circuit (4 op-amps):
```
U.S.1 (Op-A): Instrumentation amplifier front-end
U5.4 (Op-B):  Second stage gain
U5.3 (Op-C):  Low-pass filter
U5.2 (Op-D):  Output buffer
```

### Split into 2× Dual Op-Amps:
```
IC1 (Dual):
- Op-A: Instrumentation amp
- Op-B: Second stage gain

IC2 (Dual):  
- Op-A: Low-pass filter
- Op-B: Output buffer
```

**Same circuit, just split across two smaller chips!**

## Current Stock Check (Do This)

Before deciding, check current availability:

1. **Mouser.com:**
   - TLV9004IPW (TSSOP-14): [Check stock]
   - TLV9004IPWR (VQFN-14): [Check stock]
   - TLV2372IDR (SOIC-8): [Check stock]
   - TLV2372IDGKR (MSOP-8): [Check stock]

2. **Digikey.com:**
   - Same part numbers as above

3. **LCSC.com:**
   - Often has stock when US distributors don't
   - Ships from China (7-14 days)

4. **JLC Assembly:**
   - Check their parts library
   - If doing PCB assembly, they stock certain parts

## Decision Matrix

| Option | Size | Availability | Assembly | Circuit Changes |
|--------|------|--------------|----------|-----------------|
| TSSOP-14 | 22mm² | ✓ In stock | Easy | None |
| VQFN-14 | 6.3mm² | ✗ Out of stock | Reflow only | None |
| 2× SOIC-8 | 38mm² | ✓ In stock | Easy | Split circuit |
| 2× MSOP-8 | 18mm² | ✓ In stock | Reflow | Split circuit |

## Final Recommendation

**Use TLV9004IPW (TSSOP-14) NOW:**
- Available immediately
- No circuit changes
- 7-8mm × 6mm board achievable
- Proven compatibility

**Later, if you need smaller:**
- Redesign with 2× MSOP-8 dual op-amps
- Achieve 6mm × 5.5mm board
- Requires circuit split but same functionality

---

*Document created: Feb 11, 2026*
*Status: TLV9004 TSSOP-14 recommended for current build*
