# doc
## query
1. DES trace
```{sql}
https://dataexplorer.azure.com/clusters/pbipkustppe/databases/pbipppe?query=H4sIAAAAAAAAA41UwXLbNhC9%2ByswulCaESWKpEgqiXuJ3daZNOmEaic3DwgsZdQkyAKgHWXyCf2k%2FFN%2BoQtSlsQRbUUXSdh9uw9v92E%2BJ1DWZnsxn5MCDLnNVPWoQa1pdsPJJXFKiOKN83ofbzCYgtaikja88gI%2File5G7B85YY8XLlZ7AUuzZaZn3sJjShDdAtllVLADCJvuLbgaMmSHPLIZVGG4DgEN0sSzw1WUciTFV%2FGdNm1NqqBZyl%2BZXm%2BfZ5iEgW%2Bz%2BLQ5Szw3DBMAjfzGXUTP0uyZRgtFy3FJ%2FQJyyThi8TLcjdiceKGXsLcDPzMzfIkXzFGeRwHT1fUhiqzFiUgkFMDBn%2BOfc9ful7o%2Bv7aS155i1eLYLYI%2FMkOA5KfQUQHBCsabUCNR3UmWCFAGj0Dqk2jR5MZ4mlGNYxHtK6F1GJzZ%2Bw5Q0xVXj%2FY7ItvpJFWmYFK0MxkpcwdNKqq4WcLonDfyOMdKCCWPEpQ1uQXQjfV2E%2F45GIoeCQUlfwo9Gavxx4nqRXnkoyuBN3IShvB1ooyGB137hhdIU7auevZ%2Bj0WehCqknhkiJBk7Pz56eOVM0GUc%2FV57UyJ80d60363gYsXa30x3b8PezbX6VkGrCrrSmL%2FFsUqaSiqiO1R15Sh5gxmKZRU4p3eC3nvzOfPx15s1vfEZd8kLyJ7brnsuefc9XBomm6OL%2FYbKAkK15jIykBWVfckp6IA7hCsxRRgiBMJj%2Fv4j%2B%2F%2Fkbft%2BQFCW%2FsRBbopTEfC0HsgC%2Fv7%2BovB6Ruy%2Fuu2rYeZacMYAMfKdplqqjQQYQjV5F368cPzc22tXtDO6%2B3le%2Ba33VKaQ7ElsGuK3iBOx8shuYBip227Hpx8aiN%2F06Kxdja4q0rIzXiI6qyrMjnWuAfHFbOvXrtjb6sGd9hejgtdF3RrPfwvZnVFyIOF7Jyom7KkSnwFsq4MLTqTIhtma4wnU5t1%2FFljk67%2BLkfk40Eip9BfaaHPYnObNABO0e4FtHmWXokDvtVgjvFTsvBagbLtQcwzTrfvTa2qf3CKL%2B3s9DRY4%2FHvgnOQA0F8nfCNHQj0dug0Pj28bdMzzAeKH5t4IHxi1cPsn6aNm9nq3JO4t%2BatyINu%2Bh9qTtv9HAgAAA%3D%3D

// empty
// let _browserTabId = 'me67g';
// let _userSession = '9032679f-3cf9-4d49-b703-ab5b2f08a6ac';
let _correctionIds = '65c8fef6-c6b9-474e-b880-3964d89d57a5';
// true
// let _browserTabId = 'zcffy';
// let _userSession = '86322c74-dc30-4483-b2ca-82b8b54651ac';
// let _correctionIds = '88d180bf-6c78-408c-be2b-bf8f9ccad773';
let _startTime = datetime(2025-04-22T08:01:13.132);
let _endTime = datetime(2025-04-22T08:06:13.132);
cluster("pbiclients.eastus").database("appinsights").customEvents
| union cluster("pbiclientseu.northeurope").database("appinsights").customEvents
// | where timestamp > ago(28d)
| where timestamp > _startTime and timestamp < _endTime
| where name == "DiagnosticTrace"
// | where customDimensions.TL_environment in ('PROD')// 'DXT', 'MSIT', 'PROD'
| where customDimensions.TL_extensionName == "DES"
// | where customDimensions.TL_componentName contains 'DataScience.SemanticLink'//'DataScience.SemanticLink'
// | where customDimensions.browserTabId == _browserTabId
// | where customDimensions.userSession == _userSession
// | where customDimensions.TL_message contains 'Gernerate notebook failed' // created new notebook， Create notebook action result
// | take 1
// Extract TU_creationSucceeded and parse it as JSON
| where customDimensions.correlationId == _correctionIds
// Safely extract the 'result' field
// | extend ResultValue = tostring(TU_creationSucceeded.result)
// | where ResultValue == "true"
// Count and display unique result values
// | summarize TotalEvents = count(),
//             TrueCount = countif(ResultValue == "true"),
//             FalseCount = countif(ResultValue == "false"),
//             SampleValues = make_set(ResultValue, 10)
// by tostring(customDimensions.TL_environment)
| project customDimensions.TL_message, customDimensions.pageHidden, customDimensions.status, customDimensions.correlationId, customDimensions, timestamp,customDimensions.TL_environment, customDimensions.browserTabId, customDimensions.userSession
// | summarize count(), examples = make_set(_correctionId, 10) TU_creationSucceeded
```

