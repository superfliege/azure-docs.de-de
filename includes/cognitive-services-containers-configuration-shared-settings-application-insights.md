---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 31a0acbfd0c9714bf2cbdf20f9f1f82edb07f05a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977376"
---
Die `ApplicationInsights`-Einstellung ermöglicht das Hinzufügen von Unterstützung für [Azure Application Insights](https://docs.microsoft.com/azure/application-insights)-Telemetriedaten in Ihrem Container. Application Insights ermöglicht eine tief greifende Überwachung Ihrer Container. Sie können ganz einfach die Verfügbarkeit, Leistung und Nutzung Ihrer Container überwachen. Außerdem können Sie schnell Fehler in Ihrem Container erkennen und diagnostizieren.

In der folgenden Tabelle werden die Konfigurationseinstellungen beschrieben, die unter dem Abschnitt `ApplicationInsights` unterstützt werden.

|Erforderlich| NAME | Datentyp | BESCHREIBUNG |
|--|------|-----------|-------------|
|Nein | `InstrumentationKey` | Zeichenfolge | Der Instrumentierungsschlüssel der Application Insights-Instanz, an die Telemetriedaten für den Container gesendet werden. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Beispiel:<br>`InstrumentationKey=123456789`|

