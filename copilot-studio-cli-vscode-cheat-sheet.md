# Copilot Studio CLI in VS Code: One-Page Cheat Sheet

This cheat sheet is focused on the commands you are most likely to use for **Copilot Studio work inside the VS Code integrated terminal**.

Important:

- The **Copilot Studio CLI** is the **`pac copilot`** command group inside the **Microsoft Power Platform CLI (`pac`)**.
- Most Copilot Studio commands require you to **authenticate first**.
- Command examples below work in the **VS Code integrated terminal** on **Windows** and **macOS**. Replace placeholder values like `<environment-id>` and `<bot-id>` with your own values.

---

## 1) Quick install and verification

### Install PAC CLI with the Power Platform Tools extension (easy path)

Use this when you want `pac` available inside VS Code quickly.

```text
VS Code > Extensions > search for "Power Platform Tools" > Install
```

### Install PAC CLI with .NET tool (works inside and outside VS Code)

```bash
dotnet tool install --global Microsoft.PowerApps.CLI.Tool
```

### Verify the CLI is available

#### Example 1: show help and version line
```bash
pac
```

#### Example 2: Windows - show the resolved command path
```powershell
Get-Command pac | Format-List
```

#### Example 3: macOS - show the resolved command path
```bash
which pac
```

---

## 2) `pac help`

### What it does
Shows the top-level PAC CLI command groups and general help.

### Parameters
This help call is commonly run with no parameters in quick-start usage.

### Examples

#### Example 1
```bash
pac help
```

#### Example 2
```bash
pac copilot help
```

#### Example 3
```bash
pac auth help
```

---

## 3) `pac auth create`

### What it does
Creates and stores an authentication profile on your machine so PAC can talk to your tenant and environment.

### Parameters

- `--applicationId` or `-id`  
  Application ID used for app-based authentication.
- `--azureDevOpsFederated` or `-adof`  
  Uses Azure DevOps federation. Requires `--tenant` and `--applicationId`.
- `--certificateDiskPath` or `-cdp`  
  Path to a certificate file for certificate-based auth.
- `--certificatePassword` or `-cp`  
  Password for the certificate.
- `--clientSecret` or `-cs`  
  Client secret for service principal auth.
- `--cloud` or `-ci`  
  Cloud instance. Supported values: `Public`, `UsGov`, `UsGovHigh`, `UsGovDod`, `China`.
- `--deviceCode` or `-dc`  
  Uses device code sign-in.
- `--environment` or `-env`  
  Default environment. Can be environment ID, URL, unique name, or partial name.
- `--githubFederated` or `-ghf`  
  Uses GitHub federation. Requires `--tenant` and `--applicationId`.
- `--managedIdentity` or `-mi`  
  Uses Azure Identity / managed identity.
- `--name` or `-n`  
  Friendly name for the auth profile. Maximum 30 characters.
- `--password` or `-p`  
  Password for username/password auth.
- `--tenant` or `-t`  
  Tenant ID for app-based auth.
- `--url` or `-u`  
  Deprecated. Use `--environment` instead.
- `--username` or `-un`  
  Username for sign-in if you do not want the prompt to decide.

### Examples

#### Example 1: interactive sign-in
```bash
pac auth create
```

#### Example 2: sign in and bind to a specific environment
```bash
pac auth create --environment "HR-Dev"
```

#### Example 3: device code sign-in for remote/locked-down setups
```bash
pac auth create --environment "00000000-0000-0000-0000-000000000000" --deviceCode
```

#### Example 4: named profile
```bash
pac auth create --name "Contoso Dev" --environment "https://contosodev.crm.dynamics.com"
```

#### Example 5: service principal auth
```bash
pac auth create --name "Contoso-SPN" --applicationId "00000000-0000-0000-0000-000000000000" --clientSecret "<client-secret>" --tenant "00000000-0000-0000-0000-000000000000"
```

---

## 4) `pac auth list`

### What it does
Lists the authentication profiles stored on your machine.

### Parameters
No parameters are documented for the common usage shown in the Microsoft Learn synopsis.

