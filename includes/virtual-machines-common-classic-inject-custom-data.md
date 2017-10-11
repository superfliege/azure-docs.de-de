


In diesem Thema wird beschrieben, wie Sie:

* Fügen Sie Daten in virtuellen Azure-Computer (VM), wenn es bereitgestellt wird.
* Für Windows und Linux abzurufen.
* Verwenden Sie spezielle Tools verfügbar auf einige Systeme erkennen und lösen benutzerdefinierte Daten automatisch.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie benutzerdefinierte Daten können mithilfe eines virtuellen Computers mit der Azure-Dienstverwaltungs-API erstellt eingegeben werden. Gewusst wie: Verwenden Sie die Azure-Ressourcen-API finden Sie unter [die Beispielvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Räumen benutzerdefinierte Daten in Ihrem virtuellen Azure-Computer
Dieses Feature ist zurzeit nur in unterstützt die [Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-xplat-cli). Im folgenden erstellen wir eine `custom-data.txt` , unsere Daten enthält, und fügen Sie, dass in dem virtuellen Computer während der Bereitstellung. Auch wenn Sie eine der Optionen für verwenden können, können die `azure vm create` Befehl das folgende Beispiel zeigt ein sehr einfaches Ansatz besteht darin:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Verwenden von benutzerdefinierten Daten auf dem virtuellen Computer
* Wenn Ihre Azure-VM ist ein Windows-basierten virtuellen Maschine, die benutzerdefinierte Datendatei gespeichert ist `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Obwohl es base64-codierte aus dem lokalen Computer mit dem neuen virtuellen Computer übertragen wurde, kann sofort es wird automatisch decodiert und geöffnet oder verwendet werden.
  
  > [!NOTE]
  > Wenn die Datei vorhanden ist, wird sie überschrieben. Die Sicherheit für das Verzeichnis festgelegt ist, um **System Vollzugriff** und **Administratoren auf Vollzugriff**.
  > 
  > 
* Wenn Ihre Azure-VM einen virtuellen Linux-basierte Computer ist, wird die benutzerdefinierte Datendatei in einem der folgenden Orte abhängig von Ihrer Distribution befinden. Die Daten können base64-codierte sein, daher Sie möglicherweise die Daten zuerst zu decodieren müssen:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Cloud-Init in Azure
Wenn Ihre Azure-VM aus einem Image Ubuntu oder CoreOS ist, können Sie die CustomData-Funktion verwenden, Cloud-Init-cloudkonfiguration an. Oder ist Ihrer benutzerdefinierten Datendatei ein Skript, dann Cloud Init kann einfach ausgeführt wird.

### <a name="ubuntu-cloud-images"></a>Ubuntu-Cloud-Images
In den meisten Azure Linux-Images, bearbeiten Sie "/ etc/waagent.conf" So konfigurieren die temporäre Ressourcendatenträger und Auslagerungsdatei. Finden Sie unter [Azure Linux-Agent-Benutzerhandbuch](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) für Weitere Informationen.

Auf den Ubuntu-Cloud-Images, Sie jedoch müssen verwenden Cloud Init so konfigurieren Sie die Datenträger mit der Ressource (d. h. das "flüchtig" Datenträger), und tauschen Sie die Partition. Finden Sie im Wiki Ubuntu detaillierte Informationen auf die folgende Seite: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Nächste Schritte: mit Cloud-Initialisierung
Weitere Informationen finden Sie unter der [Cloud-Init-Dokumentation für Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Hinzufügen der Rolle Service Management REST-API-Referenz](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-xplat-cli)

