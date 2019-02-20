---
title: Erstellen eines Azure Automation-Integrationsmoduls
description: Dieses Tutorial führt Sie durch die Erstellung, das Testen und eine Beispielverwendung der Integrationsmodule in Azure Automation.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9122cf5cc908d578d8b781c6fdc49d7b04b0ab58
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990341"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation-Integrationsmodule

PowerShell ist die grundlegende Technologie hinter Azure Automation. Da Azure Automation auf PowerShell basiert, sind PowerShell-Module für die Erweiterbarkeit von Azure Automation von zentraler Bedeutung. Dieser Artikel enthält spezifische Informationen zur Verwendung von PowerShell-Modulen in Azure Automation (sogenannte Integrationsmodule). Außerdem werden empfohlene Methoden für die Erstellung eigener PowerShell-Module vorgestellt, die als Integrationsmodule in Azure Automation verwendet werden können. 

## <a name="what-is-a-powershell-module"></a>Was ist ein PowerShell-Modul?

Ein PowerShell-Modul ist eine Gruppe von PowerShell-Cmdlets wie **Get-Date** oder **Copy-Item**, die an folgenden Orten verwendet werden können:

* In der PowerShell-Konsole
* In Skripts
* In Workflows
* In Runbooks
* In DSC-Ressourcen

