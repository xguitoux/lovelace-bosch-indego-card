# bosch-indego-card

Simple card for your bosch indego mower in Home Assistant's Lovelace UI

based on the work of benct https://github.com/benct/lovelace-xiaomi-vacuum-card

[![GH-release](https://img.shields.io/github/v/release/xguitoux/lovelace-bosch-indego-card.svg?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/releases)
[![GH-downloads](https://img.shields.io/github/downloads/xguitoux/lovelace-bosch-indego-card/total?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/releases)
[![GH-last-commit](https://img.shields.io/github/last-commit/xguitoux/lovelace-bosch-indego-card.svg?style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card/commits/master)
[![GH-code-size](https://img.shields.io/github/languages/code-size/xguitoux/lovelace-bosch-indego-card.svg?color=red&style=flat-square)](https://github.com/xguitoux/lovelace-bosch-indego-card)
[![hacs_badge](https://img.shields.io/badge/HACS-manual-red.svg?style=flat-square)](https://github.com/hacs)

If you like my work feel free to buy me a cofee

[![BMC](https://www.buymeacoffee.com/assets/img/custom_images/white_img.png)](https://www.buymeacoffee.com/xguitoux)

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


_OR_ install using [HACS](https://hacs.xyz/) and add this (if in YAML mode):
Add this repository (https://github.com/xguitoux/lovelace-bosch-indego-card) as a [custom repository](https://hacs.xyz/docs/faq/custom_repositories/)

Install it, reload and follow below instructions


The above configuration can be managed directly in the Configuration -> Lovelace Dashboards -> Resources panel when not using YAML mode,
or added by clicking the "Add to lovelace" button on the HACS dashboard after installing the plugin.

If you want to use the Bosch S350 background image, download and add
[img/s350.jpg](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/img/s350.jpg)
to `<config>/www/img/` or configure your own preferred path.

## Card Configuration

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| type | string | **Required** | `custom:bosch-indego-card`
| entity | string | **Required** | `sensor.indego_XXXXXXXXX_mower_state`
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

Default card
![bosch-indego-card](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/default.png)

No title
![bosch-indego-card-no-title-no-image](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/no-title-no-image.png)

With provided image
![bosch-indego-card-image](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/with-image.png)

No action buttons
![bosch-indego-card-no-buttons](https://raw.githubusercontent.com/xguitoux/lovelace-bosch-indego-card/master/examples/no-buttons.png)

## Examples

Basic configuration:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.indego_XXXXXXXXX_mower_state
```

```yaml
- type: custom:bosch-indego-card
  entity: sensor.indego_XXXXXXXXX_mower_state
  image: /local/custom/folder/background.png
  name: My S+ 350
```

Hide state, attributes and/or buttons:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.indego_XXXXXXXXX_mower_state
  state: false
  attributes: false
  buttons: false
```

Translations:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.indego_XXXXXXXXX_mower_state
  attributes:
   mowing_mode:
     label: 'Mode de tonte: '
   next_mow:
     label: 'Prochaine tonte: ' 
   last_completed:
     label: 'Dernière tonte: ' 
   mowtime_total:
     label: 'Total tonte: '  
  buttons:
    start:
      label: C'est parti !
    pause:
      label: Halte !
    return:
      label: A la niche
```

Hide specific state values, attributes and/or buttons:
```yaml
- type: custom:bosch-indego-card
  entity: sensor.indego_XXXXXXXXX_mower_state
  buttons:
    pause: false
  attributes:
    mowing_mode: false
    mowtime_total: false
``` 



## Disclaimer

This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with the Bosch Corporation,
or any of its subsidiaries or its affiliates.