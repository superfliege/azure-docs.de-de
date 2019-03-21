---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: ed512c0f56d8da5cb8e47b20f2495054fdedf020
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56740873"
---
Dieser Container hat die folgenden Konfigurationseinstellungen:

|Erforderlich|Einstellung|Zweck|
|--|--|--|
|Ja|[ApiKey](#apikey-configuration-setting)|Wird zum Nachverfolgen von Abrechnungsinformationen verwendet.|
|Nein |[ApplicationInsights](#applicationinsights-setting)|Ermöglicht das Hinzufügen von Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten in Ihrem Container.|
|Ja|[Abrechnung](#billing-configuration-setting)|Gibt den Endpunkt-URI der Dienstressource in Azure an.|
|Ja|[Eula](#eula-setting)| Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.|
|Nein |[Fluentd](#fluentd-settings)|Schreibt Protokoll- und optional auch Metrikdaten auf einen Fluentd-Server.|
|Nein |[HTTP-Proxy](#http-proxy-credentials-settings)|Konfigurieren Sie einen HTTP-Proxy für ausgehende Anforderungen.|
|Nein |[Protokollierung](#logging-settings)|Bietet Unterstützung für die ASP.NET Core-Protokollierung für Ihren Container. |
|Nein |[Mounts](#mount-settings)|Liest und schreibt Daten vom Hostcomputer in den Container und umgekehrt.|
