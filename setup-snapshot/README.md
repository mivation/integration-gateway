# Setup Type Definition by Snapshot

The setup types are the configuration feed for customer/partner data to come into RacingSnail from policy management systems.  Many systems would call this a "valid values table".  

The key difference with the snapshot is that all valid values for a given `type`/`org_unit` combination are expected in the `values` array.  Any omitted entries are considered deleted.  Where the "setup-type" format is is imperative, this format is declarative. 

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `type` | Yes | string | `business_type` | The category of setup being defined.  This value much match a recognized type option.|
| `org_unit` | Yes | string | `org1` | This field is used to direct this setup type to a specific sub-account within the organization hierarchy. |
| `line_of_business` | No** | string | `Auto` | The line of business for certain setup types that vary by line.  `policy_type`,`business_type`, and `followup` all require `line_of_business`.  For other types it is ignored. |
| `values` | Yes | {object} [] | - | An array of values that the are valid for that org unit.
| `{values}.value` | Yes | string | `new` | The value used by the source system for the setup entry.  Ideally human readable, but not required to be. |
| `{values}.label` | No | string | `New Business` | The user-friendly label for the type being defined. If not supplied, the `value` will be displayed. |


### Example

```yaml
{
  "format": "setup-snapshot",
  "payload": [
    { 
      "type": "location",
      "org_unit": "org1",
      "values": [
        {
          "value": "L123456",
          "label": "Main Office",
        },
        {
          "value": "L123457",
          "label": "Satalite Office",
        }
      ]
    },
    {
      "type": "business_type",
      "org_unit": "org1",
      "line_of_business": "Auto",
      "values": [
        {
          "value": "New",
        },
        {
          "value": "Renewal",
        }
      ]
    }
  ]
}
```


 
