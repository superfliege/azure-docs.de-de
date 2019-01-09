---
title: 'Tutorial: Erweitern von Windows-Dateiservern mit der Azure-Dateisynchronisierung | Microsoft-Dokumentation'
description: Es wird der gesamte Prozess beschrieben, mit dem Sie Windows-Dateiserver mit der Azure-Dateisynchronisierung erweitern.
services: storage
author: wmgries
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 3ebf450f4e84fed572307a18f20f36013e32c7a5
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630698"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Tutorial: Erweitern von Windows-Dateiservern mit der Azure-Dateisynchronisierung
In diesem Tutorial werden die grundlegenden Schritte zum Erweitern der Speicherkapazität einer Windows Server-Instanz mit der Azure-Dateisynchronisierung veranschaulicht. Wir nutzen zwar einen virtuellen Azure-Computer mit Windows Server für dieses Tutorial, aber normalerweise führen Sie diesen Prozess für Ihre lokalen Server durch. Wenn Sie für die Bereitstellung der Azure-Dateisynchronisierung in Ihrer eigenen Umgebung bereit sind, hilft Ihnen der Artikel [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md) weiter.

> [!div class="checklist"]
> * Bereitstellen des Speichersynchronisierungsdiensts
> * Vorbereiten von Windows Server für die Verwendung mit der Azure-Dateisynchronisierung
> * Installieren des Azure-Dateisynchronisierungs-Agents
> * Registrieren eines Windows-Servers beim Speichersynchronisierungsdienst
> * Erstellen einer Synchronisierungsgruppe und eines Cloudendpunkts
> * Erstellen eines Serverendpunkts

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung
Es gibt einige Dinge, die Sie für dieses Tutorial einrichten müssen, bevor Sie die Azure-Dateisynchronisierung bereitstellen. Zusammen mit einem Azure Storage-Konto und einer Dateifreigabe erstellen Sie eine VM mit Windows Server 2016 Datacenter und bereiten diesen Server für die Azure-Dateisynchronisierung vor.

### <a name="create-a-folder-and-txt-file"></a>Erstellen eines Ordners und einer TXT-Datei

Erstellen Sie auf Ihrem lokalen Computer einen neuen Ordner mit dem Namen *FilesToSync*, und fügen Sie eine Textdatei mit dem Namen *mytestdoc.txt* hinzu. Sie laden diese Datei später im Tutorial auf eine Dateifreigabe hoch.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Als Nächstes erstellen Sie eine Dateifreigabe.

1. Klicken Sie auf **Zu Ressource wechseln**, nachdem die Bereitstellung des Azure Storage-Kontos abgeschlossen ist.
1. Klicken Sie im Speicherkontobereich auf **Dateien**.

    ![Klicken auf „Dateien“](./media/storage-sync-files-extend-servers/click-files.png)

