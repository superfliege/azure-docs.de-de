---
title: Beheben von Fehlern bei Azure Automation-Runbooks
description: Erfahren Sie, wie Sie Probleme mit Azure Automation-Runbooks beheben.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 41eb31ecabb20ec9eec3db13d5eda9f9cfbe6c69
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015465"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Beheben von Fehlern bei Runbooks

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Authentifizierungsfehler beim Verwenden von Azure Automation-Runbooks

### <a name="sign-in-failed"></a>Szenario: Fehler beim Anmelden beim Azure-Konto

#### <a name="issue"></a>Problem

Beim Arbeiten mit den Cmdlets `Add-AzureAccount` oder `Connect-AzureRmAccount` wird der folgende Fehler angezeigt.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn der Objektname für die Anmeldeinformationen ungültig ist oder Benutzername und Kennwort, die Sie zur Einrichtung des Automation-Anmeldeinformationsobjekts verwendet haben, nicht gültig sind.

#### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, wo der Fehler liegt:  

1. Stellen Sie sicher, dass keine Sonderzeichen (einschließlich des Zeichens **@**) im Namen des Objekts mit den Automationanmeldeinformationen enthalten sind, die Sie zum Herstellen der Verbindung mit Azure verwenden.  
2. Überprüfen Sie, ob Sie den Benutzernamen und das Kennwort aus den Azure Automation-Anmeldeinformationen in Ihrem lokalen PowerShell ISE-Editor verwenden können. Sie können überprüfen, ob Benutzername und Kennwort korrekt sind, indem Sie die folgenden Cmdlets in der PowerShell ISE ausführen:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Wenn die Authentifizierung lokal fehlschlägt, bedeutet dies, dass Sie Ihre Azure Active Directory-Anmeldeinformationen nicht richtig eingerichtet haben. Informationen zur richtigen Einrichtung des Azure Active Directory-Kontos finden Sie im Blogbeitrag [Authenticating to Azure using Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) (Authentifizieren in Azure mit Azure Active Directory).  

4. Wenn es sich um einen vorübergehenden Fehler zu handeln scheint, versuchen Sie, eine Wiederholungslogik zu Ihrer Authentifizierungsroutine hinzuzufügen, um die Authentifizierung zuverlässiger zu gestalten.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Szenario: Azure-Abonnement nicht gefunden

#### <a name="issue"></a>Problem

Beim Arbeiten mit den Cmdlets `Select-AzureSubscription` oder `Select-AzureRmSubscription` wird der folgende Fehler angezeigt:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Error

Dieser Fehler tritt auf, wenn der Name des Abonnements ungültig ist oder der Azure Active Directory-Benutzer, der versucht, die Abonnementdetails abzurufen, nicht als Administrator des Abonnements konfiguriert ist.

#### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, ob die Authentifizierung in Azure richtig erfolgt ist und ob Sie Zugriff auf das Abonnement haben, das Sie auswählen möchten:  

1. Testen Sie Ihr Skript außerhalb von Azure Automation, um sicherzustellen, dass es eigenständig verwendet werden kann.
2. Achten Sie darauf, das Cmdlet **Add-AzureAccount** auszuführen, bevor Sie das Cmdlet **Select-AzureSubscription** ausführen.  
3. Wenn diese Fehlermeldung weiterhin angezeigt wird, sollten Sie Ihren Code ändern, indem Sie den Parameter **-AzureRmContext** nach dem Parameter **Add-AzureAccount** hinzufügen und dann den Code ausführen.

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Szenario: Fehler beim Authentifizieren bei Azure, da die mehrstufige Authentifizierung aktiviert ist

#### <a name="issue"></a>Problem

Sie erhalten bei der Authentifizierung bei Azure mit Ihrem Azure-Benutzernamen und -Kennwort die folgende Fehlermeldung:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Ursache

Falls für Ihr Azure-Konto die mehrstufige Authentifizierung eingerichtet ist, können Sie für die Authentifizierung bei Azure keinen Azure Active Directory-Benutzer verwenden. Stattdessen müssen Sie ein Zertifikat oder einen Dienstprinzipal für die Authentifizierung gegenüber Azure verwenden.

