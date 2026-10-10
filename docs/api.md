# API reference

## `progress.bar`

Renderers take `(frac: float, w: int)` (clamped, `w <= 0` → `"[]"`).

| Function | Signature | Notes |
|----------|-----------|-------|
| `blocks` | `(frac, w) -> str` | `█`/`░` |
| `partial` | `(frac, w) -> str` | 1/8-cell precision |
| `shaded` | `(frac, w) -> str` | quartile `░▒▓█` |
| `ascii` | `(frac, w) -> str` | `#`/`-` |
| `arrow` | `(frac, w) -> str` | `=` fill chasing `>` |
| `thin` | `(frac, w) -> str` | `─`/`·` (style const is `STYLE_LINE`) |
| `dots` | `(frac, w) -> str` | `●`/`○` |
| `circle` | `(frac, w) -> str` | quarter fills |
| `squares` | `(frac, w) -> str` | `■`/`□` |
| `spectrum` | `(frac, w) -> str` | rainbow lerp |
| `marquee` | `(tick, w, size: int) -> str` | bouncing block (pure in tick) |
| `stages` | `(done, total: int) -> str` | `●●●○○ 3/5` |
| `transfer` | `(label: str, done, total, w: int) -> str` | download line with `done|total` |
| `gradient` | `(frac, w) -> str` | truecolor lerp + dim track |
| `vbar` | `(frac, h: int) -> [str]` | vertical rows, filled from bottom |
| `pct` | `(frac) -> str` | `"62.5%"` |
| `count` | `(done: int, total: int) -> str` | `"12/48"` |
| `rate` | `(done, elapsed_ms: int) -> str` | `"3.4/s"`, `"--/s"` clockless |
| `eta` | `(done, total, elapsed_ms: int) -> str` | `"01:12"`, `"--:--"` stalled |
| `clock` | `(ms: int) -> str` | `"01:12"`, `"--:--"` negative |
| `defaults` | `() -> Opts` | label "", style blocks, w 20, pct on |
| `render` | `(o: Opts, frac) -> str` | bar + label + pct |
| `line` | `(o: Opts, done, total, elapsed_ms) -> str` | render + count/rate/eta per flags |

`Opts{ label, style, w, pct, count, rate, eta }` (flags are ints).
Style consts `STYLE_BLOCKS`…`STYLE_GRADIENT` (1–9). Cross-module
struct literals need an aliased import (`import progress.bar as bar`).

## `progress.spin`

| Function | Signature | Notes |
|----------|-----------|-------|
| `frames` | `(style: int) -> [str]` | frame list (unknown styles → dots) |
| `at` | `(style: int, tick: int) -> str` | frame for ever-increasing tick (pure) |
| `run` | `(label: str, style, ticks, interval_ms: int) -> int` | live `\r` loop + newline; returns ticks |

Style consts `SPIN_DOTS`…`SPIN_STAR` (1–19): dots, line, arc,
bounce, clock, moon, earth, box, arrows, toggle, grow, burger,
crescent, orbit, pulse, pie, diamond, sweep, star.
