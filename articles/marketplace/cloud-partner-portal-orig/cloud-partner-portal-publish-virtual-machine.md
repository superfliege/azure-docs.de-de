---
title: Erstellen eines Angebots für virtuelle Computer | Microsoft-Dokumentation
description: Veröffentlichen Sie einen virtuellen Computer in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 519867b1e0607a769948c86af263c172e810d107
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078115"
---
# <a name="publish-a-virtual-machine-to-azure-marketplace"></a>Veröffentlichen eines virtuellen Computers in Azure Marketplace

Dieser Artikel enthält die Schritte zum Veröffentlichen eines Angebots für virtuelle Computer im Azure Marketplace.

## <a name="prerequisites"></a>Voraussetzungen

Für die Veröffentlichung eines virtuellen Computers im Azure Marketplace gelten folgende technische und nicht technische Voraussetzungen

### <a name="technical"></a>Technisch

-   [Technische Voraussetzungen für das Erstellen eines VM-Images für den Azure Marketplace](../cloud-partner-portal/virtual-machine/cpp-create-technical-assets.md)
-   [Erstellen und Hochladen einer virtuellen Linux-Festplatte](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
-   [Erstellen und Testen eines virtuellen Linux-Computers aus einem Image](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)
-   [Erstellen und Hochladen einer Windows-VHD ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)
-   [Erstellen und Testen eines virtuellen Windows-Computers aus einem Image](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)
-   [Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten](../cloud-partner-portal/virtual-machine/cpp-common-vhd-creation-issues.md)
-   [Sicherheitsempfehlungen für Azure Marketplace-Images](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)


### <a name="non-technical-business-requirements"></a>Nicht technisch (Geschäftsanforderungen)

 -   Der Standort Ihres Unternehmens (oder der Niederlassung) befindet sich in einem vom Azure Marketplace unterstützten Verkäuferland.
-   Ihr Produkt muss auf eine Weise lizenziert sein, die mit den vom Azure Marketplace unterstützten Abrechnungsmodellen kompatibel ist.
-   Sie sind dafür zuständig, technischen Support für Kunden in einer wirtschaftlich vertretbaren Weise zur Verfügung zu stellen. Der Support kann kostenlos, kostenpflichtig oder mittels der Unterstützung durch die Community erfolgen.
-   Sie sind für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware verantwortlich.
-   Sie stellen Inhalte bereit, die die Kriterien erfüllen, damit Ihr Angebot im Azure Marketplace und im Azure-Portal aufgelistet wird.
-   Sie stimmen den Bedingungen der Azure Marketplace-Teilnahmerichtlinien und der Herausgebervereinbarung zu.
-   Stimmen Sie der Einhaltung der [Nutzungsbedingungen](https://azure.microsoft.com/support/legal/website-terms-of-use/), der [Datenschutzerklärung von Microsoft](https://www.microsoft.com/privacystatement/default.aspx) und der [Microsoft Azure Certified-Programmvereinbarung](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/) zu.

## <a name="before-you-begin"></a>Voraussetzungen

Wenn alle Voraussetzungen erfüllt sind, können Sie damit beginnen, Ihr Angebot einer Lösungsvorlage zu erstellen. Bevor Sie beginnen, überprüfen Sie die folgenden Informationen zum Angebot und zur SKU.

**Angebot**

Ein Angebot für eine Azure-Anwendung entspricht einer Produktangebotsklasse eines Herausgebers. Wenn Sie über eine neue Art von Lösung/Anwendung verfügen, die Sie im Azure Marketplace zur Verfügung stellen möchten, ist ein neues Angebot die beste Option. Ein Angebot ist eine Sammlung von SKUs. Jedes Angebot wird als eigene Entität im Azure Marketplace angezeigt.

**SKU**

Eine SKU ist die kleinste kostenpflichtige Einheit eines Angebots. SKUs befinden sich zwar in derselben Produktklasse (Angebot), ermöglichen es Ihnen aber, zwischen verschiedenen unterstützten Features zu unterscheiden. So kann das Angebot beispielsweise verwaltet oder nicht verwaltet sein. Außerdem werden verschiedene Abrechnungsmodelle unterstützt.

Fügen Sie in folgenden Szenarien mehrere SKUs hinzu:
- Sie möchten verschiedene Abrechnungsmodelle unterstützen, z.B. BYOL (Bring Your Own License), nutzungsbasierte Bezahlung (Pay As You Go, PAYG) usw.
- Jede SKU soll unterschiedliche Funktionsmerkmale unterstützen und jeweils separate Preise aufweisen.

Eine SKU wird im Azure Marketplace unter dem übergeordneten Angebot, im Azure-Portal jedoch als eigene Kaufeinheit angezeigt.

## <a name="to-create-a-new-offer"></a>So erstellen Sie ein neues Angebot

1.  Melden Sie sich beim [Cloud-Partnerportal](http://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Navigationsleiste **+ Neues Angebot** und dann **Virtual Machines** aus.

    ![Neues Angebot für virtuelle Computer](./media/cloud-partner-portal-publish-virtual-machine/publishvm1.png)

3.  Wählen Sie unter **Neues Angebot** die Option **Editor** aus.

![Editor für neues Angebot](./media/cloud-partner-portal-publish-virtual-machine/publishvm2.png)

4.  Geben Sie im **Editor** Informationen in den folgenden Ansichten an:
    - Angebotseinstellungen
    - SKUs
    - Marketplace
    - Support – jede Ansicht enthält einen Satz von Feldern, die Sie ausfüllen. Erforderliche Felder sind mit einem roten Sternchen (\*) gekennzeichnet.

## <a name="to-configure-offer-settings"></a>So konfigurieren Sie Angebotseinstellungen

1. Konfigurieren Sie die folgenden Felder zur **Angebots-ID** in den Angebotseinstellungen.

    **Angebots-ID**

     Ein eindeutiger Bezeichner für das Angebot innerhalb eines Herausgeberprofils. Diese ID ist in den Produkt-URLs, den Azure Resource Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Sie dürfen nur klein geschriebene alphanumerische Zeichen oder Bindestriche (-) verwenden. Die ID darf nicht mit einem Bindestrich enden und höchstens 50 Zeichen umfassen. Wenn beispielsweise der Herausgeber **Contoso** ein Angebot mit der Angebots-ID **sample-vm** veröffentlicht, wird diese im Azure Marketplace wie folgt angezeigt: **https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview**  
    >[!Note]
    >Dieses Feld wird gesperrt, sobald ein Angebot live geschaltet wird.

    **Herausgeber-ID**

    Eine Dropdownliste für das Herausgeberprofil. Wählen Sie das Profil aus, unter dem das Angebot veröffentlicht werden soll. 
    >[!Note]
    >Dieses Feld wird gesperrt, sobald ein Angebot live geschaltet wird.

    **Name**

    Der Anzeigenamen für Ihr Angebot. Dieser Name wird im Azure Marketplace und im Azure-Portal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Befolgen Sie diese Hinweise zum Angebotsnamen:
    -  Verwenden Sie einen Markennamen mit hohem Wiedererkennungswert für Ihr Produkt. 
    - Verwenden Sie an dieser Stelle nicht den Namen Ihres Unternehmens, es sei denn, das Angebot wird unter diesem Namen beworben.
    - Wenn Sie dieses Angebot auf Ihrer eigenen Website bewerben, stellen Sie sicher, dass der Name mit dem Namen auf Ihrer Website identisch ist.

2. Wählen Sie **Speichern** aus, um die Angebotseinstellungen zu beenden.

## <a name="to-create-a-sku"></a>So erstellen Sie eine SKU

1. Wählen Sie **SKUs** aus. 
2. Wählen Sie **SKU hinzufügen** aus, um eine SKU-ID einzugeben. Die SKU-ID ist ein eindeutiger Bezeichner für die SKU in einem Angebot. Diese ID ist in den Produkt-URLs, den Azure Resource Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Die SKU-ID:
    - Darf maximal 50 Zeichen umfassen.
    - Darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen.
    - Die ID darf nicht mit einem Gedankenstrich enden.

    ![Hinzufügen einer SKU](./media/cloud-partner-portal-publish-virtual-machine/publishvm4.png)


## <a name="configure-sku-details"></a>Konfigurieren von SKU-Details

Nachdem Sie eine SKU hinzugefügt haben, wird sie in der Liste der SKUs in der Ansicht „SKUs“ angezeigt. Um Details zur SKU anzuzeigen, wählen Sie den Namen der SKU aus. In der Detailansicht können Sie in den folgenden Feldern Informationen hinzufügen.

### <a name="hide-this-sku"></a>Diese SKU ausblenden

Verwenden Sie diese Einstellung, um die Sichtbarkeit der SKU zu verwalten. Wenn „Diese SKU ausblenden“ deaktiviert ist, wird die SKU in [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) für Kunden angezeigt. Sie sollten die SKU nur dann ausblenden, wenn sie über Lösungsvorlagen verfügbar und nicht einzeln erhältlich sein soll.

### <a name="cloud-availability"></a>Cloudverfügbarkeit

Dieses Feld ermöglicht es Ihnen, die Verfügbarkeit Ihrer SKU in den verschiedenen Azure-Clouds festzulegen.

**Azure, öffentlich**

Diese SKU kann für Kunden in allen öffentlichen Azure-Regionen bereitgestellt werden, die über Marketplace-Integration verfügen.

**Azure Government-Cloud**

Diese SKU kann in der Azure Government-Cloud bereitgestellt werden. Den Herausgebern wird empfohlen, vor der Veröffentlichung in [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) zu überprüfen und zu testen, ob ihre Lösung erwartungsgemäß funktioniert. Fordern Sie zum Bereitstellen und Testen ein [Probekonto](https://azure.microsoft.com/offers/ms-azr-usgov-0044p) an. 

>[!Note]
>Microsoft Azure Government ist eine Government-Community-Cloud mit kontrolliertem Zugriff für Kunden der US-Behörden auf Bundes-, Bundesstaats-, Gemeinde- und Stammesgebietsebene UND für Partner, die Dienste für diese Entitäten erbringen können.

### <a name="countryregion-availability"></a>Verfügbarkeit in Land/Region

Über dieses Feld werden die Regionen identifiziert, in denen die SKU erhältlich sein soll. Sie müssen sorgfältig überlegen, wo Sie die SKUs zur Verfügung stellen. Einige Länder sind als „Microsoft-Steuererlassländer“ klassifiziert.

-   In den „Microsoft-Steuererlassländern“ zieht Microsoft Steuern von den Kunden ein und zahlt Steuern an die Behörden.

-   In anderen Ländern sind die Partner für die Einnahme der Steuern bei den Kunden und die Steuerzahlung an den Staat verantwortlich. Wenn Sie in diesen Ländern als Verkäufer tätig werden möchten, müssen Sie in der Lage sein, dort Steuern zu berechnen und zu zahlen.

![Ausgewählte Verfügbarkeit in Land/Region](./media/cloud-partner-portal-publish-virtual-machine/publishvm5.png)

### <a name="pricing"></a>Preise

Derzeit werden zwei Preismodelle unterstützt.

#### <a name="bring-your-own-license-byol"></a>Verwendung Ihrer eigenen Lizenz

Sie übernehmen die Lizenzierung der Software, die auf dem virtuellen Computer ausgeführt wird. Microsoft berechnet lediglich die Kosten für die Infrastruktur.

#### <a name="usage-based-monthly-billed-sku"></a>Nutzungsbasierte, monatlich abgerechnete SKU

Die Abrechnung für den Kunden erfolgt auf Stundenbasis gemäß den Gebühren, die von den Herausgebern für die VM-Größen festgelegt sind. Beim **stündlichen Abrechnungsmodell** ergibt sich der Gesamtpreis aus der Summe der vom Herausgeber in Rechnung gestellten Softwarekosten und der von Microsoft in Rechnung gestellten Infrastrukturkosten. Diese Gesamtkosten werden dem Kunden als stündlicher und monatlicher Preis angezeigt, wenn er den Kauf erwägt. In diesem Fall erfolgt die Abrechnung monatlich.

Für das nutzungsbasierte Modell müssen Sie zusätzliche Einstellungen festlegen.

**Kostenlose Testversion**

Sie können angeben, ob Sie eine kostenlose Testversion für Ihre Kunden bereitstellen möchten.
Hier werden dem Kunde für die ersten 30/90 Tage (von der Auswahl abhängig) nach der Bereitstellung des virtuellen Computers keine Softwarekosten in Rechnung gestellt. Nachdem die kostenlose Testversion abgelaufen ist, erfolgt die Abrechnung für den Kunden auf Stundenbasis gemäß der Gebühren, die von den Herausgebern im stündlichen Abrechnungsmodell festgelegt sind.

**Preis pro Kern**

Sie können einen Preis pro Kern für die SKU festlegen. Für diese Option müssen Sie lediglich den Grundpreis für einen einzelnen Kern eingeben. Anschließend werden die Preise für die restlichen Kerne automatisch berechnet. Geben Sie die Preise im Portal in US-Dollar ein, und die Preise für andere Regionen werden automatisch berechnet. Sie können die Preise in den anderen Regionen überprüfen, indem Sie **Preisdaten exportieren** verwenden.

![Preis pro Kern](./media/cloud-partner-portal-publish-virtual-machine/publishvm6.png)


**Separate Preise**

Sie können die Preise für jeden Kernsatz einzeln festlegen, wenn Sie für jeden Kern einen separaten Preis angeben möchten.

![Separate Preise](./media/cloud-partner-portal-publish-virtual-machine/publishvm7.png)

**Exportieren und Importieren von Preisen**

Sie können die im Portal konfigurierten Preise exportieren, um über die Excel-Schnittstelle Änderungen vorzunehmen. Mit dieser Option können Sie auch die Preise pro Region und die Preise in lokalen Währungen überprüfen.
Klicken Sie auf **Exportpreise**, um eine Excel-Datei mit den bereits eingetragenen Preisdetails herunterzuladen. Diese Details können Sie dann in Excel bearbeiten. Verwenden Sie anschließend **Importpreise**, um die vorgenommenen Änderungen zu importieren.
Die importierten Preise werden auch im Portal widergespiegelt.

In dieser Excel-Preistabelle werden die Preise für die verschiedenen Regionen in den lokalen Währungen aufgeführt. Der verwendete Wechselkurs wird täglich aktualisiert.

![Exportieren und Importieren von Preisen mit Beispielwechselkursen](./media/cloud-partner-portal-publish-virtual-machine/publishvm8.png)

>[!IMPORTANT]
>-   Nachdem ein Angebot live geschaltet wurde, können Preise nicht mehr geändert werden. Die unterstützten Regionen können Sie aber möglicherweise weiterhin hinzufügen oder entfernen.
>-   Dieser Preis wird dem Benutzer zusätzlich zu den  [Preisen für virtuelle Azure-Computer](https://aka.ms/vmpricingdetails) berechnet.
>-   Die Preise werden für alle Regionen in der lokalen Währung mit den zum Zeitpunkt der Preisfestlegung verfügbaren Wechselkursen festgelegt.
>-   Um die Preise der jeweiligen Regionen einzeln festzulegen oder anzuzeigen, exportieren Sie die Preiskalkulationstabelle und importieren Sie sie mit den benutzerdefinierten Preisen.

## <a name="vm-images"></a>VM-Images

Als Nächstes wird der Abschnitt für die VM-Images bearbeitet. Bevor Sie mit diesem Abschnitt beginnen, sollten Sie die VHD vorbereiten, die Sie veröffentlichen möchten. Die folgenden Links helfen Ihnen beim Erstellen der VHD:

-   [Technische Voraussetzungen für das Erstellen eines VM-Images für den Azure Marketplace](../cloud-partner-portal/virtual-machine/cpp-prerequisites.md)
-   [Erstellen und Hochladen einer Linux-VHD](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
-   [Erstellen und Testen eines virtuellen Linux-Computers aus einem Image](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)
-   [Erstellen und Hochladen einer Windows-VHD ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)
-   [Erstellen und Testen eines virtuellen Windows-Computers aus einem Image](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)
-   [Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten](../cloud-partner-portal/virtual-machine/cpp-common-vhd-creation-issues.md)

Wenn die VHD vorbereitet ist, können Sie beginnen, diesen Abschnitt zu bearbeiten.  Hier finden Sie einige Details für einige der Felder.

### <a name="recommended-vm-sizes"></a>Empfohlene VM-Größen

Wählen Sie bis zu sechs empfohlene VM-Größen aus. Diese Empfehlungen werden für Kunden im Azure Marketplace und auf dem Blatt „Tarif“ im Azure-Portal angezeigt, wenn sie Ihr Image erwerben und bereitstellen möchten. *Bei diesen Größen handelt es sich nur um Empfehlungen. Der Kunde kann eine beliebige VM-Größe auswählen, die sich für die in Ihrem Image festgelegten Datenträger eignet.*  Der folgende Screenshot zeigt die empfohlenen VM-Größen, die einem Kunden im Azure-Portal angezeigt werden.


![Empfohlene VM-Größen](./media/cloud-partner-portal-publish-virtual-machine/publishvm9.png)


### <a name="open-ports"></a>Offene Ports

Geben Sie die Ports an, die Sie öffnen und verfügbar machen möchten. Diese Ports werden während der Bereitstellung des virtuellen Computers geöffnet.

### <a name="adding-vm-images"></a>Hinzufügen von VM-Images

Als Nächstes fügen Sie ein VM-Image für die SKU hinzu. Sie können bis zu acht Datenträgerversionen pro SKU hinzufügen. Im Azure Marketplace wird nur die höchste Datenträger-Versionsnummer für eine SKU angezeigt. Die weiteren Nummern sind über APIs sichtbar.

Wählen Sie unter der **Datenträgerversion** die Option **+ Neue Version** aus. Mit dieser Option werden die folgenden Felder angezeigt, die Sie ausfüllen müssen.

#### <a name="vm-image-version"></a>VM-Imageversion

Die VM-Imageversion muss dem Format der [semantischen Version](http://semver.org/) entsprechen. Versionen sollten im Format „X.Y.Z“ angegeben werden. „X“, „Y“ und „Z“ sind dabei ganze Zahlen. Images in verschiedenen SKUs können verschiedene Haupt- und Nebenversionen enthalten. Bei Versionen innerhalb einer SKU sollte es sich nur um inkrementelle Änderungen handeln, die die Patchversion erhöhen („Z“ aus „X.Y.Z“).

#### <a name="os-vhd-url"></a>URL DER BETRIEBSSYSTEM-VHD

Geben Sie den [Shared Access Signature-URI](../cloud-partner-portal/virtual-machine/cpp-get-sas-uri.md) ein, der für die Betriebssystem-VHD erstellt wurde.

Wenn der SKU Datenträger zugeordnet sind, können Sie diese Datenträger hinzufügen, indem Sie den Link **+ Neuer Datenträger** auswählen. Mit dieser Aktion werden zusätzliche Felder angezeigt, die Sie ausfüllen können.

#### <a name="lun-vhd-url"></a>URL DER LUN-VHD

Geben Sie den [Shared Access Signature-URI](../cloud-partner-portal/virtual-machine/cpp-get-sas-uri.md) für den Datenträger ein.

#### <a name="lun-number"></a>LUN-Nummer

Weisen Sie dieser LUN eine Nummer zu. Diese Nummer wird für diesen Datenträger in dieser SKU reserviert.

>[!Note]
>Sobald Sie eine SKU mit einer bestimmten VM-Version und Datenträgern veröffentlichen, werden diese gesperrt und können nicht mehr geändert werden. Bei jeder weiteren VM-Version, die der SKU hinzugefügt wird, kann die Anzahl der Datenträger, die unterstützt werden müssen, nicht geändert werden.

#### <a name="common-sas-url-issues--fixes"></a>Häufige Probleme mit der SAS-URL und Behebung

| Problem                                                                 | Message                                                                           | Behebung                                                           |  Link zur Dokumentation                                                                                |
|---------------------------------------------------------------------  |-------------------------------------------------------------------------------    |-----------------------------------------------------------    |---------------------------------------------------------------------------------------------------    |
| Fehler beim Kopieren von Bildern: „?“ nicht in der SAS-URL enthalten                | Fehler: Image wird kopiert. Blob kann mit dem angegebenen SAS-URI nicht heruntergeladen werden.       | Aktualisieren Sie die SAS-URL mithilfe empfohlener Tools.                    | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fehler beim Kopieren von Bilder: Parameter „st“ und „se“ nicht in SAS-URL enthalten   | Fehler: Image wird kopiert. Blob kann mit dem angegebenen SAS-URI nicht heruntergeladen werden.        | Aktualisieren Sie die SAS-URL mit Start- und Enddatum.             | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fehler beim Kopieren von Bildern: „sp=rl“ nicht in der SAS-URL enthalten                    | Fehler: Image wird kopiert. Blob kann mit dem angegebenen SAS-URI nicht heruntergeladen werden.         | Aktualisieren Sie die SAS-URL durch Festlegen der Berechtigungen „Lesen“ und „Auflisten“.     | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fehler beim Kopieren von Bildern: SAS-URL enthält Leerzeichen im VHD-Namen.     | Fehler: Image wird kopiert. Blob kann mit dem angegebenen SAS-URI nicht heruntergeladen werden.        | Entfernen Sie die Leerzeichen aus der SAS-URL.                       | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fehler beim Kopieren von Bildern: SAS-URL-Autorisierungsfehler               | Fehler: Image wird kopiert. Blob aufgrund eines Autorisierungsfehlers nicht heruntergeladen werden.     | Erstellen Sie die SAS-URL neu.                                        | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |


## <a name="to-configure-the-marketplace"></a>So konfigurieren Sie den Marketplace

Verwenden Sie die Marketplace-Ansicht, um die Felder zu konfigurieren, die für das Angebot im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) angezeigt werden.

### <a name="preview-subscription-ids"></a>Preview-Abonnement-IDs

Die Liste der Azure-Abonnement-IDs, die nach der Veröffentlichung des Angebots Zugriff auf dieses haben sollen. Mithilfe dieser zugelassenen Abonnements können Sie das Angebot in der Vorschau testen, bevor es online geschaltet wird. Im Partnerportal können Sie bis zu 100 Abonnements zulassen.

### <a name="suggested-categories"></a>Vorgeschlagene Kategorien

Wählen Sie bis zu fünf Kategorien aus der vorhandenen Liste, denen Ihr Angebot am besten zugeordnet werden kann. Die ausgewählten Kategorien werden verwendet, um Ihr Angebot den Produktkategorien zuzuordnen, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) verfügbar sind.

Die folgenden Beispiele zeigen die Marketplace-Informationen im Azure Marketplace und im Azure-Portal.

**Azure Marketplace**


![publishvm10](./media/cloud-partner-portal-publish-virtual-machine/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-virtual-machine/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-virtual-machine/publishvm15.png)


**Azure-Portal**


![publishvm12](./media/cloud-partner-portal-publish-virtual-machine/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-virtual-machine/publishvm13.png)


### <a name="logo-guidelines"></a>Richtlinien für Logos

Befolgen Sie die folgenden Richtlinien für Logos, die Sie im Cloud-Partnerportal hochladen:

-   Die Farbpalette des Azure-Designs ist einfach und geradlinig. Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben.

-   Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihre Logos. Verwenden Sie eine Farbe, die Ihre Logos im Azure-Portal markant hervorstechen lässt. Sie sollten einfache Primärfarben verwenden.

    >[!Note] 
    >Wenn Sie transparenten Hintergrund verwenden, stellen Sie sicher, dass Logos und Text nicht weiß, schwarz oder blau sind.

-   Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.

-   Platzieren Sie nach Möglichkeit keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen.  Das Erscheinungsbild Ihres Logos sollte *klar und direkt* und frei von Farbverläufen sein.

-   Das Logo sollte nicht gestreckt werden.

#### <a name="hero-logo"></a>Herologo

Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Nachdem das Logo hochgeladen wurde, kann es jedoch nicht mehr gelöscht werden. Der Partner muss die Azure Marketplace-Richtlinien für Herosymbole einhalten.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlinien für das Herologosymbol

-   Anzeigename des Herausgebers, Plantitel und ausführliche Angebotsübersicht werden in weißer Schrift angezeigt. Vermeiden Sie helle Farben im Hintergrund. Ein schwarzer, weißer oder transparenter Hintergrund ist für Herosymbole nicht zulässig.

-   Der Anzeigename des Herausgebers, der Plantitel, die ausführliche Angebotsübersicht und die Schaltfläche „Erstellen“ werden programmgesteuert in das Herosymbol eingebettet, wenn das Angebot live geschaltet wird. Geben Sie beim Entwerfen des Herosymbols keinen Text ein. Lassen Sie einen Bereich auf der rechten Seite des Logos leer. Dieser Bereich sollte 415 x 100 Pixel groß und 370 Pixel von links versetzt sein.

![Beispiel für Herologo](./media/cloud-partner-portal-publish-virtual-machine/publishvm14.png)

### <a name="lead-management"></a>Leadverwaltung

Um die Einstellungen für die Leadverwaltung des Angebots zu konfigurieren, befolgen Sie [diese Anweisungen](./cloud-partner-portal-get-customer-leads.md).

## <a name="to-configure-support"></a>So konfigurieren Sie den Support

Verwenden Sie die Support-Ansicht, um die folgenden Informationen anzugeben:

- Supportkontakte Ihres Unternehmens, z.B. Technik
- Kundensupportkontakte

## <a name="to-publish-the-offer"></a>So veröffentlichen Sie das Angebot

Der letzte Schritt ist das Veröffentlichen des Angebots. Befolgen Sie [diese Anweisungen, um Ihr Angebot live zu schalten](./cloud-partner-portal-make-offer-live-on-azure-marketplace.md).
