# Copilot Studio CLI in VS Code Integrated Terminal (Windows)

## What this guide covers

This guide shows how to install and use the **Copilot Studio CLI** from the **VS Code integrated terminal** on **Windows**.

Important terminology:

- The "Copilot Studio CLI" is the **`pac copilot`** command group inside the **Microsoft Power Platform CLI (`pac`)**.
- In VS Code, the easiest way to get `pac` inside the integrated terminal is to install the **Power Platform Tools** extension.

---

## What you need

Before you start, make sure you have:

- A Windows machine.
- **Visual Studio Code** installed.
- Internet access for installation and sign-in.
- A Microsoft account that has access to **Power Platform / Copilot Studio**.
- Permission to access the target environment and agents.

### Official references

- Power Platform CLI overview: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction
- Power Platform Tools for VS Code: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension
- PAC CLI .NET tool install: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-net-tool
- PAC CLI Windows MSI install: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-msi
- PAC CLI Copilot commands: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/copilot
- PAC CLI auth commands: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/auth

---

## Recommended install path for VS Code users

If your goal is **"I want to use Copilot Studio CLI in the VS Code integrated terminal"**, the recommended path is:

1. Install **Visual Studio Code**.
2. Install the **Power Platform Tools** extension in VS Code.
3. Open the integrated terminal.
4. Run `pac` commands from there.

Microsoft documents that installing the Power Platform Tools extension makes the latest **Power Platform CLI (`pac`)** available in the **VS Code terminal**.

---

## Option A: Install with the Power Platform Tools VS Code extension (recommended)

### Step 1: Install the extension

In VS Code:

1. Open **Extensions**.
2. Search for **Power Platform Tools**.
3. Install the Microsoft extension.

You can also install from the marketplace:

- https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode

### Step 2: Open the integrated terminal

In VS Code:

- Go to **Terminal > New Terminal**.

### Step 3: Verify the CLI is available

Run:

```powershell
pac
```

You should see Power Platform CLI help text and command groups.

If you only installed PAC CLI through the VS Code extension, Microsoft notes that `pac` is available by default inside the **VS Code terminal**.

### Step 4: Confirm Windows can resolve the command

Optional verification:

```powershell
Get-Command pac | Format-List
```

This helps confirm where `pac.exe` is coming from.

---

## Option B: Install with the .NET tool (good if you also want PAC outside VS Code)

If you want `pac` available more broadly, install it as a global .NET tool.

### Step 1: Install .NET

Microsoft says the .NET tool installation requires **.NET** and recommends **.NET 10.0 or higher**.

Download .NET for Windows:

- https://dotnet.microsoft.com/download

### Step 2: Install PAC CLI

Run this in **PowerShell**, **Command Prompt**, or the **VS Code integrated terminal**:

```powershell
dotnet tool install --global Microsoft.PowerApps.CLI.Tool
```

### Step 3: Open or reopen the VS Code integrated terminal

After installation, open a new terminal in VS Code and run:

```powershell
pac
```

### Step 4: Verify where the global tool lives

On Windows, Microsoft documents that the default global tool location is:

```text
%USERPROFILE%\.dotnet\tools
```

If `pac` is not recognized immediately, restart VS Code and open a fresh terminal.

### Update later

```powershell
dotnet tool update --global Microsoft.PowerApps.CLI.Tool
```

### Uninstall later

```powershell
dotnet tool uninstall --global Microsoft.PowerApps.CLI.Tool
```

---

## Option C: Windows MSI install (optional)

Windows also supports an MSI installer for PAC CLI.

Use this if you specifically want the **Windows MSI** deployment model or want to manage installed CLI versions with `pac use`.

Official guide:

- https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-msi

Notes from Microsoft:

- The MSI installation is **Windows only**.
- Some commands such as `pac data` and certain `pac package` commands are Windows-only.
- Microsoft notes those Windows-only commands require either the **VS Code extension** or the **Windows MSI** installation method, not just the .NET tool.

---

## Sign in to your Power Platform environment

Once `pac` is available in the VS Code terminal, sign in.

### Basic sign-in

```powershell
pac auth create
```

### Sign in and target a specific environment

```powershell
pac auth create --environment "YourEnvironmentNameOrId"
```

### Useful auth commands

List saved profiles:

```powershell
pac auth list
```

Select a profile:

```powershell
pac auth select --index 1
```

Show the active profile:

```powershell
pac auth who
```

If you are working with multiple customers or environments, PAC supports multiple authentication profiles.

---

## Use Copilot Studio commands from the integrated terminal

After authentication, you can run Copilot Studio commands.

### List agents

```powershell
pac copilot list
```

### Clone an agent to your local folder

```powershell
pac copilot clone --bot "<bot-id-or-schema-name>" --output-dir .
```

### Pull remote changes into your local workspace

```powershell
pac copilot pull --project-dir .
```

### Push local changes back to Copilot Studio

```powershell
pac copilot push --project-dir .
```

### Publish an agent

```powershell
pac copilot publish --bot "<bot-id-or-schema-name>"
```

### Show all Copilot command help

```powershell
pac copilot help
```

---

## Example first-run workflow in VS Code

Open the integrated terminal and run:

```powershell
pac auth create --environment "YourEnvironmentNameOrId"
pac copilot list
pac copilot clone --bot "<bot-id-or-schema-name>" --output-dir .
```

That gives you a local workspace you can inspect in VS Code.

---

## Optional: Add the Copilot Studio VS Code extension

This is **not required** to use the CLI.

However, if you also want a richer authoring experience for cloned agents, you can install the **Copilot Studio** extension for VS Code.

Official guide:

- https://learn.microsoft.com/en-us/microsoft-copilot-studio/visual-studio-code-extension-install-configure

The extension is best for:

- browsing agents visually
- editing YAML with IntelliSense
- previewing/getting/applying changes through the UI

The CLI is best for:

- terminal-based workflows
- repeatable commands
- automation and scripting

---

## Troubleshooting

### `pac` is not recognized in the VS Code terminal

Try these fixes:

1. Close all VS Code windows.
2. Reopen VS Code.
3. Open a **new** integrated terminal.
4. Run `pac` again.

If you installed with the .NET tool, make sure the global tool path is available to your user profile.

### You installed only the VS Code extension and `pac` works in VS Code but not in standalone PowerShell

That can be expected. Microsoft notes that when you install PAC only through the VS Code extension, it is available by default inside the VS Code terminal.

### Sign-in issues

Try device code flow:

```powershell
pac auth create --environment "YourEnvironmentNameOrId" --deviceCode
```

### Need command details

Use built-in help:

```powershell
pac help
pac auth help
pac copilot help
```

---

## Summary

For Windows, the fastest route is:

1. Install **VS Code**.
2. Install **Power Platform Tools**.
3. Open **Terminal > New Terminal**.
4. Run `pac auth create`.
5. Run `pac copilot` commands.

If you want `pac` available outside VS Code too, add the **.NET tool** installation.

---

## Official source links

- Power Platform CLI overview: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction
- Install Power Platform Tools in VS Code: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension
- Install PAC CLI with .NET Tool: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-net-tool
- Install PAC CLI with Windows MSI: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-msi
- PAC CLI auth reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/auth
- PAC CLI copilot reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/copilot
- Copilot Studio VS Code extension install: https://learn.microsoft.com/en-us/microsoft-copilot-studio/visual-studio-code-extension-install-configure
