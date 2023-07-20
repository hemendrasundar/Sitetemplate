# Sitetemplate
### Features

- Capture site template, upload to a public GitHub repo (provide link as response)

- Create a new site and apply template to it (provide link as response)

- Modify template to include the new years for Global Country Holiday, include ANZ public holidays and update GitHub repo with PR

- Create script to apply this template to 10000 sites. Remember title, description and URL will be different for each of these sites. Add this script as README.md to GitHub repo

- Explain your approach for applying this template on demand via Azure

- Explain Your approach for integrating a solution in step 5 into other systems

####Code Blocks

######Capture site template.

```powershell
    Connect-PnPOnline -Url "https://your.sharepoint.com/sites/TheLanding"
    # Specify the output path and filename for the .pnp file
    $exportFilePath = "C:\yourpath\YourSiteTemplateWithContent.pnp"
    # Export the site as a template and save it to the specified path
    Get-PnPSiteTemplate -Out $exportFilePath
```

######Create a new site and apply template to it
```powershell
# Initialize a new Git repository in a local folder
$localRepoPath = "C:\yourpath\WSP"
New-Item -ItemType Directory -Path $localRepoPath -Force
cd $localRepoPath
git init
# Add and commit the template to the repository
git add .
git commit -m "Add SharePoint site template"
# Set the GitHub repository URL
$githubRepoUrl = "https://github.com/hemendrasundar/Sitetemplate.git"
# Add the GitHub repository as a remote
git remote add origin $githubRepoUrl
# Push the template to the GitHub repository
git push -u origin master
```
######upload to a public GitHub repo
```powershell
# This script assumes you have the necessary permissions to create sites and apply templates.
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yoursharepoint-admin.sharepoint.com/"
# Define the new site information
$siteUrl = "https://yoursharepoint.sharepoint.com/sites/test"
$siteTitle = "test"
$siteDescription = "Your Site Description"
# Create the new site
New-PnPTenantSite -Title $siteTitle -Url $siteUrl -Template "SITEPAGEPUBLISHING#0" -Owner "atest@domain.com" -TimeZone 3
# Wait for a few seconds to ensure the site is created before applying the template
Start-Sleep -Seconds 10
# Apply the site template to the new site
Invoke-PnPSiteTemplate -Path "C:\yourpath\YourSiteTemplateWithContent.pnp" -ResourceFolder "C:\yourpath\YourSiteTemplateWithContent" -Parameters @{"Title"=$siteTitle; "Url"=$siteUrl}
```
######Create script to apply this template to 10000 sites
|  CSV Column Names |
| ------------ |
| Title  |
| Description  |
|URL|

######Example CSV Data
|  Title |  Description | URL  |
| ------------ | ------------ | ------------ |
|Site 1| Description for Site 1| https://site1url.sharepoint.com/sites/site1 |
| Site 2| Description for Site 2 | https://site2url.sharepoint.com/sites/site2 |
|Site 3 | Description for Site 3 |https://site3url.sharepoint.com/sites/site3  |

```powershell
# Install SharePoint PnP PowerShell module if you haven't already
Install-Module SharePointPnPPowerShellOnline -Force
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://your-sharepoint-tenant-admin-url" -Credentials (Get-Credential)
# Specify the path to the .pnp file containing the site template
$templateFilePath = "C:\Path\To\YourSiteTemplate.pnp"
# Read the CSV file with site details
$sites = Import-Csv "C:\Path\To\sites.csv"
# Loop through each site in the CSV and apply the site template
foreach ($site in $sites) {
$siteTitle = $site.Title
$siteDescription = $site.Description
$siteUrl = $site.URL
# Create the new site
New-PnPTenantSite -Title $siteTitle -Url $siteUrl -Template "STS#0" -Owner "user@domain.com" -TimeZone 3 -Description $siteDescription
# Wait for a few seconds to ensure the site is created before applying the template
Start-Sleep -Seconds 10
# Apply the site template to the new site
Invoke-PnPSiteTemplate -Path $templateFilePath -ResourceFolder "C:\Path\To\YourSiteTemplate" -Parameters @{"Title"=$siteTitle; "Url"=$siteUrl}
}
# Disconnect from SharePoint Online
Disconnect-PnPOnline
```


###End
