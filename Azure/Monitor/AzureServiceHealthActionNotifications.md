# Azure Service Health Action Notifications

## Query Information

#### Description
Detects Azure Service Health informational action notifications from `AzureActivity` and extracts key fields from `Properties` for triage and operational follow-up.


#### References
- https://learn.microsoft.com/azure/service-health/
- https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups

## Sentinel
```KQL
AzureActivity
| where OperationNameValue contains "Microsoft.ServiceHealth/informational/action"
| extend ParsedProperties = parse_json(Properties)
| extend Title = ParsedProperties.['title'], Service = ParsedProperties.['service'], Region = ParsedProperties.['region'], Communication = ParsedProperties.['communication'], incidentType = ParsedProperties.['incidentType']
| project-reorder Level, ActivityStatus, Title, SubscriptionId, Service, Region, Communication, incidentType
```