Die gesamten Funktionen von PowerShell werden über Cmdlets und DSC-Ressourcen verfügbar gemacht. Allen Cmdlets/DSC-Ressourcen liegt ein PowerShell-Modul zugrunde, von denen viele bereits in PowerShell integriert sind. So ist beispielsweise das Cmdlet **Get-Date** Teil des PowerShell-Moduls `Microsoft.PowerShell.Utility`, das Cmdlet **Copy-Item** ist Teil des PowerShell-Moduls `Microsoft.PowerShell.Management`, und die DSC-Ressource „Package“ ist Teil des PowerShell-Moduls „PSDesiredStateConfiguration“. Diese Module sind in PowerShell integriert. Andere PowerShell-Module sind dagegen nicht in PowerShell enthalten. Diese Module werden zusammen mit Erst- oder Drittanbieterprodukten oder beispielsweise über den PowerShell-Katalog bereitgestellt. Die Module sind praktisch, da sie komplexe Aufgaben über gekapselte Funktionen vereinfachen.  Erfahren Sie mehr über [PowerShell-Module auf MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Was ist ein Azure Automation-Integrationsmodul?

Ein Integrationsmodul unterscheidet sich nicht von einem PowerShell-Modul. Es ist lediglich ein PowerShell-Modul, das optional eine zusätzliche Datei enthält. Dies ist eine Metadatendatei, die einen Azure Automation-Verbindungstyp festlegt, der mit den Cmdlets des Moduls in Runbooks verwendet werden soll. Diese PowerShell-Module können in Azure Automation importiert werden, damit ihre Cmdlets in Runbooks und ihre DSC-Ressourcen in DSC-Konfigurationen verwendet werden können. Azure Automation speichert diese Module im Hintergrund und lädt sie während der Ausführung der Runbook- und DSC-Kompilierungsaufträge in die Azure Automation-Sandboxes, wo Runbooks ausgeführt und DSC-Konfigurationen kompiliert werden. DSC-Ressourcen in Modulen werden ebenfalls automatisch auf dem Automation DSC-Pullserver gespeichert. Sie können von Computern gepullt werden, wenn sie DSC-Konfigurationen anwenden.  

Im Lieferumfang von Azure Automation sind bereits einige Azure PowerShell-Module enthalten. Sie können aber auch PowerShell-Module für die Integration in beliebige Systeme, Dienste oder Tools importieren.

> [!NOTE]
> Bestimmte Module werden im Automation-Dienst als **globale Module** bereitgestellt. Diese globalen Module stehen Ihnen zur Verfügung, wenn Sie ein Automation-Konto erstellen. Sie werden von Zeit zu Zeit aktualisiert, und die Pushübertragung in Ihr Automation-Konto erfolgt automatisch. Falls Sie die automatische Aktualisierung nicht wünschen, können Sie dasselbe Modul auch selbst importieren. Dieser Vorgang hat dann Vorrang vor der globalen Version des Moduls, das wir über den Dienst bereitstellen.

Das Format, in dem Sie ein Integrationsmodulpaket importieren, ist eine komprimierte Datei mit demselben Namen wie das Modul und der Erweiterung „.zip“. Sie enthält das Windows PowerShell-Modul sowie unterstützende Dateien, einschließlich einer Manifestdatei (.psd1), sofern diese im Modul enthalten ist.

Wenn das Modul einen Azure Automation-Verbindungstyp enthalten soll, muss auch eine Datei mit dem Namen „`<ModuleName>-Automation.json`“ enthalten sein, mit der die Eigenschaften des Verbindungstyps angegeben werden. Hierbei handelt es sich um eine JSON-Datei, die sich im Modulordner Ihrer komprimierten ZIP-Datei befindet. Diese Datei enthält die Felder einer **Verbindung**, die für die Verbindungsherstellung mit dem System oder Dienst erforderlich ist, das bzw. den das Modul darstellt. Durch diese Konfiguration wird ein Verbindungstyp in Azure Automation erstellt. Mithilfe dieser Datei können Sie für den Verbindungstyp des Moduls die Feldnamen und -typen festlegen und angeben, ob die Felder verschlüsselt und/oder optional sein sollen. Das folgende Beispiel ist eine Vorlage im JSON-Dateiformat:

```json
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Wenn Sie Service Management Automation bereitgestellt und Integrationsmodulpakete für Ihre Automation-Runbooks erstellt haben, sollte Ihnen diese Vorlage bekannt sein.

## <a name="authoring-best-practices"></a>Bewährte Methoden für die Erstellung

Bei Integrationsmodulen handelt es sich zwar um PowerShell-Module, trotzdem gibt es mehrere Punkte, die Sie beim Erstellen eines PowerShell-Moduls für Azure Automation beachten sollten. Einige Empfehlungen tragen dazu bei, dass Ihre Module im PowerShell-Workflow reibungslos funktionieren.

1. Fügen Sie eine Zusammenfassung, eine Beschreibung und einen Hilfe-URI für jedes Cmdlet im Modul ein. In PowerShell können Sie mit dem Cmdlet **Get-Help** bestimmte Hilfeinformationen für Cmdlets definieren, um den Benutzern bei der Verwendung der Cmdlets zu helfen. So können Sie z. B. eine Zusammenfassung und einen Hilfe-URI für ein in einer PSM1-Datei erstelltes PowerShell-Modul definieren. 
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='https://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```

   Durch die Angabe dieser Informationen kann diese Hilfe über das Cmdlet **Get-Help** in der PowerShell-Konsole angezeigt werden. Die Informationen werden darüber hinaus auch in Azure Automation verfügbar gemacht.  Ein Beispiel hierfür ist das Einfügen von Aktivitäten beim Erstellen von Runbooks. Durch Klicken auf „Detaillierte Hilfe anzeigen“ wird der URI auf einer anderen Registerkarte des Webbrowsers geöffnet, den Sie für den Zugriff auf Azure Automation verwenden.

   ![Hilfe zu Integrationsmodulen](media/automation-integration-modules/automation-integration-module-activitydesc.png)

2. Wenn das Modul für ein Remotesystem ausgeführt wird, gilt Folgendes:

   1. Es sollte eine Integrationsmodul-Metadatendatei enthalten, die die für die Verbindung mit dem Remotesystem benötigten Informationen enthält, d.h. den Verbindungstyp.
   2. Jedes Cmdlet im Modul sollte ein Verbindungsobjekt (eine Instanz dieses Verbindungstyps) als Parameter laden können.  

    Cmdlets im Modul können in Azure Automation einfacher verwendet werden, wenn das Übergeben eines Objekts mit den Feldern des Verbindungstyps als Parameter an das Cmdlet zugelassen ist. Dadurch können Benutzer die Parameter des Verbindungsobjekts bei jedem Aufruf eines Cmdlets den entsprechenden Parametern des Cmdlets zuordnen.

    Runbook-Beispiel wird ein Twilio-Verbindungsobjekt namens „CorpTwilio“ für den Zugriff auf Twilio verwendet, und alle Telefonnummern im Konto werden zurückgegeben.  Beachten Sie, wie die Felder der Verbindung den Parametern des Cmdlets zugeordnet werden:

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Eine einfachere und bessere Herangehensweise für dieses Verhalten besteht jedoch darin, das Verbindungsobjekt direkt an das Cmdlet zu übergeben:

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```

    Sie können das Verhalten aus dem vorherigen Beispiel für Ihre Cmdlets aktivieren, indem Sie zulassen, dass ein Verbindungsobjekt direkt als Parameter akzeptiert wird (nicht nur Verbindungsfelder für Parameter). In der Regel empfiehlt sich jeweils ein Parametersatz, damit ein Benutzer, der Azure Automation nicht benutzt, Ihre Cmdlets aufrufen kann, ohne eine Hashtabelle erstellen zu müssen, die als Verbindungsobjekt dient. Der Parametersatz **SpecifyConnectionFields** wird verwendet, um die Verbindungsfeldeigenschaften einzeln zu übergeben. **UseConnectionObject** können Sie die Verbindung direkt übergeben. Wie Sie sehen, ermöglicht das Cmdlet „Send-TwilioSMS“ im [Twilio-PowerShell-Modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) beides:

    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='https://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```

3. Definieren Sie einen Ausgabetyp für alle Cmdlets im Modul. Wenn Sie einen Ausgabetyp für ein Cmdlet definieren, können Sie bei der Erstellung mithilfe von IntelliSense die Ausgabeeigenschaften des Cmdlets festlegen, die während der Erstellung verwendet werden sollen. Dies ist bei der grafischen Erstellung von Automation-Runbooks hilfreich, da Kenntnisse zum Zeitpunkt des Entwurfs von zentraler Bedeutung sind, um eine hohe Benutzerfreundlichkeit Ihres Moduls zu gewährleisten.

   ![Ausgabetyp für grafische Runbooks](media/automation-integration-modules/runbook-graphical-module-output-type.png)

   Dieses Verhalten ähnelt der Textvervollständigung der Ausgabe eines Cmdlets in der PowerShell ISE, ohne dass diese ausgeführt werden muss.

   ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)