### Examples

#### Example 1
```bash
pac auth list
```

#### Example 2: common follow-up after creating multiple profiles
```bash
pac auth create --name "Customer A" --environment "CustomerA-Dev"
pac auth create --name "Customer B" --environment "CustomerB-Dev"
pac auth list
```

---

## 5) `pac auth select`

### What it does
Makes one stored authentication profile the active profile.

### Parameters

- `--index` or `-i`  
  Numeric index of the profile to activate.
- `--name` or `-n`  
  Profile name to activate.

### Examples

#### Example 1: select by index
```bash
pac auth select --index 2
```

#### Example 2: select by name
```bash
pac auth select --name "Contoso Dev"
```

---

## 6) `pac auth who`

### What it does
Shows information about the currently selected authentication profile.

### Parameters
No parameters are documented for the common usage shown in the Microsoft Learn synopsis.

### Examples

#### Example 1
```bash
pac auth who
```

#### Example 2: common validation flow
```bash
pac auth select --name "Contoso Dev"
pac auth who
```

---

## 7) `pac auth update`

### What it does
Updates the name and/or target environment of an existing auth profile.

### Parameters

- `--index` or `-i`  
  Required. Profile index to update.
- `--environment` or `-env`  
  New default environment.
- `--name` or `-n`  
  New friendly name for the profile.

### Examples

#### Example 1: rename and change environment URL
```bash
pac auth update --index 1 --name "Contoso Dev" --environment "https://contosodev.crm.dynamics.com"
```

#### Example 2: change only the environment
```bash
pac auth update --index 1 --environment "00000000-0000-0000-0000-000000000000"
```

---

## 8) `pac auth delete`

### What it does
Deletes a stored authentication profile.

### Parameters

- `--index` or `-i`  
  Profile index to delete.
- `--name` or `-n`  
  Profile name to delete.

### Examples

#### Example 1: delete by index
```bash
pac auth delete --index 2
```

#### Example 2: delete by name
```bash
pac auth delete --name "Old Sandbox"
```

---

## 9) `pac copilot list`

### What it does
Lists Copilot Studio agents in the current or target Dataverse environment.

### Parameters

- `--environment` or `-env`  
  Environment ID or URL to query. If omitted, PAC uses the active auth profile's environment.

### Examples

#### Example 1: list in the active environment
```bash
pac copilot list
```

#### Example 2: list in a specific environment by ID
```bash
pac copilot list --environment "00000000-0000-0000-0000-000000000000"
```

#### Example 3: list in a specific environment by URL
```bash
pac copilot list --environment "https://contosodev.crm.dynamics.com"
```

---

## 10) `pac copilot clone`

### What it does
Clones a Copilot Studio agent to a local workspace folder.

### Parameters

- `--bot` or `-id`  
  Required. Copilot ID or schema name.
- `--component-collection` or `-cc`  
  Component collection ID to clone. Repeat for multiple collections.
- `--display-name` or `-dn`  
  Display name used for folder naming.
- `--environment` or `-env`  
  Target Dataverse environment.
- `--output-dir` or `-od`  
  Root output folder. Defaults to the current directory.

### Examples

#### Example 1: clone to the current folder
```bash
pac copilot clone --bot "my_agent_schema_name" --output-dir .
```

#### Example 2: clone using a GUID and explicit environment
```bash
pac copilot clone --bot "11111111-2222-3333-4444-555555555555" --environment "00000000-0000-0000-0000-000000000000" --output-dir "./agents"
```

#### Example 3: clone with a display name override
```bash
pac copilot clone --bot "my_agent_schema_name" --display-name "Sales Coach" --output-dir "./workspaces"
```

---

## 11) `pac copilot pull`

### What it does
Pulls remote changes from Copilot Studio into a local workspace and merges them.

### Parameters

- `--project-dir` or `-pd`  
  Path to the local project directory. Defaults to the current directory.

### Examples

#### Example 1: pull in the current folder
```bash
pac copilot pull
```

#### Example 2: pull from a specific workspace folder
```bash
pac copilot pull --project-dir "./agents/sales-coach"
```