2. pbi trace
```
   https://dataexplorer.azure.com/clusters/pbipam/databases/pbip?query=H4sIAAAAAAAAA51UXW%2FaQBB8969Y%2BQVbMlDyRaKKSgQlUqQqiUqqPlRVdD6v4Rp8Z92tkxDlx%2FfOxfggKSjlzd6Z2d3ZwQskuM%2BRUaXxmhVoRtlSskLw6GfQmWLBJAn%2BVciHa0WYKvVwh0W5YISmE8Cv%2BDMEsHASjJN4FLR8o3F7fjXRVh8bgUutikakkwTgEE1tooqiklbGdWwa8EVlCHUUlqngC4GSTA%2BZocqEcS9jxFJmMApZWQppxGxO7v2UmMyYzsQLZpOa9A1LpUnIWfAKtoeS8I4wVj1pUXOstCrxP%2FX7fXiFpzlqhDth7SBWlPAF2ExFx1kcNLUbLWZCMkeZon4UHGE0snaoJ9RwfgU%2FMIVxWXbWhMv2TDBn5p7JJUQbx4thDR57B%2FHQG3fy4L42V5KY3RXCnQEI3%2B%2FVsnee3rHxmVBma%2FqYSIu0stIwAlJNiEgZ%2B17Oore4OG5VLiTp5a0Skmr2Pzm9FhhDv3%2FSKjSjXWV7FFqgUzhoFaYV52jMHvoKtdX9Zlrbt4e7Qm31PdfqyaCuS7v5HvKvhhfW9fQjCElX9kR10VRFwbQNOmRcVZKii2fklYvtd6t0k%2F5GTs6JdPlOpBM%2FWclGUhLvZIlnftIMkqw8Sfz9PjSTv50rTK2q%2B%2Be7DQdHg9P8mA%2B7hzjMu0d5Ouye8Zx3s%2BHgID8dnNkSC32B1RB3LHUBsQqHn3R28nGT1t%2BELW%2B8JZONZok%2F%2B7aHzZP9INkvoudda6jNiTdfAPbnzjtxc9q41POKPHpz%2FjipsTlbmJ3gGhDGFmy3S4WMvA0HWfwHIlPT2WMGAAA%3D

let _featureNames=dynamic([
'SemanticLinkNotebookTemplates'
 ]); 
 let _activityNames=dynamic([
'PBICreateNotebookFromTemplate',
 'PBINotebookCommunityLink'
 ]); 
cluster("pbiclients.eastus").database("appinsights").StandardizedClientReporting
| union cluster("pbiclientseu.northeurope").database("appinsights").StandardizedClientReporting
// | where Timestamp > ago(5d)
| where OriginatingService == 'Power BI Web App'
| where FeatureName has_any (_featureNames) 
| where ActivityName has_any (_activityNames) 
| where FeatureName contains "SemanticLinkNotebookTemplates"
| where ActivityName contains "PBICreateNotebookFromTemplate"
| extend ActivityAttributes = todynamic(tostring(ActivityAttributes))
| extend EntryPoint = tostring(ActivityAttributes.EntryPoint) //6
| extend TemplateId = tostring(ActivityAttributes.TemplateId) //2
| extend Success = tostring(ActivityAttributes.Success) //6
| extend OSName  = tostring(ActivityAttributes.OSName ) //2
| extend BrowserName = tostring(ActivityAttributes.BrowserName) //2
// | where Success == "true"
// | summarize dcount(ExecutingUserObjectId) by OriginatingService, FeatureName, ActivityName, EntryPoint, TemplateId, Success, OSName, BrowserName
// | summarize dcount(ExecutingUserObjectId)
// | where UserSession == "1418f5c7-3e7f-4fb7-9cfc-d712f8195c7a"
// | where BrowserTabId == "30rd6"
// | summarize dcount(ExecutingUserObjectId) by Timestamp, FeatureName, BrowserName, BrowserTabId, UserSession, ActivityName, ActivityStatus, Success, TemplateId
|  summarize 
    trueCount = countif(Success == "true"),
    falseCount = countif(Success == "false")
  by bin(Timestamp, 1d)  
```

