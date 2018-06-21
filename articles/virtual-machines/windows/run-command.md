---
title: Ausführen von PowerShell-Skripts in einer Windows-VM unter Azure
description: In diesem Thema wird beschrieben, wie PowerShell-Skripts auf einem virtuellen Azure Windows-Computer mithilfe von „Befehl ausführen“ ausgeführt werden
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: a2a1c916543da07f25b2b9727e309709632afe00
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267270"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>Ausführen von PowerShell-Skripts in Ihrer Windows-VM mit „Befehl ausführen“

„Befehl ausführen“ verwendet den VM-Agent, um PowerShell-Skripts innerhalb einer Azure Windows-VM auszuführen. Diese Skripts können für die allgemeine Verwaltung von Computern oder Anwendungen sowie für die schnelle Diagnose und Korrektur von Zugriffs- und Netzwerkproblemen in VMs verwendet werden und allgemein, um die VM wieder in einen guten Zustand zu bringen.

## <a name="benefits"></a>Vorteile

Es können mehrere Optionen für den Zugriff auf Ihre virtuellen Computer verwendet werden. „Befehl ausführen“ kann Skripts mithilfe des VM-Agents remote auf Ihren virtuellen Computern ausführen. „Befehl ausführen“ kann über das Azure-Portal, die [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), das [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) oder [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) verwendet werden.

Diese Möglichkeit ist in allen Szenarien nützlich, in denen Sie ein Skript auf einem virtuellen Computer ausführen möchten, und stellt eine der wenigen Möglichkeiten zur Problembehandlung und Wartung von virtuellen Computern dar, deren RDP- oder SSH-Port aufgrund einer nicht ordnungsgemäßen Netzwerk- oder Administratorkonfiguration nicht geöffnet ist.

## <a name="restrictions"></a>Einschränkungen

Die Verwendung von „Befehl ausführen“ unterliegt den folgenden Einschränkungen:

* Die Ausgabe ist auf die letzten 4096 Bytes beschränkt
* Der Mindestzeitraum für die Ausführung eines Skripts liegt bei etwa 20 Sekunden
* Skripts werden unter Windows als „System“ ausgeführt
* Es kann zu jeder Zeit jeweils nur ein Skript ausgeführt werden
* Die Ausführung von Skripts kann nicht abgebrochen werden
* Der Maximalzeitraum beim Ausführen von Skripts ist auf 90 Minuten beschränkt, danach tritt ein Timeout auf

## <a name="run-a-command"></a>Ausführen eines Befehls

Navigieren Sie in [Azure](https://portal.azure.com) zu einer VM, und wählen Sie unter **VORGÄNGE** **Befehl ausführen** aus. Es wird eine Liste mit den für die Ausführung auf der VM verfügbaren Befehlen angezeigt.

![Ausführen-Befehlsliste](./media/run-command/run-command-list.png)

Wählen Sie einen auszuführenden Befehl aus. Einige der Befehle weisen möglicherweise optionale oder erforderliche Eingabeparameter auf. Bei diesen Befehlen werden Ihnen die Parameter in Form von Textfeldern angezeigt, für die Sie die Eingabewerte bereitstellen müssen. Für jeden Befehl können Sie das ausgeführte Skript anzeigen, indem Sie **Skript anzeigen** aufklappen. **RunPowerShellScript** unterscheidet sich insofern von den anderen Befehlen, als es Ihnen die Angabe eines eigenen, benutzerdefinierten Skripts ermöglicht.

> [!NOTE]
> Die integrierten Befehle können nicht bearbeitet werden.

Nachdem Sie den Befehl ausgewählt haben, klicken Sie auf **Ausführen**, um das Skript auszuführen. Das Skript wird ausgeführt und gibt nach dem Abschluss die Ausgabe und eventuelle Fehler im Ausgabefenster zurück. Der folgende Screenshot zeigt eine Beispielausgabe für die Ausführung des **RDPSettings**-Befehls.

![Skriptausgabe von „Befehl ausführen“](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Befehle

Diese Tabelle enthält die Liste der für virtuelle Windows-Computer verfügbaren Befehle. Der Befehl **RunPowerShellScript** kann für die Ausführung jedes beliebigen benutzerdefinierten Skripts verwendet werden.

|**Name**|**Beschreibung**|
|---|---|
|**RunPowerShellScript**|Führt ein PowerShell-Skript aus|
|**EnableRemotePS**|Konfiguriert den Computer für die Aktivierung von remote-PowerShell.|
|**EnableAdminAccount**|Überprüft, ob das lokale Administratorkonto deaktiviert ist, und aktiviert es, wenn das der Fall ist.|
|**IPConfig**| Zeigt detaillierte Informationen für die IP-Adresse, die Subnetzmaske und das Standardgateway für jeden an TCP/IP gebundenen Adapter an.|
|**RDPSettings**|Überprüft Registrierungseinstellungen und Domänen-Richtlinieneinstellungen. Schlägt Richtlinienaktionen vor, wenn der Computer Teil einer Domäne ist, oder ändert die Einstellungen in Standardwerte.|
|**ResetAccountPassword**| Setzt das Kennwort des integrierten Administratorkontos zurück.|
|**ResetRDPCert**|Entfernt das SSL-Zertifikat, das an den RDP-Listener gebunden ist, und stellt die Standardwerte für die Sicherheit des RDP-Listeners wieder her. Verwenden Sie dieses Skript, wenn irgendwelche Probleme in Verbindung mit dem Zertifikat auftreten.|
|**SetRDPPort**|Legt die standardmäßige oder vom Benutzer angegebene Portnummer für Remote Desktop-Verbindungen fest. Aktiviert die Firewallregel für eingehenden Zugriff auf den Port.|

## <a name="powershell"></a>PowerShell

Das folgende Beispiel verwendet das [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)-Cmdlet zum Ausführen eines PowerShell-Skripts in einer Azure-VM.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Einschränken des Zugriffs auf „Befehl ausführen“

Das Auflisten der ausführbaren Befehle oder das Anzeigen der Details zu einem Befehl erfordert die Berechtigung `Microsoft.Compute/locations/runCommands/read`, über die die integrierte [Leser](../../role-based-access-control/built-in-roles.md#reader)-Rolle und höhere Rollen verfügen.

Für das Ausführen eines Befehls ist die Berechtigung `Microsoft.Compute/virtualMachines/runCommand/action` erforderlich, über die die [Mitwirkender](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)-Rolle und höhere Rollen verfügen.

Um „Befehl ausführen“ zu verwenden, können Sie eine der [integrierten](../../role-based-access-control/built-in-roles.md) Rollen verwenden oder eine [benutzerdefinierte](../../role-based-access-control/custom-roles.md) Rolle erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informationen über andere Möglichkeiten zur Remoteausführung von Skripts und Befehlen finden Sie unter [Ausführen von Skripts in Ihrer Windows-VM](run-scripts-in-vm.md).