---
title: Bereitstellen von Azure File Sync (Vorschau) | Microsoft-Dokumentation
description: "Informationen über sämtliche Schritte zum Bereitstellen von Azure File Sync."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 42a0e7a3816e0f1d96951feac210e5770add4fe1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Bereitstellen von Azure File Sync (Vorschau)
Verwenden Sie Azure File Sync (Vorschau), um die Dateifreigaben Ihrer Organisation in Azure Files zu zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit Azure File Sync werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, NFS und FTPS. Sie können weltweit so viele Caches wie nötig nutzen.

Es wird dringend empfohlen, die Anleitungen [Planning for an Azure Files deployment](storage-files-planning.md) (Planung für eine Azure Files-Bereitstellung, in englischer Sprache) und [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md) zu lesen, bevor Sie die in diesem Artikel beschriebenen Schritte ausführen.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure Storage-Konto und eine Azure-Dateifreigabe in der gleichen Region, in der Sie die Azure-Dateisynchronisierung bereitstellen möchten. Weitere Informationen finden Sie unter:
    - [Region availability](storage-sync-files-planning.md#region-availability) (Regionale Verfügbarkeit, in englischer Sprache) für Azure File Sync.
    - Eine Schritt-für-Schritt-Beschreibung zum Erstellen eines Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    - Eine Schritt-für-Schritt-Beschreibung zum Erstellen einer Dateifreigabe finden Sie unter [Erstellen einer Dateifreigabe](storage-how-to-create-file-share.md).
* Mindestens eine unterstützte Instanz von Windows Server oder Windows Server-Cluster für die Synchronisierung mit Azure File Sync. Weitere Informationen zu unterstützten Versionen von Windows Server finden Sie unter [Interoperability with Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability) (Interoperabilität mit Windows Server, in englischer Sprache).

## <a name="deploy-the-storage-sync-service"></a>Bereitstellen des Speichersynchronisierungsdiensts 
Der Speichersynchronisierungsdienst ist die Azure-Ressource der obersten Ebene für Azure File Sync. Navigieren Sie zum Bereitstellen eines Speichersynchronisierungsdiensts zum [Azure-Portal](https://portal.azure.com/), und suchen Sie dann nach „Azure File Sync“. Wählen Sie in den Suchergebnissen **Azure File Sync (Vorschau)** aus, und klicken Sie auf **Erstellen**, um die Registerkarte **Speichersynchronisierung bereitstellen** zu öffnen.

Geben Sie in dem neuen Bereich, der geöffnet wird, Folgendes ein:

- **Name**: Ein eindeutiger Name (pro Abonnement) für den Speichersynchronisierungsdienst.
- **Abonnement**: Das Abonnement, in dem Sie den Speichersynchronisierungsdienst erstellen möchten. Abhängig von der Konfigurationsstrategie Ihrer Organisation haben Sie möglicherweise Zugriff auf ein oder mehrere Abonnements. Die Abrechnung für die einzelnen Clouddienste (z.B. Azure Files) erfolgt innerhalb eines Azure-Abonnements.
- **Ressourcengruppe**: Eine Ressourcengruppe ist eine logische Gruppe von Azure-Ressourcen, z.B. ein Speicherkonto oder Speichersynchronisierungsdienst. Sie können für Azure File Sync eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen. (Wir empfehlen, Ressourcengruppen als Container zu verwenden, um Ressourcen für Ihre Organisation logisch voneinander zu trennen, etwa indem Sie alle Personalressourcen oder alle Ressourcen für ein bestimmtes Projekt gruppieren.)
- **Standort**: Die Region, in der Sie Azure File Sync bereitstellen möchten. In dieser Liste sind nur die unterstützten Regionen verfügbar.

Wenn Sie fertig sind, wählen Sie **Erstellen** aus, um den Speichersynchronisierungsdienst bereitzustellen.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Vorbereiten von Windows Server für die Verwendung mit Azure File Sync
Führen Sie für jeden Server, den Sie mit Azure File Sync verwenden möchten, einschließlich der Serverknoten in einem Failovercluster, die folgenden Schritte aus:

1. Deaktivieren Sie die **Verstärkte Sicherheitskonfiguration für Internet Explorer**. Dies ist nur für die anfängliche Serverregistrierung erforderlich. Sie können sie nach dem Registrieren des Servers erneut aktivieren.
    1. Öffnen Sie den Server-Manager.
    2. Klicken Sie auf **Lokaler Server**:  
        ![„Lokaler Server“ auf der linken Seite der Server Manager-Benutzeroberfläche](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Wählen Sie im Unterbereich **Eigenschaften** den Link für **Verstärkte Sicherheitskonfiguration für IE** aus.  
        ![Bereich „Verstärkte Sicherheitskonfiguration für IE“ auf der Server Manager-Benutzeroberfläche](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Wählen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** für **Administratoren** und **Benutzer** **Aus** aus:  
        ![Popupfenster „Verstärkte Sicherheitskonfiguration für Internet Explorer“, in dem „Aus“ gewählt ist](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Stellen Sie sicher, dass mindestens PowerShell 5.1 ausgeführt wird.\* (Unter Windows Server 2016 ist PowerShell 5.1 die Standardversion.) Sie können anhand des Werts der **PSVersion**-Eigenschaft des **$PSVersionTable**-Objekts überprüfen, ob PowerShell 5.1.\* ausgeführt wird:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Wenn die PSVersion niedriger als 5.1.\* ist, wie dies bei den meisten Installationen von Windows Server 2012 R2 der Fall ist, können Sie problemlos ein Upgrade ausführen, indem Sie [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616) herunterladen und installieren. Das Paket, das für Windows Server 2012 R2 heruntergeladen und installiert werden sollte, lautet **Win8.1AndW2K12R2-KB\*\*\*\*\*\*\*-x64.msu**.

3. [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Es wird empfohlen, die neueste Version der Azure PowerShell-Module zu verwenden.

## <a name="install-the-azure-file-sync-agent"></a>Installieren des Azure-Dateisynchronisierungs-Agents
Der Azure File Sync-Agent ist ein herunterladbares Paket, mit dem ein Windows Server-Computer mit einer Azure-Dateifreigabe synchronisiert werden kann. Sie können den Agent aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257) herunterladen. Doppelklicken Sie nach Abschluss des Downloads auf das MSI-Paket, um die Installation des Azure File Sync-Agents zu starten.

> [!Important]  
> Wenn Sie Azure File Sync mit einem Failovercluster verwenden möchten, muss der Azure File Sync-Agent auf jedem Knoten im Cluster installiert werden.

Das Installationspaket für den Azure File Sync-Agent wird relativ schnell und ohne viele zusätzliche Eingabeaufforderungen installiert. Folgendes Vorgehen wird empfohlen:
- Übernehmen Sie den Standardinstallationspfad („C:\Programme\Microsoft Files\Azure\StorageSyncAgent“), um die Problembehandlung und Serverwartung zu vereinfachen.
- Aktivieren Sie Microsoft Update, um Azure File Sync auf dem aktuellen Stand zu halten. Alle Updates des Azure File Sync-Agents, einschließlich Funktionsupdates und Hotfixe, erfolgen über Microsoft Update. Es wird empfohlen, das neueste Update von Azure File Sync zu installieren. Weitere Informationen finden Sie unter [Updaterichtlinie für Azure File Sync](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Wenn die Installation des Azure File Sync-Agents abgeschlossen ist, wird automatisch die Benutzeroberfläche der Serverregistrierung geöffnet. Informationen zum Registrieren dieses Servers bei Azure File Sync finden Sie im nächsten Abschnitt.

## <a name="register-windows-server-with-storage-sync-service"></a>Registrieren eines Windows-Servers beim Speichersynchronisierungsdienst
Durch das Registrieren des Windows-Servers bei einem Speichersynchronisierungsdienst wird eine Vertrauensstellung zwischen dem Server (oder Cluster) und dem Speichersynchronisierungsdienst geschaffen. Die Benutzeroberfläche der Serverregistrierung sollte sich nach der Installation des Azure File Sync-Agents automatisch öffnen. Wenn das nicht der Fall ist, können Sie sie an Ihrem Dateispeicherort manuell öffnen: „C:\Programme\Azure\StorageSyncAgent\ServerRegistration.exe“. Wenn die Benutzeroberfläche der Serverregistrierung geöffnet ist, wählen Sie **Anmelden** aus, um mit der Registrierung zu beginnen.

Nach der Anmeldung werden die folgenden Informationen abgefragt:

![Screenshot der Serverregistrierungs-Benutzeroberfläche](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-Abonnement**: Das Abonnement, das den Speichersynchronisierungsdienst enthält (siehe [Bereitstellen des Speichersynchronisierungsdiensts](#deploy-the-storage-sync-service)). 
- **Ressourcengruppe**: Die Ressourcengruppe, die den Speichersynchronisierungsdienst enthält.
- **Speichersynchronisierungsdienst**: Der Name des Speichersynchronisierungsdiensts, bei dem Sie den Server registrieren möchten.

Nachdem Sie die entsprechenden Informationen ausgewählt haben, wählen Sie **Registrieren** aus, um die Serverregistrierung abzuschließen. Während des Registrierungsvorgangs werden Sie erneut aufgefordert, sich anzumelden.

## <a name="create-a-sync-group"></a>Erstellen einer Synchronisierungsgruppe
Eine Synchronisierungsgruppe definiert die Synchronisierungstopologie für einen Satz von Dateien. Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Eine Synchronisierungsgruppe muss mindestens einen Cloudendpunkt, der eine Azure-Dateifreigabe darstellt, und einen Serverendpunkt, der einen Pfad auf einem Windows-Server darstellt, enthalten. Um eine Synchronisierungsgruppe zu erstellen, navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem Speichersynchronisierungsdienst, und wählen Sie dann **+ Synchronisierungsgruppe** aus:

![Erstellen einer neuen Synchronisierungsgruppe im Azure-Portal](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Geben Sie im Bereich, der jetzt geöffnet wird, die folgenden Informationen ein, um eine Sychronisierungsgruppe bei einem Cloudendpunkt zu erstellen:

- **Name der Synchronisierungsgruppe**: Der Name der zu erstellenden Synchronisierungsgruppe. Dieser Name muss innerhalb des Speichersynchronisierungsdiensts eindeutig sein, es kann jedoch ein beliebiger Name sein, der für Sie Sinn ergibt.
- **Abonnement**: Das Abonnement, in dem Sie unter [Bereitstellen des Speichersynchronisierungsdiensts](#deploy-the-storage-sync-service) weiter oben den Speichersynchronisierungsdienst bereitgestellt haben.
- **Speicherkonto**: Wenn Sie **Speicherkonto auswählen** auswählen, wird ein weiterer Bereich angezeigt, in dem Sie das Speicherkonto für die Azure-Dateifreigabe auswählen können, mit der Sie synchronisieren möchten.
- **Azure-Dateifreigabe**: Der Name der Azure-Dateifreigabe, mit der synchronisiert werden soll.

Um einen Serverendpunkt hinzuzufügen, wechseln Sie zu der neu erstellten Synchronisierungsgruppe, und wählen Sie dann **Serverendpunkt hinzufügen** aus.

![Hinzufügen eines neuen Serverendpunkts im Bereich „Synchronisierungsgruppe“](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Geben Sie im Bereich **Serverendpunkt hinzufügen** die folgenden Informationen ein, um einen Serverendpunkt zu erstellen:

- **Registrierter Server**: Der Name des Servers oder Clusters, auf dem Sie den Serverendpunkt erstellen möchten.
- **Pfad**: Der Windows Server-Pfad, der als Teil der Synchronisierungsgruppe synchronisiert werden soll.
- **Cloudtiering**: Ein Schalter, mit dem Cloudtiering aktiviert oder deaktiviert wird. Mit Cloudtiering kann für selten verwendete oder selten einem Zugriff ausgesetzte Dateien Tiering nach Azure Files festgelegt werden.
- **Freier Speicherplatz auf Volume**: Die Menge des freien Speicherplatzes auf dem Volume, auf dem sich der Serverendpunkt befindet. Wenn z.B. für ein Volume mit einem einzigen Serverendpunkt „Freier Speicherplatz auf Volume“ auf 50 % festgelegt ist, wird ungefähr die Hälfte der Daten nach Azure Files ausgelagert. Die Azure-Dateifreigabe enthält immer eine vollständige Kopie der Daten in der Synchronisierungsgruppe, unabhängig davon, ob Cloudtiering aktiviert ist.

Wählen Sie **Erstellen** aus, um den Serverendpunkt hinzuzufügen. Ihre Dateien bleiben jetzt zwischen der Azure-Dateifreigabe und Windows Server synchron. 

> [!Important]  
> Sie können Änderungen an jedem Cloudendpunkt oder Serverendpunkt in der Synchronisierungsgruppe vornehmen und Ihre Dateien mit den anderen Endpunkten in der Synchronisierungsgruppe synchronisieren. Wenn Sie eine Änderung direkt am Cloudendpunkt (Azure-Dateifreigabe) vornehmen, müssen Änderungen zunächst von einem Azure File Sync-Auftrag zum Erkennen von Änderungen entdeckt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird nur einmal alle 24 Stunden ausgegeben. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md#afs-change-detection).

## <a name="next-steps"></a>Nächste Schritte
- [Hinzufügen/Entfernen eines Azure File Sync-Serverendpunkts](storage-sync-files-server-endpoint.md)
- [Registrieren/Aufheben der Registrierung eines Servers bei Azure File Sync](storage-sync-files-server-registration.md)
