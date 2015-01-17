title: Set backend field column width via API

----

version: 1.0.1

----

authors: dragan

----

tags: fields, backend, width

----

problem:
You want to set backend field widths, bypassing the admin interface

----

solution:
Set inputfield column widths via API:
```PHP
// If not inside template
$fields = wire("fields");

$prodFields = array("title", "meta_description", "headline", "longtitle", "summary", "body", "images", "product_images", "files", "hidden_france", "modxid", "product_specs", "show_calcoolator", "product_catalogue", "product_features", "prod_cooling_capacity", "prod_compressor", "prod_refrigerant", "prod_operating_temp_range", "prod_max_air_volume_flow", "prod_dimensions", "prod_weight", "prod_cut_out_dimensions", "prod_rating_operating_voltage", "prod_frequency", "prod_rated_current", "prod_starting_current", "prod_power_consumption", "prod_mounting_cut_out", "prod_lamp", "prod_cable_length", "prod_fan_air_flow", "prod_degree_separation", "prod_luminosity", "prod_cabinet_wall", "prod_max_power_consumtion", "prod_filtering_class", "prod_working_pressure_water", "prod_control_gear", "prod_installtion", "prod_lifetime_at_40", "prod_max_air_flow", "prod_fan_lifetime", "prod_fuse_rating", "prod_connection", "prod_mounting", "prod_ordernumber", "prod_material", "prod_color", "prod_protection", "prod_includes", "prod_note", "prod_addons", "prod_approvals", "prod_ean", "prod_customs", "download_manual", "download_datasheet", "download_cad", "download_certification"); // example fields
foreach ($prodFields as $k=>$v) {
	$field = $fields->get($v);
	$field->set("columnWidth", 100); // percent
	$field->save();
}
```

----

resources:
* [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/setColumnWidth.php)
