---
title: "Add fields to template programmatically"
version: 1.0.1
authors:
  - dragan
tags:
  - pages
  - fields
date: 2015-01-17
---

## Problem

You want to add fields to a certain template, bypassing the admin interface.

## Solution

Add several fields to template via API

```php
// Bootstrap ProcessWire
include('./index.php');
$fields = wire("fields");
$templates = wire("templates");
$modules = wire("modules");
$template = $templates->get("product");
$prodFields = array( "prod_cabinet_wall", "prod_cable_length", "prod_control_gear", "prod_degree_separation", "prod_fan_air_flow", "prod_fan_lifetime", "prod_filtering_class", "prod_installtion", "prod_lamp", "prod_lifetime_at_40", "prod_luminosity", "prod_max_power_consumtion", "prod_working_pressure_water", "prod_max_air_flow", "prod_mounting_cut_out", "prod_noise_level", "prod_nominal_power_output", "prod_operating_voltage", "prod_rated_power", "prod_recommended_fuse", "prod_heat_exchanger", "prod_static_pressure", "prod_lamp_switch", "prod_temp_control", "prod_voltage");
foreach ($prodFields as $k=>$v) {
	$template->fields->add($v);
	$template->fields->save();
}
```

---

### Resources

- [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/addFieldsToTemplate.php)