#### <a name="resolution"></a>Lösung

Informationen zum Verwenden eines Zertifikats mit den klassischen Azure-Bereitstellungsmodell finden Sie unter [Managing Azure Services with the Microsoft Azure Automation Preview Service](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) (Verwalten von Azure Services mit dem Microsoft Azure Automation Preview-Dienst). Informationen zum Verwenden eines Dienstprinzipals mit Azure Resource Manager-Cmdlets finden Sie unter [Erstellen eines Dienstprinzipals mit dem Azure-Portal](../../active-directory/develop/howto-create-service-principal-portal.md) und [Authentifizieren eines Dienstprinzipals mit dem Azure Resource Manager](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="common-errors-when-working-with-runbooks"></a>Häufige Fehler beim Verwenden von Runbooks

### <a name="task-was-cancelled"></a>Szenario: Beim Runbook tritt folgender Fehler auf: Eine Aufgabe wurde abgebrochen.

#### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```
Exception: A task was canceled.
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann durch veraltete Azure-Module verursacht werden.

#### <a name="resolution"></a>Lösung

Dieser Fehler kann behoben werden, indem Sie Ihre Azure-Module auf die neueste Version aktualisieren.

Klicken Sie in Ihrem Automation-Konto auf **Module** und anschließend auf **Azure-Module aktualisieren**. Das Update dauert ungefähr 15 Minuten. Führen Sie anschließend das Runbook, bei dem der Fehler aufgetreten ist, erneut aus. Weitere Informationen zum Aktualisieren Ihrer Module finden Sie unter [Aktualisieren von Azure-Modulen in Azure Automation](../automation-update-azure-modules.md).

### <a name="child-runbook-auth-failure"></a>Szenario: Fehler beim untergeordneten Runbook, wenn mehrere Abonnements verwendet werden

#### <a name="issue"></a>Problem

Beim Ausführen untergeordneter Runbooks mit `Start-AzureRmRunbook` kann das untergeordnete Runbook Azure-Ressourcen nicht verwalten.

#### <a name="cause"></a>Ursache

Das untergeordnete Runbook verwendet beim Ausführen nicht den richtigen Kontext.

#### <a name="resolution"></a>Lösung

Wenn Sie mit mehreren Abonnements arbeiten, geht beim Aufrufen untergeordneter Runbooks möglicherweise der Abonnementkontext verloren. Um sicherzustellen, dass der Abonnementkontext an die untergeordneten Runbooks übergeben wird, fügen Sie den Parameter `AzureRmContext` zum Cmdlet hinzu, und übergeben Sie darin den Kontext.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Szenario: Runbookfehler aufgrund eines fehlenden Cmdlets

#### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. Das Modul, das das Cmdlet enthält, wird nicht in das Automationkonto importiert.
2. Das Modul, das das Cmdlet enthält, wird zwar importiert, ist jedoch veraltet.

#### <a name="resolution"></a>Lösung

Dieser Fehler kann behoben werden, indem Sie eine der folgenden Aufgaben ausführen:

Wenn es sich bei dem Modul um ein Azure-Modul handelt, finden Sie weitere Informationen zum Aktualisieren Ihrer Module in Ihrem Automation-Konto unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md).

Wenn es ein separates Modul ist, sollten Sie sicherstellen, dass es in Ihr Automationkonto importiert wird.

### <a name="job-attempted-3-times"></a>Szenario: Es wurde dreimal erfolglos versucht, den Runbookauftrag zu starten

#### <a name="issue"></a>Problem