3.MAU
```
https://dataexplorer.azure.com/clusters/pbipkustppe/databases/pbipppe?query=H4sIAAAAAAAAA51VbW%2FbNhD%2Bnl%2FB6YulwnIkWbajbt3gxi5gNLGD2N06DENBUaeEi0QKFOWXoj9%2BR8lq7NZpsjGArJDPPffC504ZaDJZToPpCkq9AkGFJm9IQjX%2BxRkQW9AcXpNSKy7unL861OuHnheH7oU%2FYm44DGKX%2BuHQHQRBPwhpSNOAdrqkE12wcJD6kTtiFHGeF7gXI3xEoyQK%2B3TAWBx2%2Fv6ZnLGsKjUo2ypizjIOQpc9oKWuSsvpmThiWoJt0aLgouR399rsLzUVCVUJ%2FwzJZW10C4VUGmMkZ19IJbgU5AQzVCUVXBuznkD8PVRKFtB7QKTsbbhI5KbsCdD%2F1%2Ff5OflCNveggCwKUFRjIGig9IrnQH7BOuK7TBO6s4Xc2I7zjMWv9V2Axnc78IKB23cD79hI8TsuqHG%2FBLXmDAgXdmeC0S8ZRseg4xzFNd0Cqwz8QwlqEf8DTM8S8tMb0vH2yz3xaFfniOvS1C0H1QinobFGQxpGid93fTZK3XDQD92IIk1Ekzjse%2F6Q0dQiWMSnQ7FQRoHPAjRnMXVD8GP3gg4j1x8lnt8fjiKI%2B89xhOGIRlHouUEUIMeFj6KNEs%2BNUxh4g5E%2F9PqxRTCbGyWRJ5gSjT2Aj7oJDPf1crY6n3xcPR5W6ISkSubkFtIMtkZtsEWThFw2csPu0TKTGxTefsc5AN0ovsb7vOLi4Wuz4ZGiTNude62L8vX5ud179Zvj0oJjI%2FndlrhL9K4Amdr7XjykfY5LQcIVVubFhFOx5kqKHGpWcsbT1G6doFKkgFhukc1azKdvFx%2BtLkJ42mbcyLzccH1PrDYSVmqLSEV%2BhMFOcw0OiS%2BXq5YVd%2Bc4hWrPGOqnDcSfCgUFXlvHMO4DY1Joio1KLC4aV98dQCLvDPnNzRR%2FjtJ6BCWUZzuDmoxnV382OH4Kt33CC2aGLVk7QvE8TZCXvE7VyOxb1GF1NjTmddS3i0lbE14KqSEv9M7%2BTlNOjZ39Pl5Nr2bz98bE%2BjB%2Fv%2Fhjbjn7Za56Pw4ObpoLYjdenEcpzMoJrK9lArWyG63YY6b5muvdWOP%2FcYW90ePl1BiUOL%2F2Bkdz6kUW9RjSqoKO6cuUZ0YostIkgTVksqij1JBBDlrtvubwDqiuFLQisZaQYxk4M5SZdRjFq4Py4%2BAX15BLtRsLmu0%2Bg6qngdl%2Bi71%2BY2Yxg%2FZsjuWOpXxo2Moqz6nCTwCZ8BKnD9Nm%2FFzKqvl8MvNiH42m8XgySxzj4OTEami1LIhP4t2pTwEt2al61knXF3cin26T5Q%2FSqW%2F6MZszguulKbWxo95qu5XUNGsNGrzTnGBGh%2FF%2BW8NTht3%2FWln0caDl7pHDfwH0TP8Y5ggAAA%3D%3D

let DSE2ETestTenant = datatable (name: string)['a03400b4-817c-462b-a146-522324a4af2a', '98c45f19-7cac-4002-8702-97d943a5ccb4']; 
cluster("pbiclients.eastus").database("appinsights").StandardizedClientReporting 
| union cluster("pbiclientseusanitized.northeurope.kusto.windows.net").database("appinsights").StandardizedClientReporting 
// | where OperationStartTime < startofday(now())
// | where OperationStartTime >= datetime(2025-3-20)
// | where OriginatingService in('DataScience') 
// | where ExecutingUserObjectId != '00000000-0000-0000-0000-000000000000' 
// | where CustomerTenantId != "76a49d13-1c7f-4534-9a00-9adb43016caf" and ExecutingUserObjectId != "af221c2f-4cba-4e1b-8a69-17d013679eb3" and ExecutingUserObjectId != "447a9940-292a-4814-89d0-bfe50571603b" // Prod E2E test tenant and MSIT/DXT E2E test user from Reflex 
| extend Cluster = tolower(Cluster) 
| extend PrivateLinkTenant = extract('https://(.*?)-api', 1, Cluster, typeof(string)) 
| extend Tenant = extract('https://(.*?)-redirect', 1, Cluster, typeof(string)) 
| extend Environment =  
iff(Tenant == 'onebox', "ONEBOX", 
iif(Cluster startswith "https://cst" or Cluster startswith "https://app-cst", "CST", 
iif(appName == 'tri_web_preprod' or Tenant contains "int" or Tenant contains "edog", "PPE",  
iff(Tenant contains "daily", "DAILY",  
iif(Tenant contains "dxt" or Tenant contains "staging", "DXT",  
iif(Tenant contains "msit", "MSIT",  
iif(Tenant startswith "wabi", "PROD", 
iif(isnotempty(PrivateLinkTenant), "PRIVATELINK", 
"UNKOWN")))))))) 
| where Environment in ("PROD")
| extend IsDevMode = tostring(ActivityAttributes.isExtensionDevMode)
// | where ActivityAttributes.isExtensionDevMode != 'true' // filter out development telemetry
| where FeatureName == "SemanticModel"
// | where * contains "openMemoryAnalyzer" // openBestPraticeAnalyzerNotebook
// | summarize DistinctUserCount = dcount(ExecutingUserAADId) // ExecutingUserObjectId
// | top 1 by OperationStartTime asc
// | where ActivityName in ("openMemoryAnalyzer", "openBestPraticeAnalyzerNotebook")
| summarize 
    DistinctUserCount = dcount(ExecutingUserObjectId), 
    TotalCount = count() 
    by ActivityName
// | summarize TotalCount = count(), DistinctUserCount = dcount(ExecutingUserAADId) by Environment, ActivityName
```

