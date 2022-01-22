# AzureRT 

Powershell module implementing various cmdlets to interact with Azure and Azure AD from an offensive perspective.

Helpful utilities dealing with access token based authentication, easily switching from `Az` to `AzureAD` and `az cli` interfaces, easy to use pre-made attacks such as Runbook-based command execution and more.

---

## Use Cases

Cmdlets implemented in this module came helpful in following use & attack scenarios:

- Juggling with access tokens from `Az` to `AzureAD` and back again.
- Nicely print authentication context (aka _whoami_) in `Az`,  `AzureAD`, `Microsoft.Graph` and `az cli` at the same time
- Display available permissions granted to the user on a target Azure VM
- Display accessible Azure Resources along with permissions we have against them
- Easily read all accessible _Azure Key Vault_ secrets
- Authenticate as a Service Principal to leverage _Privileged Role Administrator_ role assigned to that Service Principal
- Execute attack against Azure Automation via malicious Runbook

I'm developing next cmdlets along the way of learning Azure & Azure AD threat surface and offensive perspective. Therefore, most of these cmdlets came handy to me at least once on my journey.

---

## Installation

This module depends on Powershell `Az` and `AzureAD` modules pre-installed. `az cli` is optional and not required. 
Before one starts crafting around Azure, following commands may be used to prepare one's offensive environment:

```
Install-Module Az -Force -Confirm -AllowClobber
Install-Module AzureAD -Force -Confirm -AllowClobber
Install-Module Microsoft.Graph -Force -Confirm -AllowClobber
Install-Module MSOnline -Force -Confirm -AllowClobber
Install-Module AzureADPreview -Force -Confirm -AllowClobber
```

Even though only first two modules are required by `AzureRT`, its good to have others pre-installed too.

Then to load this module, simply type:

```
PS> . .\AzureRT.ps1
```

And you're good to go.

---

## Batteries Included

The module will be gradually receiving next tools and utilities, naturally categorised onto subsequent kill chain phases. 

Every cmdlet has a nice help message detailing parameters, description and example usage:

```
PS C:\> Get-Help Connect-ART
```

**Currently, following batteries are included:**

### Authentication & Token mechanics 

- **`Get-ARTWhoami`** - Displays _and validates_ our authentication context on `Azure`, `AzureAD`, `Microsoft.Graph` and on `AZ CLI` interfaces.

- **`Connect-ART`** - Invokes `Connect-AzAccount` to authenticate current session to the Azure Portal via provided Access Token or credentials. Skips the burden of providing Tenant ID and Account ID by automatically extracting those from provided Token.

- **`Connect-ARTAD`** - Invokes `Connect-AzureAD` (and optionally `Connect-MgGraph`) to authenticate current session to the Azure Active Directory via provided Access Token or credentials. Skips the burden of providing Tenant ID and Account ID by automatically extracting those from provided Token.

- **`Connect-ARTADServicePrincipal`** - Invokes `Connect-AzAccount` to authenticate current session to the Azure Portal via provided Access Token or credentials. Skips the burden of providing Tenant ID and Account ID by automatically extracting those from provided Token. Then it creates self-signed PFX certificate and associates it with Service Principal for authentication. Afterwards, authenticates as that Service Principal to AzureAD and deassociates that certificate to cleanup

- **`Get-ARTAccessTokenAzCli`** - Acquires access token from az cli, via `az account get-access-token`

- **`Get-ARTAccessTokenAz`** - Acquires access token from Az module, via `Get-AzAccessToken` .

- **`Get-ARTAccessTokenAzureAD`** - Gets an access token from Azure Active Directory. Authored by [Simon Wahlin, @SimonWahlin ](https://blog.simonw.se/getting-an-access-token-for-azuread-using-powershell-and-device-login-flow/)

- **`Remove-ARTServicePrincipalKey`** - Performs cleanup actions after running `Connect-ARTADServicePrincipal`


### Recon & Situational Awareness

- **`Get-ARTAccess`** - Performs Azure Situational Awareness.

- **`Get-ARTADAccess`** - Performs Azure AD Situational Awareness.

- **`Get-ARTResource`** - Authenticates to the https://management.azure.com using provided Access Token and pulls accessible resources and permissions that token Owner have against them.

- **`Get-ARTRoleAssignment`** - Displays a bit easier to read representation of assigned Azure RBAC roles to the currently used Principal.

- **`Get-ARTADRoleAssignment`** - Displays Azure AD Role assignments on a current user or on all Azure AD users.

- **`Get-ARTRolePermissions`** - Displays all granted permissions on a specified Azure RBAC role.

- **`Get-ARTADRolePermissions`** - Displays all granted permissions on a specified Azure AD role.

- **`Get-ARTKeyVaultSecrets`** - Lists all available Azure Key Vault secrets. This cmdlet assumes that requesting user connected to the Azure AD with KeyVaultAccessToken (scoped to https://vault.azure.net) and has "Key Vault Secrets User" role assigned (or equivalent).



### Privilege Escalation

- **`Add-ARTUserToGroup`** - Adds a specified Azure AD User to the specified Azure AD Group.

- **`Add-ARTUserToRole`** - Adds a specified Azure AD User to the specified Azure AD Role.

- **`Add-ARTADAppSecret`** - Add client secret to the Azure AD Applications. Authored by [Nikhil Mittal, @nikhil_mitt](https://twitter.com/nikhil_mitt)


### Lateral Movement

- **`Invoke-ARTAutomationRunbook`** - Creates an Automation Runbook under specified Automation Account and against selected Worker Group. That Runbook will contain Powershell commands to be executed on all the affected Azure VMs.


### Misc

- **`Get-ARTUserId`** - Acquires current user or user specified in parameter ObjectId via `Az` module

- **`Parse-JWTtokenRT`** - Parses input JWT token and prints it out nicely.

- **`Get-ARTRESTMethod`** - Takes Access Token and invokes GET REST method API request against a specified URI. It also verifies whether provided token has required audience set.


---

### ☕ Show Support ☕

This and other projects are outcome of sleepless nights and **plenty of hard work**. If you like what I do and appreciate that I always give back to the community,
[Consider buying me a coffee](https://github.com/sponsors/mgeeky) _(or better a beer)_ just to say thank you! 💪 

---

```
Mariusz Banach / mgeeky, (@mariuszbit)
<mb [at] binary-offensive.com>
```