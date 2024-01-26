# Setup Type Definition

The setup types are the configuration feed for customer/partner data to come into RacingSnail from policy management systems.  Many systems would call this a "valid values table".

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `type` | Yes | string | `business-type` | The category of setup being defined.  This value much match a recognized type option.|
| `value` | Yes | string | `new` | The value used by the source system for the setup entry.  Ideally human readable, but not required to be. |
| `label` | No | string | `New Business` | The user-friendly label for the type being defined. If not supplied, the `value` will be displayed. |
| `org_unit` | Yes | string | `org1` | This field is used to direct this setup type to a specific sub-account within the organization hierarchy. |
|-|-|-|-|-|
| `delete` | No | boolean | `false` | Used in combination with `value` to delete a previously sent Setup.|


### Example

```yaml
{
  "format": "setup-type",
  "payload": [
    { 
      "type": "location",
      "value": "L123456",
      "label": "Main Office",
      "org_unit": "org1"
    },
    {
      "type": "business-type",
      "value": "old-type-1",
      "org_unit": "org1",
      "delete": true
    }
  ]
}
```


 
