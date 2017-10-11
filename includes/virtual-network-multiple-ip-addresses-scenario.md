## <a name="scenario"></a>Szenario
Ein virtuellen Computer mit einer einzigen NIC wird erstellt und mit einem virtuellen Netzwerk verbunden. Der virtuelle Computer erfordert drei verschiedene *private* -IP-Adressen und zwei *öffentlichen* IP-Adressen. Die folgenden IP-Konfigurationen werden die IP-Adressen zugewiesen:

* **"Ipconfig"-1:** weist eine *statische* private IP-Adresse und ein *statische* öffentliche IP-Adresse.
* **"Ipconfig"-2:** weist eine *statische* private IP-Adresse und ein *statische* öffentliche IP-Adresse.
* **"Ipconfig" – 3:** weist eine *statische* private IP-Adresse und keine öffentliche IP-Adresse.
  
    ![Mehrere IP-Adressen](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Die IP-Konfigurationen sind an die NIC zugeordnet, wenn die NIC erstellt wird und die NIC wird während der Erstellung des virtuellen Computers mit dem virtuellen Computer angeschlossen. Die Typen von IP-Adressen, die für das Szenario verwendet werden zur Veranschaulichung. Sie können beliebige IP-Adresse und Zuweisung von Typen zuweisen, die Sie benötigen.

> [!NOTE]
> Obwohl die Schritte in diesem Artikel werden alle IP-Konfigurationen auf eine einzelne NIC zuweist, können Sie auch mehrere IP-Konfigurationen an eine beliebige NIC auf einem virtuellen Multi-NIC-Computer zuweisen. Informationen zum Erstellen eines virtuellen Computers mit mehreren NICs, lesen Sie die [erstellen Sie einen virtuellen Computer mit mehreren NICs](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md) Artikel.