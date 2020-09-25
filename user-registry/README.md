# User Registry

The user registry format is the method for customer/partner user information to come into Mivation applications from corporate directories.  The organization and email domain must be pre-configured by Mivation to accept the user registry.  Using this format requires the organization own and control the email domain.

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `id` | Yes | string | `john.smith@mycompany.com` | A unique identifier for this user as known by your systems. Any string, up to 255 chars in length, is valid. The use of email address is recommended but another identifier is also valid.  If not an email address, this will be used to match the `user_alias` supplied with activity data. Subsequent records received from the same source with the same "id" will be treated as updates.|
| `first_name` | Yes | string | `John` | The user's first (given) name. |
| `last_name` | Yes | string | `Smith` | The user's last (family) name. |
| `email` | Yes | email address | `john.smith@mycompany.com` | The user's email address.  It is recommended but not required that this match the `id`. | 
| `mobile_phone` | No | [ITU E.164](https://en.wikipedia.org/wiki/E.164) | `+15095551212` | The user's mobile phone number, can be used for SMS notifications.|
| `time_zone` | No | string | `America/New_York` | The [IANA Canonical Timezone Code](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) where the user normally operates from. |
| `apps` | Yes | {object} [] | - | An array of applications that the user should have access to.
| `{apps}.app` | Yes | string | `leaderboard_legends` | The code for the application, provided by Mivation.
| `{apps}.role` | Option | string | `Team Member` | The user's role for the app, from a pre-defined list between Mivation and customer.
| `{apps}.roles` | Option | string [] | `["Producer","Admin"]` | An array of roles for the user in the app.
| `{apps}.org_unit` | Option | string | `org1` | The organizational unit the user will be added in, based on external keys or business units configured in app.
| `{apps}.org_units` | Option | string [] | `["org1","org2"]` | An array organizational units for the user in the app.
| `{apps}.user_group` | Option | string | `sales` | The user group (team) the user will be added in, based on codes defined for groups/teams in app.
| `{apps}.user_groups` | Option | string [] | `["sales","service"]` | An array groups/teams for the user in the app.
| `invite_by_email` | No | Boolean | `true` | Should the system send this user an email, notifying them of their new account in the app. Ignored on updates. | 

### Notes
When defining user app access, supply either the singular form of `role`/`org_unit`/`user_group` or the plural form `roles`/`org_units`/`user_groups`, but not both, for the same type.  If both are supplied, the plural (array) is used and the singular is ignored.  It is acceptable to mix between singular and plural for different types and/or apps.

### Example

```yaml
{
  "format": "user-registry",
  "payload": [
     {
       "id": "john.smith@mycompany.com",
       "first_name": "John",
       "last_name": "Smith",
       "email": "john.smith@mycompany.com",
       "time_zone": "America/New_York",
       "apps": [
         {
           "app": "leaderboard_legends",
           "role": "Team Member",
           "org_unit": "org1",
           "user_group": "sales"
         }
       ]
    }		
  ]
}
```

### Advanced
There is an advanced from of the user-registry format where roles and groups can be specified per app per org unit.  This is for more fine grained permissions in situations where users have access to multiple org units, with differing role assignments.  See "[advanced-example.json](advanced-example.json)".
