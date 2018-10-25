---
title: Container | Microsoft-Dokumentation
description: Schritte zur Veröffentlichung eines Containerimage.
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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5653e4b2ec9aa5e21a107611b9d4142168630d4a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806380"
---
<a name="publishing-a-container-image-in-the-cloud-partner-portal"></a>Veröffentlichen Containerimage im Cloud-Partnerportal
========================================================

In diesem Artikel ist beschrieben, wie Sie ein neues Containerimage im Cloud-Partnerportal veröffentlicht wird.

Gehen Sie wie folgt vor, um ein neues Containerimage zu veröffentlichen.

1. Wählen Sie **Neues Angebot** aus, und wählen Sie dann **Container** aus.

    ![Option „Container“ für neues Angebot auswählen](media/cpp-containers-guide/azure-container-offer.png)

2. Geben Sie auf der Registerkarte „Angebotseinstellungen“ unter „Angebotsidentität“ die Werte für **Angebots-ID**, **Herausgeber-ID** und **Name** ein.

    ![Angebotsidentität](media/cpp-containers-guide/containers-offer-settings.png)

3. Wählen Sie auf der Registerkarte „SKUs“ die Option **Neue SKU** aus.
    >[!NOTE]
    >Sie können mehrere SKUs hinzufügen.

    ![Eingabeaufforderung für neue SKU](media/cpp-containers-guide/containers-sku-settings.png)

4. Geben Sie die SKU- und die Containerinformationen an. Jede SKU entspricht einem Containerimage. Eine SKU besteht aus zwei Teilen:

    -   SKU-Metadaten
    -   Containermetadaten

    Die SKU-Metadaten enthalten die Anzeigeinformationen für die Containerimages.

    ![SKU-Metadaten](media/cpp-containers-guide/containers-sku-details.png)

    Die Containermetadaten enthalten Referenzinformationen aus den Image-Repository-Details in der Azure-Containerregistrierung (ACR). Azure Marketplace kopiert dieses Image dann in die öffentliche Marketplace-Registrierung und macht das Image für Kunden nach der Zertifizierung verfügbar. Jede Anforderung eines Azure-Benutzers, ein Containerimage zu nutzen, wird über die Marketplace-Containerregistrierung verarbeitet.

   ![Containermetadaten](media/cpp-containers-guide/containers-image-repository.png)

    Die Image-Repository-Details im vorherigen Bildschirmfoto enthalten die folgenden Felder:

    -   **Abonnement-ID**: Die ID des Azure-Abonnements, in dem die ACR-Registrierung vorhanden ist.
    -   **Ressourcengruppenname**: Der Ressourcengruppenname der ACR-Registrierung.
    -   **Registrierungsname**: Der Name der ACR-Registrierung.
    -   **Repositoryname**: Der Repositoryname. Dieser Wert kann, nachdem er festgelegt wurde, nicht mehr geändert werden. Sie müssen hierfür einen eindeutigen Namen vergeben, damit kein anderes Angebot in Ihrem Konto denselben Namen hat.
    -   **Benutzername**: Der Benutzername, der der ACR zugeordnet ist (Administratorbenutzername).
    -   **Kennwort**: Das Kennwort, das der ACR zugeordnet ist.

    >[!NOTE]
    >Der Benutzername und das Kennwort sind erforderlich, um sicherzustellen, dass Partner Zugriff auf die ACR haben, der im Veröffentlichungsprozess angegeben wurde.

    Beim Veröffentlichen eines Containerimage können Sie auch Imagetags bereitstellen. Zusätzlich zu einem Imagetag können Partnern auch SHA-Digests bereitstellen. Sie sollten Ihrem Image unbedingt ein **Testtag** hinzufügen, damit Sie das Image beim Testen erkennen können.

5. Fügen Sie auf der Registerkarte „Marketplace“ Ihren marketingspezifischen Inhalt hinzu.

    ![Marketplace-Informationen](media/cpp-containers-guide/containers-marketplace-tab.png)

6. Geben Sie auf der Registerkarte „Support“ Ihre Engineering-Kontakt- und Kundensupportdaten ein.

   ![Supportinformationen](media/cpp-containers-guide/containers-support-tab.png)

7. Wählen Sie **Veröffentlichen** aus, um das Angebot zu veröffentlichen. Nachdem Sie „Veröffentlichen“ ausgewählt haben, sehen Sie eine Zeitachse, in der Schritte aufgeführt sind, die zum Veröffentlichen Ihres Containerimage auszuführen sind.
