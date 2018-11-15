---
title: Veröffentlichen einer Lösungsvorlage | Microsoft-Dokumentation
description: Veröffentlichen einer Lösungsvorlage im Azure Marketplace
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 5320b8d5ca7456a6f1b0fdd1372c9f39ac1edfb0
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219408"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Veröffentlichen einer Lösungsvorlage im Azure Marketplace

Dieser Artikel enthält die Schritte zum Veröffentlichen eines Angebots für eine Lösungsvorlage im Azure Marketplace.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden technischen und sonstigen Voraussetzungen gelten für Listings von Lösungsvorlagen im Azure Marketplace.

### <a name="technical"></a>Technisch

- [Verstehen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

- Azure-Schnellstartvorlagen:

    - [Dokumentation zur Azure-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/)

    - [Dokumentation zum Azure-Schnellstart auf GitHub](https://github.com/azure/azure-quickstart-templates)

 - [Erstellen einer Benutzeroberflächendatei für das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

### <a name="non-technical-business-requirements"></a>Nicht technisch (Geschäftsanforderungen)

-   Der Standort Ihres Unternehmens (oder der Niederlassung) muss sich in einem vom Azure Marketplace unterstützten Verkäuferland befinden.

-   Ihr Produkt muss auf eine Weise lizenziert sein, die mit den vom Azure Marketplace unterstützten Abrechnungsmodellen kompatibel ist.

-   Sie müssen in wirtschaftlich vertretbarem Maße technischen Support für Kunden anbieten – kostenlos, kostenpflichtig oder über die Community.

-   Sie sind für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware verantwortlich.

-   Sie stellen Inhalte bereit, die die Kriterien erfüllen, damit Ihr Angebot im Azure Marketplace und im Azure-Verwaltungsportal aufgelistet wird.

-   Sie stimmen den Bedingungen der Azure Marketplace-Teilnahmerichtlinien und der Herausgebervereinbarung zu.

-   Stimmen Sie der Einhaltung der Nutzungsbedingungen, der Datenschutzerklärung von Microsoft und der Microsoft Azure Certified-Programmvereinbarung zu.

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

1.  Melden Sie sich beim [Cloudpartnerportal](http://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Navigationsleiste **+ Neues Angebot** und dann **Azure-Anwendungen** aus.

    ![Erstellen eines neuen Angebots](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Wählen Sie unter **Neues Angebot** die Option **Editor** aus.

    ![Editor für neues Angebot](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  Geben Sie im **Editor** Informationen in den folgenden Ansichten an:
    - Angebotseinstellungen
    - SKUs
    - Marketplace
    - Support

Jede Ansicht enthält eine Reihe von Feldern, die Sie ausfüllen. Erforderliche Felder sind mit einem roten Sternchen (\*) gekennzeichnet.

## <a name="to-configure-offer-settings"></a>So konfigurieren Sie Angebotseinstellungen

1. Konfigurieren Sie die folgenden Felder zur **Angebots-ID** in den Angebotseinstellungen.

    **Angebots-ID**

     Ein eindeutiger Bezeichner für das Angebot innerhalb eines Herausgeberprofils. Diese ID ist in den Produkt-URLs, den ARM-Vorlagen und den Abrechnungsberichten sichtbar. Sie dürfen nur klein geschriebene alphanumerische Zeichen oder Bindestriche (-) verwenden. Die ID darf nicht mit einem Bindestrich enden und höchstens 50 Zeichen umfassen. 
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
.

## <a name="to-create-skus"></a>So erstellen Sie SKUs
------------------

1. Wählen Sie **SKUs** aus. 

    ![Neue SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    Die SKU-ID ist ein eindeutiger Bezeichner für die SKU in einem Angebot. Diese ID ist in den Produkt-URLs, den ARM-Vorlagen und den Abrechnungsberichten sichtbar. Die SKU-ID:
    - Darf maximal 50 Zeichen umfassen.
    - Darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen.
    - Die ID darf nicht mit einem Gedankenstrich enden.

    >[!Note]
    >Nach dem Hinzufügen einer SKU wird diese in der Liste der SKUs in der Ansicht „SKUs“ angezeigt. Um Details zur SKU anzuzeigen, wählen Sie den Namen der SKU aus. 

2. Wählen Sie **Neue SKU** aus, um die im folgenden Screenshot gezeigten Informationen anzugeben. 

    ![SKU-Details](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>SKU-Einstellungen

Geben Sie die folgenden SKU-Einstellungen an.

- **Titel:** ein Titel für die SKU. Dieser Titel wird im Katalog für dieses Element angezeigt.

- **Zusammenfassung:** eine kurze Beschreibung der SKU. (Die maximale Länge beträgt 100 Zeichen.)

- **Beschreibung:** eine ausführliche Beschreibung der SKU.

- **SKU-Typ:** eine Dropdownliste mit den folgenden Werten: „Verwaltete Anwendung (Vorschau)“ und „Lösungsvorlage“. Wählen Sie für dieses Szenario **Lösungsvorlage** aus.

- **Cloudverfügbarkeit:** der Standort der SKU. Der Standardwert ist öffentliches Azure.

### <a name="package-details"></a>Paketdetails

Nachdem Sie mit den SKU-Einstellungen fertig sind, geben Sie die folgenden Paketdetails an.

![Paketdetails](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Aktuelle Version:** die Version des Pakets, das Sie hochladen. Sie sollte das folgende Format aufweisen: ..

- **Paketdatei:** Dieses Paket enthält die folgenden Dateien, die in einer ZIP-Datei gespeichert werden.

    -   MainTemplate.json: Die Vorlagendatei für die Bereitstellung, die zum Bereitstellen der Lösung/Anwendung und zum Erstellen der darin definierten Ressourcen für die Lösung verwendet wird. Weitere Informationen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).

    -   createUIDefinition.json: Diese Datei wird vom Azure-Portal verwendet, um die Benutzeroberfläche für die Bereitstellung dieser Lösung/Anwendung zu generieren. Weitere Informationen finden Sie unter [Erstellen einer Benutzeroberfläche im Azure-Portal für die verwaltete Anwendung](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

    >[!IMPORTANT]
    >Dieses Paket sollte alle anderen geschachtelten Vorlagen oder Skripts enthalten, die erforderlich sind, um diese Anwendung erfolgreich bereitzustellen. Die Dateien „mainTemplate.json“ und „createUiDefinition.json“ müssen im Stammverzeichnis enthalten sein.

## <a name="to-configure-the-marketplace"></a>So konfigurieren Sie den Marketplace

Verwenden Sie die Marketplace-Ansicht, um die Felder, die für das Angebot im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) angezeigt werden, zu konfigurieren.

### <a name="preview-subscription-ids"></a>Preview-Abonnement-IDs

Die Liste der Azure-Abonnement-IDs, die nach der Veröffentlichung des Angebots Zugriff auf dieses haben sollen. Mithilfe dieser zugelassenen Abonnements können Sie das Angebot in der Vorschau testen, bevor es online geschaltet wird. Im Partnerportal können Sie bis zu 100 Abonnements zulassen.

### <a name="suggested-categories"></a>Vorgeschlagene Kategorien

Wählen Sie bis zu 5 Kategorien aus der Liste, denen Ihr Angebot am besten zugeordnet werden kann. Die ausgewählten Kategorien werden verwendet, um Ihr Angebot den Produktkategorien zuzuordnen, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) verfügbar sind.

Die folgenden Beispiele zeigen die Marketplace-Informationen im Azure Marketplace und im Azure-Portal.

**Azure Marketplace**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**Azure-Portal**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


### <a name="logo-guidelines"></a>Richtlinien für Logos

Befolgen Sie die folgenden Richtlinien für Logos, die Sie im Cloud-Partnerportal hochladen:

-   Die Farbpalette des Azure-Designs ist einfach und geradlinig. Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben.

-   Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihre Logos. Verwenden Sie eine Farbe, die Ihre Logos im Azure-Portal markant hervorstechen lässt. Sie sollten einfache Primärfarben verwenden.

    >[!Note] 
    >Wenn Sie transparenten Hintergrund verwenden, stellen Sie sicher, dass Logos und Text nicht weiß, schwarz oder blau sind.

-   Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.

-   Vermeiden Sie die Platzierung von Text auf dem Logo. Dies schließt Ihren Firmen- oder Markennamen ein. Das Erscheinungsbild Ihres Logos sollte *klar und direkt* und frei von Farbverläufen sein.

-   Das Logo sollte nicht gestreckt werden.

#### <a name="hero-logo"></a>Herologo

Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Nachdem das Logo hochgeladen wurde, kann es jedoch nicht mehr gelöscht werden. Der Partner muss die Azure Marketplace-Richtlinien für Herosymbole einhalten.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlinien für das Herologosymbol

-   Anzeigename des Herausgebers, Plantitel und ausführliche Angebotsübersicht werden in weißer Schrift angezeigt. Vermeiden Sie helle Farben im Hintergrund. Ein schwarzer, weißer oder transparenter Hintergrund ist für Herosymbole nicht zulässig.

-   Der Anzeigename des Herausgebers, der Plantitel, die ausführliche Angebotsübersicht und die Erstellen-Schaltfläche werden programmgesteuert in das Herosymbol eingebettet, wenn das Angebot live geschaltet wird. Geben Sie beim Entwerfen des Herosymbols keinen Text ein. Lassen Sie einen Bereich auf der rechten Seite des Logos leer. Dieser Bereich sollte 415 x 100 Pixel groß und 370 Pixel von links versetzt sein.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>So konfigurieren Sie den Support

Verwenden Sie die Support-Ansicht, um die folgenden Informationen anzugeben:

- Supportkontakte Ihres Unternehmens, z.B. Technik
- Kundensupportkontakte

## <a name="to-publish-the-offer"></a>So veröffentlichen Sie das Angebot

Der letzte Schritt ist das Veröffentlichen des Angebots. Wählen Sie **Veröffentlichen**.