---

## 12) `pac copilot push`

### What it does
Pushes local workspace changes up to Copilot Studio.

### Parameters

- `--project-dir` or `-pd`  
  Path to the local project directory. Defaults to the current directory.

### Examples

#### Example 1: push current folder
```bash
pac copilot push
```

#### Example 2: push a specific local workspace
```bash
pac copilot push --project-dir "./agents/sales-coach"
```

---

## 13) `pac copilot publish`

### What it does
Publishes a Copilot Studio agent.

### Parameters

- `--bot` or `-id`  
  Required. Copilot ID or schema name.
- `--environment` or `-env`  
  Optional target Dataverse environment.

### Examples

#### Example 1: publish from the active environment
```bash
pac copilot publish --bot "my_agent_schema_name"
```

#### Example 2: publish a specific bot in a specific environment
```bash
pac copilot publish --bot "11111111-2222-3333-4444-555555555555" --environment "https://contosodev.crm.dynamics.com"
```

---

## 14) `pac copilot init`

### What it does
Creates a new Copilot Studio agent workspace from a template.

### Parameters

- `--name` or `-n`  
  Required. Agent display name.
- `--publisher-prefix` or `-pp`  
  Required. Publisher customization prefix.
- `--environment` or `-env`  
  Target Dataverse environment.
- `--instructions` or `-ins`  
  System instructions for the agent.
- `--project-dir` or `-pd`  
  Target workspace directory. Defaults to the current directory.
- `--schema-name` or `-sn`  
  Full schema name. If omitted, PAC derives one.
- `--template` or `-t`  
  Template profile. Microsoft documents `default` and `minimal`, and `default` is the default.

### Examples

#### Example 1: minimal new workspace
```bash
pac copilot init --name "Sales Coach" --publisher-prefix "aj"
```

#### Example 2: specify project folder and template
```bash
pac copilot init --name "Sales Coach" --publisher-prefix "aj" --project-dir "./sales-coach" --template minimal
```

#### Example 3: specify instructions and schema name
```bash
pac copilot init --name "Sales Coach" --publisher-prefix "aj" --schema-name "aj_salescoach" --instructions "Help sellers answer product questions and route escalations."
```

---

## 15) `pac copilot extract-template`

### What it does
Extracts a reusable YAML template from an existing Copilot Studio agent.

### Parameters

- `--bot` or `-id`  
  Required. Copilot ID or schema name.
- `--templateFileName`  
  Required. Output YAML file path.
- `--environment` or `-env`  
  Target environment.
- `--overwrite` or `-o`  
  Overwrite the output file if it already exists.
- `--templateName`  
  Template name. Microsoft says it defaults to `kickStartTemplate` if not supplied.
- `--templateVersion`  
  Template version in `X.X.X` format. Microsoft says it defaults to `1.0.0`.

### Examples

#### Example 1: basic extraction
```bash
pac copilot extract-template --bot "11111111-2222-3333-4444-555555555555" --templateFileName "./SalesCoach.yaml"
```

#### Example 2: extract from a specific environment
```bash
pac copilot extract-template --environment "00000000-0000-0000-0000-000000000000" --bot "11111111-2222-3333-4444-555555555555" --templateFileName "./SalesCoach.yaml"
```

#### Example 3: overwrite with explicit template metadata
```bash
pac copilot extract-template --bot "my_agent_schema_name" --templateFileName "./SalesCoach.yaml" --overwrite --templateName "SalesCoachTemplate" --templateVersion "1.0.0"
```

---

## 16) `pac copilot create`

### What it does
Creates a new Copilot Studio agent using an extracted template YAML file.

### Parameters

- `--displayName`  
  Required. Display name of the new copilot.
- `--schemaName`  
  Required. Schema name of the new copilot.
- `--solution` or `-s`  
  Required. Solution name.
- `--templateFileName`  
  Required. Path to the source template YAML file.
- `--environment` or `-env`  
  Target Dataverse environment.

### Examples

