---
title: Behandeln von Problemen mit der Azure Automation-Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)
description: Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c8afa671a323e37a99be8b5a43d0a4823fe1877a
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58800875"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Behandeln von Problemen mit Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)

Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Häufige Fehler beim Verwenden der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)

### <a name="unsupported-characters"></a>Szenario: Eine Konfiguration mit speziellen Zeichen kann nicht aus dem Portal gelöscht werden

#### <a name="issue"></a>Problem

Beim Versuch, eine DSC-Konfiguration aus dem Portal zu löschen, wird der folgende Fehler angezeigt:

```error
An error occured while deleteing the DSC configuration '<name>'.  Error-details: The arguement configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Ursache

Der Fehler ist ein vorübergehendes Problem, das gelöst werden soll.

#### <a name="resolution"></a>Lösung

* Verwenden Sie das Azure-Cmdlet „Remove-AzAutomationDscConfiguration“, um die Konfiguration zu löschen.
* Die Dokumentation für dieses Cmdlet wurde noch nicht aktualisiert.  Nutzen Sie bis dahin bitte die Dokumentation für das AzureRM-Modul.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-not-found"></a>Szenario: Knoten hat den Fehlerstatus „Nicht gefunden“

#### <a name="issue"></a>Problem

Der Knoten hat den Status **Fehler** mit der folgenden Meldung zurückgegeben:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt normalerweise auf, wenn der Knoten einem Konfigurationsnamen (z.B. ABC) anstatt einem Knotenkonfigurationsnamen (z.B. ABC.WebServer) zugewiesen ist.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie den Knoten mit dem „Knotenkonfigurationsnamen“ und nicht mit dem „Konfigurationsnamen“ zuweisen.
* Einem Knoten können Sie über das Azure-Portal oder mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuweisen.

  * Um über das Azure-Portal einem Knoten eine Knotenkonfiguration zuzuweisen, öffnen Sie die Seite **DSC-Knoten**, wählen Sie dann einen Knoten aus, und klicken Sie auf die Schaltfläche **Knotenkonfiguration zuweisen**.  
  * Um einem Knoten mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuzuweisen, verwenden Sie das Cmdlet **Set-AzureRmAutomationDscNode**.

### <a name="no-mof-files"></a>Szenario: Bei der Kompilierung einer Konfiguration wurden keine Knotenkonfigurationen (MOF-Dateien) erstellt

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wird mit folgendem Fehler abgebrochen:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Ursache

Wenn der Ausdruck nach dem Schlüsselwort **Node** in der DSC-Konfiguration mit `$null` ausgewertet wird, werden keine Knotenkonfigurationen erstellt.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Stellen Sie sicher, dass der Ausdruck neben dem Schlüsselwort **Node** in der Konfigurationsdefinition nicht mit „$null“ ausgewertet wird.
* Wenn Sie bei der Kompilierung der Konfiguration ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte übergeben, die für die Konfiguration aus [ConfigurationData](../automation-dsc-compile.md#configurationdata)erforderlich sind.

### <a name="dsc-in-progress"></a>Szenario: Der DSC-Knotenbericht bleibt mit dem Status „In Bearbeitung“ hängen

#### <a name="issue"></a>Problem

Der DSC-Agent gibt Folgendes aus:

```error
No instance found with given property values
```

#### <a name="cause"></a>Ursache

Sie haben Ihre WMF-Version aktualisiert und WMI beschädigt.

#### <a name="resolution"></a>Lösung

Befolgen Sie zum Beheben des Problems die Anweisungen im Artikel [Bekannte Probleme und Einschränkungen bei DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc).

### <a name="issue-using-credential"></a>Szenario: In einer DSC-Konfiguration können keine Anmeldeinformationen verwendet werden

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wurde mit folgendem Fehler abgebrochen:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Ursache

Sie haben Anmeldeinformationen in einer Konfiguration verwendet, aber keine ordnungsgemäßen **ConfigurationData** angegeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf TRUE festgelegt wird.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie die ordnungsgemäßen **ConfigurationData** übergeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf „true“ festgelegt wird. Weitere Informationen finden Sie unter [Assets in Azure Automation DSC](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
