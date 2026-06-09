# Copilot Studio CLI in VS Code Integrated Terminal (macOS)

## What this guide covers

This guide shows how to install and use the **Copilot Studio CLI** from the **VS Code integrated terminal** on **macOS**.

Important terminology:

- The "Copilot Studio CLI" is the **`pac copilot`** command group inside the **Microsoft Power Platform CLI (`pac`)**.
- In VS Code, the easiest way to get `pac` inside the integrated terminal is to install the **Power Platform Tools** extension or install PAC as a **global .NET tool**.

---

## What you need

Before you start, make sure you have:

- A Mac.
- **Visual Studio Code** installed.
- Internet access for installation and sign-in.
- A Microsoft account that has access to **Power Platform / Copilot Studio**.
- Permission to access the target environment and agents.

### Official references

- Power Platform CLI overview: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction
- Power Platform Tools for VS Code: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension
- PAC CLI .NET tool install: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-net-tool
- PAC CLI Copilot commands: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/copilot
- PAC CLI auth commands: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/auth
- Copilot Studio VS Code extension install: https://learn.microsoft.com/en-us/microsoft-copilot-studio/visual-studio-code-extension-install-configure

---

## Recommended install path for VS Code users

If your goal is **"I want to use Copilot Studio CLI in the VS Code integrated terminal"**, the cleanest path on macOS is:

1. Install **Visual Studio Code**.
2. Install the **Power Platform Tools** extension in VS Code.
3. Open the integrated terminal.
4. Run `pac` commands from there.

You can also install PAC as a global **.NET tool** if you want `pac` to work outside VS Code too.

---

## Option A: Install with the Power Platform Tools VS Code extension (recommended)

### Step 1: Install the extension

In VS Code:

1. Open **Extensions**.
2. Search for **Power Platform Tools**.
3. Install the Microsoft extension.

You can also use the marketplace page:

- https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode

Microsoft documents this as the recommended way to install Power Platform CLI for VS Code users.

### Step 2: Open the integrated terminal

In VS Code:

- Go to **Terminal > New Terminal**.

### Step 3: Verify the CLI is available

Run:

```bash
pac
```

You should see help text and available command groups.

If you installed PAC only through the VS Code extension, Microsoft notes that `pac` is available by default inside the VS Code terminal.

---

## Option B: Install with the .NET tool (recommended if you want PAC everywhere)

macOS supports installing PAC CLI by using the .NET tool.

### Step 1: Install .NET

Microsoft says the .NET tool installation requires **.NET** and recommends **.NET 10.0 or higher**.

Download .NET for macOS:

- https://dotnet.microsoft.com/download

### Step 2: Install PAC CLI

Run this in **Terminal** or in the **VS Code integrated terminal**:

```bash
dotnet tool install --global Microsoft.PowerApps.CLI.Tool
```

### Step 3: Open or reopen the VS Code integrated terminal

After installation, run:

```bash
pac
```

### Step 4: Know the default global tool path

Microsoft documents the default location for the tool on macOS as:

```text
$HOME/.dotnet/tools
```

If `pac` is not recognized immediately, close and reopen VS Code, then open a fresh integrated terminal.

### Update later

```bash
dotnet tool update --global Microsoft.PowerApps.CLI.Tool
```

### Uninstall later

```bash
dotnet tool uninstall --global Microsoft.PowerApps.CLI.Tool
```

---

## Sign in to your Power Platform environment

Once `pac` works in the VS Code integrated terminal, sign in.

### Basic sign-in

```bash
pac auth create
```

### Sign in and target a specific environment

```bash
pac auth create --environment "YourEnvironmentNameOrId"
```

### Useful auth commands

List saved profiles:

```bash
pac auth list
```

Select a profile:

```bash
pac auth select --index 1
```

Show the active profile:

```bash
pac auth who
```

If you manage multiple environments or customers, PAC supports multiple auth profiles.

---

## Use Copilot Studio commands from the integrated terminal

After authentication, you can run Copilot Studio commands.

### List agents

```bash
pac copilot list
```

### Clone an agent to your local folder

```bash
pac copilot clone --bot "<bot-id-or-schema-name>" --output-dir .
```

### Pull remote changes into your local workspace

```bash
pac copilot pull --project-dir .
```

### Push local changes back to Copilot Studio

```bash
pac copilot push --project-dir .
```

### Publish an agent

```bash
pac copilot publish --bot "<bot-id-or-schema-name>"
```

### Show all Copilot command help

```bash
pac copilot help
```

---

## Example first-run workflow in VS Code

Open the integrated terminal and run:

```bash
pac auth create --environment "YourEnvironmentNameOrId"
pac copilot list
pac copilot clone --bot "<bot-id-or-schema-name>" --output-dir .
```

That gives you a local workspace you can inspect in VS Code.

---

## Verify the command path on macOS

If you want to verify that the terminal can find `pac`, run:

```bash
which pac
```

If you installed with the .NET tool, the result usually points into:

```text
$HOME/.dotnet/tools
```

---

## Optional: Add the Copilot Studio VS Code extension

This is **not required** to use the CLI.

If you also want a full editor experience for cloned agents, install the **Copilot Studio** extension for VS Code.

Official guide:

- https://learn.microsoft.com/en-us/microsoft-copilot-studio/visual-studio-code-extension-install-configure

The extension is best for:

- browsing agents visually
- editing YAML with IntelliSense
- previewing/getting/applying changes through the UI

The CLI is best for:

- terminal-based workflows
- repeatable commands
- scripting and automation

---

## Troubleshooting

### `pac` is not recognized in the VS Code terminal

Try these fixes:

1. Close VS Code.
2. Reopen VS Code.
3. Open a **new** integrated terminal.
4. Run `pac` again.

If you installed with the .NET tool and the command still is not found, confirm your shell can see `$HOME/.dotnet/tools`.

### Need an alternative sign-in flow

If interactive sign-in is awkward in your setup, try:

```bash
pac auth create --environment "YourEnvironmentNameOrId" --deviceCode
```

### Need command details

Use built-in help:

```bash
pac help
pac auth help
pac copilot help
```

---

## Summary

For macOS, the fastest route is:

1. Install **VS Code**.
2. Install **Power Platform Tools**.
3. Open **Terminal > New Terminal**.
4. Run `pac auth create`.
5. Run `pac copilot` commands.

If you also want `pac` outside VS Code, install it with the **.NET tool**.

---

## Official source links

- Power Platform CLI overview: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction
- Install Power Platform Tools in VS Code: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-vs-code-extension
- Install PAC CLI with .NET Tool: https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-net-tool
- PAC CLI auth reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/auth
- PAC CLI copilot reference: https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/copilot
- Copilot Studio VS Code extension install: https://learn.microsoft.com/en-us/microsoft-copilot-studio/visual-studio-code-extension-install-configure
