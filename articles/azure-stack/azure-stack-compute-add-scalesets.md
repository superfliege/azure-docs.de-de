---
title: Bereitstellen von VM-Skalierungsgruppen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Cloudbediener dem Azure Stack-Marketplace VM-Skalierungsgruppen hinzufügen können.
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 06/05/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: ddde2e6bad8a373df405ac05e78a5dbccd0257fc
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "34800639"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Bereitstellen von VM-Skalierungsgruppen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

VM-Skalierungsgruppen sind eine Azure Stack-Computeressource. Verwenden Sie sie zum Bereitstellen und Verwalten eines Satzes identischer virtueller Computer. Da alle virtuellen Computer gleich konfiguriert sind, ist bei Skalierungsgruppen keine Vorabbereitstellung der virtuellen Computer erforderlich. Es ist einfacher, umfangreiche Dienste zu erstellen, die auf hohe Rechenleistung, Big Data und Workloads in Containern ausgelegt sind.

Dieser Artikel beschreibt, wie Skalierungsgruppen im Azure Stack-Marketplace verfügbar gemacht werden. Wenn Sie dieses Verfahren abgeschlossen haben, können Ihre Benutzer VM-Skalierungsgruppen zu ihren Abonnements hinzufügen.

VM-Skalierungsgruppen in Azure Stack sind wie VM-Skalierungsgruppen in Azure. Weitere Informationen finden Sie in den folgenden Videos:
* [Mark Russinovich spricht über Azure-Skalierungsgruppen](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [VM-Skalierungsgruppen mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

In Azure Stack unterstützen VM-Skalierungsgruppen keine automatische Skalierung. Sie können einer Skalierungsgruppe weitere Instanzen hinzufügen, indem Sie Resource Manager-Vorlagen, die CLI oder PowerShell verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- **Marketplace-Syndikation**  
    Registrieren Sie Azure Stack global für Azure, um die Marketplace-Syndikation zu ermöglichen. Befolgen Sie die Anleitung unter [Registrieren von Azure Stack in Azure](azure-stack-registration.md).
- **Betriebssystemimage**  
    Wenn Sie dem Azure Stack-Marketplace kein Betriebssystemimage hinzugefügt haben, helfen Ihnen die Informationen unter [Hinzufügen eines Azure Stack-Marketplace-Elements aus Azure](asdk/asdk-marketplace-item.md) weiter.

## <a name="add-the-virtual-machine-scale-set"></a>Hinzufügen der VM-Skalierungsgruppe

1. Öffnen Sie den Azure Stack-Marketplace, und stellen Sie eine Verbindung mit Azure her. Wählen Sie **Marketplace management**> **+ Add from Azure** (Marketplace-Verwaltung > + Aus Azure hinzufügen).

    ![Marketplace-Verwaltung](media/azure-stack-compute-add-scalesets/image01.png)

2. Fügen Sie das Marketplace-Element für VM-Skalierungsgruppen (Virtual Machine Scale Sets) hinzu, und laden Sie es herunter.

    ![VM-Skalierungsgruppe](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Aktualisieren von Images in einer VM-Skalierungsgruppe

Nach der Erstellung einer VM-Skalierungsgruppe können Benutzer Images in der Skalierungsgruppe aktualisieren, ohne dass diese neu erstellt werden muss. Die Vorgehensweise zum Aktualisieren eines Images hängt von den folgenden Szenarien ab:

1. In der Bereitstellungsvorlage der VM-Skalierungsgruppe ist **latest** für *version* angegeben:  

   Wenn im Abschnitt *imageReference* der Vorlage für eine Skalierungsgruppe *version* auf **latest** festgelegt ist, nutzen Vorgänge zum zentralen Hochskalieren für die Skalierungsgruppe die neueste verfügbare Version des Images für die Skalierungsgruppeninstanzen. Nach dem Abschluss eines Vorgangs zum zentralen Hochskalieren können Sie ältere VM-Skalierungsgruppeninstanzen löschen.  (Die Werte für *publisher*, *offer* und *sku* bleiben unverändert). 

   Hier sehen Sie ein Beispiel für die Angabe von *latest*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Bevor beim zentralen Hochskalieren ein neues Image verwendet werden kann, müssen Sie dieses Image herunterladen:  

   - Wenn das Image im Marketplace neuer ist als das Image in der Skalierungsgruppe: Laden Sie das neue Image herunter, das das ältere Image ersetzt. Nachdem der Benutzer das Image ersetzt hat, kann er mit dem zentralen Hochskalieren fortfahren. 

   - Wenn die Imageversion im Marketplace der Imageversion in der Skalierungsgruppe entspricht: Löschen Sie das in der Skalierungsgruppe verwendete Image, und laden Sie das neue Image herunter. In der Zeit zwischen dem Entfernen des ursprünglichen Images und dem Herunterladen des neuen Images können Sie nicht zentral hochskalieren. 
      
     Bei diesem Vorgang müssen Images neu syndiziert werden, die das in Version 1803 eingeführte Sparsedateiformat verwenden. 
 

2. In der Bereitstellungsvorlage der VM-Skalierungsgruppe ist **nicht** „latest“ für *version*, sondern eine Versionsnummer angegeben:  

    Wenn Sie ein Image mit einer neueren Version herunterladen (wodurch sich die verfügbare Version ändert), kann die Skalierungsgruppe nicht zentral hochskaliert werden. Dies ist beabsichtigt, da die in der Skalierungsgruppenvorlage angegebene Imageversion verfügbar sein muss.  

Weitere Informationen finden Sie unter [Einführung in virtuelle Azure Stack-Computer](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Entfernen einer VM-Skalierungsgruppe

Führen Sie zum Entfernen eines Katalogelements einer VM-Skalierungsgruppe den folgenden PowerShell-Befehl aus:

```PowerShell  
    Remove-AzsGalleryItem
````

> [!NOTE]
> Das Katalogelement wird unter Umständen nicht sofort entfernt. Möglicherweise müssen Sie das Portal mehrmals aktualisieren, bevor das Element aus dem Marketplace angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte
[Häufig gestellte Fragen zu Azure Stack](azure-stack-faq.md)