#### <a name="to-create-a-new-service"></a>Zum Erstellen eines neuen Diensts

1.  Mithilfe Ihrer Microsoft-Kontoanmeldeinformationen, melden Sie sich beim Azure-Portal unter dieser URL: <https://portal.azure.com/>. Wenn das Gerät im Government-Portal bereitstellen möchten, melden Sie sich an: <https://portal.azure.us/>

2.  Klicken Sie im Azure-Portal auf **+ neu** &gt; **Speicher** &gt; **virtuellen StorSimple-Serie**.

    ![Erstellen eines neuen Diensts](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  In der **StorSimple-Geräte-Manager** Blade, der geöffnet wird, gehen Sie folgendermaßen vor:

    1.  Geben Sie einen eindeutigen **Ressourcenname** für Ihren Dienst. Der Ressourcenname ist ein Anzeigename, der zum Identifizieren des Diensts verwendet werden kann. Der Name kann zwischen 2 und 50 Zeichen sein, die Buchstaben, Zahlen und Bindestriche sein können. Der Name muss beginnen und mit einem Buchstaben oder einer Ziffer enden.

    2.  Wählen Sie eine **Abonnement** aus der Dropdown-Liste. Das Abonnement wird mit Ihrem Abrechnungskonto verknüpft. Dieses Feld ist nicht vorhanden, wenn Sie nur ein Abonnement verfügen.

    3.  Für **Ressourcengruppe**, wählen Sie eine vorhandene oder eine neue Gruppe erstellen. Weitere Informationen finden Sie unter [Azure-Ressourcengruppen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Geben Sie einen **Speicherort** für Ihren Dienst. Finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services) für Weitere Informationen darüber, welche Dienste in der Region stehen. Im Allgemeinen wählen Sie eine **Speicherort** am nächsten gelegenen die geografische Region, in dem das Gerät bereitgestellt werden soll. Möglicherweise möchten Sie auch Folgendes berücksichtigen:

        -   Wenn Sie vorhandene arbeitsauslastungen in Azure, die Sie auch Bereitstellen mit Ihrer StorSimple-Gerät möchten haben, wird empfohlen, dass Sie dieses Rechenzentrum verwenden.

        -   Ihr StorSimple-Geräte-Manager und Azure-Speicher kann in zwei separaten Speicherorten sein. In diesem Fall müssen Sie das StorSimple-Geräte-Manager und Azure-Speicherkonto separat zu erstellen. Um ein Azure-Speicherkonto zu erstellen, wechseln Sie an den Azure Storage-Dienst in Azure-Portal, und führen Sie die Schritte in [Erstellen eines Azure-Speicherkontos](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account). Nach der Erstellung dieses Kontos hinzufügen mit dem StorSimple-Geräte-Manager-Dienst anhand der Schritte in [Konfigurieren eines neuen Speicherkontos für den Dienst](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Wenn das virtuelle Gerät im Portal Government bereitstellen zu können, ist der StorSimple-Geräte-Manager-Dienst an uns Iowa und uns Virginia Speicherorten verfügbar.

    5.  Wählen Sie **erstellen Sie ein neues Azure-Speicherkonto** automatisch ein Speicherkonto mit dem Dienst erstellen. Geben Sie einen **speicherkontoname**. Wenn Sie Ihre Daten in einem anderen Speicherort benötigen, deaktivieren Sie dieses Kontrollkästchen.

    6.  Überprüfen Sie **an Dashboard anheften** Wunsch einen schnellen Link auf diesen Dienst auf dem Dashboard.

    7.  Klicken Sie auf **erstellen** StorSimple-Geräte-Manager zu erstellen.

        ![Erstellen eines neuen Diensts](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Sie werden angewiesen, die **Service** Zielseite. Die diensterstellung dauert einige Minuten. Nachdem der Dienst erfolgreich erstellt wurde, Sie entsprechend benachrichtigt werden und der Status des Diensts ändert sich zu **Active**.