4. Cmdlets im Modul sollten keine komplexen Objekttypen für Parameter verwenden. Der PowerShell-Workflow unterscheidet sich von PowerShell darin, dass er komplexe Typen in deserialisierter Form speichert. Primitive Typen bleiben primitiv. Komplexe Typen werden dagegen in die deserialisierten Versionen konvertiert, die im Wesentlichen Eigenschaftenbehälter sind. Wenn Sie z.B. das Cmdlet **Get-Process** in einem Runbook (oder auch einem PowerShell-Workflow) verwenden, wird ein Objekt des Typs [Deserialized.System.Diagnostic.Process] zurückgegeben, nicht der erwartete Typ [System.Diagnostic.Process]. Dieser Typ verfügt über die gleichen Eigenschaften wie der nicht deserialisierte Typ, aber keine der Methoden. Wenn Sie versuchen, diesen Wert als Parameter an ein Cmdlet zu übergeben, und das Cmdlet einen [System.Diagnostic.Process]-Wert für diesen Parameter erwartet, erhalten Sie folgende Fehlermeldung: *Die Argumenttransformation für den Parameter „Prozess“ kann nicht verarbeitet werden. Fehler „Der Wert ‚System.Diagnostics.Process (CcmExec)‘ des Typs ‚Deserialized.System.Diagnostics.Process‘ kann nicht in Typ ‚System.Diagnostics.Process‘ konvertiert werden“.*   Das liegt daran, dass ein Typenkonflikt zwischen dem erwarteten Typ [System.Diagnostic.Process] und dem angegebenen Typ [Deserialized.System.Diagnostic.Process] vorliegt. Dieses Problem kann vermieden werden, wenn Sie sicherstellen, dass die Cmdlets Ihres Moduls für Parameter keine komplexen Typen akzeptieren. Die im Folgenden dargestellte Methode ist falsch:

    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ```

    Die korrekte Methode besteht darin, einen primitiven Typ zu verwenden, der intern vom Cmdlet verwendet wird, um das komplexe Objekt abzurufen und zu verwenden. Da Cmdlets im Kontext von PowerShell und nicht des PowerShell-Workflows ausgeführt werden, wird „$process“ im Cmdlet zum korrekten [System.Diagnostic.Process]-Typ.  

    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```

   Verbindungsobjekte in Runbooks sind Hashtabellen (also ein komplexer Typ). Dennoch können diese Hashtabellen problemlos und ohne Umwandlungsausnahme für den Parameter „-Connection“ übergeben werden. Technisch gesehen können einige PowerShell-Typen von der serialisierten Form in die deserialisierte Form umgewandelt und somit in Cmdlets für Parameter, die den nicht deserialisierten Typ akzeptieren, übergeben werden. Dazu zählen auch Hashtabellen. Es ist möglich, dass die vom Autor des Moduls definierten Typen ebenfalls so implementiert werden, dass eine korrekte Deserialisierung möglich ist. Allerdings müssen dabei einige Nachteile berücksichtigt werden. Der Typ benötigt einen Standardkonstruktor, all seine Eigenschaften müssen öffentlich sein, und er muss über einen PSTypeConverter verfügen. Bei bereits definierten Typen, die nicht im Besitz des Modulautors sind, besteht keine Möglichkeit, diese entsprechend anzupassen. Aus diesem Grund wird empfohlen, komplexe Typen für Parameter komplett zu vermeiden. Tipp für die Runbook-Erstellung: Wenn Ihre Cmdlets einen Parameter mit komplexem Typ benötigen, können Sie in PowerShell-Workflows das Cmdlet, das den komplexen Typ generiert, und das Cmdlet, das den komplexen Typ nutzt, in die gleiche InlineScript-Aktivität einschließen. Da der Inhalt von InlineScript als PowerShell und nicht als PowerShell-Workflow ausgeführt wird, erzeugt das Cmdlet, das den komplexen Typ erstellt, den korrekten Typ und nicht den deserialisierten komplexen Typ.

