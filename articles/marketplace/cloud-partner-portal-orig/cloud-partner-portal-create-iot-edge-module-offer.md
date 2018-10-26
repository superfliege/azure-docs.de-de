---
title: Erstellen eines IoT Edge-Modulangebots | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein neues IoT Edge-Modul für den Marketplace veröffentlichen.
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
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: d3cc1a09963c5f7fee613af24c63fd15b1cfffee
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806052"
---
# <a name="how-to-publish-a-new-iot-edge-module-in-the-cloud-partner-portal"></a>Veröffentlichen eines neuen IoT Edge-Moduls im Cloud-Partnerportal

In diesem Artikel werden die Schritte zum Veröffentlichen eines neuen IoT Edge-Modulangebots beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie ein IoT Edge-Modul für den Azure Marketplace veröffentlichen möchten, müssen folgende Voraussetzungen erfüllt sein:

-   Sie müssen über Zugriff auf das [Cloud-Partnerportal (CPP)](https://cloudpartner.azure.com/#alloffers) verfügen. Weitere Informationen finden Sie im [Leitfaden zur Veröffentlichung](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   Ihr IoT Edge-Modul muss in einer Azure Container Registry gehostet werden.

-   Sie müssen unter anderem über folgende Metadaten Ihres IoT Edge-Moduls verfügen:

    -   Titel

    -   Beschreibung (einfaches HTML-Format)

    -   Ein Logo im PNG-Bildformat und in folgenden Größen: 40 x 40 Pixel, 90 x 90 Pixel, 115 x 115 Pixel, 255 x 115 Pixel.

    -   Nutzungsbedingungen und Datenschutzrichtlinie

<a name="prepare-your-iot-edge-module-listing-in-cpp"></a>Vorbereiten Ihres IoT Edge-Modulangebots im CPP
-------------------------------------------

### <a name="create-a-new-offer-of-the-type-iot-edge-module"></a>Erstellen eines neuen Angebots vom Typ „IoT Edge-Modul“ 

Gehen Sie wie folgt vor, um Ihr IoT Edge-Modulangebot vorzubereiten:

-   Melden Sie sich bei Ihrem [CPP-Konto](https://cloudpartner.azure.com/) an.

>[!Note]
>Allgemeine Informationen zum Cloud-Partnerportal finden Sie in [dieser Dokumentation](https://cloudpartner.azure.com/#learn).

-   Klicken Sie auf **Neues Angebot** und anschließend auf **IoT Edge-Modul**.

>[!NOTE]
>Bei einem IoT Edge-Modul handelt es sich um einen Container, der speziell für die Ausführung in IoT Edge konzipiert ist. Die von einem IoT Edge-Modul abgedeckten Szenarien müssen auf den Kontext von IoT Edge ausgerichtet sein. Der Container enthält auch Standardkonfigurationseinstellungen, um die Bereitstellung auf einem IoT Edge-Gerät zu vereinfachen. Darüber hinaus kann der Container das SDK für IoT Edge-Module enthalten, um die Kommunikation mit edgeHub und IoT Hub zu ermöglichen.

### <a name="define-your-offer-settings"></a>Definieren Ihrer Angebotseinstellungen

Geben Sie auf der Registerkarte „Angebotseinstellungen“ die Informationen für Ihr Angebot ein.

![Angebots-ID](./media/cloud-partner-portal-create-iot-edge-module-offer/offer-identity.png)


-   Die **Angebots-ID** dient zur eindeutigen Identifizierung Ihres Angebots im CPP und kann in kundenorientierten URLs verwendet werden.

-   Der **Name** ist nur für Sie selbst sichtbar und dient als Angebotsreferenz im CPP.

### <a name="create-one-or-more-skus"></a>Erstellen von SKUs

Jede SKU entspricht einem Containerimage. Es muss mindestens eine SKU vorhanden sein, und Sie können mehrere hinzufügen. Eine SKU besteht aus zwei Teilen:

-   SKU-Metadaten

-   Containermetadaten

**So erstellen Sie eine SKU:**

Klicken Sie auf die Registerkarte **SKUs** und anschließend auf **Neue SKU**.

![Neue SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKUs.png)

Die SKU-Metadaten enthalten folgende Felder, bei denen es sich jeweils um Pflichtfelder handelt:
- SKU-ID: Ein eindeutiger Bezeichner.
- Titel: Der SKU-Titel (bis zu 50 Zeichen).
- Zusammenfassung: Eine kurze Beschreibung (bis zu 100 Zeichen).
- Beschreibung: Eine lange Beschreibung.
- „Hide this SKU“ (Diese SKU ausblenden): Standardmäßig auf **Nein** festgelegt.
   
![SKU-Details](./media/cloud-partner-portal-create-iot-edge-module-offer/SKU-settings.png)

#### <a name="iot-edge-module-metadata-and-the-container-registry"></a>Metadaten des IoT Edge-Moduls und die Containerregistrierung

Die Metadaten des IoT Edge-Moduls enthalten Imagereferenzinformationen, die in der Azure-Containerregistrierung (ACR) gespeichert sind. Das Image wird vom Azure Marketplace in die öffentliche Marketplace-Registrierung kopiert und ist nach der Zertifizierung für Kunden verfügbar. Alle Benutzeranforderungen zur Nutzung eines IoT Edge-Modulimages werden über die Marketplace-Containerregistrierung abgewickelt.

![Containerimages](./media/cloud-partner-portal-create-iot-edge-module-offer/container-images.png)

**Containerimages**

Die Imagerepositorydetails umfassen folgende Pflichtfelder:

-   **Abonnement-ID**: Die ID des Azure-Abonnements, in dem sich die ACR-Registrierung befindet.

-   **Ressourcengruppenname**: Der Ressourcengruppenname der ACR-Registrierung.

-   **Registrierungsname**: Der Name der ACR-Registrierung.

-   **Repositoryname**: Der Repositoryname. Dieser Wert kann, nachdem er festgelegt wurde, nicht mehr geändert werden. Der Name muss eindeutig sein, damit in Ihrem Konto kein anderes Angebot mit dem gleichen Namen vorhanden ist.

-   **Benutzername**: Der Benutzername, der der ACR zugeordnet ist (Administratorbenutzername).

-   **Kennwort**: Das Kennwort, das der ACR zugeordnet ist.

    >[!Note]
    >Der Benutzername und das Kennwort müssen angegeben werden, damit Partner auf die im Veröffentlichungsprozess beschriebene ACR zugreifen können.

Beim Veröffentlichen eines IoT Edge-Modulimages können Imagetags angeben. Fügen Sie Ihrem Modul das Tag „latest“ (Standardtag) hinzu, um es beim Testen problemlos erkennen zu können.

Darüber hinaus können Sie auch folgende IoT Edge-Moduldetails angeben:

-   **createOptions**: Die standardmäßigen Erstellungsoptionen, die übergeben werden sollen, damit dieses IoT Edge-Modul direkt gestartet werden kann.

-   **twin**: Der Standardzwilling, der übergeben werden soll, damit dieses IoT Edge-Modul bei Verwendung des SDK für IoT-Module direkt gestartet werden kann.

-   **routes**: Die Standardrouten, die übergeben werden sollen, damit dieses IoT Edge-Modul bei Verwendung des SDK für IoT-Module direkt gestartet werden kann.

### <a name="describe-your-iot-edge-module-for-your-customers"></a>Beschreiben des IoT Edge-Moduls für Ihre Kunden

Fügen Sie auf der Registerkarte „Marketing“ Ihren marketingspezifischen Inhalt hinzu. Diese Informationen sind öffentlich sichtbar und werden im Azure Marketplace angezeigt.

![Übersicht über das IoT Edge-Modul](./media/cloud-partner-portal-create-iot-edge-module-offer/overview.png)

-   **Titel**: Der öffentlich sichtbare Titel Ihres IoT Edge-Moduls.

-   **Zusammenfassung**: Die Zusammenfassung Ihres IoT Edge-Moduls. Diese ist auf den meisten Seiten öffentlich sichtbar (beispielsweise auf den Seiten zum Durchstöbern).

-   **Lange Zusammenfassung**: Die Zusammenfassung Ihres IoT Edge-Moduls, die öffentlich sichtbar ist, wenn Ihr Modul empfohlen wird. 

-   **Beschreibung**: Die Beschreibung Ihres IoT Edge-Moduls, die auf der Produktdetailseite öffentlich sichtbar ist. (Die Beschreibung kann mithilfe einfacher HTML-Tags formatiert werden.)

-   **Marketing identifier** (Marketingbezeichner): Ein eindeutiger Bezeichner zum Erstellen Ihrer Produkt-URL. Diese URL hat folgendes Format: *azuremarketplace.microsoft.com/enus/marketplace/apps/IhreHerausgeberID.MarketingbezeichnerIhresIoTEdgeModuls*.

-   **Preview subscription Ids** (Preview-Abonnement-IDs): Benutzern mit Zugriff auf diese Abonnements wird das IoT Edge-Modul nach dem Zertifizierungsschritt, aber noch vor der Liveschaltung angezeigt.

-   **Nützliche Links**: Sie können bis zu zehn Links hinzufügen, die dann auf Ihrer Produktdetailseite angezeigt werden.

-   **Suggested categories** (Vorgeschlagene Kategorien): Wählen Sie bis zu fünf Kategorien aus. Diese werden auf Ihrer Produktdetailseite angezeigt. Derzeit werden alle IoT Edge-Module auf den Seiten zum Durchstöbern unter der Kategorie *Internet der Dinge \> IoT Edge-Modul* angezeigt.

-   **Logos**: Laden Sie die Logobilder für Ihr IoT Edge-Modul im PNG-Format hoch. Verwenden Sie exakt die folgenden Größen: 40 x 40 Pixel, 90 x 90 Pixel, 115 x 115 Pixel, 255 x 115 Pixel.

-   **Screenshots**: Screenshots werden auf Ihrer Produktdetailseite angezeigt. Damit können Sie sehr gut die Features und die Funktionsweise Ihres IoT Edge-Moduls veranschaulichen. Sie können beispielsweise Architekturdiagramme oder Anwendungsfälle präsentieren.

-   **Videos**: Videos werden auf Ihrer Produktdetailseite angezeigt. Damit können Sie sehr gut die Features und die Funktionsweise Ihres IoT Edge-Moduls veranschaulichen.

-   **Leadverwaltung**: Hier können Sie ein System zur Erfassung aller Leads auswählen, die an Ihrem Produkt interessiert sind.

-   **Datenschutz**: Sie benötigen eine URL, die zu Ihrer Datenschutzrichtlinie führt.

-   **Nutzungsbedingungen**: Sie müssen über Nutzungsbedingungen verfügen. Sie können HTML-Tags verwenden, um diese Seite zu formatieren oder auf eine Ihrer anderen Seiten zu verweisen.

### <a name="enter-your-support-contact-information"></a>Eingeben der Kontaktinformationen Ihres Supports

Geben Sie auf der Registerkarte „Support“ **Engineering-Kontaktdaten** und Informationen zum **Kundensupport** ein.

![Supportkontakte](./media/cloud-partner-portal-create-iot-edge-module-offer/support-contact-info.png)


## <a name="certify-your-iot-edge-module"></a>Zertifizieren Ihres IoT Edge-Moduls

Klicken Sie nach Angabe aller erforderlichen Informationen auf **Veröffentlichen**, um Ihr IoT Edge-Modul zur Zertifizierung zu übermitteln. Die Schritte des Zertifizierungsprozesses werden auf einer Zeitachse dargestellt.

**Modulüberprüfung**

Ihr Modul wird von unserem Zertifizierungsteam überprüft. Nach der Zertifizierung des Moduls erhalten Sie einen privaten Link zum Testen Ihres Moduls. Sollten nach dem Testen Änderungen erforderlich sein, bearbeiten Sie die Metadaten Ihres Angebots, und übermitteln Sie das Modul erneut an das Zertifizierungsteam. 

## <a name="publish-your-iot-edge-module"></a>Veröffentlichen Ihres IoT Edge-Moduls

Wenn Ihr IoT Edge-Modul nach Abschluss der Testphase für die Veröffentlichung bereit sind, klicken Sie auf **Live schalten**.

>[!Important]
>Falls Ihr IoT Edge-Modul im Rahmen der Ignite-Veranstaltung angekündigt werden soll, muss es bis zum 23.09.2018 veröffentlicht worden sein.
