# KitsunePower

🦊 **Part of the [Kitsune Systems Collection](https://github.com/Kitsune-Den)** ~
[KitsuneZombieReach](https://github.com/Kitsune-Den/KitsuneZombieReach) ·
[KitsuneTrapXP](https://github.com/Kitsune-Den/KitsuneTrapXP) ·
[KitsuneFuelSaver](https://github.com/Kitsune-Den/KitsuneFuelSaver) ·
[KitsuneKitchen7D](https://github.com/Kitsune-Den/KitsuneKitchen7D) ·
[KitsuneFoxacary](https://github.com/Kitsune-Den/KitsuneFoxacary)

![KitsunePower](kitsune-power.jpg)

**A retuned, reskinned power chain for 7 Days to Die 2.x — Solar + Generator Banks feeding a Battery Bank built around quality-6 car batteries.**

Vanilla power is fine until you live on it. Solar trickles, generators chew gas, and Battery Banks charge so slowly that storing power for blood moons feels like a part-time job. KitsunePower tunes the three blocks of the power chain so they work together the way you'd expect:

- **Foxfire Solar Bank** — pulls more power per solar cell, your clean daytime source.
- **Foxfire Generator Bank** — burns gas twice as efficiently with a deeper tank, your anytime backup.
- **Spirit Battery Bank** — drinks power twice as fast and is balanced around **quality-6 car batteries** for serious sustained output.

No new blocks, no new crafting tree — it's the same Solar/Generator → Battery grid you already build, just tuned so the whole chain is worth it. Energy conversion stays an honest 1:1; this makes power *faster and longer-lasting*, not free.

## How the chain works (and one engine truth)

In 7 Days to Die, a **Generator is a power source, not a store** — you can't literally "charge" a generator. Your two sources are the **Solar Bank** (clean, daytime) and the **Generator Bank** (gas, anytime); both feed downstream into the **Battery Bank**, which is the only block that actually stores power, charging the car batteries you slot into it. Slot **quality-6 (max-tier) car batteries** for the highest output — battery output scales with cell quality, and this mod raises the ceiling so level-6 cells really pay off.

```
 [Solar Bank]  ─┐
                ├─►  [Battery Bank]  ─►  loads (lights, turrets, doors…)
 [Generator]   ─┘     (quality-6 car batteries)
```

## Requirements

- 7 Days to Die V2.0+
- Server-side only. Install on the server (or in single player). Clients don't need anything.
- EAC-safe. Pure XML config patches, no DLL, no Harmony.

## Installation

1. Drop the `KitsunePower` folder from the release zip into your `Mods/` folder on the server.
2. Restart the server.

Done. No DLL, no client install.

## What it patches

All patches are XPath `set` operations against vanilla `blocks.xml`. The blocks themselves (models, icons, recipes, unlock perks, repair costs) are untouched — only the values below change, plus display names and tooltips.

### Foxfire Solar Bank — `solarbank` (`Class=SolarPanel`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `OutputPerStack` | 30 | **45** | More power per solar cell |
| `MaxPower` | 180 | **270** | Higher array output ceiling |

### Foxfire Generator Bank — `generatorbank` (`Class=Generator`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `MaxFuel` | 1000 | **2000** | Deeper gas tank — fewer refuel trips |
| `OutputPerFuel` | 11250 | **22500** | Burns gas twice as efficiently (same power, lasts 2× longer) |

### Spirit Battery Bank — `batterybank` (`Class=BatteryBank`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `InputPerTick` | 5 | **10** | Charges twice as fast from upstream sources |
| `OutputPerStack` | 50 | **75** | Higher per-battery output — rewards quality-6 cells |
| `MaxPower` | 400 | **600** | Higher total output ceiling |
| `ChargePerInput` | 1 | *1 (unchanged)* | Conversion stays 1:1 — faster, not free |

## Why these numbers

The bottleneck in vanilla battery charging is `InputPerTick` (5) — the bank only sips 5 power per tick no matter how big the sources upstream are, so a full battery bank takes ages to top off. Doubling it to 10 halves the charge time while keeping `ChargePerInput` at 1, so there's no free energy. On the source side, the solar buff means daytime alone can keep batteries fed, and the generator's doubled efficiency + tank means one fuel load runs the loop far longer between refuels. The battery bank's raised `OutputPerStack` (75) makes quality-6 car batteries the clear endgame: a full set delivers enough sustained load to run a serious base through a horde.

## What it does NOT touch

- Recipes, unlock perks, or repair costs — craft and unlock exactly as in vanilla.
- Power switches, relays, wire, or any load blocks (turrets, lights, doors).
- Any custom art — reuses the vanilla solar, generator, and battery models and icons.

## Credits

Made by AdaInTheLab.