5. Legen Sie alle Cmdlets im Modul als statusfrei fest. Der PowerShell-Workflow führt jedes im Workflow aufgerufene Cmdlet in einer anderen Sitzung auf. Das heißt, dass Cmdlets in PowerShell-Workflow-Runbooks nicht funktionieren, wenn sie von einem Sitzungszustand abhängen, der von anderen Cmdlets im gleichen Modul erstellt/geändert wird.  Hier sehen Sie ein Beispiel für eine falsche Vorgehensweise:
   
    ```powershell
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```

6. Das Modul sollte vollständig in einem Xcopy-fähigen Paket enthalten sein. Azure Automation-Module werden an die Automation-Sandboxes verteilt, wenn Runbooks ausgeführt werden müssen. Die Module müssen unabhängig von dem Host funktionieren, auf dem sie ausgeführt werden. Es muss daher möglich sein, ein Modulpaket zu komprimieren, zu verschieben und nach dem Import in die PowerShell-Umgebung eines anderen Hosts ganz normal zu verwenden. Hierzu darf das Modul nicht von anderen Dateien außerhalb des Modulordners abhängig sein. Dieser Ordner wird komprimiert, wenn das Modul in Azure Automation importiert wird. Darüber hinaus darf das Modul nicht von spezifischen Registrierungseinstellungen auf einem Host abhängig sein (etwa von den Einstellungen, die bei der Installation eines Produkts festgelegt werden). Andernfalls kann das Modul in Azure Automation nicht verwendet werden.  

7. Falls Sie in Ihrem Modul auf [Azure PowerShell Az-Module](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) verweisen, achten Sie darauf, dass Sie auch auf `AzureRM` verweisen. Das `Az`-Modul kann nicht zusammen mit `AzureRM`-Modulen verwendet werden. `Az` wird in Runbooks zwar unterstützt, aber standardmäßig nicht importiert. Weitere Informationen zu `Az`-Modulen sowie wichtige Aspekte, die es zu berücksichtigen gilt, finden Sie unter [Az module support in Azure Automation](az-modules.md) (Unterstützung des Az-Moduls in Azure Automation).

## <a name="next-steps"></a>Nächste Schritte

* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)
* Weitere Informationen zum Erstellen von PowerShell-Modulen finden Sie unter [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)