Ihr Runbook schlägt mit dem folgenden Fehler fehl:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. Arbeitsspeicherlimit. Es gibt dokumentierte Beschränkungen, wie viel Speicherplatz den [Automatisierungsdienstgrenzwerten](../../azure-subscription-service-limits.md#automation-limits) einer Sandbox zugeordnet sind. Ein Auftrag schlägt also möglicherweise fehl, wenn mehr als 400 MB Arbeitsspeicher verwendet werden.

1. Netzwerksockets. Azure-Sandboxes sind auf 1.000 gleichzeitige Netzwerksockets beschränkt, wie unter [Automatisierungsgrenzwerte](../../azure-subscription-service-limits.md#automation-limits) beschrieben.

1. Modul inkompatibel. Dieser Fehler kann auftreten, wenn Modulabhängigkeiten nicht korrekt sind. Wenn dies der Fall ist, gibt Ihr Runbook in der Regel die Benachrichtigung „Befehl wurde nicht gefunden“ oder „Der Parameter kann nicht gebunden werden“.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Die empfohlenen Methoden für die Einhaltung des Arbeitsspeichergrenzwerts sind die Aufteilung der Workload zwischen mehreren Runbooks und die Verarbeitung von weniger Daten im Arbeitsspeicher, die Vermeidung des Schreibens von unnötigen Ausgaben aus den Runbooks oder die Ermittlung, wie viele Prüfpunkte Sie in Ihre PowerShell-Workflow-Runbooks schreiben. Sie können die clear-Methode verwenden, z.B. `$myVar.clear()`, um die Variable zu löschen, und dann `[GC]::Collect()` verwenden, um sofort die Garbage Collection durchzuführen. Auf diese Weise wird der Speicherbedarf Ihres Runbooks während der Laufzeit reduziert.

* Aktualisieren Sie Ihre Azure-Module, indem Sie die Schritte unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) befolgen.  

* Eine andere Lösung ist das Ausführen des Runbooks in einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Hybrid Worker unterliegen nicht den vom Arbeitsspeicher und Netzwerk vorgegebenen Grenzwerten, die für Azure-Sandboxes gelten.

### <a name="fails-deserialized-object"></a>Szenario: Runbookfehler aufgrund eines deserialisierten Objekts

#### <a name="issue"></a>Problem

Ihr Runbook schlägt mit dem folgenden Fehler fehl:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Ursache

Wenn es sich bei Ihrem Runbook um einen PowerShell-Workflow handelt, werden komplexe Objekte in einem deserialisierten Format gespeichert, um den Runbookstatus beizubehalten, sobald der Workflow angehalten wird.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit einer der folgenden drei Lösungen beheben:

1. Wenn Sie komplexe Objekte von einem Cmdlet an ein anderes übergeben, sollten Sie diese Cmdlets mit einem InlineScript umschließen.
2. Übergeben Sie den Namen oder Wert, den Sie aus dem komplexen Objekt benötigen, anstatt das gesamte Objekt zu übergeben.
3. Verwenden Sie anstelle eines PowerShell-Workflow-Runbooks ein PowerShell-Runbook.

### <a name="quota-exceeded"></a>Szenario: Fehler beim Runbookauftrag, da das zugeordnete Kontingent überschritten wurde

#### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn die Auftragsausführung das kostenlose Kontingent von 500 Minuten für Ihr Konto überschreitet. Dieses Kontingent gilt für alle Arten von Auftragsausführungsaufgaben, z. B. Testen eines Auftrags, Starten eines Auftrags im Portal, Ausführen eines Auftrags per Webhook und Planen der Ausführung eines Auftrags per Azure-Portal oder in Ihrem Rechenzentrum. Weitere Informationen zu den Preisen für Automation finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Lösung

Wenn Sie mehr als 500 Minuten an Verarbeitungszeit pro Monat nutzen möchten, müssen Sie Ihr Abonnement vom Tarif „Free“ auf den Tarif „Basic“ umstellen. Sie können das Upgrade auf den Tarif „Basic“ mit den folgenden Schritten durchführen:  

1. Melden Sie sich bei Ihrem Azure-Abonnement an.  
2. Wählen Sie das Automation-Konto aus, das Sie aktualisieren möchten.  
3. Klicken Sie auf **Einstellungen** > **Preise**.
4. Klicken Sie am unteren Seitenrand auf **Aktivieren**, um Ihr Konto mit dem Tarif **Basic** zu aktualisieren.

### <a name="cmdlet-not-recognized"></a>Szenario: Cmdlet wird beim Ausführen eines Runbooks nicht erkannt

#### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird verursacht, wenn die PowerShell-Engine das Cmdlet nicht findet, das Sie in Ihrem Runbook verwenden. Dies kann der Fall sein, weil das Modul mit dem Cmdlet unter dem Konto nicht vorhanden ist, ein Namenskonflikt mit einem Runbooknamen besteht oder das Cmdlet auch in einem anderen Modul vorhanden ist und Automation den Namen nicht auflösen kann.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:  

* Überprüfen Sie, ob Sie den Cmdlet-Namen richtig eingegeben haben.  
* Stellen Sie sicher, dass das Cmdlet unter Ihrem Automation-Konto vorhanden ist und dass keine Konflikte bestehen. Überprüfen Sie wie folgt, ob das Cmdlet vorhanden ist: Öffnen Sie ein Runbook im Bearbeitungsmodus, und suchen Sie in der Bibliothek nach dem gewünschten Cmdlet, oder führen Sie `Get-Command <CommandName>` aus. Nachdem Sie überprüft haben, dass das Cmdlet für das Konto verfügbar ist und dass keine Namenskonflikte mit anderen Cmdlets oder Runbooks bestehen, sollten Sie es der Canvas hinzufügen und sicherstellen, dass Sie in Ihrem Runbook einen gültigen Parametersatz verwenden.  
* Falls ein Namenskonflikt vorliegt und das Cmdlet in zwei unterschiedlichen Modulen verfügbar ist, können Sie dies beheben, indem Sie den vollqualifizierten Namen für das Cmdlet verwenden. Sie können beispielsweise **ModuleName\CmdletName** verwenden.  
* Wenn Sie das Runbook lokal in einer Hybrid Worker-Gruppe ausführen, stellen Sie sicher, dass das Modul und das Cmdlet auf dem Computer installiert sind, auf dem der Hybrid Worker gehostet wird.

### <a name="long-running-runbook"></a>Szenario: Ein Runbook mit langer Ausführungszeit kann nicht abgeschlossen werden

#### <a name="issue"></a>Problem

Ihr Runbook befindet sich nach 3 Stunden Laufzeit im Status **Angehalten**. Möglicherweise wird diese Fehlermeldung angezeigt:

```
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Dies ist das in Azure-Sandboxes vorgesehene Verhalten aufgrund der Überwachung der Prozesse in Azure Automation auf gleichmäßige Verteilung, weshalb ein Runbook automatisch angehalten wird, wenn es länger als drei Stunden ausgeführt wird. Der Status eines Runbooks, der das Zeitlimit für die gleichmäßigen Verteilung überschreitet, ist je nach Runbooktyp unterschiedlich. PowerShell- und Python-Runbooks werden auf einen Status **Angehalten** festgelegt. PowerShell-Workflow-Runbooks werden auf **Fehlerhaft** festgelegt.

#### <a name="cause"></a>Ursache

Das Runbook hat den von der gleichmäßigen Verteilung in einer Azure-Sandbox vorgegebenen Grenzwert von 3 Stunden überschritten.

#### <a name="resolution"></a>Lösung

Die empfohlene Lösung ist das Ausführen des Runbooks in einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md).

Hybrid Worker unterliegen nicht dem von der [gleichmäßigen Verteilung](../automation-runbook-execution.md#fair-share) in einer Azure-Sandbox vorgegebenen Grenzwert von 3 Stunden. Während Hybrid Runbook Worker nicht durch die 3-stündige Begrenzung für die gleichmäßige Verteilung begrenzt sind, sollten Runbooks, die auf Hybrid Runbook Workers laufen, weiterhin entwickelt werden, um das Neustartverhalten bei unerwarteten lokalen Infrastrukturproblemen zu unterstützen.

Eine weitere Möglichkeit ist das Optimieren des Runbooks durch die Erstellung von [untergeordneten Runbooks](../automation-child-runbooks.md). Wenn Ihr Runbook für mehrere Ressourcen eine Schleife durch die gleiche Funktion durchführt, z.B. für einen Datenbankvorgang in mehreren Datenbanken, können Sie diese Funktion in ein untergeordnetes Runbook verschieben. Jedes dieser untergeordneten Runbooks wird parallel in separaten Prozessen ausgeführt, sodass die Gesamtdauer für die Ausführung des übergeordneten Runbooks verringert wird.

Die PowerShell-Cmdlets für dieses Szenario, die das untergeordnete Runbook aktivieren, sind:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) – Mit diesem Cmdlet können Sie ein Runbook starten und Parameter an das Runbook übergeben.

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) – Mit diesem Cmdlet können Sie den Auftragsstatus für jedes untergeordnete Element überprüfen, wenn Vorgänge vorhanden sind, die nach Abschluss des untergeordneten Runbooks durchgeführt werden müssen.

### <a name="expired webhook"></a>Szenario: Status 400 (ungültige Anforderung) beim Aufrufen eines Webhooks

#### <a name="issue"></a>Problem

Beim Versuch, einen Webhook für ein Azure Automation-Runbook aufzurufen, tritt der folgende Fehler auf:

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Ursache

Der Webhook, den Sie aufrufen möchten, ist entweder deaktiviert oder abgelaufen.

#### <a name="resolution"></a>Lösung

Falls der Webhook deaktiviert ist, können Sie ihn über das Azure-Portal wieder aktivieren. Falls der Webhook abgelaufen ist, muss er gelöscht und neu erstellt werden. Das [Verlängern eines Webhooks](../automation-webhooks.md#renew-webhook) ist nur möglich, solange er noch nicht abgelaufen ist.

### <a name="429"></a>Szenario: 429: The request rate is currently too large. Please try again (Die Anforderungsrate ist derzeit zu hoch. Versuchen Sie es noch mal.)

#### <a name="issue"></a>Problem

Beim Ausführen des Cmdlets `Get-AzureRmAutomationJobOutput` wird folgende Fehlermeldung angezeigt:

```
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann beim Abrufen der Auftragsausgabe eines Runbooks auftreten, das über zahlreiche [ausführliche Datenströme](../automation-runbook-output-and-messages.md#verbose-stream) verfügt.

#### <a name="resolution"></a>Lösung

Es gibt zwei Möglichkeiten, diesen Fehler zu beheben:

* Bearbeiten Sie das Runbook, und verringern Sie die Anzahl ausgegebener Auftragsdatenströme.
* Verringern Sie die Anzahl von Datenströmen, die beim Ausführen des Cmdlets abgerufen werden. Hierzu können Sie den Parameter `-Stream Output` für das Cmdlet `Get-AzureRmAutomationJobOutput` angeben, um ausschließlich Ausgabedatenströme abzurufen. 

## <a name="common-errors-when-importing-modules"></a>Häufige Fehler beim Importieren von Modulen

### <a name="module-fails-to-import"></a>Szenario: Fehler beim Modulimport oder Ausführung von Cmdlets nach Import nicht möglich

#### <a name="issue"></a>Problem

Ein Modul kann nicht importiert werden, oder sein Import ist erfolgreich, aber es werden keine Cmdlets extrahiert.

#### <a name="cause"></a>Ursache

Es folgen häufige Ursachen, warum ein Modul nicht erfolgreich in Azure Automation importiert wird:

* Die Struktur stimmt nicht mit der Struktur überein, die für Automation erforderlich ist.
* Das Modul ist von einem anderen Modul abhängig, das für Ihr Automation-Konto nicht bereitgestellt wurde.
* Für das Modul fehlen die Abhängigkeiten im Ordner.
* Das Cmdlet `New-AzureRmAutomationModule` wird zum Hochladen des Moduls verwendet, und Sie haben nicht den vollständigen Speicherpfad angegeben oder das Modul nicht mit einer öffentlich zugänglichen URL geladen.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Stellen Sie sicher, dass das Modul das folgende Format beachtet: Modulname.Zip **->** Modulname oder Versionsnummer **->** (Modulname.psm1, Modulname.psd1)
* Öffnen Sie die PSD1-Datei, und prüfen Sie, ob für das Modul Abhängigkeiten bestehen. Wenn ja, laden Sie diese Module in das Automation-Konto hoch.
* Stellen Sie sicher, dass alle referenzierten DLLs im Modulordner vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.