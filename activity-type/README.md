# Activity Type Definition

The activity types are the configuration feed for customer/partner data to come into Mivation applications from activity management solutions and phone systems.

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `id` | Yes | string | `call` | The identifier for the type of activity being defined.  This value will be used as the `type` in "activity"|
| `name` | No | string | `Call` | The user-friendly name for the type being defined. If not supplied, the `id` will be used as the `name`. |
| `org_unit` | No | string | `org1` | This field is used by larger organizations to direct this activity to a specific workgroup or sub-account within the organization hierarchy. |
| `fields` | No | {object} [] | - | An array of fields that will come with the activity data.  |
| `{fields}.id` | Yes | string | `direction` | The identifier for the filed that will match the field (key) in the JSON of the activity record. |
| `{fields}.label` | No | string | `Call Direction` | The user-friendly label for the field being defined. If not supplied, the `id` will be displayed. |
| `{fields}.input` | No | string | `select` | Visual control to be used for the field.  Options are: `text`, `number`, `textarea`, `radio`, `checkbox`, `select` and `multi-select`.  If not supplied, the system will use an appropriate input control for the data type. |
| `{fields}.data_type` | No | string | `string` | Type of data contained in the field.  Options are: `string`, `integer`, `double`, `boolean`, `datetime`, `date`, `duration`.  The default is `string`. |
| `{fields}.values` | No | string [] *or* {key/value} | `["Inbound","Outbound"]` | A list of possible values for the field, to be displayed when the `input` is set to `select` or `checkbox`.  If sent as a JSON object it will be treated as key/value pairs, supporting data value vs. display label. |
| `{fields}.operators` | No | string [] | `["in","not_in"]` | *(Advanced Configuration)* A list of supported comparison operators for the field.  It's normally recommended to omit this and let the system choose appropriate operators for the data type and input. |
| `{fields}.filter_options` | No | {object} | - | *(Advanced Configuration)* Advanced options for filter setup. Detail will be provided if this is required for your configuration.  |
|-|-|-|-|-|
| `delete` | No | boolean | `false` | Used in combination with `id` to delete a previously sent activity type.  When `delete` is `true`, `id` is required and all other values are ignored.|


### Example

```yaml
{
  "format": "activity-type",
  "payload": [
    { 
      "id": "call",
      "name": "Call",
      "fields": [
        {
          "id": "direction",
          "input": "select",
          "values": {
            "in": "Inbound",
            "out": "Outbound"
          }
        },
        {
          "id": "end_timestamp",
          "label": "End Time",
          "data_type": "datetime"
        },
        {
          "id": "disposition",
          "input": "checkbox",
          "values": [ "Made Sale", "Quoted", "Left Message", "Declined" ],
          "operators": [ "in", "not_in" ]
        }
      ]
    },
    {
      "id": "old-type-1",
      "delete": true
    }
  ]
}
```


 