1. Klicken Sie auf **+ Dateifreigabe**.

    ![Klicken auf die Schaltfläche „Dateifreigabe hinzufügen“](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Geben Sie der neuen Dateifreigabe den Namen *afsfileshare*, und geben Sie unter **Kontingent** den Wert „1“ ein. Klicken Sie anschließend auf **Erstellen**. Das Kontingent kann maximal auf 5 TiB festgelegt werden, aber für dieses Tutorial benötigen Sie nur 1 GB.

    ![Angeben eines Namens und Kontingents für die neue Dateifreigabe](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Wählen Sie die neue Dateifreigabe aus, und klicken Sie für den Speicherort der Dateifreigabe dann auf **Hochladen**.

    ![Hochladen einer Datei](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Navigieren Sie zum Ordner *FilesToSync*, in dem Sie Ihre TXT-Datei erstellt haben, wählen Sie *mytestdoc.txt*, und klicken Sie auf **Hochladen**.

    ![Durchsuchen der Dateifreigabe](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Sie haben jetzt ein Azure Storage-Konto und eine Dateifreigabe mit einer Datei in Azure erstellt. Nun erstellen Sie die Azure VM mit Windows Server 2016 Datacenter, die in diesem Tutorial als lokaler Server dient.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Bereitstellen eines virtuellen Computers und Anfügen eines Datenträgers

1. Erweitern Sie als Nächstes das Menü auf der linken Seite des Portals, und wählen Sie im Azure-Portal oben links die Option **Ressource erstellen**.
1. Suchen Sie im Suchfeld oberhalb der Liste mit den **Azure Marketplace**-Ressourcen nach **Windows Server 2016 Datacenter**, und wählen Sie den Eintrag aus. Klicken Sie anschließend auf **Erstellen**.
1. Wählen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** die Ressourcengruppe aus, die Sie für dieses Tutorial erstellt haben.

   ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Geben Sie unter **Instanzdetails** einen VM-Namen an, z.B. *myVM*.
1. Übernehmen Sie die Standardeinstellungen für **Region**, **Verfügbarkeitsoptionen**, **Image** und **Größe**.
1. Geben Sie unter **Administratorkonto** einen **Benutzernamen** und ein **Kennwort** für den virtuellen Computer an.
1. Wählen Sie unter **Regeln für eingehende Ports** die Option **Ausgewählte Ports zulassen** aus, und wählen Sie dann **RDP (3389)** und **HTTP** aus der Dropdownliste aus.

   Vor der Erstellung der VM müssen Sie einen Datenträger für Daten erstellen.

1. Klicken auf **Next:Disks** (Weiter: Datenträger)

   ![Hinzufügen von Datenträgern](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. Übernehmen Sie auf der Registerkarte **Datenträger** unter **Datenträgeroptionen** die Standardwerte.
1. Klicken Sie unter **DATENTRÄGER** auf **Create and attach a new disk** (Neuen Datenträger erstellen und anfügen).

1. Übernehmen Sie die Standardwerte, aber ändern Sie die Option **Größe (GiB)** für dieses Tutorial in **1 GB**.

   ![Datenträgerdetails](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Klicken Sie auf **OK**.
1. Klicken Sie auf **Überprüfen + erstellen**.
1. Klicken Sie auf **Create**.

   Sie können auf das Symbol **Benachrichtigungen** klicken, um den **Bereitstellungsstatus** zu verfolgen. Das Erstellen eines neuen virtuellen Computers dauert einige Minuten.

1. Klicken Sie nach der Bereitstellung des virtuellen Computers auf **Zu Ressource wechseln**.

   ![Zu Ressource wechseln](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Sie haben jetzt einen neuen virtuellen Computer erstellt und einen Datenträger angefügt. Nun müssen Sie eine Verbindung mit dem virtuellen Computer herstellen.

### <a name="connect-to-your-vm"></a>Herstellen einer Verbindung mit Ihrer VM

1. Klicken Sie im Azure-Portal auf der Eigenschaftenseite des virtuellen Computers auf **Verbinden**.

   ![Herstellen einer Verbindung mit einem virtuellen Azure-Computer über das Portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Übernehmen Sie auf der Seite zum **Herstellen der Verbindung mit dem virtuellen Computer** die Standardoptionen, um basierend auf der **IP-Adresse** eine Verbindung über Port 3389 herzustellen. Klicken Sie anschließend auf **RDP-Datei herunterladen**.

   ![Herunterladen der RDP-Datei](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden.
1. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie den Benutzernamen im Format *localhost\benutzername* sowie das Kennwort ein, das Sie für die VM erstellt haben, und klicken Sie dann auf **OK**.

   ![Weitere Optionen](./media/storage-sync-files-extend-servers/local-host2.png)

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** oder **Weiter**, um die Verbindung zu erstellen.

### <a name="prepare-the-windows-server"></a>Vorbereiten von Windows Server
Deaktivieren Sie für den **Windows Server 2016 Datacenter**-Server die Option **Verstärkte Sicherheitskonfiguration für Internet Explorer**. Dieser Schritt ist nur für die anfängliche Serverregistrierung erforderlich. Sie können sie nach dem Registrieren des Servers erneut aktivieren.

Auf der VM mit **Windows Server 2016 Datacenter** wird der **Server-Manager** automatisch geöffnet.  Wenn der **Server-Manager** nicht standardmäßig geöffnet wird, können Sie im Explorer danach suchen.

1. Klicken Sie im **Server-Manager** auf **Lokaler Server**.

   ![„Lokaler Server“ auf der linken Seite der Server-Manager-Benutzeroberfläche](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Wählen Sie im Unterbereich **Eigenschaften** den Link für **Verstärkte Sicherheitskonfiguration für IE** aus.  

    ![Bereich „Verstärkte Sicherheitskonfiguration für Internet Explorer“ auf der Server-Manager-Benutzeroberfläche](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. Wählen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** für **Administratoren** und **Benutzer** die Option **Aus**:

    ![Popupfenster „Verstärkte Sicherheitskonfiguration für Internet Explorer“ mit Auswahl von „Aus“](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Sie können den Datenträger jetzt dem virtuellen Computer hinzufügen.

### <a name="add-the-data-disk"></a>Hinzufügen des Datenträgers

1. Klicken Sie auf der VM mit **Windows Server 2016 Datacenter** auf **Dateien und Speicherdienste** > **Volumes** > **Datenträger**.

    ![Datenträger](media/storage-sync-files-extend-servers/your-disk.png)

1. Klicken Sie mit der rechten Maustaste auf den 1-GB-Datenträger mit dem Namen **Msft Virtual Disk**, und klicken Sie dann auf **Neues Volume**.
1. Schließen Sie den Assistenten mit den Standardeinstellungen ab, notieren Sie sich den zugewiesenen Laufwerkbuchstaben, und klicken Sie auf **Erstellen**.
1. Klicken Sie auf **Schließen**.

   Sie haben den Datenträger jetzt in den Onlinezustand versetzt und ein Volume erstellt. Sie können bestätigen, dass das Hinzufügen des Datenträgers erfolgreich war, indem Sie auf der VM den Explorer öffnen und sich vergewissern, dass das neue Laufwerk vorhanden ist.

1. Erweitern Sie im Explorer auf der VM die Option **Dieser PC**, und doppelklicken Sie auf das neue Laufwerk. In diesem Beispiel ist dies das Laufwerk „F:“.
1. Klicken Sie mit der rechten Maustaste, und wählen Sie **Neu** > **Ordner**. Geben Sie dem Ordner den Namen *FilesToSync*.
1. Doppelklicken Sie auf den Ordner **FilesToSync**.
1. Klicken Sie mit der rechten Maustaste, und wählen sie **Neu** > **Textdokument**. Geben Sie der Textdatei den Namen *MyTestFile*.

    ![Hinzufügen einer neuen Textdatei](media/storage-sync-files-extend-servers/new-file.png)

1. Schließen Sie **Explorer** und **Server-Manager**.

### <a name="download-the-azure-powershell-module"></a>Herunterladen des Azure PowerShell-Moduls
Installieren Sie als Nächstes auf dem virtuellen Computer mit **Windows Server 2016 Datacenter** das **Azure PowerShell-Modul** auf dem Server.

1. Öffnen Sie auf der VM ein PowerShell-Fenster mit erhöhten Rechten.
1. Führen Sie den folgenden Befehl aus:

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

   > [!NOTE]
   > Wenn Sie eine ältere NuGet-Version als 2.8.5.201 haben, werden Sie aufgefordert, die aktuelle NuGet-Version herunterzuladen und zu installieren.

   Standardmäßig ist der PowerShell-Katalog nicht als vertrauenswürdiges Repository für PowerShellGet konfiguriert. Bei der ersten Verwendung des PowerShell-Katalogs wird die folgende Meldung angezeigt:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Antworten Sie mit `Yes` oder `Yes to All`, um die Installation fortzusetzen.

Das Modul `Az` ist ein Rollupmodul für die Azure PowerShell-Cmdlets. Wenn Sie es installieren, werden alle verfügbaren Azure Resource Manager-Module heruntergeladen und die zugehörigen Cmdlets für die Nutzung zur Verfügung gestellt.

Sie haben jetzt die Einrichtung Ihrer Umgebung für das Tutorial abgeschlossen und können mit dem Bereitstellen des **Speichersynchronisierungsdiensts** beginnen.

## <a name="deploy-the-service"></a>Bereitstellen des Diensts 
Die Bereitstellung der Azure-Dateisynchronisierung beginnt mit der Platzierung einer **Speichersynchronisierungsdienst**-Ressource in einer Ressourcengruppe für Ihr ausgewähltes Abonnement. Der Speichersynchronisierungsdienst erbt Zugriffsberechtigungen aus dem Abonnement und der Ressourcengruppe, unter dem bzw. der Sie ihn bereitgestellt haben.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**, und suchen Sie dann nach **Azure-Dateisynchronisierung**.
1. Wählen Sie in den Suchergebnissen die Option **Azure-Dateisynchronisierung**.
1. Wählen Sie **Erstellen**, um die Registerkarte **Speichersynchronisierung bereitstellen** zu öffnen.

   ![Speichersynchronisierung bereitstellen](media/storage-sync-files-extend-servers/afs-info.png)

   Geben Sie in dem neuen Bereich, der geöffnet wird, Folgendes ein:

   | Wert | BESCHREIBUNG |
   | ----- | ----- |
   | **Name** | Ein eindeutiger Name (pro Abonnement) für den Speichersynchronisierungsdienst.<br><br>In diesem Tutorial verwenden wir *afssyncservice02*. |
   | **Abonnement** | Das Abonnement, das Sie für dieses Tutorial verwenden. |
   | **Ressourcengruppe** | Die Ressourcengruppe, die Sie für dieses Tutorial verwenden.<br><br>In diesem Tutorial verwenden wir *afsresgroup101918*. |
   | **Location** | USA (Ost) |

1. Wenn Sie fertig sind, können Sie **Erstellen** wählen, um den **Speichersynchronisierungsdienst** bereitzustellen.
1. Klicken Sie auf die Registerkarte **Benachrichtigungen** und dann auf **Zu Ressource wechseln**.

## <a name="install-the-agent"></a>Installieren des Agents
Der Azure-Dateisynchronisierungs-Agent ist ein herunterladbares Paket, mit dem ein Windows Server-Computer mit einer Azure-Dateifreigabe synchronisiert werden kann.

1. Wechseln Sie zurück zur VM mit **Windows Server 2016 Datacenter**, und öffnen Sie **Internet Explorer**.
1. Navigieren Sie zum [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Scrollen Sie nach unten zum Abschnitt **Azure-Dateisynchronisierungs-Agent**, und klicken Sie auf **Herunterladen**.

   ![Synchronisierungs-Agent – Download](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Aktivieren Sie das Kontrollkästchen **StorageSyncAgent_V3_WS2016.EXE**, und klicken Sie auf **Weiter**.

   ![Agent-Auswahl](media/storage-sync-files-extend-servers/select-agent.png)

1. Wählen Sie **Einmal erlauben** > **Ausführen** > **Öffnen**, um die Datei zu öffnen.
1. Schließen Sie das PowerShell-Fenster, wenn Sie dies noch nicht getan haben.
1. Akzeptieren Sie die Standardwerte im **Setup-Assistenten für den Speichersynchronisierungs-Agent**.
1. Klicken Sie auf **Installieren**.
1. Klicken Sie auf **Fertig stellen**.

Sie haben den Azure-Synchronisierungsdienst bereitgestellt und den Agent auf der VM mit **Windows Server 2016 Datacenter** installiert. Als Nächstes müssen Sie die VM beim **Speichersynchronisierungsdienst** registrieren.

## <a name="register-windows-server"></a>Registrieren von Windows Server
Durch das Registrieren des Windows-Servers bei einem Speichersynchronisierungsdienst wird eine Vertrauensstellung zwischen dem Server (oder Cluster) und dem Speichersynchronisierungsdienst geschaffen. Ein Server kann nur bei einem Speichersynchronisierungsdienst registriert und mit anderen Servern und Azure-Dateifreigaben synchronisiert werden, die demselben Speichersynchronisierungsdienst zugeordnet sind.

Die Benutzeroberfläche der Serverregistrierung sollte sich nach der Installation des **Azure-Dateisynchronisierungs-Agents** automatisch öffnen. Wenn dies nicht der Fall ist, können Sie sie aus ihrem Dateispeicherort manuell öffnen: „C:\Programme\Azure\StorageSyncAgent\ServerRegistration.exe“.

1. Klicken Sie auf **OK**, wenn die Benutzeroberfläche für die Serverregistrierung auf dem virtuellen Computer geöffnet wird.
1. Klicken Sie auf **Anmelden**, um zu beginnen.
1. Melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen an, und klicken Sie auf **Anmelden**.
1. Geben Sie die folgenden Informationen ein:

   ![Screenshot der Serverregistrierungs-Benutzeroberfläche](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Wert | BESCHREIBUNG |
   | **Azure-Abonnement** | Das Abonnement, das den Speichersynchronisierungsdienst für dieses Tutorial enthält. |
   | **Ressourcengruppe** | Die Ressourcengruppe, die den Speichersynchronisierungsdienst für dieses Tutorial enthält. Wir haben in diesem Tutorial *afsresgroup101918* verwendet. |
   | **Speichersynchronisierungsdienst** | Der Name des Speichersynchronisierungsdiensts, den Sie für dieses Tutorial verwendet haben. Wir haben in diesem Tutorial *afssyncservice02* verwendet. |

1. Klicken Sie auf **Registrieren**, um die Serverregistrierung abzuschließen.
1. Während des Registrierungsvorgangs werden Sie erneut aufgefordert, sich anzumelden. Melden Sie sich an, und klicken Sie auf **Weiter**.
1. Klicken Sie auf **OK**.

## <a name="create-a-sync-group"></a>Erstellen einer Synchronisierungsgruppe
Eine Synchronisierungsgruppe definiert die Synchronisierungstopologie für einen Satz von Dateien. Eine Synchronisierungsgruppe muss einen Cloudendpunkt enthalten, der eine Azure-Dateifreigabe und einen oder mehrere Serverendpunkte darstellt. Ein Serverendpunkt stellt einen Pfad auf einem registrierten Server dar.

1. Wählen Sie zum Erstellen einer Synchronisierungsgruppe im [Azure-Portal](https://portal.azure.com/) die Option **+ Synchronisierungsgruppe** für den Speichersynchronisierungsdienst aus, den Sie für dieses Tutorial erstellt haben. Wir haben in diesem Tutorial *afssyncservice02* als Beispiel verwendet.

   ![Erstellen einer neuen Synchronisierungsgruppe im Azure-Portal](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Geben Sie im Bereich, der jetzt geöffnet wird, die folgenden Informationen ein, um eine Synchronisierungsgruppe bei einem Cloudendpunkt zu erstellen:

   | Wert | BESCHREIBUNG |
   | ----- | ----- |
   | **Name der Synchronisierungsgruppe** | Dieser Name muss innerhalb des Speichersynchronisierungsdiensts eindeutig sein, es kann jedoch ein beliebiger Name sein, der für Sie Sinn ergibt. In diesem Tutorial verwenden wir *afssyncgroup*.|
   | **Abonnement** | Das Abonnement, unter dem Sie den Speichersynchronisierungsdienst für dieses Tutorial bereitgestellt haben. |
   | **Speicherkonto** |Klicken Sie auf **Speicherkonto auswählen**. Wählen Sie im angezeigten Bereich das Speicherkonto mit der Azure-Dateifreigabe aus, die Sie für dieses Tutorial erstellt haben. Wir haben *afsstoracct101918* verwendet. |
   | **Azure-Dateifreigabe** | Der Name der Azure-Dateifreigabe, die Sie für dieses Tutorial erstellt haben. Wir haben *afsfileshare* verwendet. |

1. Klicken Sie auf **Create**.

Wenn Sie Ihre Synchronisierungsgruppe auswählen, sehen Sie, dass Sie jetzt über einen **Cloudendpunkt** verfügen.

## <a name="add-a-server-endpoint"></a>Hinzufügen eines Serverendpunkts
Ein Serverendpunkt stellt einen bestimmten Speicherort auf einem registrierten Server dar, z. B. einen Ordner auf einem Servervolume.

1. Um einen Serverendpunkt hinzuzufügen, wählen Sie die neu erstellte Synchronisierungsgruppe und dann die Option **Serverendpunkt hinzufügen** aus.

   ![Hinzufügen eines neuen Serverendpunkts im Bereich „Synchronisierungsgruppe“](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. Geben Sie im Bereich **Serverendpunkt hinzufügen** die folgenden Informationen ein, um einen Serverendpunkt zu erstellen:

   | | |
   | ----- | ----- |
   | Wert | BESCHREIBUNG |
   | **Registrierter Server** | Der Name des Servers, den Sie für dieses Tutorial erstellt haben. Wir haben in diesem Tutorial *afsvm101918* verwendet. |
   | **Path** | Der Windows Server-Pfad zu dem Laufwerk, das Sie für dieses Tutorial erstellt haben. In unserem Beispiel ist dies *f:\filestosync*. |
   | **Cloudtiering** | Lassen Sie diese Option für dieses Tutorial deaktiviert. |
   | **Freier Volumespeicherplatz** | Lassen Sie diese Option für dieses Tutorial leer. |

1. Klicken Sie auf **Create**.

Ihre Dateien sind zwischen der Azure-Dateifreigabe und Windows Server jetzt synchron.

![Azure Storage erfolgreich synchronisiert](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden die grundlegenden Schritte zum Erweitern der Speicherkapazität einer Windows Server-Instanz mit der Azure-Dateisynchronisierung beschrieben. Unter dem unten angegebenen Link finden Sie ausführlichere Informationen zur Planung für eine Bereitstellung der Azure-Dateisynchronisierung.

> [!div class="nextstepaction"]
> [Planung für die Bereitstellung der Azure-Dateisynchronisierung](./storage-sync-files-planning.md)