#### Example 1: create from a template
```bash
pac copilot create --displayName "Sales Coach" --schemaName "aj_salescoach" --solution "AJSalesSolution" --templateFileName "./SalesCoach.yaml"
```

#### Example 2: create in a specific environment
```bash
pac copilot create --displayName "HR Coach" --schemaName "aj_hrcoach" --solution "AJHRSolution" --templateFileName "./HRCoach.yaml" --environment "https://contosodev.crm.dynamics.com"
```

---

## 17) `pac copilot pack`

### What it does
Packages a local Copilot Studio workspace into a solution zip file.

### Parameters

- `--publisher-prefix` or `-pp`  
  Required. Publisher customization prefix.
- `--output-path` or `-op`  
  Output folder for the solution zip. Defaults to the current directory.
- `--project-dir` or `-pd`  
  Agent workspace directory. Defaults to the current directory.
- `--solution-name` or `-sn`  
  Solution unique name. If omitted, PAC derives it from the agent schema name.

### Examples

#### Example 1: pack the current workspace
```bash
pac copilot pack --publisher-prefix "aj"
```

#### Example 2: pack a specific workspace to a specific output folder
```bash
pac copilot pack --publisher-prefix "aj" --project-dir "./sales-coach" --output-path "./dist"
```

#### Example 3: force a specific solution name
```bash
pac copilot pack --publisher-prefix "aj" --project-dir "./sales-coach" --solution-name "AJSalesCoach"
```

---

## 18) `pac copilot status`

### What it does
Polls the deployment status of a specific Copilot Studio agent.

### Parameters

- `--bot-id` or `-id`  
  Required. Copilot ID.
- `--environment` or `-env`  
  Optional target environment.

### Examples

#### Example 1: check status in the active environment
```bash
pac copilot status --bot-id "11111111-2222-3333-4444-555555555555"
```

#### Example 2: check status in a specific environment
```bash
pac copilot status --environment "00000000-0000-0000-0000-000000000000" --bot-id "11111111-2222-3333-4444-555555555555"
```

---

## 19) `pac copilot mcp`

### What it does
Starts the built-in PAC CLI MCP server for natural-language interaction from MCP-aware tools.

### Parameters

- `--run` or `-r`  
  Runs the local MCP server.

### Examples

#### Example 1: start MCP from an installed PAC CLI
```bash
pac copilot mcp --run
```

#### Example 2: start MCP without installing PAC globally first
```bash
dnx Microsoft.PowerApps.CLI.Tool --yes copilot mcp --run
```

---

## 20) Fastest common workflow

### Clone an existing agent and work locally
```bash
pac auth create --environment "<environment-id-or-url>"
pac copilot list
pac copilot clone --bot "<bot-id-or-schema-name>" --output-dir .
pac copilot pull --project-dir .
pac copilot push --project-dir .
pac copilot publish --bot "<bot-id-or-schema-name>"
```

### Create a new agent from scratch locally
```bash
pac auth create --environment "<environment-id-or-url>"
pac copilot init --name "My Agent" --publisher-prefix "aj" --project-dir "./my-agent"
pac copilot pack --publisher-prefix "aj" --project-dir "./my-agent" --output-path "./dist"
```

### Template-based agent creation flow
```bash
pac auth create --environment "<environment-id-or-url>"
pac copilot extract-template --bot "<source-bot-id>" --templateFileName "./Template.yaml"
pac copilot create --displayName "New Agent" --schemaName "aj_newagent" --solution "AJSolution" --templateFileName "./Template.yaml"
```

---

## Official references

- Power Platform CLI overview: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction
- Power Platform Tools for VS Code: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension
- Install PAC CLI with .NET Tool: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-net-tool
- Install PAC CLI with Windows MSI: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-msi
- PAC CLI auth reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/auth
- PAC CLI copilot reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/copilot
- PAC CLI MCP guide: https://learn.microsoft.com/en-us/power-platform/developer/howto/use-mcp
- PAC CLI without install (`dnx`): https://learn.microsoft.com/en-us/power-platform/developer/howto/dnx-cli
