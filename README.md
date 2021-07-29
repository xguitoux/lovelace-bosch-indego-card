# bosch-indego-card

Simple card for your bosch indego mower in Home Assistant's Lovelace UI

based on the work of benct https://github.com/benct/lovelace-xiaomi-vacuum-card

[![GH-release](https://img.shields.io/github/v/release/xguitoux/lovelace-bosch-indego-card.svg?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/releases)
[![GH-downloads](https://img.shields.io/github/downloads/xguitoux/lovelace-bosch-indego-card/total?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/releases)
[![GH-last-commit](https://img.shields.io/github/last-commit/xguitoux/lovelace-bosch-indego-card.svg?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/commits/master)
[![GH-code-size](https://img.shields.io/github/languages/code-size/xguitoux/lovelace-bosch-indego-card.svg?color=red&style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card)
[![hacs_badge](https://img.shields.io/badge/HACS-todo-red.svg?style=flat-square)](https://github.com/hacs)

Integrated support for Bosch Indego mowers supported on https://github.com/jm-73/Indego

## Integration installation

**First install integration from https://github.com/jm-73/Indego**

## Card installation

Manually add [bosch-indego-card.js](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/bosch-indego-card.js)
to your `<config>/www/` folder and add the link to the resource file (Manage Resource files menu) :
```yaml
url: /local/bosch-indego-card.js?v=1.0.0
```
Resource type = **Javascript file**

----- TODO ----

_OR_ install using [HACS](https://hacs.xyz/) and add this (if in YAML mode):
```yaml
lovelace:
  resources:
    - url: /hacsfiles/lovelace-bosch-indego-card/bosch-indego-card.js
      type: module
```
------------------------

The above configuration can be managed directly in the Configuration -> Lovelace Dashboards -> Resources panel when not using YAML mode,
or added by clicking the "Add to lovelace" button on the HACS dashboard after installing the plugin.

If you want to use the Bosch S350 background image, download and add
[img/s350.jpg](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/img/s350.jpg)
to `<config>/www/img/` or configure your own preferred path.

## Template Configuration

For the moment you have to create a template because the main integration creates a sensor for each property of your indego.
And the card needs one entity (the main sensor) with attributes.

Open your sensor YAML file or create a new one if you manage multiple files.

Find all "indego" entities that were created by the indego integration, they should look like sensor.indego_XXXXX_NAMEOFTHEPROPERTY.

In the following template code replace XXXXX by your indego serial number (it should be present in the entity name).

Code :

```yaml
- platform: template
  sensors:
    your_bosch_mower:
      friendly_name: Bosch Indego S+ 350
      value_template: states.binary_sensor.indego_XXXXX_online.state
      attribute_templates:
        battery: >
          {{states.sensor.indego_XXXXX_battery_percentage.state}}
        details: >
          {{states.sensor.indego_XXXXX_mower_state_detail.state}}
        status: >
          {{states.sensor.indego_XXXXX_mower_state.state}} / {{states.sensor.indego_XXXXX_mower_state_detail.state}}
        connection: >
          {{states.binary_sensor.indego_XXXXX_online.state}}
        lawn_mowed: >
          {{states.sensor.indego_XXXXX_lawm_mowed.state}}
        mow_mode: >
          {{states.sensor.indego_XXXXX_mowing_mode.state}}
        last_completed: >
          {{ relative_time(states.sensor.indego_XXXXX_last_completed.state|as_datetime) }}
        mowtime_total: >
          {{states.sensor.indego_XXXXX_runtime_total.state}}
        next_mow: >
          {{states.sensor.indego_XXXXX_next_mow.state}}
```

Check if your Hass configuration is valid and reload template entitys (There is an option below "Reload scripts", you don't have to reload all Home-assistant)

## Card Configuration

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| type | string | **Required** | `custom:bosch-indego-card`
| entity | string | **Required** | `sensor.your_bosch_mower`
| name | string/bool | `friendly_name` | Override friendly name (set to `false` to hide)
| image | string/bool | `false` | Set path/filename of background image (i.e. `/local/img/s350.jpg`)
| state | [Entity Data](#entity data) | *(see below)* | Set to `false` to hide all states
| attributes | [Entity Data](#entity data) | *(see below)* | Set to `false` to hide all attributes
| buttons | [Button Data](#button data) | *(see below)* | Set to `false` to hide button row

### Entity Data

Default indego attributes under each list:
- `state` (**left list**) include `status`, `battery` and `lawn_mowed`.
- `attributes` (**right list**) include `mow_mode`, `mowtime_total`, `next_mow` and `last_completed`.

See [examples](#examples) on how to customize, hide or add custom attributes.

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| key | string | **Required** | Attribute/state key on vacuum entity
| icon | string | | Optional icon
| label | string | | Optional label text
| unit | string | | Optional unit

### Button Data --- TO COMPLETE

Default buttons include `start`, `pause`, and `return`.
See [examples](#examples) on how to customize, hide or add custom buttons/actions.

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| icon | string | **Required** | Show or hide stop button
| show | bool | `true` | Show or hide button
| label | string | | Optional label on hover

## Screenshots

![bosch-indego-card](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/default.png)

![bosch-indego-card-no-title](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/no-title.png)

![bosch-indego-card-image](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/with-image.png)

![bosch-indego-card-no-buttons](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/no-buttons.png)

## Examples

Basic configuration:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.my_bosch_mower
```

```yaml
- type: custom:bosch-indego-card
  entity: sensor.my_bosch_mower
  image: /local/custom/folder/background.png
  name: My S+ 350
```

Hide state, attributes and/or buttons:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.my_bosch_mower
  state: false
  attributes: false
  buttons: false
```

---- TODO -----
Hide specific state values, attributes and/or buttons:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.my_bosch_mower
  state:
    mode: false
  attributes:
    main_brush: false
    side_brush: false
  buttons:
    pause: false
    locate: false
``` 

---- TODO -----
Translations:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.my_bosch_mower
  attributes:
    main_brush:
      label: 'Hovedkost: '
      unit: ' timer'
    side_brush:
      label: 'Sidekost: '
      unit: ' timer'
    filter:
      label: 'Filtere: '
    sensor:
      label: 'Sensorer: '
  buttons:
    start:
      label: Start!
    pause:
      label: Stopp!
    stop:
      label: Hammertime
```

## Disclaimer

This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with the Bosch Corporation,
or any of its subsidiaries or its affiliates.


[![BMC](https://www.buymeacoffee.com/assets/img/custom_images/white_img.png)](https://www.buymeacoffee.com/xguitoux)
