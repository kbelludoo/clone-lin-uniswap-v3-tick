# clone-lin-uniswap-v3-tick

Experimental i64-scale LIN clone of Uniswap V3 `TickMath.getSqrtRatioAtTick`.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repository is the kernel copy only.

- **Upstream:** [Uniswap/v3-core](https://github.com/Uniswap/v3-core) tag `v1.0.0` `contracts/libraries/TickMath.sol`
- **License:** GPL-2.0-or-later (BUSL-1.1 Change Date 2023-04-01 elapsed)
- **Class:** EXPERIMENTAL. Reduced **Q32** (`2^32`) bit table, `|tick| <= 16383` (14 bits). Not Q64.96 / uint160 / `MAX_TICK=887272` mainnet parity.
- **Not this kernel:** `getNextSqrtPriceFromInput` ([clone-lin-uniswap-v3-next-sqrt](https://github.com/kbelludoo/clone-lin-uniswap-v3-next-sqrt)), `getAmount0Delta` ([clone-lin-uniswap-v3-sqrt](https://github.com/kbelludoo/clone-lin-uniswap-v3-sqrt)).

## Files

- `src/lin_uniswap_v3_tick.lin` — LIN kernel (`v3t_sqrt_at_tick`)
- `test/oracles/uniswap_v3_tick_c11.c` — independent C11 `__int128` + limb oracle

## Named contrasts (auditable)

| Path | tick=0 | tick=1 | tick=8192 |
|---|---|---|---|
| Q32 bit table | 4294967296 | 4295182039 | 6469012455 |
| Linear `Q*(1+tick/20000)` | 4294967296 | 4295182044 | 6054185900 |
| Naive i64 `(Q*Q)/ratio` invert | n/a | **0** (wrap) | n/a |

Linearizing ticks underprices concentrated liquidity. Wrapping i64 invert hides the price at 0.

Re-run proofs from lin-open:

```
python3 test/prove_uniswap_v3_tick_external.py
```