4. pbi feature usage
```
https://dataexplorer.azure.com/clusters/pbiclients.eastus/databases/appinsights?query=H4sIAAAAAAAAAz3MuwrCQBBG4d6nmC7aaWUVQQMBQYKYvMCY%2FYnDZi9sRkXx4dUiWx%2B%2B0yp7w8nIG6YaBV4viCGp%2BGHxoecNCdSJw6TsIu2Ih7DcmlVuNVjvCQ07UFlS0cKxV%2BlP4m0TFNcQbAcXR1ZMRWb7XuUh%2BsrufDhW6ffCjOoU3Az%2FTtmCNusv1VxI0bAAAAA%3D

StandardizedClientReporting
| where Timestamp > ago(7d)
| where FeatureName == 'SemanticLinkNotebookTemplates'
| where ActivityName == 'PBICreateNotebookFromTemplate'
| take 10
```

5. FS resolve 
```
https://dataexplorer.azure.com/clusters/pbipinternal/databases/pbip?query=H4sIAAAAAAAAA33MvQ6CMBQG0J2nuGHSocHwY2ExYXAwsQmRvsBt%2B4FNoBiK0cGHd3J1Pzltr1e2SD70umMF6Ys697pVHZ2Ix2WXH2Yf9kmW0U8oxMgjNN4b2SVs7EOktCkKlrKRwtj8KMraQdRVbUQxmApOuhKG0%2BT%2FoVfvELbuaSZv24eP6qoWh%2BmGwDPSL5ydZNirAAAA

ASTrace
| where TIMESTAMP > ago(20min)
// | where MessageText contains "933a7797-bc26-48de-858b-3fb5ed7d4eba"
| where MessageText contains "TridentPublicApisMLModelRename"  
```
