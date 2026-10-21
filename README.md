# KitsunePower

🦊 **Part of the [Kitsune Systems Collection](https://github.com/Kitsune-Den)** ~
[KitsuneZombieReach](https://github.com/Kitsune-Den/KitsuneZombieReach) ·
[KitsuneTrapXP](https://github.com/Kitsune-Den/KitsuneTrapXP) ·
[KitsuneFuelSaver](https://github.com/Kitsune-Den/KitsuneFuelSaver) ·
[KitsuneKitchen7D](https://github.com/Kitsune-Den/KitsuneKitchen7D) ·
[KitsuneFoxacary](https://github.com/Kitsune-Den/KitsuneFoxacary)

![KitsunePower](kitsune-power.jpg)

**Lenient power for 7 Days to Die 3.x. Solar, generator and battery banks that each run a whole base.**

Vanilla power is fine until you live on it. Solar trickles, generators chew gas, and a battery bank barely covers a few turrets. KitsunePower retunes the three blocks of the power chain so power stops being something you budget:

- **Foxfire Solar Bank**: each solar cell makes 25 times the vanilla power. Free, by day.
- **Foxfire Generator Bank**: each engine makes 7.5 times the vanilla power, gas goes twice as far, and the tank holds 4.5 times as much. Your anytime source.
- **Spirit Battery Bank**: each battery puts out 15 times the vanilla power, and a charge holds twice as much, so batteries fill twice as fast and last twice as long.

With six quality-6 items slotted, every bank tops out at **4500W**. For scale, a well-defended base (six turrets, ten blade traps, lights and fences) draws around 300 to 400W.

No new blocks, no new crafting tree. It's the same Solar/Generator → Battery grid you already build.

## How the chain works

In 7 Days to Die, a **generator is a power source, not a store**: you can't "charge" a generator. Your two sources are the **Solar Bank** (free, daytime) and the **Generator Bank** (gas, anytime). Both feed downstream into the **Battery Bank**, the only block that stores power, in the car batteries you slot into it.

```
 [Solar Bank]  ─┐
                ├─►  [Battery Bank]  ─►  loads (lights, turrets, doors…)
 [Generator]   ─┘     (car batteries)
```

Each slot's output scales with the item's quality: half at quality 1, full at quality 6.

## Requirements

- 7 Days to Die V3.x. Patches checked against V3.3.
- Server-side only. Install on the server (or in single player). Clients don't need anything.
- EAC-safe. Pure XML config patches, no DLL, no Harmony.

## Installation

1. Drop the `KitsunePower` folder from the release zip into your `Mods/` folder on the server.
2. Restart the server.

Done. No DLL, no client install.

## What it patches

All patches are XPath `set` operations against vanilla `blocks.xml`. The blocks themselves (models, icons, recipes, unlock perks, repair costs) are untouched. Only the values below change, plus display names and descriptions.

### Foxfire Solar Bank — `solarbank` (`Class=SolarPanel`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `OutputPerStack` | 30 | **750** | 25× the power per solar cell. Six cells: 180W → 4500W |
| `MaxPower` | 180 | **4500** | Matches six cells. The game works this out from the cells anyway |

### Foxfire Generator Bank — `generatorbank` (`Class=Generator`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `OutputPerStack` | 100 | **750** | 7.5× the power per engine. Six engines: 600W → 4500W |
| `MaxFuel` | 1000 | **4500** | 4.5× the gas tank |
| `OutputPerFuel` | 11250 | **22500** | Each unit of gas makes twice the power |
| `MaxPower` | 12250 | **27000** | The generator's internal store. It has to hold a whole unit of gas's power (see below) |

### Spirit Battery Bank — `batterybank` (`Class=BatteryBank`)

| Property | Vanilla | Mod | Effect |
|---|---:|---:|---|
| `OutputPerStack` | 50 | **750** | 15× the power per battery. Six batteries: 300W → 4500W |
| `OutputPerCharge` | 90 | **180** | Each bit of charge holds twice the power |
| `MaxPower` | 400 | **4500** | The bank's internal store, so it can keep up with 4500W |
| `InputPerTick` | 5 | *5 (unchanged)* | The power each bit of charge costs (see below) |

## Why these numbers

**Generators burn gas by the unit.** A generator bank keeps an internal store of power. It burns one unit of gas, adding `OutputPerFuel` to the store, only when that whole amount fits. So `MaxPower` has to be at least `OutputPerFuel`, or the generator never runs. Vanilla leaves 1000 of headroom (11250 + 1000); the mod leaves room for a full 4500W of output (22500 + 4500). Gas use follows the power your base actually draws, so a bigger output doesn't burn more gas for the same base.

**Batteries charge at a fixed pace.** Every tick, a battery bank adds one bit of charge to one battery, and that bit costs `InputPerTick` power. So raising `InputPerTick` doesn't charge faster; it only makes charging cost more, and a source with less spare power than `InputPerTick` can't charge the bank at all. The mod leaves it at 5 and doubles `OutputPerCharge` instead: each bit of charge gives back twice the power, so batteries fill twice as fast and last twice as long.

## What it does NOT touch

- Recipes, unlock perks, or repair costs. Craft and unlock exactly as in vanilla.
- Power switches, relays, wire, or any load blocks (turrets, lights, doors).
- Any custom art. It reuses the vanilla solar, generator and battery models and icons.

## Credits

Made by AdaInTheLab.

## License

[CC BY-NC-SA 4.0](LICENSE), with an extra permission for servers that make money: see [NOTICE.md](NOTICE.md). In short: play it, change it and share it, with credit, not for sale, and with changed versions under the same license.
