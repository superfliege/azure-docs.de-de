---
title: Marketplace für IoT Edge-Module – häufig gestellte Fragen | Microsoft-Dokumentation
description: Marketplace für IoT Edge-Module – häufig gestellte Fragen.
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
ms.openlocfilehash: 2c2e7729eb70a5dd37dc4df10605eec9429e1043
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806148"
---
# <a name="iot-edge-module-marketplace-frequently-asked-questions"></a>Häufig gestellte Fragen zu Marketplace für IoT Edge-Module


## <a name="what-is-the-edge-module-marketplace"></a>Was ist der Marketplace für Edge-Module?


Der Marketplace für IoT Edge-Module ist eine Liste *zertifizierter* vorkonfigurierter Edge-Module, die über den Azure Marketplace *findbar* sind.

![IoT Edge-Module](./media/cloud-partner-portal-iot-edge-module-faq/iot-edge-modules.png)

## <a name="where-will-edge-modules-be-visible"></a>Wo sind Edge-Module sichtbar? 


Im [Azure-Portal-Marketplace](https://ms.portal.azure.com/) (authentifizierter Zugriff) unter der Kategorie „Internet der Dinge“ mit der Kennzeichnung „IoT Edge-Modul“.

Und auf der [Azure Marketplace-Website](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1) (anonymer Zugriff) unter der Kategorie „Internet der Dinge (IoT)“ mit der Kennzeichnung „IoT Edge-Module“.

## <a name="is-it-open-to-partners"></a>Ist der Marketplace für Partner offen?


Bisher nicht. Derzeit werden nur von Microsoft autorisierte Module im IoT Edge-Abschnitt des Marketplace veröffentlicht. 

## <a name="why-should-partners-publish-their-iot-edge-modules"></a>Warum sollten Partner ihre IoT Edge-Module veröffentlichen?


-   Um ihre Produktpräsenz zu erhöhen, indem dieser Vertriebskanal hinzugefügt wird und ihre Lösungen präsentiert werden.

-   Um mehr Leads auf sich aufmerksam zu machen. Im Rahmen des Azure Marketplace können sie ausführliche Informationen darüber abrufen, wer an ihren Lösungen interessiert ist.

-   Um zu den Ersten zu gehören und so weitere Monetarisierungsmöglichkeiten zu nutzen.

## <a name="what-is-the-onboarding-process"></a>Was ist mit Onboardingprozess gemeint?


Der derzeit noch nicht mögliche Onboardingprozess wird über den Azure Marketplace erfolgen. Ausführliche Richtlinien finden Sie im [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). Suchen Sie nach dem Transaktionsauflistungstyp für Container. 

Ein Partner muss zunächst ein Herausgeber für den Azure Marketplace werden. Danach kann er seine Edge-Module mit dem Onboardingprozess über das [Cloud-Partnerportal](./cloud-partner-portal-getting-started-with-the-cloud-partner-portal.md) einbringen.

## <a name="are-there-any-monetization-capabilities"></a>Gibt es irgendwelche Monetarisierungsmöglichkeiten?


Nicht unmittelbar zum jetzigen Zeitpunkt. Unser erstes Ziel ist es, einen Marketplace mit *kostenlosen* oder *Bring-Your-Own-License*-Edge-Modulen zu öffnen. Wir werden geeignete Monetarisierungsmöglichkeiten hinzufügen, z. B. ein nutzungsbezogenes Abrechnungsmodell.

## <a name="what-is-bring-your-own-license-byol"></a>Was ist BYOL (Bring Your Own License)?


Die offizielle Definition in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) lautet wie folgt:

- *Kunden erwerben außerhalb des Azure Marketplace das Recht zum Zugreifen auf oder Nutzen des Angebots und müssen keine Azure Marketplace-Gebühren für die Nutzung des Angebots im Azure Marketplace zahlen.*

Es liegt in der Verantwortung der Partner, ihre Software zu lizenzieren und für die Software fällige Einnahmen zu kassieren.

## <a name="can-partners-publish-a-freemium-module"></a>Können Partner ein „Freemium“-Modul veröffentlichen?


Ja. Freemium-Module, d. h. Module, die kostenlos, aber mit einigen Einschränkungen verfügbar sind, werden so wie jede andere Veröffentlichung behandelt.

## <a name="is-intellectual-property-protected"></a>Ist geistiges Eigentum geschützt?


Ein Edge-Modul ist ein Docker-kompatibler Container. Es liegt in der Verantwortung der Partner, ihr geistiges Eigentum zu schützen, das in den verteilten Containern enthalten ist.

## <a name="where-will-the-modules-be-hosted"></a>Wo werden die Module gehostet?


IoT Edge-Module werden in einer Microsoft-eigenen Containerregistrierung gehostet, die anonym abgefragt werden kann, etwa Docker Hub.

## <a name="what-are-the-integration-plans-between-the-azure-marketplace-and-the-azure-iot-tools"></a>Gibt es Integrationspläne für den Azure Marketplace und die Azure IoT-Tools?

Wir arbeiten an einer engeren Integration für den Azure Marketplace und die Azure IoT-Tools. So schwebt uns beispielsweise vor, zu ermöglichen, dass der Marketplace für IoT Edge-Module direkt aus dem IoT Hub-Portal oder direkt aus Visual Studio-Code durchsucht wird.

## <a name="which-operating-system-or-architecture-should-my-container-support"></a>Welches Betriebssystem oder welche Architektur muss mein Container unterstützen?

IoT Edge-Module müssen dieselbe Betriebssystem/Architektur-Matrix unterstützen wie IoT Edge im Einsatz. Diese Liste finden Sie unter [Azure IoT Edge-Support](https://docs.microsoft.com/azure/iot-edge/support). Beispielsweise muss ein Modul derzeit Linux x64 und Linux ARM32 unterstützen.

## <a name="are-there-any-other-certification-constraints-to-be-aware-of"></a>Gibt es sonstige Zertifizierungseinschränkungen, die zu beachten sind?

Wir arbeiten noch an der genauen Festlegung der Zertifizierungsbeschränkungen. Unsere Leitprinzipien sind:

-   Qualität geht vor Quantität

-   IoT Edge-spezifische Container (keine zufälligen Container)

-   Produktionsqualität

-   1-Click-Bereitstellung (mit mindestens einem Satz von standardmäßigen Konfigurationswerten)

## <a name="any-other-questions"></a>Weitere Fragen?


Wenden Sie sich an [Azure IoT Edge-Onboarding](mailto:azureiotedgeonboarding@service.microsoft.com).
