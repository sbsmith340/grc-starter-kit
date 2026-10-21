# Inventory collection queries

These are read-only queries for filling each tab of the [asset inventory workbook](asset-inventory-template.xlsx). Run them with least-privilege read roles.

## Hardware
**Defender Advanced Hunting**: the latest record per device, with onboarding and exposure:
```kql
DeviceInfo
| where Timestamp > ago(30d)
| summarize arg_max(Timestamp, *) by DeviceId
| project DeviceName, DeviceType, OSPlatform, OSVersion, OSBuild, Model, Vendor,
          OnboardingStatus, IsInternetFacing, ExposureLevel, LastSeen = Timestamp,
          PublicIP, LoggedOnUsers
```
**Microsoft Graph (PowerShell)**: Intune-managed devices with encryption and compliance:
```powershell
Connect-MgGraph -Scopes DeviceManagementManagedDevices.Read.All
Get-MgDeviceManagementManagedDevice -All |
  Select-Object DeviceName, Manufacturer, Model, SerialNumber, OperatingSystem, OsVersion,
                IsEncrypted, ComplianceState, UserPrincipalName, LastSyncDateTime |
  Export-Csv hardware-intune.csv -NoTypeInformation
```
**Servers**: `Invoke-ServerOSInventory.ps1` in the [PowerShell Security Toolkit](https://github.com/sbsmith340/powershell-security-toolkit) adds lifecycle status.

## Software
```kql
DeviceTvmSoftwareInventory
| summarize InstallCount = dcount(DeviceId), Versions = make_set(SoftwareVersion, 10)
    by SoftwareVendor, SoftwareName, EndOfSupportStatus
| join kind=leftouter (
    DeviceTvmSoftwareVulnerabilities
    | where VulnerabilitySeverityLevel == "Critical"
    | summarize CriticalCVEs = dcount(CveId) by SoftwareVendor, SoftwareName
  ) on SoftwareVendor, SoftwareName
| project SoftwareName, SoftwareVendor, Versions, InstallCount, EndOfSupportStatus, CriticalCVEs = coalesce(CriticalCVEs, 0)
| order by InstallCount desc
```

## Cloud
**Azure Resource Graph**: storage accounts that allow public blob access:
```bash
az graph query -q "
resources
| where type =~ 'microsoft.storage/storageaccounts'
| project name, resourceGroup, subscriptionId, location,
          publicBlobAccess = tostring(properties.allowBlobPublicAccess),
          publicNetwork = tostring(properties.publicNetworkAccess),
          minTls = tostring(properties.minimumTlsVersion)" --first 1000 -o table
```
**Azure Resource Graph**: VMs with a public IP:
```bash
az graph query -q "
resources
| where type =~ 'microsoft.network/publicipaddresses' and isnotempty(properties.ipConfiguration.id)
| project publicIp = tostring(properties.ipAddress), attachedTo = tostring(properties.ipConfiguration.id), subscriptionId" -o table
```
**AWS**: S3 buckets and whether public access is blocked:
```bash
for b in $(aws s3api list-buckets --query 'Buckets[].Name' --output text); do
  echo -n "$b  "; aws s3api get-public-access-block --bucket "$b" \
    --query 'PublicAccessBlockConfiguration' --output json 2>/dev/null | tr -d '\n ' || echo "NO-BLOCK-CONFIG"; echo
done
```

## External domains
- Export DNS zones (Azure DNS: `az network dns record-set list`; Route 53: `aws route53 list-resource-record-sets`)
- Find forgotten subdomains through certificate transparency at `https://crt.sh/?q=%25.contoso.com`
- Check mail authentication: `Resolve-DnsName -Type TXT contoso.com` (SPF), `_dmarc.contoso.com` (DMARC)

## External authentication
**Every app users can sign in to via Entra ID**, with how many people actually use it:
```kql
SigninLogs
| where TimeGenerated > ago(30d) and ResultType == "0"
| summarize Users = dcount(UserPrincipalName),
            MfaShare = round(100.0 * countif(AuthenticationRequirement == "multiFactorAuthentication") / count(), 1),
            LegacyClients = countif(ClientAppUsed in ("Exchange ActiveSync", "IMAP4", "POP3", "Authenticated SMTP", "Other clients"))
    by AppDisplayName
| order by Users desc
```
Anything **not** behind SSO (local-account SaaS, VPN appliances, vendor portals) has to be added by hand. Those are usually the riskiest rows.

## Third parties
- Accounts-payable vendor list for the last 12 months (finance)
- Enterprise applications in Entra ID: `Get-MgServicePrincipal -All -Filter "tags/any(t:t eq 'WindowsAzureActiveDirectoryIntegratedApp')"`
- OAuth apps users consented to (see the consent detection in the [KQL library](https://github.com/sbsmith340/kql-detection-library))
