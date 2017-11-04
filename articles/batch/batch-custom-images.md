---
title: "Bereitstellen von Azure Batch-Pools über benutzerdefinierte Images | Microsoft-Dokumentation"
description: "Sie können einen Batch-Pool über ein benutzerdefiniertes Image erstellen, um Serverknoten bereitzustellen, die die Software und die Daten enthalten, die Sie für Ihre Anwendung benötigen. Benutzerdefinierte Images sind eine effiziente Möglichkeit zum Konfigurieren von Computeknoten, um Ihre Batch-Workloads auszuführen."
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 10/11/2017
ms.author: v-dotren
ms.openlocfilehash: d62abd673f89fd51edba721119d1680762a60c76
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="use-a-managed-custom-image-to-create-a-pool-of-virtual-machines"></a>Verwenden eines verwalteten benutzerdefinierten Images zum Erstellen eines VM-Pools 

Wenn Sie einen Azure Batch-Pool mithilfe der Konfiguration des virtuellen Computers erstellen, geben Sie das Image eines virtuellen Computers (VM) an, das das Betriebssystem für jeden Computeknoten im Pool bereitstellt. Sie können einen Pool von virtuellen Computern entweder mit einem Azure Marketplace-Image oder mit einem benutzerdefinierten Image (ein VM-Image, dass Sie selbst erstellt und konfiguriert haben) erstellen. Das benutzerdefinierte Image muss eine *verwalteten Image*-Ressource in dem gleichen Azure-Abonnement und in der gleichen Region wie das Batch-Konto sein.

## <a name="why-use-a-custom-image"></a>Gründe für die Verwendung eines benutzerdefinierten Images
Wenn Sie ein benutzerdefiniertes Image bereitstellen, haben Sie die Kontrolle über die Konfiguration und den Typ des Betriebssystems sowie über die zu verwendenden Datenträger. Ihr benutzerdefiniertes Image kann Anwendungen und Verweisdaten enthalten, die auf allen Knoten des Batch-Pools verfügbar werden, sobald diese bereitgestellt wurden.

Das Verwenden eines benutzerdefinierten Images spart Zeit beim Vorbereiten der Computekonten des Pools für die Ausführung Ihrer Batch-Workload. Obwohl Sie ein Azure Marketplace-Image verwenden und Software auf jedem Computeknoten installieren können, nachdem er bereitgestellt wurde, ist die Verwendung eines benutzerdefinierten Images möglicherweise effizienter.

Das Verwenden eines benutzerdefinierten Images, das für Ihr Szenario konfiguriert wurde, kann mehrere Vorteile bieten:

- **Betriebssystem konfigurieren**: Sie können eine spezielle Konfiguration des Betriebssystems auf dem Betriebssystemdatenträger des benutzerdefinierten Images ausführen. 
- **Anwendungen vorab installieren**: Sie können ein benutzerdefiniertes Image mit vorinstallierten Anwendungen auf dem Betriebssystemdatenträger erstellen, was effizienter und weniger fehleranfällig ist, als die Installation von Anwendungen nach der Bereitstellung der Computeknoten mit StartTask.
- **Zeit beim Neustart von virtuellen Computern sparen**: Die Anwendungsinstallation erfordert in der Regel den Neustart des virtuellen Computers, was sich als zeitaufwändig erweist. Sie können Zeit beim Neustart sparen, indem Sie Anwendungen vorab installieren. 
- **Sehr große Datenmengen einmal kopieren**: Sie können statische Daten zu einem Teil des verwalteten benutzerdefinierten Images machen, indem Sie sie auf den Datenträger eines verwalteten Images kopieren. Dieser Vorgang muss nur einmal ausgeführt werden und stellt Daten für jeden Knoten des Pools zur Verfügung.
- **Datenträgertypen auswählen**: Sie können ein verwaltetes benutzerdefiniertes Image über eine virtuelle Festplatte, über einen virtuellen Datenträger eines virtuellen Azure-Computers, eine Momentaufnahme dieser Datenträger oder eine eigene Linux- oder Windows-Installation erstellen, die Sie konfiguriert haben. Sie können auswählen, ob Sie Storage Premium für den Betriebssystemdatenträger und den Datenträger verwenden.
- **Pools auf beliebige Größe skalieren**: Wenn Sie ein verwaltetes benutzerdefiniertes Image zum Erstellen eines Pools verwenden, kann der Pool auf eine beliebige Größe anwachsen, die Sie anfordern. Sie müssen keine Kopien von virtuellen Festplatten des Image-Blobs erstellen, um die Anzahl von virtuellen Computern aufzunehmen. 


