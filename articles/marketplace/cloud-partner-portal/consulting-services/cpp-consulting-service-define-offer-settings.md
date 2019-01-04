---
title: Azure- und Dynamics 365-Beratungsdienstangebot – Definieren von Angebotseinstellungen | Microsoft-Dokumentation
description: Dies ist ein Leitfaden für die Definition von Angebotseinstellungen eines Azure- oder Dynamics 365-Beratungsdienstangebots im Cloud-Partnerportal.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 6ce51dc9bbcdc7b46593a4800c4fce07a42f3171
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345066"
---
# <a name="offer-settings-tab"></a>Registerkarte „Angebotseinstellungen“

Der erste Schritt unter **Neues Angebot** besteht darin, die Angebots-ID zu erstellen. Eine Angebots-ID besteht aus drei Teilen: **Angebots-ID**, **Herausgeber-ID** und **Name**. Jeder dieser Teile ist in den nachstehenden Abschnitten beschrieben.

![Erstellen eines neuen Beratungsdienstangebots – Registerkarte „Angebotseinstellungen“](media/consultingoffer-settings-tab.png)

*Angebots-ID*

Dieser Bezeichner ist ein eindeutiger Name, den Sie bei der ersten Übermittlung des Angebots erstellen. Dieser Name darf nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten. Die **Angebots-ID** ist in der URL sichtbar und wirkt sich auf Suchmaschinenergebnisse aus. Beispiel: *yourcompanyname_exampleservice*.

Wie im Beispiel gezeigt, wird die **Angebots-ID** an Ihre Herausgeber-ID angefügt, um einen eindeutigen Bezeichner zu erstellen. Dieser Bezeichner wird als permanenter Link verfügbar gemacht, der abonniert werden kann und von Suchmaschinen indiziert wird.

*Nachdem ein Angebot live geschaltet wurde, kann der Bezeichner nicht mehr aktualisiert werden.*

*Herausgeber-ID*

Dieser Bezeichner bezieht sich auf Ihr Konto. Wenn Sie sich mit Ihrem Organisationskonto angemeldet haben, wird Ihre **Herausgeber**-ID im Dropdownmenü angezeigt.

*Name*

Dies ist die Zeichenfolge, die als Angebotsname in AppSource oder Azure Marketplace angezeigt wird. Das Feld *Name* ist auf 50 Zeichen begrenzt.  Der Prüfer muss Ihren Titel ggf. bearbeiten, damit die Dauer und der Angebotstyp an den Angebotsnamen angefügt werden können.

>[!Note]
>Wichtig: Geben Sie hier nur den Namen des eigentlichen Diensts ein. Fügen Sie weder die Dauer noch den Typ des Diensts hinzu.

Das folgende Beispiel von Edgewater Fullscope zeigt, wie der Angebotsname aufgebaut ist. Der Angebotsname wird wie folgt angezeigt:

![Erstellen eines neuen Beratungsdienstangebots](media/cppsampleconsultingoffer.png)

Ein Angebotsname besteht aus vier Teilen:

-   **Dauer**: Die Dauer wird im Editor auf der Registerkarte **Details der digitalen Ladenzeile** definiert. Eine Dauer kann in Stunden, Tagen oder Wochen ausgedrückt werden.
-   **Diensttyp**: Der Diensttyp wird im Editor auf der Registerkarte **Details der digitalen Ladenzeile** definiert. Die Diensttypen lauten `Assessment` (Bewertung), `Briefing` (Einweisung), `Implementation` (Implementierung), `Proof of concept` und `Workshop`.
-   **Präposition**: Die Präposition wird vom Prüfer eingefügt.
-   **Name**: Der Name wird auf der Seite **Angebotseinstellungen** definiert.

>[!Note]
>Das Feld „Name“ ist auf 50 Zeichen begrenzt. Der von Ihnen übermittelte Name muss vom Prüfer ggf. bearbeitet werden, damit die Dauer und der Angebotstyp an den Angebotsnamen angefügt werden können.

In der folgenden Liste sind einige ordnungsgemäß benannte Angebote aufgeführt:

-   Das Wichtigste für professionelle Dienstleistungen: 1-Std. Einweisung
-   Plattform für Cloudmigration: 1-Std. Einweisung
-   PowerApps und Microsoft Flow: 1-Tag Workshop
-   Azure Machine Learning Services: 3-Wo PoC
-   Einzelhandelslösung für Ladengeschäft und Onlineshop: 1-Std. Einweisung
-   Aufbereitung Ihrer Daten: 1-Wo Workshop
-   Cloud-Analysen: 3-Tag Workshop
-   Power BI-Schulung: 3-Tag Workshop
-   Vertriebsmanagementlösung: 1-Wo Implementierung
-   CRM-Schnellstart: 1-Tag Workshop
-   Dynamics 365 for Sales: 2-Tag Bewertung

Nachdem Sie die Registerkarte **Angebotseinstellungen** ausgefüllt haben, können Sie Ihr Angebot speichern. Der Angebotsname wird jetzt über dem Editor angezeigt, und Sie finden ihn in „Alle Angebote“.

**Nächste Schritte**

Jetzt können Sie [Details der digitalen Ladenzeile eingeben und festlegen, ob in Azure Marketplace oder in AppSource veröffentlicht werden soll](./cpp-consulting-service-storefront-details.md).