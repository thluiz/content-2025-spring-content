---
created: 2025-03-04T18:44:45 (UTC -03:00)
tags: [cmdpowershell,windows11,windowsfeatures,software,coding,development,engineering,inclusive,community]
source: https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest
author: Vigneshwaran Vijayakumar
---

# Enable Microsoft Print to PDF using CMD & PowerShell on Windows 11 - DEV Community

> ## Excerpt
> Microsoft Print to PDF is a built-in Feature on Windows 11 which allows users to save documents to...

---
[![Cover image for Enable Microsoft Print to PDF using CMD & PowerShell on Windows 11](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FEnable-Microsoft-Print-to-PDF-using-CMD-PowerShell.png)](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FEnable-Microsoft-Print-to-PDF-using-CMD-PowerShell.png)

**Microsoft Print to PDF** is a built-in Feature on Windows 11 which allows users to **save documents to PDF files** without using any **3rd-party applications**. Though, you can [Enable Microsoft Print to PDF via GUI](https://winsides.com/how-to-enable-microsoft-print-to-pdf-in-windows-11/), some users prefer to enable or disable this feature on Windows 11 using the Command Prompt, or Windows PowerShell. Using CMD or PowerShell for enabling this feature is ideal for users who want to enable it on **multiple PCs using Scripts** , **Automation** , **Remote Management** , etc. Let’s get Started.

## [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#microsoft-print-to-pdf-availability-on-various-windows-editions)Microsoft Print to PDF Availability on various Windows Editions

| **Windows Editions** | **Availability** |
| --- | --- |
| Windows Servers | Yes |
| Windows 11 Home | Yes |
| Windows 11 Professional | Yes |
| Windows 11 Education | Yes |
| Windows 11 Enterprise | Yes |
| Windows 11 Pro Education | Yes |
| [Windows 11 SE](https://learn.microsoft.com/en-us/education/windows/windows-11-se-overview) | Varies based on Configuration |
| [Windows 11 IoT Enterprise](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-iot-enterprise-ltsc) | Yes |

## [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#easy-way-to-turn-on-microsoft-print-to-pdf-on-windows-11-editions-using-the-command-prompt)Easy Way to Turn on Microsoft Print to PDF on Windows 11 Editions using the Command Prompt

We will use DISM Tool via the Command Prompt to Enable this feature on Windows 11. However, DISM require Elevated Permissions.

-   Go to the **Run Command** using the keyboard shortcut WinKey + R.
-   In the Run Command, type the command `cmd`, and press CTRL + SHIFT + ENTER.

[![Type cmd and press ctrl + shift + enter](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FType-cmd-and-press-ctrl-shift-enter.webp "Type cmd and press ctrl + shift + enter")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FType-cmd-and-press-ctrl-shift-enter.webp)  
_Type cmd and press ctrl + shift + enter_

-   The **User Account Control** will confirm and open Command Prompt with Administrative Privileges.
-   Once the Command Prompt opens, execute the following command in the terminal. `dism /Online /Enable-Feature /FeatureName:"Printing-PrintToPDFServices-Features" /NoRestart`

[![Execute the Microsoft Print to PDF Enable Command in the cmd](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FExecute-the-Microsoft-Print-to-PDF-Enable-Command-in-the-cmd.webp "Execute the Microsoft Print to PDF Enable Command in the cmd")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FExecute-the-Microsoft-Print-to-PDF-Enable-Command-in-the-cmd.webp)  
_Execute the Microsoft Print to PDF Enable Command in the cmd_

-   The DISM will enable Microsoft Print to PDF on Winows 11 via the Command Prompt. Once you get the message “ **The operation completed successfully** “. That’s it. This feature is now enabled on your Windows 11.

[![The operation completed successfully](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FThe-operation-completed-successfully-1.webp "The operation completed successfully")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FThe-operation-completed-successfully-1.webp)  
_The operation completed successfully_

> **_Note_** : If you want to disable Microsoft Print to PDF on Windows 11, kindly execute the following command in the Command Prompt. `dism /Online /Disable-Feature /FeatureName:"Printing-PrintToPDFServices-Features" /NoRestart`

### [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#decoding-microsoft-print-to-pdf-enable-cmd-command)Decoding Microsoft Print to PDF Enable CMD Command

[![Decoding Microsoft Print to PDF CMD Command](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FDecoding-Microsoft-Print-to-PDF-CMD-Command.png "Decoding Microsoft Print to PDF CMD Command")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FDecoding-Microsoft-Print-to-PDF-CMD-Command.png)  
_Decoding Microsoft Print to PDF CMD Command_

## [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#quick-way-to-enable-microsoft-print-to-pdf-using-windows-powershell)Quick Way to Enable Microsoft Print to PDF using Windows PowerShell

Here, we will check out how to enable this feature using the Windows PowerShell.

-   Open the **Run Command** using the shortcut WinKey + R.
-   Type the following command `powershell` in the Run Command, and press CTRL + SHIFT + ENTER.

[![Type powershell and press ctrl + shift + enter](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FType-powershell-and-press-ctrl-shift-enter.webp "Type powershell and press ctrl + shift + enter")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FType-powershell-and-press-ctrl-shift-enter.webp)  
_Type powershell and press ctrl + shift + enter_

-   PowerShell will open as Administrator.
-   In the PowerShell, execute the following command. This command will enable Microsoft Print to PDF via Windows PowerShell. `Enable-WindowsOptionalFeature -Online -FeatureName Printing-PrintToPDFServices-Features -NoRestart`

[![Execute the Print to PDF Enable Command in the PowerShell](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FExecute-the-Print-to-PDF-Enble-Command-in-the-PowerShell.webp "Execute the Print to PDF Enable Command in the PowerShell")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FExecute-the-Print-to-PDF-Enble-Command-in-the-PowerShell.webp)  
_Execute the Print to PDF Enable Command in the PowerShell_

-   PowerShell will enable Microsoft Print to PDF on Windows 11. Under **Restart Needed** , the value is **true** , which indicates that a Restart is essential.

[![Restart Needed after the feature is enabled](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FRestart-Needed-after-the-feature-is-enabled.webp "Restart Needed after the feature is enabled")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FRestart-Needed-after-the-feature-is-enabled.webp)  
_Restart Needed after the feature is enabled_

> **_Note_** : If you want to disable Microsoft Print to PDF on Windows 11 via Windows Powershell, kindly execute the following command in the PowerShell. `Disable-WindowsOptionalFeature -Online -FeatureName Printing-PrintToPDFServices-Features -NoRestart`

### [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#decoding-print-to-pdf-enable-command-via-windows-powershell)Decoding Print to PDF Enable Command via Windows PowerShell

[![Decoding Microsoft Print to PDF PowerShell Command](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FDecoding-Microsoft-Print-to-PDF-PowerShell-Command.png "Decoding Microsoft Print to PDF PowerShell Command")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fwinsides.com%2Fwp-content%2Fuploads%2F2025%2F02%2FDecoding-Microsoft-Print-to-PDF-PowerShell-Command.png)  
_Decoding Microsoft Print to PDF PowerShell Command_

## [](https://dev.to/winsides/enable-microsoft-print-to-pdf-using-cmd-powershell-on-windows-11-knm?context=digest#take-away)Take Away

Microsoft Print to PDF on Windows 11 is an **essential utility feature** that eliminates the need of 3rd party applications to save files as PDF on Windows 11 OS. If you have any **queries** , Kindly let us know in the **comment** section. For more interesting articles, stay tuned to **[Winsides.com](https://winsides.com/)**. **Happy Computing! Peace out!**
