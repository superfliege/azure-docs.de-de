

VM-Erweiterungen können Ihnen helfen:

* Ändern Sie Sicherheit und Identität Funktionen, z. B. beim Zurücksetzen von Kontowerten und Verwendung von Antischadsoftware
* Starten, beenden oder Konfigurieren von Überwachung und Diagnose
* Zurücksetzen oder Installieren von Konnektivitätsfunktionen, z. B. RDP und SSH
* Diagnose, Überwachung und Verwaltung Ihrer virtuellen Computer

Es gibt viele weitere Funktionen aus. Regelmäßig werden neue VM-Erweiterungsfunktionen veröffentlicht. Dieser Artikel beschreibt Azure VM-Agents für Windows und Linux und wie sie die Funktionalität von VM-Erweiterungen unterstützen. Eine Auflistung von VM-Erweiterungen nach Kategorie "Funktion" finden Sie [Azure VM-Erweiterungen und Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Azure VM-Agents für Windows und Linux
Der Azure-virtuelle Computer-Agent (VM-Agent) ist ein geschützter, Lightweight-Prozess, der installiert, konfiguriert und VM-Erweiterungen für Instanzen von Azure Virtual Machines entfernt. Der VM-Agent dient als die sicherer lokaler Steuerungsdienst für Ihren Azure-virtuellen Computer. Die Erweiterungen, die der Agent lädt bieten bestimmte Funktionen, um Ihre Produktivität beim Verwenden der Instanz zu erhöhen.

Azure-VM-Agents vorhanden sind, eine für virtuelle Windows-Computer und eine für virtuelle Linux-Computer.

Wenn Sie die Instanz eines virtuellen Computers auf einen oder mehrere VM-Erweiterungen verwenden möchten, muss die Instanz einen installierten VM-Agent verfügen. Image des virtuellen Computers erstellt, die mithilfe von Azure-Portal und ein Bild aus der **Marketplace** installiert automatisch bei der Erstellung einer VM-Agent. Wenn die Instanz eines virtuellen Computers nicht über ein VM-Agent verfügt, können Sie der VM-Agent installieren, nachdem die virtuelle Computerinstanz erstellt wurde. Oder Sie können den Agent installieren, in eine benutzerdefinierte VM-Image, das Sie dann hochladen.

> [!IMPORTANT]
> Diese VM-Agents sind sehr ressourcenschonende, Dienste, die sichere Verwaltung der virtuellen Computerinstanzen zu aktivieren. Es gibt möglicherweise Fälle, in denen Sie nicht der VM-Agent sollen. Wenn dies der Fall ist, achten Sie darauf, dass Sie virtuelle Computer erstellen, die nicht der VM-Agent über die Azure-CLI oder PowerShell installiert haben. Obwohl der VM-Agent physisch entfernt werden kann, ist das Verhalten des VM-Erweiterungen für die Instanz nicht definiert. Daher ist das Entfernen der Installation eines VM-Agents nicht unterstützt.
>

Der VM-Agent ist in den folgenden Situationen aktiviert:

* Beim Erstellen einer Instanz eines virtuellen Computers mithilfe von Azure-Portal, und wählen ein Image aus dem **Marketplace**,
* Beim Erstellen einer Instanz eines virtuellen Computers mithilfe der [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) oder [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx) Cmdlet. Sie können einen virtuellen Computer ohne ein VM-Agent erstellen, durch Hinzufügen der **– DisableGuestAgent** Parameter an die [Add-AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) -Cmdlet

* Wenn Sie manuell herunterladen und Installieren der VM-Agent auf eine vorhandene VM-Instanz, und legen Sie die **ProvisionGuestAgent** Wert **"true"**. Verwenden Sie dieses Verfahren für Windows und Linux-Agents mithilfe eines PowerShell-Befehls oder einer REST-aufrufen. (Wenn Sie nicht festlegen, die **ProvisionGuestAgent** Wert nach dem manuellen Installieren des VM-Agents, das Hinzufügen der VM-Agent nicht ordnungsgemäß erkannt wird.) Im folgenden Codebeispiel wird veranschaulicht, wie Sie diesen Vorgang mit PowerShell, in dem die `$svc` und `$name` Argumente bereits ermittelt wurde:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* Beim Erstellen eines VM-Images, schließt die Installation eines VM-Agents. Sobald das Abbild mit dem VM-Agent vorhanden ist, können Sie das Abbild in Azure hochladen. Für eine Windows-VM herunterladen der [Windows-VM-Agent-MSI-Datei](http://go.microsoft.com/fwlink/?LinkID=394789) und der VM-Agent installieren. Installieren Sie den VM-Agent für eine Linux-VM aus dem GitHub-Repository, die am <https://github.com/Azure/WALinuxAgent>. Weitere Informationen zum Installieren des VM-Agents unter Linux finden Sie unter der [Azure Linux VM-Agent-Benutzerhandbuch](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> In PaaS heißt der VM-Agent heißt **WindowsAzureGuestAgent**, und ist immer auf Web- und Workerrollen-VMs verfügbar. (Weitere Informationen finden Sie unter [Azure-Rollenarchitektur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).) Der VM-Agent für Rollen-VMs können jetzt Erweiterungen zum Cloud-Dienst-VMs auf die gleiche Weise hinzufügen, die sie für dauerhafte virtuelle Computer ausführt. Der größte Unterschied zwischen VM-Erweiterungen für Rollen-VMs und persistenten VMs ist, wenn die VM-Erweiterungen hinzugefügt werden. Mit der Rolle "VMs" werden Erweiterungen zuerst zum Cloud-Dienst aus, und klicken Sie dann zu den Bereitstellungen innerhalb dieses Cloud-Diensts hinzugefügt.
>
> Verwenden der [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) Cmdlet, um die Liste aller verfügbaren VM Erweiterungen für Rollen.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>Suchen, hinzufügen, aktualisieren und Entfernen von VM-Erweiterungen
Weitere Informationen zu diesen Aufgaben finden Sie unter [hinzufügen, suchen, aktualisieren und Entfernen der Azure VM-Erweiterungen](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
