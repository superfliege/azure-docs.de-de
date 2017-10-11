Der Azure-CLI-2.0 können Sie zum Erstellen und Verwalten Ihrer Azure-Ressourcen auf MacOS, Linux und Windows. In diesem Artikel werden einige der am häufigsten verwendeten Befehle zum Erstellen und Verwalten von virtuellen Computern (VMs).

In diesem Artikel ist erforderlich, die Azure-CLI-Version 2.0.4 oder höher. Führen Sie `az --version` auf die Version zu ermitteln. Wenn Sie aktualisieren möchten, finden Sie unter [Installieren von Azure-CLI 2.0](/cli/azure/install-azure-cli). Sie können auch [Cloud Shell](/azure/cloud-shell/quickstart) in Ihrem Browser.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Grundlegende Azure Resource Manager-Befehle in Azure-CLI
Ausführliche Hilfe zu bestimmten Befehlszeilenschalter und Optionen können die Optionen und online-Befehls Hilfe dazu `az <command> <subcommand> --help`.

### <a name="create-vms"></a>Erstellen virtueller Computer
| Aufgabe | Azure-CLI-Befehlen |
| --- | --- |
| Erstellen einer Ressourcengruppe | `az group create --name myResourceGroup --location eastus` |
| Erstellen Sie einen virtuellen Linux-Computer | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Erstellen eines Windows-VM | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>Verwalten von VM-Status
| Aufgabe | Azure-CLI-Befehlen |
| --- | --- |
| Starten Sie einen virtuellen Computer | `az vm start --resource-group myResourceGroup --name myVM` |
| Beenden eines virtuellen Computers | `az vm stop --resource-group myResourceGroup --name myVM` |
| Aufheben der Zuordnung eines virtuellen Computers | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Starten Sie einen virtuellen Computer neu. | `az vm restart --resource-group myResourceGroup --name myVM` |
| Stellen Sie einen virtuellen Computer erneut bereit. | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| Löschen eines virtuellen Computers | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>Abrufen von VM-Informationen
| Aufgabe | Azure-CLI-Befehlen |
| --- | --- |
| Liste virtuelle Computer | `az vm list` |
| Abrufen von Informationen zu einem virtuellen Computer | `az vm show --resource-group myResourceGroup --name myVM` |
| Abrufen der Nutzung der VM-Ressourcen | `az vm list-usage --location eastus` |
| Abrufen Sie aller verfügbare VM-Größen | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Datenträger und images
| Aufgabe | Azure-CLI-Befehlen |
| --- | --- |
| Fügen Sie einen Datenträger an einen virtuellen Computer | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| Entfernen eines Datenträgers von einem virtuellen Computer | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Ändern Sie die Größe eines Datenträgers | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Eine Momentaufnahme ein Datenträgers | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Image eines virtuellen Computers erstellen | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Erstellen Sie die virtuellen Computer aus Images | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Beispiele von der CLI-Befehlen finden Sie in der [erstellen und Verwalten von Linux-VMs mit der Azure-CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md) Lernprogramm.

