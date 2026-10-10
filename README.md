```
┌──────────────────────────────────┐
│  progress — bars and spinners    │
└──────────────────────────────────┘
```

Determinate bars in ten styles, tick spinners, readouts, one-call
status lines. Every renderer is pure: same inputs, same string.

## Install

```bash
zz add progress
```

## Bars

```zz
import progress.bar

bar.blocks(0.625, 8)    // [█████░░░]
bar.ascii(0.625, 8)     // [#####---]
bar.arrow(0.625, 8)     // [=====>..]
bar.gradient(0.625, 8)  // truecolor green→yellow→red
```

| Style | Const | Notes |
|-------|-------|-------|
| `blocks` | `STYLE_BLOCKS` | `█`/`░` |
| `partial` | `STYLE_PARTIAL` | 1/8-cell `▏…█` precision |
| `shaded` | `STYLE_SHADED` | quartile `░▒▓█` |
| `ascii` | `STYLE_ASCII` | `#`/`-`, log-safe |
| `arrow` | `STYLE_ARROW` | `=` fill chasing `>` |
| `dots` | `STYLE_DOTS` | `●`/`○` |
| `thin` | `STYLE_LINE` | `─`/`·` (const is `STYLE_LINE`) |
| `circle` | `STYLE_CIRCLE` | quarter fills `◔◑◒◓` |
| `squares` | `STYLE_SQUARES` | `■`/`□` download-style |
| `spectrum` | `STYLE_SPECTRUM` | rainbow lerp |
| `marquee` | `(tick, w, size)` | bouncing block for unknown totals |
| `stages` | `(done, total)` | `●●●○○ 3/5` pipeline steps |
| `transfer` | `(label, done, total, w)` | `dl [■■□□] 1|25 4.0%` download line |
| `mini` | `(frac, w)` | `▰`/`▱` minimalist |
| `arrow_at` | `(frac, w, tick)` | arrow with throbbing head |
| `gradient` | `STYLE_GRADIENT` | truecolor fill + dim track |
| `vbar` | — | vertical column, returns row strings |

All clamp `frac` to [0, 1]; `w <= 0` yields `"[]"`.

## Readouts

```zz
bar.pct(0.625)          // 62.5%
bar.count(30, 48)       // 30/48
bar.rate(30, 2000)      // 15.0/s  (--/s when clockless)
bar.eta(30, 48, 2000)   // 00:01   (--:-- when stalled)
bar.clock(61000)        // 01:01
```

## One call

```zz
import progress.bar as bar

o := bar.defaults()
o.label = "dl"
o.count = 1
println(bar.line(o, 30, 48, 2000))
// dl [████████████░░░░░░░░] 62.5% 30/48
```

`Opts` flags pick the readouts; `style` picks the renderer. `fg`/`bg`
take 256-color codes (`-1` = plain) to color the bar in one flag —
renderers themselves stay monochrome so they work in logs and pipes. (Import
with an alias: cross-module struct types need it.)

## Spinners

```zz
import progress.spin

spin.at(1, tick)             // frame for an ever-increasing tick
spin.frames(2)               // ["|", "/", "-", "\\"]
spin.run("working", 1, 6, 120)  // label + 6 ticks, returns ticks
```

Styles: dots, line, arc, bounce, clock, moon, earth, box, arrows,
toggle, grow, burger, crescent, orbit, pulse, pie, diamond, sweep,
star (consts `SPIN_DOTS`…`SPIN_STAR`).
Crescent/earth like emoji fonts; everything else is DejaVu-safe.

Try them all live (~3 s): `d_zz run examples/gallery.zz spin`.

## Live loop recipe

```zz
t := time.now_ms()
i := 0
while i < total {
    print("\r" + bar.line(o, i, total, time.now_ms() - t))
    // ... work ...
    i = i + 1
}
println("")
```

Needs `import std.time`. One `print` per tick plus a final newline.

## Docs

- [`docs/api.md`](docs/api.md) — every function and signature

MIT — see [LICENSE](LICENSE).
