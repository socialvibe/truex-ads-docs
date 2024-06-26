# Reporting API

## Publisher Performance Report (JSON)

### URL:
```
http://api.truex.com/v1/publisher/performance.json
```

### Parameters:

- `api_key` (STRING) - The partner-specific reporting api key provided by true[X]
- `start_date` (STRING, YYYY-MM-DD) - Begin report on the date specified.
- `end_date` (STRING, YYYY-MM-DD) - End report on the date specified 
[Optional, default is today]


### HTTP Method(s):
`GET`

### Response Format:
`JSON`

### Response:
A JSON object intended for programmatic consumption. Revenue information and key performance indicators are broken out by placement key, day, and campaign (identified by a CAMPAIGN_ID). 

#### Sample Request/Response:
```
http://api.truex.com/v1/publisher/performance.json?api_key=[API_KEY]&start_date=YYYY-MM-DD
```

```json
{
  "PLACEMENT_KEY_ONE" : {
    "YYYY-MM-DD" : {
      CAMPAIGN_ID: {
        "day" : "YYYY-MM-DD",
        "placement_hash" : "Your Placement Hash",
        "placement_name" : "Your Placement",
        "company" : "Your Company",
        "campaign_id" : 1000,
        "campaign_name" : "Brand Campaign",
        "campaign_start_date" : "YYYY-MM-DD",
        "campaign_end_date" : "YYYY-MM-DD",
        "daily_frequency_cap" : 12,
        "lifetime_frequency_cap" : 10,
        "country_targets" : "US,CA",
        "state_targets" : "CA,NV",
        "dma_targets" : "618,623,807,862,866,868",
        "postal_code_targets" : "90291",
        "initial_engagements": 42,
        "completed_engagements": 37,
        "advertiser_cost" : "100.00",
        "average_cpe" : "0.12",
        "net_revenue" : "80.00",
        "cap_type": "units"
      },
      CAMPAIGN_ID: {
        "day" : "YYYY-MM-DD",
        "placement_hash" : "Your Placement Hash",
        "placement_name" : "Your Placement",
        "company" : "Your Company",
        "campaign_id" : 1000,
        "campaign_name" : "Brand Campaign",
        "campaign_start_date" : "YYYY-MM-DD",
        "campaign_end_date" : "YYYY-MM-DD",
        "daily_frequency_cap" : 12,
        "lifetime_frequency_cap" : 10,
        "country_targets" : "US,CA",
        "state_targets" : "CA,NV",
        "dma_targets" : "618,623,807,862,866,868",
        "postal_code_targets" : "90291",
        "initial_engagements": 42,
        "completed_engagements": 37,
        "advertiser_cost" : "100.00",
        "average_cpe" : "0.12",
        "net_revenue" : "80.00",
        "cap_type" : ""units"
      }
    }
  }
}
```

## Publisher Performance Report (CSV)

### URL:
```
http://api.truex.com/v1/publisher/performance.csv
```

### Parameters:

- `api_key` (STRING) - The partner-specific reporting api key provided by true[X]
- `start_date` (STRING, YYYY-MM-DD) - Begin report on the date specified.
- `end_date` (STRING, YYYY-MM-DD) - End report on the date specified 
[Optional, default is today]


### HTTP Method(s):
`GET`

### Response Format:
`CSV`

### Response:
The same data as the JSON report but in CSV format. Useful for importing into a spreadsheet for further manipulation. For programmatic consumption we recommend using the JSON format instead.


#### Sample Request/Response:
```
http://api.truex.com/v1/publisher/performance.csv?api_key=[API_KEY]&start_date=YYYY-MM-DD
```
```csv
Placement Hash,Day,Campaign Id,Campaign Name,Campaign Start Date,Campaign End Date,Daily Frequency Cap,Lifetime Frequency Cap,Country Targets,State Targets,DMA Targets,Postal Code Targets,Initial Engagements, Completed Engagements,Placement Name,Net Revenue,Advertiser Cost,Company,Average CPE, Cap Type
PLACEMENT_HASH_ONE,YYYY-MM-DD,1000,CAMPAIGN_NAME,YYYY-MM-DD,YYYY-MM-DD,12,20,US,CA,618,90291,0.30,2,100,PLACEMENT_NAME,80.00,100,COMPANY_NAME,0.25,units
PLACEMENT_HASH_TWO,YYYY-MM-DD,1000,CAMPAIGN_NAME,YYYY-MM-DD,YYYY-MM-DD,12,20,US,CA,618,90291,42,37,100,PLACEMENT_NAME,80.00,100,COMPANY_NAME,0.25,units
```