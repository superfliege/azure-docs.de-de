


## <a name="using-vm-extensions"></a>Verwenden von VM-Erweiterungen
Azure VM-Erweiterungen implementieren Verhalten oder Funktionen, mit denen eine andere Programme, die auf Azure-VMs funktionieren (z. B. die **WebDeployForVSDevTest** -Erweiterung ermöglicht Visual Studio für Web Deploy-Lösungen auf Azure-VM) oder bieten die Möglichkeit für die Interaktion mit dem virtuellen Computer, die einige andere Verhalten zu unterstützen (z. B. können Sie die VM-Access-Extensions Windows PowerShell, die Azure-CLI und der REST-Clients zurücksetzen oder Ändern der RAS-Werte auf Ihre Azure-VM).

> [!IMPORTANT]
> Eine vollständige Liste der Erweiterungen, geordnet nach Funktionen, die sie unterstützen, finden Sie unter [Azure VM-Erweiterungen und Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Da jede VM-Erweiterung ein bestimmtes Feature unterstützt wird, hängt genau wie können Sie und nicht mit der Erweiterung von der Erweiterung. Aus diesem Grund Ihren virtuellen Computer ändern, stellen Sie sicher, dass Sie die Dokumentation für die VM-Erweiterung gelesen haben, die Sie verwenden möchten. Entfernen einige VM-Erweiterungen wird nicht unterstützt. andere besitzen Eigenschaften, die festgelegt werden können, die VM-Verhalten grundlegend ändern.
> 
> 

Die am häufigsten verwendeten Aufgaben definiert sind:

1. Suchen nach verfügbaren Erweiterungen
2. Aktualisieren geladener Erweiterungen
3. Hinzufügen von Erweiterungen
4. Entfernen von Erweiterungen

## <a name="find-available-extensions"></a>Suchen nach verfügbaren Erweiterungen
Suchen Sie nach Erweiterung und erweiterten Informationen verwenden:

* PowerShell
* Azure plattformübergreifende Command Line Interface (Azure-CLI)
* Dienstverwaltungs-REST-API

### <a name="azure-powershell"></a>Azure PowerShell
Einige Erweiterungen verfügen über PowerShell-Cmdlets, die gelten, die ihre Konfiguration über PowerShell vereinfachen; aber die folgenden Cmdlets gelten für alle VM-Erweiterungen.

Sie können die folgenden Cmdlets zum Abrufen von Informationen zu den verfügbaren Erweiterungen verwenden:

* Für Instanzen von Webrollen oder Workerrollen können Sie die [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) Cmdlet.
* Für Instanzen von virtuellen Computern, können Sie die [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) Cmdlet.
  
   Beispielsweise wird im folgenden Codebeispiel wird gezeigt, wie die Informationen für die **IaaSDiagnostics** Erweiterung mithilfe von PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Azure Command Line Interface (Azure-CLI)
Einige Erweiterungen verfügen über Azure-CLI-Befehle, die für sie spezifisch sind (die Docker VM-Erweiterung ist ein Beispiel), die möglicherweise die Konfiguration vereinfachen; jedoch die folgenden Befehle für alle VM-Erweiterungen ausgeführt werden.

Können Sie die **Azure Vm-Erweiterungsliste** Befehl zum Abrufen von Informationen zu den verfügbaren Erweiterungen, und verwenden Sie die **–-Json** Option, um alle verfügbaren Informationen über eine oder mehrere Erweiterungen anzuzeigen. Wenn Sie einen Erweiterungsnamen nicht verwenden, gibt der Befehl eine JSON-Beschreibung aller verfügbaren Erweiterungen zurück.

Beispielsweise wird im folgenden Codebeispiel wird gezeigt, wie die Informationen für die **IaaSDiagnostics** Erweiterung mithilfe der Azure-CLI **Azure Vm-Erweiterungsliste** Befehl und verwendet die **–-Json** Option, um vollständige Informationen zurückzugeben.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Dienstverwaltungs-REST-APIs
Sie können die folgenden REST-APIs zum Abrufen von Informationen zu den verfügbaren Erweiterungen verwenden:

* Für Instanzen von Webrollen oder Workerrollen können Sie die [verfügbare Erweiterungen auflisten](https://msdn.microsoft.com/library/dn169559.aspx) Vorgang. Um die Versionen der verfügbaren Erweiterungen auflisten möchten, können Sie [Erweiterungsversionen](https://msdn.microsoft.com/library/dn495437.aspx).
* Für Instanzen von virtuellen Computern, können Sie die [Ressourcenerweiterungen auflisten](https://msdn.microsoft.com/library/dn495441.aspx) Vorgang. Um die Versionen der verfügbaren Erweiterungen auflisten möchten, können Sie [Erweiterungsversionen](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Fügen Sie hinzu, aktualisieren und deaktiveren von Erweiterungen
Erweiterungen können hinzugefügt werden, wenn eine Instanz erstellt wird, oder sie mit einer ausgeführten Instanz hinzugefügt werden können. Erweiterungen können aktualisiert, deaktiviert oder entfernt werden. Sie können diese Aktionen mithilfe von Azure PowerShell-Cmdlets oder über die Dienstverwaltungs-REST-API-Vorgänge ausführen. Parameter sind zum Installieren und Einrichten einiger Erweiterungen erforderlich. Öffentliche und private Parameter werden für Erweiterungen unterstützt.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell-Cmdlets verwenden, ist die einfachste Möglichkeit zum Hinzufügen und Aktualisieren von Erweiterungen. Wenn Sie die Erweiterungs-Cmdlets verwenden, ist der größte Teil der Konfiguration der Erweiterung für Sie ausgeführt. In einigen Fällen müssen Sie möglicherweise eine Erweiterung programmgesteuert hinzufügen. Wenn Sie dies durchführen müssen, müssen Sie die Konfiguration der Erweiterung bereitstellen.

Sie können die folgenden Cmdlets verwenden, wissen Sie, ob eine Erweiterung die Konfiguration öffentlicher und privater Parameter erfordert:

* Für Instanzen von Webrollen oder Workerrollen können Sie die **Get-AzureServiceAvailableExtension** Cmdlet.
* Für Instanzen von virtuellen Computern, können Sie die **Get-AzureVMAvailableExtension** Cmdlet.

### <a name="service-management-rest-apis"></a>Dienstverwaltungs-REST-APIs
Wenn Sie eine Liste der verfügbaren Erweiterungen mithilfe der REST-APIs abrufen, erhalten Sie Informationen dazu, wie die Erweiterung konfiguriert werden. Die zurückgegebenen Informationen möglicherweise Parameterinformationen durch ein öffentliches Schema und ein privates Schema dargestellt angezeigt. Öffentliche Parameterwerte werden in Abfragen zu den Instanzen zurückgegeben. Private Parameterwerte werden nicht zurückgegeben.

Sie können die folgenden REST-APIs verwenden, um zu wissen, ob eine Erweiterung die Konfiguration öffentlicher und privater Parameter erfordert:

* Für Instanzen von Webrollen oder Workerrollen, die **PublicConfigurationSchema** und **PrivateConfigurationSchema** Elemente enthalten, die Informationen in der Antwort von der [verfügbare Erweiterungen auflisten](https://msdn.microsoft.com/library/dn169559.aspx) Vorgang.
* Für Instanzen von virtuellen Computern, die **PublicConfigurationSchema** und **PrivateConfigurationSchema** Elemente enthalten, die Informationen in der Antwort von der [Ressourcenerweiterungen auflisten](https://msdn.microsoft.com/library/dn495441.aspx) Vorgang.

> [!NOTE]
> Erweiterungen können auch definierte Konfigurationen mit JSON. Wenn diese Typen von Erweiterungen verwendet werden, nur die **SampleConfig** Element verwendet wird.
> 
> 

