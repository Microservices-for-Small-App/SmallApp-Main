# Publishing .NET 7 NuGet Packages

## 1. Create and publish package to Local Folder using dotnet CLI

### 1.1. Creating required Variables

```powershell
$localpackagesfolder="C:\LordKrishna\SSP\Packages"
```

### 1.2. Publishing the packages

```powershell
dotnet clean
dotnet build
dotnet pack -o $localpackagesfolder
```

### 1.3. Adding local packages folder as NuGet source

```powershell
dotnet nuget add source $localpackagesfolder -n Local-Packages
```

## Add the GitHub package source

### Creating required Variables

```powershell
$owner="Microservices-for-Small-App"
$username="vishipayyallore"
$gh_pat="ghp_Your_GitHib_Classic_PAT"

dotnet nuget remove source gHmicroservices

dotnet nuget add source --username $username --password $gh_pat --store-password-in-clear-text --name gHmicroservices "https://nuget.pkg.github.com/$owner/index.json"

dotnet nuget list source
```

## Create and publish package to GitHub using PowerShell

```powershell
$version="1.0.18"
$owner="Microservices-for-Small-App"
$username="vishipayyallore"
$repo="Libraries-Common"
$gh_pat="ghp_Your_GitHib_Classic_PAT"

dotnet clean
dotnet build -c Release
dotnet pack --configuration Release -o C:\LordKrishna\SSP\Packages -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/$repo

dotnet nuget push C:\LordKrishna\SSP\Packages\CommonLibrary.$version.nupkg --source "gHmicroservices" --api-key $gh_pat
```

```powershell
$version="1.0.18"
$owner="Microservices-for-Small-App"
$username="vishipayyallore"
$repo="Libraries-Common"
$gh_pat="ghp_Your_GitHib_Classic_PAT"

dotnet clean
dotnet build -c Release
dotnet pack --configuration Release -o C:\LordKrishna\SSP\Packages

dotnet nuget push C:\LordKrishna\SSP\Packages\CommonLibrary.$version.nupkg --source "gHmicroservices" --api-key $gh_pat
```
