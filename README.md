The `json` ingredient is designed to extend EPrints 3.4 by adding a new JSON-based Compound MetaField to EPrints. 

Like a Compound field, this field is designed to have a number of subfields which are all stored and displayed under the same parent field. However, unlike the Compound field, instead of storing each one in its own field or table, we store all the subfields in a single database field as a JSON string.

# Installation
This module requires the `HTML::Entities` and `JSON::Parse` libraries installed.

# Usage
The field works by providing a `json_config` parameter, for example:

```
{ name => "fieldname", type => "json", json_config => [
		{
			name => "subfield1",
			type => "text"
		},
		{
			name => "subfield2",
			type => "number"
		},
		{
			name => "subfield3",
			type => "richtext"
		},
		{
			name => "subfield4",
			type => "namedset"
			set_name => "example_set"
		}
	],
}
```

The `json_config` parameter is required.

## Field types
For the subfield type, the following fields are currently compatible:
* text
* number
* longtext
* boolean
* richtext
* namedset

`namedset` expects a `set_name` parameter provided

These all render as their standard EPrints counterparts would, however are rendered by JavaScript instead of the standard EPrints form renderer.

The corresponding phrases for each subfield are in the format:
* `eprint_fieldname_${fieldname}_${subfieldname}`
* `eprint_fieldhelp_${fieldname}_${subfieldname}`

The same convention is used for the `ep_eprint` classes added to inputs

## Table view
The JSON field can also be used to render a table, with a fixed number of rows. In this scenario, the JSON string stored is a JSON array.

Use `render_table` to turn on the table mode, and `table_row_count` to specify the number of rows.

In some tables, you may want to skip certain subfields - e.g. the third row doesn't have subfield 3. If the value of the subfield is `__json_field_control__skip`, the subfield will not render.

If using resizeable richtext in your tables, you can use the reset button to put the fields/table columns back to their default size.

## List view
The JSON field can also be used to display the fields in multiple columns, instead of the standard one-after-the-other. This is helpful in scenarios where you have lots of small fields, e.g. a series of boolean checkboxes.

Use `display_as_list` to turn on the list mode, and `display_as_list_cols` to specify the number of columns you want.

## Other parameters
You can also specify the following:
* `richtext_init_fun` - determines the function which initialises the TinyMCE richtext field. Default is `initTinyMCE`, as defined in `98_richtext.js`

## Lookup
You can provide an `input_lookup_url` as per standard MetaFields.

When added, each subfield will have lookup capabilities. It will send the following as parameters to the `input_lookup_url`
* q - the query
* field - the json field name
* json_field - the subfield name, per the json_config

A basic CGI script that accepts these parameters and uses them to lookup on a particular subfield is provided.

##

Authors: Richard Cook, EPrints Services

EPrints 3.4 is supplied by EPrints Services.
