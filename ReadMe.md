# TechTask
# I am using Pnp Package 2.2.0 It needs PowerShell 7.2
Connect-PnPOnline -Url "https://7ghdmr.sharepoint.com/sites/hr-life" -Interactive

$SiteListFile=Read-Host "Please enter the File path of list of sites file"  #"C:\temp\Tech Task\ListOfSites.csv"

$SiteTemplateFile=Read-Host "Please enter the site template file path "  #"c:/temp/Tech Task/SiteTemplate.xml"

Import-Csv  -Path $SiteListFile |`
 ForEach-Object {
 try
 {
     $siteUrl= New-PnPSite -Type TeamSite -Title $_.Title -Alias $_.Url -Description $_.Description
     write-host -ForegroundColor Green "Create Site :" $siteUrl 
     Connect-PnPOnline -Url $siteUrl -Interactive
     Invoke-PnPSiteTemplate -Path $SiteTemplateFile -ClearNavigation
     write-host -ForegroundColor Green "Template invoked on Site :" $siteUrl 
 }
 catch
 {
    Write-Error $_
 }
}