## <a name="prerequisites"></a>Voraussetzungen

- **Eine verwaltete Imageressource**: Um einen VM-Pool mithilfe eines benutzerdefinierten Images zu erstellen, müssen Sie eine verwaltete Imageressource in demselben Azure-Abonnement und in derselben Region wie das Batch-Konto erstellen. Optionen zum Vorbereiten eines verwalteten Images finden Sie im folgenden Abschnitt.
- **Authentifizierung über Azure Active Directory (AAD)**: Die Batch-Client-API muss die AAD-Authentifizierung verwenden. Die Azure Batch-Unterstützung für AAD ist unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) dokumentiert.

    
## <a name="prepare-a-custom-image"></a>Vorbereiten eines benutzerdefinierten Images
Sie können ein verwaltetes Image über eine virtuelle Festplatte, über einen Azure-VM mit verwalteten Datenträgern oder über eine VM-Momentaufnahme vorbereiten. 

Beachten Sie bei der Vorbereitung Ihres Images die folgenden Punkte:

* Vergewissern Sie sich, dass das Betriebssystem-Basisimage, das Sie zum Bereitstellen Ihrer Batch-Pools verwenden, über keine vorinstallierten Azure-Erweiterungen (beispielsweise die benutzerdefinierte Skripterweiterung) verfügt. Wenn das Image eine vorinstallierte Erweiterung enthält, treten beim Bereitstellen des virtuellen Computers unter Umständen Probleme auf.
* Stellen Sie sicher, dass das von Ihnen bereitgestellte Basisbetriebssystem-Image den standardmäßigen temporären Datenträger verwendet. Der Batch-Knoten-Agent erwartet derzeit den standardmäßigen temporären Datenträger.
* Die verwaltete Imageressource, auf die ein Batch-Pool verweist, kann während der Lebensdauer des Pools nicht gelöscht werden. Wenn die verwaltete Imageressource gelöscht wird, kann der Pool nicht weiter anwachsen. 

### <a name="to-create-a-managed-image"></a>So erstellen Sie ein verwaltetes Image
Sie können einen beliebigen vorhandenen, vorbereiteten Windows- oder Linux-Betriebssystemdatenträger verwenden, um ein verwaltetes Image zu erstellen. Wenn Sie z.B. ein lokales Image verwenden möchten, dann laden Sie den lokalen Datenträger mit AzCopy oder einem anderen Uploadtool in ein Azure Storage-Konto hoch, das sich in dem gleichen Abonnement und der gleichen Region wie das Batch-Konto befindet. Ausführliche Schritte zum Hochladen einer virtuellen Festplatte und zum Erstellen eines verwalteten Images finden Sie in der Anleitung für virtuelle [Windows](../virtual-machines/windows/upload-generalized-managed.md)- oder [Linux](../virtual-machines/linux/upload-vhd.md)-Computer.

Sie können auch ein verwaltetes Image auf Basis eines neuen oder vorhandenen virtuellen Azure-Computers oder einer VM-Momentaufnahme vorbereiten. 

* Wenn Sie einen neuen virtuellen Computer erstellen, können Sie ein Azure Marketplace-Image als Basisimage für Ihr verwaltetes Image verwenden und dann anpassen. 

