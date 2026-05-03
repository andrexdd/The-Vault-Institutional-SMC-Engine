# The Vault v10 | Institutional Market Structure Engine

A professional-grade implementation of Institutional Market Structure (SMC) in Pine Script v5. This engine is architected to eliminate retail noise by utilizing volatility-adjusted displacement filters and persistent state management for structural zones.

## Technical Specifications

### 1. Market Structure Core
The engine employs a non-lagging structural break logic:
- **MSB (Market Structure Break):** Confirmed via candle close beyond established pivots within a trend-aligned bias.
- **CHoCH (Change of Character):** Detects initial trend reversals by monitoring the failure of the most recent structural pivot.
- **Pivot Persistence:** Implemented via `var` persistent floats to maintain structural integrity across historical bars.

### 2. Proprietary ADI (Andre Displacement Index)
To filter out low-probability movements, the engine uses the ADI:
- **Calculation:** Derived from a weighted average of price displacement (ATR-normalized) and volume intensity (Z-score equivalent).
- **Thresholding:** Utilizes `ta.percentile_linear` to dynamically identify the top 25% of institutional moves, ensuring only high-velocity expansions trigger zone creation.

### 3. Order Block (OB) Logic & Memory Management
- **Type-Based Architecture:** Zones are handled via `type BoxZone`, allowing for complex attribute tracking (partial mitigation, test counts, and original base color).
- **Automated Mitigation State:**
    - **Active:** Zone extended to infinity.
    - **Partial Mitigation:** Visual state transition (grayscale conversion) when price penetrates >50% of the zone.
    - **Full Invalidation:** Deletion of visual objects and removal from the active processing array upon structural violation.
- **Limit Handling:** Integrated protection against TradingView's 500-object limit through stack-based array management.

### 4. Liquidity & Session Analysis
- **Session Wrappers:** Correctly handles midnight-cross sessions (e.g., Asia/London) with UTC-offset normalization.
- **Sweep Detection:** Monitors liquidity grabs at key HTF (High Time Frame) levels, confirmed by localized volume spikes.

## Configuration

| Parameter | Function |
|-----------|----------|
| `htf_res` | Sets the HTF anchor for structural confluence. |
| `adi_multiplier` | Adjusts sensitivity to institutional displacement. |
| `rr_target` | Dynamic Risk/Reward label calculation for each zone. |

## Implementation
```pinescript
// Define the engine parameters
strategy("The Vault v10", overlay=true)
// [Paste source code from the_vault_v10.pine]
Developed for institutional analysis. Architected by 0xAndreSec.
