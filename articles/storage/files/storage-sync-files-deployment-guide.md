---
title: Bereitstellen der Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation
description: "Informationen über sämtliche Schritte zum Bereitstellen der Azure-Dateisynchronisierung."
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
ms.date: 09/19/2017
ms.author: wgries
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: d9cf205cd3e7a8017efbe9078ff495c04f065374
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="how-to-deploy-azure-file-sync-preview"></a>Bereitstellen der Azure-Dateisynchronisierung (Vorschau)
Mit der Azure-Dateisynchronisierung (Vorschau) können Freigaben lokal oder in Azure repliziert werden, und der Zugriff darauf kann über SMB- oder NFS-Freigaben in Windows Server erfolgen. Die Azure-Dateisynchronisierung ist besonders nützlich in Szenarien, in denen weit entfernt von einem Azure-Rechenzentrum gespeicherte Daten abgerufen und geändert werden müssen, z.B. in einer Zweigstelle. Daten können zwischen mehreren Windows Server-Endpunkten repliziert werden, z.B. zwischen mehreren Zweigstellen.

Es wird dringend empfohlen, vor dem Ausführen der Schritte in dieser Anleitung [Planning for an Azure Files deployment](storage-files-planning.md) (Planung für eine Azure Files-Bereitstellung, in englischer Sprache) und [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md) zu lesen.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Speicherkonto und eine Azure-Dateifreigabe in der gleichen Region, in der Sie die Azure-Dateisynchronisierung bereitstellen möchten. Weitere Informationen finden Sie unter:
    - [Region availability](storage-sync-files-planning.md#region-availability) (Regionale Verfügbarkeit, in englischer Sprache) der Azure-Dateisynchronisierung
    - [Erstellen Sie ein Speicherkonto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) mit einer Schritt-für Schritt-Anleitung zum Erstellen eines Speicherkontos
    - [Erstellen einer Dateifreigabe](storage-how-to-create-file-share.md) mit einer Schritt-für Schritt-Anleitung zum Erstellen einer Dateifreigabe
* Mindestens ein unterstützter Windows Server oder Windows Server-Cluster, der mit der Azure-Dateisynchronisierung synchronisiert werden soll. Weitere Informationen zu unterstützten Versionen von Windows Server finden Sie unter [Interoperability with Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability) (Interoperabilität mit Windows Server, in englischer Sprache).

## <a name="deploy-the-storage-sync-service"></a>Bereitstellen des Speichersynchronisierungsdiensts 
Der Speichersynchronisierungsdienst ist die Azure-Ressource der obersten Ebene, die die Azure-Dateisynchronisierung darstellt. Navigieren Sie zum Bereitstellen eines Speichersynchronisierungsdiensts zum [Azure-Portal](https://portal.azure.com/), und suchen Sie „Azure File Sync“ (Azure-Dateisynchronisierung). Wählen Sie in den Suchergebnissen „Azure File Sync (preview)“ (Azure-Dateisynchronisierung (Vorschau)) aus, und klicken Sie auf „Erstellen“, um die Registerkarte „Speichersynchronisierung bereitstellen“ zu öffnen.

Im daraufhin angezeigten Blatt müssen die folgenden Informationen angegeben werden:

- **Name**: Ein eindeutiger Name (pro Abonnement) für den Speichersynchronisierungsdienst.
- **Abonnement**: Das Abonnement, in dem der Speichersynchronisierungsdienst erstellt werden soll. Abhängig von der Konfigurationsstrategie Ihrer Organisation haben Sie möglicherweise Zugriff auf ein oder mehrere Abonnements. Die Abrechnung für die einzelnen Clouddienste (z.B. Azure Files) erfolgt innerhalb eines Azure-Abonnements.
- **Ressourcengruppe**: Eine Ressourcengruppe ist eine logische Gruppe von Azure-Ressourcen, z.B. ein Speicherkonto oder Speichersynchronisierungsdienst. Sie können für die Azure-Dateisynchronisierung eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden (es wird empfohlen, Ressourcengruppen als Container zum logischen Isolieren von Ressourcen für Ihre Organisation zu verwenden, z.B. zum Gruppieren von HR-Ressourcen oder von Ressourcen für ein bestimmtes Projekt).
- **Standort**: Die Region, in der Sie die Azure-Dateisynchronisierung bereitstellen möchten. In dieser Liste sind nur die unterstützten Regionen verfügbar.

Sobald das Formular „Speichersynchronisierung bereitstellen“ ausgefüllt wurde, klicken Sie auf „Erstellen“, um den Speichersynchronisierungsdienst bereitzustellen.

## <a name="prepare-windows-servers-for-use-with-azure-file-sync"></a>Vorbereiten von Windows-Servern für die Verwendung der Azure-Dateisynchronisierung
Führen Sie für jeden Server, mit dem Sie die Azure-Dateisynchronisierung verwenden möchten, einschließlich der Serverknoten in einem Failovercluster, die folgenden Schritte aus:

Führen Sie für jeden Server, einschließlich der Serverknoten in einem Failovercluster, den Sie mit der Azure-Dateisynchronisierung verwenden möchten, die folgenden Schritte aus:

1. Deaktivieren Sie die verstärkte Sicherheitskonfiguration für Internet Explorer. Dies ist nur für die erste Serverregistrierung erforderlich. Die Einstellung kann wieder aktiviert werden, nachdem der Server registriert wurde.
    1. Öffnen Sie den Server-Manager.
    2. Klicken Sie auf **Lokaler Server**:  
        ![„Lokaler Server“ auf der linken Seite der Server-Manager-Benutzeroberfläche](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Wählen Sie im Unterbereich „Eigenschaften“ den Link **Verstärkte Sicherheitskonfiguration für IE** aus:  
        ![„Verstärkte Sicherheitskonfiguration für IE“ auf der Server-Manager-Benutzeroberfläche](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Wählen Sie im Popupfenster „Verstärkte Sicherheitskonfiguration für Internet Explorer“ für „Administratoren“ und „Benutzer“ **Aus**:  
        ![Popupfenster „Verstärkte Sicherheitskonfiguration für Internet Explorer“, in dem „Aus“ gewählt ist](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Stellen Sie sicher, dass mindestens PowerShell 5.1 ausgeführt wird.\* (Unter Windows Server 2016 ist PowerShell 5.1 die Standardversion.) Sie können anhand des Werts der PSVersion-Eigenschaft des $PSVersionTable-Objekts überprüfen, ob PowerShell 5.1.\* ausgeführt wird:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    - Wenn die PSVersion niedriger als 5.1.\* ist, wie dies bei den meisten Installationen von Windows Server 2012 R2 der Fall ist, können Sie problemlos ein Upgrade ausführen, indem Sie [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616) herunterladen und installieren. Das Paket, das für Windows Server 2012 R2 heruntergeladen und installiert werden sollte, lautet **Win8.1AndW2K12R2-KB\*\*\*\*\*\*\*-x64.msu**.

3. [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Es wird empfohlen, immer die neueste Version der Azure PowerShell-Module zu verwenden.

## <a name="install-the-azure-file-sync-agent"></a>Installieren des Azure-Dateisynchronisierungs-Agents
Der Azure-Dateisynchronisierungs-Agent ist ein herunterladbares Paket, mit dem ein Windows Server-Computer mit einer Azure-Dateifreigabe synchronisiert werden kann. Sie können den Agent vom [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257) herunterladen. Doppelklicken Sie nach Abschluss des Downloads auf das MSI-Paket, um die Installation des Azure-Dateisynchronisierungs-Agents zu starten.

> [!Important]  
> Wenn Sie die Azure-Dateisynchronisierung mit einem Failovercluster verwenden möchten, muss der Azure-Dateisynchronisierungs-Agent auf jedem Knoten im Cluster installiert werden.

Das Installationspaket für den Azure-Dateisynchronisierungs-Agent wird relativ schnell und ohne viele zusätzliche Eingabeaufforderungen installiert. Es wird Folgendes empfohlen:
- Übernehmen Sie den Standardinstallationspfad (`C:\Program Files\Azure\StorageSyncAgent`), um die Problembehandlung und die Serverwartung zu vereinfachen.
- Aktivieren Sie Microsoft Update, um die Azure-Dateisynchronisierung auf dem neuesten Stand zu halten. Alle Updates des Azure-Dateisynchronisierungs-Agents, einschließlich Featureupdates und Hotfixes, erfolgen über Microsoft Update. Es wird empfohlen, immer das neueste Update der Azure-Dateisynchronisierung zu verwenden. Weitere Informationen finden Sie unter [Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Zum Abschluss der Installation des Azure-Dateisynchronisierungs-Agents wird automatisch die Benutzeroberfläche der Serverregistrierung geöffnet. Informationen zum Registrieren dieses Servers bei der Azure-Dateisynchronisierung finden Sie im nächsten Abschnitt.

## <a name="register-windows-server-with-storage-sync-service"></a>Registrieren eines Windows-Servers beim Speichersynchronisierungsdienst
Durch das Registrieren des Windows-Servers bei einem Speichersynchronisierungsdienst wird eine Vertrauensstellung zwischen dem Server (oder Cluster) und dem Speichersynchronisierungsdienst geschaffen. Die Benutzeroberfläche der Serverregistrierung wird normalerweise automatisch nach der Installation des Azure-Dateisynchronisierungs-Agents geöffnet. Wenn dies jedoch nicht der Fall, können Sie sie über den folgenden Speicherort manuell öffnen: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`. Sobald die Benutzeroberfläche der Serverregistrierung geöffnet ist, klicken Sie auf **Anmelden**, um mit der Registrierung zu beginnen.

Nach der Anmeldung werden die folgenden Informationen abgefragt:

![Screenshot der Serverregistrierungs-Benutzeroberfläche](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-Abonnement**: Das Abonnement, das den Speichersynchronisierungsdienst enthält (siehe [Bereitstellen des Speichersynchronisierungsdiensts](#deploy-the-storage-sync-service) weiter oben). 
- **Ressourcengruppe**: Die Ressourcengruppe, die den Speichersynchronisierungsdienst enthält.
- **Speichersynchronisierungsdienst**: Der Name des Speichersynchronisierungsdiensts, bei dem Sie den Server registrieren möchten.

Nachdem Sie die entsprechenden Informationen aus den Dropdownlisten ausgewählt haben, klicken Sie auf **Registrieren**, um die Serverregistrierung abzuschließen. Während des Registrierungsvorgangs werden Sie erneut aufgefordert, sich anzumelden.

## <a name="create-a-sync-group"></a>Erstellen einer Synchronisierungsgruppe
Eine Synchronisierungsgruppe definiert die Synchronisierungstopologie für einen Satz von Dateien. Endpunkte innerhalb einer Synchronisierungsgruppe bleiben miteinander synchron. Eine Synchronisierungsgruppe muss mindestens einen Cloudendpunkt, der eine Azure-Dateifreigabe darstellt, und einen Serverendpunkt, der einen Pfad auf einem Windows-Server darstellt, enthalten. Um eine Synchronisierungsgruppe zu erstellen, navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu dem Speichersynchronisierungsdienst, und klicken Sie auf **+ Synchronisierungsgruppe**:

![Erstellen einer neuen Synchronisierungsgruppe im Azure-Portal](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Im daraufhin angezeigten Bereich werden die folgenden Informationen zum Erstellen einer Synchronisierungsgruppe mit einem Cloudendpunkt abgefragt:

- **Name der Synchronisierungsgruppe**: Der Name der zu erstellenden Synchronisierungsgruppe. Dieser Name muss innerhalb des Speichersynchronisierungsdiensts eindeutig sein, es kann jedoch ein beliebiger Name sein, der für Sie Sinn ergibt.
- **Abonnement**: Das Abonnement, in dem Sie unter [Bereitstellen des Speichersynchronisierungsdiensts](#deploy-the-storage-sync-service) weiter oben den Speichersynchronisierungsdienst bereitgestellt haben.
- **Speicherkonto**: Wenn Sie auf „Speicherkonto auswählen“ klicken, wird ein zusätzlicher Bereich geöffnet. In diesem können Sie das Speicherkonto auswählen, das die Azure-Dateifreigabe enthält, mit der synchronisiert werden soll.
- **Azure-Dateifreigabe**: Der Name der Azure-Dateifreigabe, mit der synchronisiert werden soll.

Um einen Serverendpunkt hinzuzufügen, navigieren Sie zu der neu erstellten Synchronisierungsgruppe, und klicken Sie auf „Serverendpunkt hinzufügen“.

![Hinzufügen eines neuen Serverendpunkts im Bereich „Synchronisierungsgruppe“](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Im daraufhin angezeigte Bereich „Serverendpunkt hinzufügen“ müssen die folgenden Informationen angegeben werden, um einen Serverendpunkt zu erstellen:

- **Registrierter Server**: Der Name des Servers oder Clusters, auf dem der Serverendpunkt erstellt werden soll.
- **Pfad**: Der Pfad auf dem Windows-Server, der als Teil der Synchronisierungsgruppe synchronisiert werden soll.
- **Cloudtiering**: Ein Schalter zum Aktivieren oder Deaktivieren von Cloudtiering, ein Feature, das das Auslagern von nur selten verwendeten oder aufgerufenen Dateien in Azure Files ermöglicht.
- **Freier Speicherplatz auf Volume**: Die Menge des freien Speicherplatzes auf dem Volume, auf dem sich der Serverendpunkt befindet. Wenn z.B. für ein Volume mit einem einzigen Serverendpunkt „Freier Speicherplatz auf Volume“ auf 50 % festgelegt ist, wird ungefähr die Hälfte der Daten in Azure Files ausgelagert. Beachten Sie, dass die Azure-Dateifreigabe immer eine vollständige Kopie der Daten in der Synchronisierungsgruppe enthält, unabhängig davon, ob Cloudtiering aktiviert ist.

Klicken Sie auf „Erstellen“, um den Serverendpunkt hinzuzufügen. Ihre Dateien bleiben jetzt zwischen der Azure-Dateifreigabe und dem Windows-Server synchron. 

## <a name="next-steps"></a>Nächste Schritte
- [Hinzufügen/Entfernen eines Azure-Dateisynchronisierungsserver-Endpunkts](storage-sync-files-server-endpoint.md)
- [Registrieren/Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung](storage-sync-files-server-registration.md)