* Wenn Sie planen, das Image mithilfe des Portals zu erfassen, stellen Sie sicher, dass der virtuelle Computer mit einem verwalteten Datenträger erstellt wird. Dies ist die Standardeinstellung für den Speicher, wenn Sie einen virtuellen Computer erstellen.

* Sobald der virtuelle Computer ausgeführt wird, stellen Sie über RDP (für Windows) oder SSH (für Linux) eine Verbindung mit ihm her. Installieren Sie erforderliche Software, oder kopieren Sie die gewünschten Daten, und generalisieren Sie dann den virtuellen Computer.  

Ausführliche Schritte zum Verallgemeinern eines virtuellen Azure-Computers und zum Erstellen eines verwalteten Images finden Sie in der Anleitung für virtuelle [Windows](../virtual-machines/windows/capture-image-resource.md)- oder [Linux](../virtual-machines/linux/capture-image.md)-Computer.

Je nachdem, wie Sie einen Batch-Pool mit dem Image erstellen möchten, benötigen Sie den folgenden Bezeichner für das Image:

* Die **Ressourcen-ID** des Images im Format `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`, wenn Sie planen, einen Pool mit dem Image mithilfe der Batch-APIs zu erstellen. 
* Der **Name** des Images, wenn Sie planen, das Portal zu verwenden. 





## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Erstellen eines Pools über ein benutzerdefiniertes Image im Portal

Wenn Sie Ihr benutzerdefiniertes Image gespeichert haben und dessen Ressourcen-ID oder den Namen kennen, können Sie von diesem Image einen Batch-Pool erstellen. Die folgenden Schritte veranschaulichen, wie Sie mit dem Azure-Portal einen Pool erstellen.

> [!NOTE]
> Wenn Sie den Pool mit einer der Batch-APIs erstellen, stellen Sie sicher, dass die Identität, die Sie für die AAD-Authentifizierung verwenden, über Berechtigungen für die Imageressource verfügt. Weitere Informationen finden Sie unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md).
>

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto. Das Konto muss sich in dem gleichen Abonnement und der gleichen Region wie die Ressourcengruppe befinden, die das benutzerdefinierte Image enthält. 
2. Wählen Sie links im Fenster **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie im Fenster **Pools** den Befehl **Hinzufügen** aus.
4. Wählen Sie im Fenster **Pool hinzufügen** die Option **Benutzerdefiniertes Image (Linux/Windows)** in der Dropdownliste **Imagetyp** aus. Wählen Sie aus der Dropdownliste **Benutzerdefiniertes VM-Image** den Namen des Abbilds (Kurzform der Ressourcen-ID) aus.
5. Wählen Sie **Verleger/Angebot/SKU** für Ihr benutzerdefiniertes Image aus.
6. Geben Sie die übrigen erforderlichen Einstellungen an, einschließlich **Knotengröße**, **Ziel für dedizierte Knoten** und **Knoten mit niedriger Priorität**, sowie alle gewünschten optionalen Einstellungen.

    Für ein benutzerdefiniertes Image im Microsoft Windows Server Datacenter 2016 wird das Fenster **Pool hinzufügen** z. B. so angezeigt:

    ![Hinzufügen eines Pools aus dem benutzerdefinierten Windows-Image](media/batch-custom-images/add-pool-custom-image.png)
  
Um festzustellen, ob ein vorhandener Pool auf einem benutzerdefinierten Image basiert, überprüfen Sie die **Betriebssystem**-Eigenschaft im Abschnitt mit der Ressourcenzusammenfassung im Fenster **Pool**. Wenn der Pool aus einem benutzerdefinierten Image erstellt wurde, wird er auf **Benutzerdefiniertes VM-Image** festgelegt.

Alle benutzerdefinierten Images, die einem Pool zugeordnet sind, werden im Fenster **Eigenschaften** des Pools angezeigt.
 
## <a name="next-steps"></a>Nächste Schritte

- Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
