
Jeder Endpunkt hat eine *öffentlichen Port* und ein *privaten Port*:

* Der öffentliche Port wird von Azure-Lastenausgleich für eingehenden Datenverkehr an den virtuellen Computer aus dem Internet überwacht.
* Der private Port wird von der virtuellen Maschine zum Abhören von eingehenden Datenverkehr, in der Regel für eine Anwendung oder Dienst auf dem virtuellen Computer ausgeführt.

Standardwerte für die IP-Protokoll und die TCP- oder UDP-Ports für bekannte Netzwerk Protokolle bereitgestellt werden, wenn Sie Endpunkte mit Azure-Portal erstellen. Für benutzerdefinierte Endpunkte müssen Sie die richtige IP-Protokoll (TCP oder UDP) und die öffentlichen und privaten Ports angeben. Um nach dem Zufallsprinzip eingehenden Datenverkehr auf mehrere virtuelle Computer zu verteilen, müssen Sie einen Lastenausgleich Satz bestehend aus mehreren Endpunkten zu erstellen.

Nachdem Sie einen Endpunkt erstellt haben, können Sie eine Zugriffssteuerungsliste (ACL) verwenden, um Regeln zu definieren, die zulassen oder verweigern den eingehenden Datenverkehr an den öffentlichen Port des Endpunkts basierend auf dessen IP-Quelladresse. Wenn der virtuelle Computer in Azure virtual Network ist, sollten Sie netzwerksicherheitsgruppen jedoch stattdessen verwenden. Weitere Informationen finden Sie unter [zu netzwerksicherheitsgruppen](../articles/virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Konfiguration der Firewall für virtuelle Computer in Azure erfolgt automatisch für Ports im Zusammenhang mit remote-Konnektivität-Endpunkte, mit dem Azure automatisch eingerichtet. Für die Ports für alle anderen Endpunkte erfolgt keine Konfiguration automatisch an die Firewall des virtuellen Computers. Wenn Sie einen Endpunkt für den virtuellen Computer erstellen, müssen Sie sicherstellen, dass die Firewall des virtuellen Computers auch den Datenverkehr für das Protokoll und den privaten Port entspricht der Endpunktkonfiguration kann. Zum Konfigurieren der Firewall finden Sie in der Dokumentation oder der Onlinehilfe für das Betriebssystem auf dem virtuellen Computer ausgeführt wird.
>
>

## <a name="create-an-endpoint"></a>Erstellen Sie einen Endpunkt
1. Wenn Sie dies noch nicht getan haben, melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **VMs**, und klicken Sie dann auf den Namen des virtuellen Computers, den Sie konfigurieren möchten.
3. Klicken Sie auf **Endpunkte** in der **Einstellungen** Gruppe. Die **Endpunkte** Seite listet alle aktuellen Endpunkte für den virtuellen Computer. (In diesem Beispiel wird eine Windows-VM. Ein Linux-VM wird standardmäßig einen Endpunkt für SSH angezeigt.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Endpunkte](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. Klicken Sie in der Befehlsleiste oben die Endpunkt-Einträge auf **hinzufügen**.
5. Auf der **Endpunkt hinzufügen** Seite, geben Sie einen Namen für den Endpunkt in **Namen**.
6. In **Protokoll**, wählen Sie entweder **TCP** oder **UDP**.
7. In **öffentlicher Port**, geben Sie die Portnummer für den eingehenden Datenverkehr aus dem Internet. In **privater Port**, geben Sie die Nummer des Ports an, der den virtuellen Computer überwacht wird. Diese Portnummern können unterschiedlich sein. Stellen Sie sicher, dass die Firewall auf dem virtuellen Computer konfiguriert wurde, um den Datenverkehr für das Protokoll (in Schritt 6) und privaten Port zu ermöglichen.
10. Klicken Sie auf **OK**.

Wird der neue Endpunkt aufgeführt, auf die **Endpunkte** Seite.

![Erstellen des erfolgreich](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>Verwalten Sie die ACL für einen Endpunkt
Zum Definieren von Computern, die Datenverkehr gesendet werden kann, kann die ACL für einen Endpunkt auf Datenverkehr basierend auf IP-Quelladresse einschränken. Führen Sie folgende Schritte hinzufügen, ändern oder entfernen eine ACL für einen Endpunkt aus.

> [!NOTE]
> Ist der Endpunkt Teil einer Reihe mit Lastenausgleich, werden alle vorgenommenen Änderungen der ACL für einen Endpunkt auf alle Endpunkte in der Gruppe angewendet.
>
>

Wenn der virtuelle Computer in Azure virtual Network ist, empfiehlt es sich um netzwerksicherheitsgruppen anstelle von ACLs. Weitere Informationen finden Sie unter [zu netzwerksicherheitsgruppen](../articles/virtual-network/virtual-networks-nsg.md).

1. Wenn Sie bereits, melden Sie sich bei Azure-Portal geschehen.
2. Klicken Sie auf **VMs**, und klicken Sie dann auf den Namen des virtuellen Computers, den Sie konfigurieren möchten.
3. Klicken Sie auf **Endpunkte**. Wählen Sie aus der Liste den entsprechenden Endpunkt. Die ACL-Liste ist am unteren Rand der Seite.

   ![Geben Sie die ACL-details](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Mit der Zeilen in der Liste hinzufügen, löschen oder bearbeiten Regeln für eine ACL und ihre Reihenfolge ändern. Die **Remotesubnetz** Wert ist ein IP-Adressbereich für eingehenden Datenverkehr aus dem Internet, die Azure-Lastenausgleich zum Zulassen oder verweigern den Datenverkehr basierend auf dessen IP-Quelladresse verwendet. Achten Sie darauf, dass die IP-Adressbereich in CIDR-Format, auch bekannt als Präfix Adressformat angeben. Ein Beispiel ist `10.1.0.0/8`.

 ![Neue ACL-Eintrag](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Sie können Regeln nur Datenverkehr von bestimmten Computern, die für Ihre Computer auf das Internet zulassen oder Verweigern von bestimmten, bekannten Adressbereiche Datenverkehr verwenden.

Die Regeln werden in Reihenfolge mit der ersten Regel beginnt und endet mit der letzten Regel ausgewertet. Dies bedeutet, dass die Regeln vom am wenigsten restriktive zum restriktivste sortiert werden soll. Beispiele und Weitere Informationen finden Sie unter [was eine Netzwerk-Zugriffssteuerungsliste ist](../articles/virtual-network/virtual-networks-acl.md).
