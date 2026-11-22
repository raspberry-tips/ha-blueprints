# Home Assistant blueprints from raspberry.tips

Automation blueprints for everyday problems at home. Each one comes with a full write-up on [raspberry.tips](https://raspberry.tips/en/) that covers the hardware, the setup and what we ran into while testing it.

| Blueprint | What it does | Import |
|---|---|---|
| [NFC reminder that stops when the tag is scanned](blueprints/automation/raspberry-tips/nfc_reminder_until_scanned.yaml) | Reminds you on your phone at a set interval until you scan an NFC tag at the place of the task. The scan marks it as done, clears the reminder on every phone and confirms it. | [![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fraspberry-tips%2Fha-blueprints%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fraspberry-tips%2Fnfc_reminder_until_scanned.yaml) |
| [Window open → heating down, all closed → restore](blueprints/automation/raspberry-tips/window_open_heating_down.yaml) | Turns the heating down while a window is open and restores the previous mode and setpoint, `auto` included, once every window in the room is closed. | [![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fraspberry-tips%2Fha-blueprints%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fraspberry-tips%2Fwindow_open_heating_down.yaml) |

## NFC reminder that stops when the tag is scanned

Stick a cheap NFC tag where the task happens (dishwasher, washing machine, pill box), and the reminder keeps coming back until someone actually scans it.

- The task is a Toggle helper (`input_boolean`): on means it still needs doing. Another automation can turn it on, for example when the washing machine's power drops, or the blueprint can reopen it every day at a set time.
- Reminders only go out inside the time window you choose (07:00–22:00 by default).
- Scanning the tag turns the helper off, removes the reminder from the lock screen of every selected phone and sends a short confirmation. Scanning again says it was already done.
- Optionally saves the time of the scan to an `input_datetime`, so a dashboard can show when the medication was last taken.

Needs the Home Assistant Companion app on the phones (iOS or Android). How to write the tags with the app or with a PN532 reader on a Raspberry Pi: **[Home Assistant NFC Tags: Reminders That Stop When It's Done](https://raspberry.tips/en/smart-home/home-assistant-nfc-tags-reminders)** (German version: [NFC-Tags in Home Assistant](https://raspberry.tips/smart-home/nfc-tags-in-home-assistant-erinnerung)).

## Window open → heating down, all closed → restore

- Select every window and door contact in the room. The heating only comes back when **none** of them reports open. A contact that has gone unavailable (flat battery, lost connection) does not keep the room cold.
- Saves the thermostats' state before turning them down and restores exactly that state, so a thermostat running its own schedule (`auto`, e.g. Homematic IP) goes back to `auto` rather than being forced to `heat`.
- If a second window opens while the heating is already down, the saved state is left alone.
- While open, the thermostat either gets a low frost-protection temperature (default 5 °C, raised to the thermostat's own minimum if it doesn't accept that) or is turned off. Thermostats that are already off are left alone, so nothing starts heating in summer.
- The saved state lives in a temporary scene. If Home Assistant restarts while a window is open, the fallback mode is used on close, and only for thermostats that still look turned down. Contacts briefly going unavailable during a restart are ignored, so a restart never counts as a window being opened or closed. Thermostats that don't support the fallback mode get `heat` with the fallback temperature.

Works with any window contact (Zigbee, Z-Wave, Homematic, Shelly …) and any climate entity. Background, cheap window contacts and a Better Thermostat alternative: **[Window Open, Heating On? Home Assistant Reminds the Kids – and Turns the Heating Down](https://raspberry.tips/en/smart-home/window-open-heating-off-home-assistant)** (German version: [Fenster offen, Heizung an?](https://raspberry.tips/smart-home/fenster-offen-heizung-aus-home-assistant)).

## Installation

Click the import button above, or in Home Assistant go to **Settings → Automations & scenes → Blueprints → Import blueprint** and paste the link to the YAML file. Then create an automation from the blueprint.

Both blueprints need Home Assistant 2024.10 or newer.

## Feedback

Found a bug or have an idea? Open an issue. Pull requests are welcome.

## License

[MIT](LICENSE)
