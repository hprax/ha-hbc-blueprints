# HBC – PV Control & Battery Strategy (Dynamic Energy Tariff)

Home Assistant Blueprint to automatically control your PV output and battery strategy based on real-time electricity prices.

Designed for setups with:
- Dynamic energy tariffs (e.g. Zonneplan, Tibber, etc.)
- Controllable PV inverter limit (e.g. SAJ)
- Battery strategy controlled via `input_select`

---

## ⚡ What it does

This blueprint reacts to electricity prices:

### When price goes **negative**
- Disables (or limits) PV output
- Forces battery into **charging mode**

### When price goes **positive again**
- Restores PV output to normal
- Restores battery strategy to normal mode

---

## 🧠 Why this matters

In markets like the Netherlands, negative energy prices happen more often.

Without automation:
- You may **pay to export solar energy**
- Your battery may behave inefficiently

With this blueprint:
- You **stop exporting during negative prices**
- You **use cheap (or paid) energy to charge your battery**

---

## 🧩 Requirements

You need:

- A **tariff sensor**, for example:
  - `sensor.zonneplan_current_electricity_tariff`

- A **number entity** controlling PV power:
  - e.g. `number.saj_limit_power`

- A **battery strategy selector**:
  - e.g. `input_select.house_battery_strategy`

---

## 📦 Installation

### Option 1 – Manual

1. Create folder if it doesn't exist: config/blueprints/automation/hbc/

2. Save the blueprint file there: disable_pv_restore_tariff.yaml

3. Reload Blueprints in Home Assistant:
- Settings → Automations & Scenes → Blueprints → Reload

---

### Option 2 – Import via URL


Use: https://github.com/hprax/ha-hbc-blueprints/blob/main/disable_pv_restore_tariff.yaml


---

## ⚙️ Configuration

When creating the automation from the blueprint, set:

### Core entities

| Input | Example |
|------|--------|
| Tariff sensor | `sensor.zonneplan_current_electricity_tariff` |
| PV limit entity | `number.saj_limit_power` |
| Battery strategy | `input_select.house_battery_strategy` |

---

### Thresholds (IMPORTANT)

Use hysteresis to avoid rapid switching:

| Setting | Suggested |
|--------|----------|
| Negative threshold | `-0.001` |
| Positive threshold | `0.01` |

---

### Behavior

| Mode | PV Limit | Battery Strategy |
|------|--------|----------------|
| Negative tariff | `0` (disable PV) | `Charge` |
| Positive tariff | e.g. `0.05` | `Normal` |

---

## 🔧 Example Setup

```yaml
negative_threshold: -0.001
positive_threshold: 0.01

pv_limit_negative: 0
pv_limit_positive: 5000

negative_strategy: Charge
positive_strategy: Full Control