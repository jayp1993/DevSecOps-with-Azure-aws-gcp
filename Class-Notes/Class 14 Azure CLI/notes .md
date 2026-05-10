
---

## 🧰 What is Azure CLI?

The Azure CLI is a cross-platform command-line tool that enables you to manage Azure resources directly from your terminal or scripts. It's designed for developers and administrators to automate tasks and streamline workflows.  ([Begin your Azure CLI journey - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/get-started-tutorial-0-before-you-begin?utm_source=chatgpt.com))

---

## 🖥️ Where Can You Run Azure CLI?

- **Operating Systems**: Windows (Cmd or PowerShell), Linux, macOS
- **Azure Cloud Shell**: A browser-based shell with Azure CLI pre-installed
- **Docker Container**: Run using `docker run -it mcr.microsoft.com/azure-cli`  ([Azure CLI onboarding cheat sheet | Microsoft Learn](https://learn.microsoft.com/th-th/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com), [Begin your Azure CLI journey - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/get-started-tutorial-0-before-you-begin?utm_source=chatgpt.com), [Azure CLI onboarding cheat sheet - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## 🔐 Signing In

- **Command**: `az login`
- **Authentication Methods**: Azure username and password, service principal, managed identity, or Web Account Manager (WAM)  ([Get started with Azure Command-Line Interface (CLI) - Learn Microsoft](https://learn.microsoft.com/it-it/cli/azure/get-started-with-azure-cli?view=azure-cli-latest&utm_source=chatgpt.com), [Azure CLI onboarding cheat sheet - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## 🧾 Command Syntax

Azure CLI commands follow this structure:

`az <command group> <command subgroup> <command> --parameters` ([Azure CLI onboarding cheat sheet - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

**Example**:

`az storage account create --name myStorageAccount` ([Azure CLI onboarding cheat sheet | Microsoft Learn](https://learn.microsoft.com/th-th/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## 📚 Learning and Assistance

- **Interactive Mode**: Launch with `az interactive` for inline help and command suggestions.
- **Command Help**: Use `--help` with any command to get detailed information.
- **AI Assistance**: Use `az find "keyword"` to get command suggestions based on your input.  ([Get started with Azure Command-Line Interface (CLI) - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?utm_source=chatgpt.com), [Azure CLI onboarding cheat sheet | Microsoft Learn](https://learn.microsoft.com/th-th/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## ⚙️ Configuration Tips

- **Set Defaults**: Use `az config set defaults.location=<location> defaults.group=<resource-group>` to set default values.
- **Change Subscription**: Switch subscriptions with `az account set --subscription "Subscription Name"`.
- **Query Output**: Extract specific data using `--query` with JMESPath syntax.
- **Output Formats**: Format command output using `--output` with options like `table`, `json`, or `yaml`.  ([Azure CLI onboarding cheat sheet - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com), [Azure CLI onboarding cheat sheet | Microsoft Learn](https://learn.microsoft.com/th-th/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## 🧩 Extensions

- **Purpose**: Add extra functionality not included in the core CLI.
- **Installation**: Automatically prompted on first use or manually install using `az extension add --name <extension-name>`.
- **List Available Extensions**: Run `az extension list-available --output table`.  ([Begin your Azure CLI journey - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/get-started-tutorial-0-before-you-begin?utm_source=chatgpt.com), [Azure CLI onboarding cheat sheet | Microsoft Learn](https://learn.microsoft.com/th-th/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com))

---

## 🧪 Practice and Tutorials

- **Microsoft Learn Modules**: Engage with interactive tutorials to practice Azure CLI commands.
- **Sample Tasks**:
  - Manage default subscriptions
  - Create resources with randomized names
  - Set environment variables
  - Debug commands and parse JSON files
  - Delete resources efficiently  ([Azure CLI onboarding cheat sheet - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/cheat-sheet-onboarding?utm_source=chatgpt.com), [Get started with Azure Command-Line Interface (CLI) - Learn Microsoft](https://learn.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?utm_source=chatgpt.com), [Get started with Azure Command-Line Interface (CLI) - Learn Microsoft](https://learn.microsoft.com/it-it/cli/azure/get-started-with-azure-cli?view=azure-cli-latest&utm_source=chatgpt.com))

---
