1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Starten in der oberen linken Ecke, klicken Sie auf **neu > berechnen > Windows Server 2016 Datacenter**.

    ![Navigieren Sie zu den Azure-VM-Images im portal](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. Wählen Sie auf das Windows Server 2016-Rechenzentrum und das klassischen Bereitstellungsmodell. Klicken Sie auf erstellen.

    ![Screenshot, der zeigt, die Azure-VM-Images im Portal verfügbar](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Blatt "Grundlagen"

Das Blatt "Grundlagen" fordert Administrationsinformationen für den virtuellen Computer.

1. Geben Sie einen **Namen** für den virtuellen Computer. Im Beispiel _HeroVM_ ist der Name des virtuellen Computers. Der Name muss 1 bis 15 Zeichen lang sein und darf keine Sonderzeichen enthalten.

2. Geben Sie einen **Benutzername** und ein sicheres **Kennwort** zum Erstellen eines lokalen Kontos auf dem virtuellen Computer verwendet werden. Das lokale Konto wird verwendet, um sich anmelden und verwalten den virtuellen Computer. Im Beispiel _Azureuser_ ist der Benutzername.

 Das Kennwort muss 8 123 Zeichen lang sein und drei der vier folgenden komplexitätsanforderungen erfüllen: einen Kleinbuchstaben, einen Großbuchstaben, eine Ziffer und ein Sonderzeichen. Weitere Informationen zu [Benutzername und Kennwort Anforderungen](../articles/virtual-machines/windows/faq.md).

3. Die **Abonnement** ist optional. Eine allgemeine Einstellung ist "Nutzungsbasierte".

4. Wählen Sie eine vorhandene **Ressourcengruppe** oder geben Sie den Namen für ein neues Konto. Im Beispiel _HeroVMRG_ ist der Name der Ressourcengruppe.

5. Wählen Sie ein Azure-Rechenzentrum **Speicherort** , in dem den virtuelle Computer ausgeführt werden sollen. Im Beispiel **Osten USA** ist der Speicherort.

6. Wenn Sie fertig sind, klicken Sie auf **Weiter** , um das Blatt "Weiter" zu öffnen.

    ![Screenshot, der zeigt, die Einstellungen auf dem Blatt "Grundlagen" für das Konfigurieren einer Azure-VM](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Blatt "Größe"

Das Blatt "Größe" identifiziert die Einzelheiten der Konfiguration des virtuellen Computers und führt verschiedene Optionen, die Anzahl der Prozessoren, Datenträger-Speichertyp und geschätzten monatlichen Nutzungskosten-Betriebssystem enthalten.  

Wählen Sie eine VM-Größe, und klicken Sie dann auf **wählen** um den Vorgang fortzusetzen. In diesem Beispiel _DS1_\__V2 Standard_ ist die Größe des virtuellen Computers.

  ![Screenshot des Blatts Größe, die den Azure-VM-Größen, die zeigt, können Sie auswählen.](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Blatt "Einstellungen"

Das Blatt "Einstellungen" fordert Speicher- und Optionen an. Sie können die Standardeinstellungen übernehmen. Azure erstellt die entsprechenden Einträge bei Bedarf.

Wenn Sie eine Größe des virtuellen Computers, die dies unterstützt ausgewählt, können Sie Azure-Premium-Speicher versuchen, indem Premium (SSD) im Datenträgertyp auswählen.

Wenn Sie die Änderungen machen fertig sind, klicken Sie auf **OK**.

## <a name="4-summary-blade"></a>4. Blatt "Zusammenfassung"

Das Blatt "Zusammenfassung" werden die in den vorherigen Blades angegebenen Einstellungen aufgeführt. Klicken Sie auf **OK** Wenn jetzt können Sie das Bild zu machen.

 ![Blatt "Zusammenfassung" Bericht erteilen angegebene Einstellungen des virtuellen Computers](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

Nachdem die virtuelle Maschine erstellt wurde, wird im Portal aufgeführt die neue virtuelle Maschine unter **alle Ressourcen**, und eine Kachel des virtuellen Computers auf dem Dashboard angezeigt. Die entsprechenden Cloud-Dienst und das Speicherkonto Konto auch erstellt und aufgeführt. Sowohl die virtuellen Computer und Cloud-Dienst automatisch gestartet werden und deren Status wird als aufgeführt **ausführen**.

 ![Konfigurieren von VM-Agent und die Endpunkte des virtuellen Computers](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
