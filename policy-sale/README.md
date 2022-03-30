# Policy Sales Feed

The policy sales data feed is the standard feed for customer/partner data to come into Mivation applications from sales and production systems of insurance and financial services clients.

### Fields
| Field | Required | Type/Format | Example(s) | Description|
|-------|----------|-------------|---------|------------|
| `id` | No | string | `0276-e0f95b` | A unique record identifier for this transaction in the source system of record. Any string, up to 255 chars in length, is valid.  This value, if provided, is expected to be unique, per source. Subsequent records received from the same source with the same "id" will be treated as updates.|
| `href` | No | [URL](https://en.wikipedia.org/wiki/URL) | `https://my.crm.net/02784e36` | A url that the user can use for accessing the associated record in the source system.|
| `user_email` | Option | email address | `jonny.test@mivation.com` | The email address to identify the user that completed the sale.  One of either `use_email` or `user_alias`, but not both, must be provided.| 
| `user_alias` | Option | string | `jontes5` | The internal "alias" or "username" used by the organization or source system to identify the user that completed the sale.  The alias must be pre-registered with Mivation via "user-registry" transmission. *One of either `use_email` or `user_alias`, but not both, must be provided.|
| `org_unit` | No | string | `org1` | This field is used by larger organizations to direct this sale to a specific workgroup or sub-account within the organization hierarchy. If not supplied, the system will attempt to determine this by user membership (via alias or email). If supplied, the value needs to match your organization configuration. |
| `line_of_business` | Yes | string | `Auto` | The line of business for product of the policy or account sale.  This value should match what is defined int the application. |
| `policy_type` | Yes | string | `Motorcycle` | The sub-type of the product under the line-of-business |
| `business_type` | Yes | string | `Raw New` | Maps to the business types defined int he application for differentiating sales |
| `binding_company` | No | string | `Acme Auto` | The backing company that the policy or account is bound under.  Required if multiple binding companies are available for the LoB. |
| `policy_number` | No | string | `123456` | The policy or account number resulting form the sale |
| `prior_insurance_co` | No | string | `Progressive` | For insurance sales, The company that the customer was insured by prior to the sale.  Matches to the Insurance companies list in the application. |
| `prior_financial_co` | No | string | `WM Financial` | For financial sales, The company that the customer was invested with to the sale.  Matches to the Financial companies list in the application. |
| `customer_name` | No | string | `Jane S` | This field can be used to annotate the customer for the policy or account.  This should ideally contain non-PII data such as a partial name. |
| `household_id` | No | string | `je347rw6` | An internal household identifier for the customer, useful for reporting, if available. |
| `postal_code` | No | string | `99201` | The customer's postal code. Useful for marketing analysis reporting, if available. |
| `marketing_source` | No | string | `Internet Lead` | The general source of the opportunity. Useful for marketing analysis reporting, if available. |
| `marketing_sub_source` | No | string | `NetQuote` | The detailed source of the opportunity. Useful for marketing analysis reporting, if available. |
| `quoted_date` | No | date | `2020-08-24` | The date the policy or sale was quoted. |
| `written_date` | Yes* | date | `2020-08-30` | The date the policy or sale was written or the app was submitted. |
| `issued_date` | Yes* | date | `2020-09-02` | The date the policy or sale was issued or made effective. |
| `chargeback_date` | No | date | `2020-09-22` | A date that the policy was canceled and charged-back. |
| `term_type` | No | string | `Annual` | The term of the policy for premium purposes.  Default is "Annual" if not specified. |
| `term_premium` | No | decimal | `625.30` | The premium per-term of the policy. |
| `annual_premium` | No | decimal | `1234.50` | The annualized premium for the policy.  If not supplied will be automatically calculated from the `term_type` and `term_premium`. |
| `commissionable_premium` | No | decimal | `1100.15` | The amount of the premium that is commission eligible if some of the premium is not.  Usually only used in independent brokered insurance sales. |
| `agent_commission` | No | decimal | `215.70` | The amount of commission payable to the agent for the sale. |
| `quoted_term_premium` | No | decimal | `615.00` | The originally quoted term premium if differs from the issued term premium.  Useful for reporting. |
| `finance_charge` | No | decimal | `20.00` | A finance charge that may apply to certain financial product (loan) sales.  |
| `brokerage_fee` | No | decimal | `50.00` | A brokerage fee that may apply to some types of brokered financial or insurance products. |
| `item_count` | No | integer | `1` | The number of items on the sale.  Usually used to denote multi-car auto policy sales. |
| `opportunity_name` | No | decimal | `Jane's new car` | The name of the opportunity fro a back-end CRM.  useful fpr reporting. |
| `notes` | No | string | `Something profound` | Up to 100 chars of note on the policy or sale |
|-|-|-|-|-|
| `void` | No | boolean | `false` | Used in combination with `id` to void or delete a previously sent sale.  When `void` is `true`, `id` is required and all other values are ignored.|

### Example

```yaml
{
  "format": "policy-sale",
  "payload": [
    {
      "id": "12345w4q2",
      "href": "https://my.crm.net/12345w4q2",
      "org_unit": "01A2B3",
      "user_email": "john@mycompany.com",
      "line_of_business": "Auto",
      "policy_type": "Motorcycle",
      "business_type": "Raw New",
      "binding_company": "Acme Auto",
      "policy_number": "12345",
      "customer_name": "John Doe",
      "written_date": "2021-08-30",
      "issued_date": "2021-09-03",
      "term_type": "Semi-Annual",
      "term_premium": 625.40,
      "item_count": 1
    }, {
      "id": "3543hhy4",
      "void": true
    }
  ]
}
```


 
