

Wenn Sie ein Webanwendungsprojekt für Azure erstellen, können Sie einen virtuellen Computer in Azure bereitstellen. Sie können die virtuelle Maschine mit zusätzlichen Software konfigurieren oder verwenden die virtuelle Maschine für die Diagnose oder Debuggen.

Um einen virtuellen Computer erstellen, wenn Sie eine Webanwendung erstellen, gehen Sie folgendermaßen vor:

1. Klicken Sie in Visual Studio auf **Datei** > **neu** > **Projekt** > **Web**, und wählen Sie dann **ASP.NET-Webanwendung** (unter der **Visual C#-** oder **Visual Basic** Knoten).
2. In der **neues ASP.NET-Projekt** (Dialogfeld), wählen Sie den Typ des gewünschten Webanwendung, und stellen sicher, dass die Azure-Abschnitt des Dialogfelds (in der unteren rechten Ecke), die **Host in der Cloud** Kontrollkästchen aktiviert ist (dieses Kontrollkästchen ist mit der Bezeichnung **Remoteressourcen** in einigen Installationen).
   
    ![][0]
3. Wählen Sie für dieses Beispiel in der Dropdown-Liste unter Microsoft Azure **virtuellen Computer (v1)**, und klicken Sie dann auf die **OK** Schaltfläche.
4. Melden Sie sich bei Azure an, wenn Sie aufgefordert werden. Die **Erstellen virtueller Computer** Dialogfeld wird angezeigt.
   
    ![][2]
5. In der **DNS-Namen** Geben Sie einen Namen für den virtuellen Computer. Der DNS-Name muss in Azure eindeutig sein. Wenn der von Ihnen eingegebene Name nicht verfügbar ist, wird ein rotes Ausrufezeichen angezeigt.
6. In der **Image** wählen Sie das Bild auf dem virtuellen Computer basieren soll. Sie können die standard-Azure-VM-Images oder das Bild, das Sie in Azure hochgeladen haben.
7. Lassen Sie die **Aktivieren von IIS und Web Deploy** Kontrollkästchen aktiviert, es sei denn, Sie planen, einen anderen Webserver zu installieren. Kann nicht aus Visual Studio veröffentlichen, wenn Sie Web Deploy deaktivieren. Sie können IIS und Web Deploy an ein beliebiges Paket Windows Server-Images, einschließlich benutzerdefinierter Abbilder hinzufügen.
8. In der **Größe** wählen Sie die Größe des virtuellen Computers.
9. Geben Sie die Anmeldeinformationen für diesen virtuellen Computer an. Notieren Sie, da Sie ihnen den Zugriff auf die Computer über Remotedesktop benötigen.
10. In der **Speicherort** Liste, wählen Sie die Region zum Hosten der virtuellen Maschine.
11. Klicken Sie auf die **OK** Schaltfläche zum Starten der Erstellung des virtuellen Computers. Sie können den Fortschritt des Vorgangs im folgen der **Ausgabe** Fenster.
    
    ![][3]
12. Wenn der virtuelle Computer bereitgestellt wird, werden veröffentlichte Skripts erstellt, einem **PublishScripts** Knoten in der Projektmappe. Das veröffentlichte Skript wird ausgeführt und stellt einen virtuellen Computer in Azure bereit. Die **Ausgabe** Fenster wird der Status angezeigt. Das Skript führt die folgenden Aktionen aus, um den virtuellen Computer einzurichten:
    
    * Erstellt die virtuelle Maschine aus, wenn er nicht bereits vorhanden.
    * Erstellt ein Speicherkonto mit einem Namen, die mit beginnt `devtest`, jedoch nur, wenn in der angegebenen Region noch solches Speicherkonto nicht.
    * Erstellt einen Cloud-Dienst als Container für die virtuelle Maschine und eine Webrolle für die Webanwendung erstellt.
    * Konfigurieren von Web Deploy auf dem virtuellen Computer.
    * Konfiguriert IIS und ASP.NET auf dem virtuellen Computer an.
    
    ![][4]
13. (Optional) Sie können mit der neuen virtuellen Maschine verbinden. In **Server-Explorer**, erweitern Sie die **VMs** Knoten, wählen Sie den Knoten für den virtuellen Computer, die Sie erstellt haben, und klicken Sie im Kontextmenü, wählen Sie **mit Remotedesktop verbinden**. Alternativ können Sie in **Cloud Explorer** Sie könne **im Portal öffnen** im Kontextmenü und Herstellen einer Verbindung mit dem virtuellen Computer vorhanden.
    
    ![][5]

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die veröffentlichte Skripts anpassen, erstellen möchten, lesen Sie detailliertere Informationen [mithilfe von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
