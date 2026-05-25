
---

## 3. `docs/gcode_macro_strategy.md`

```markdown
# G‑Code Macro Strategy – No FANUC Library Required

## Why This Works

FANUC and most CNCs support **custom macro variables** (`#500`–`#999`).  
These variables retain values across cycles and can be read/written via RS232 using standard commands.

We use variables `#600`–`#612` to store OEE data **inside the controller** – no external software needed during machining.

## Variable Mapping (as used at CMTI)

| Macro | Description | Update Method |
|-------|-------------|----------------|
| #600 | Total parts produced | Increment in O9001 |
| #601 | Good parts | Increment if no reject |
| #602 | Reject parts | Increment if reject flag set |
| #603 | Run time accumulator (ms) | Added each cycle |
| #604 | Last cycle time (ms) | Calculated in O9001 |
| #610 | Planned quantity | Set by operator (per shift) |
| #611 | Ideal cycle time (sec) | Programmed constant |
| #612 | Job ID | Programmed per job |

## The Two Key Macros

### O9000 – Start of Cycle
```gcode
O9000 (OEE START)
#700 = #3001    (record start milliseconds)
#650 = 0        (reset good part flag)
M99

### O9001 – End of Cycle
#701 = #3001
#604 = #701 - #700          (cycle time in ms)
#603 = #603 + #604          (add to total run time)
#600 = #600 + 1
IF [#650 EQ 1] GOTO 10
#601 = #601 + 1             (good part)
GOTO 20
N10 #602 = #602 + 1         (reject)
N20 M99


### O1000 (MY JOB) - Production Program
#610 = 100          (planned qty)
#611 = 60           (ideal CT sec)
#612 = 101          (job ID)

M98 P9000           (start OEE)

... machining ...

M98 P9001           (end OEE)
M30
