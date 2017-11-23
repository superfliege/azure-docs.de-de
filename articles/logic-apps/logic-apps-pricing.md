---
title: "Preise und Abrechnung – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Preisdetails und Abrechnung für Azure Logic Apps funktionieren."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: LADocs; klam
ms.openlocfilehash: f2a92e45b8a759d2d8193ac188efdcfc694a3e6d
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="logic-apps-pricing-model"></a>Preismodell für Logik-Apps
Sie können automatisierte skalierbare Integrationsworkflows in der Cloud mit Azure Logic Apps erstellen und ausführen. Hier sind die Details zur Abrechnung und zu den Preisen für Logic Apps angegeben.
## <a name="consumption-pricing-model"></a>Modell für nutzungsbasierte Preise
Bei neu erstellten Logik-Apps zahlen Sie nur für die Ressourcen, die Sie wirklich nutzen. Für neue Logik-Apps werden ein Verbrauchsplan und ein Preismodell verwendet. Dies bedeutet, dass alle Ausführungen, die von einer Logik-App-Instanz durchgeführt werden, nutzungsbezogen abgerechnet werden.
### <a name="what-are-action-executions"></a>Was sind Aktionsausführungen?
Jeder Schritt einer Logik-App-Definition ist eine Aktion, die Trigger, Ablaufsteuerungsschritte und Aufrufe von integrierten Aktionen und Connectors umfasst.
### <a name="triggers"></a>Trigger
Trigger sind spezielle Aktionen, mit denen eine Logik-App-Instanz erstellt wird, wenn ein bestimmtes Ereignis eintritt. Trigger verfügen über mehrere unterschiedliche Verhalten, die sich darauf auswirken, wie die Logik-App nutzungsbezogen abgerechnet wird.
* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend überprüft, bis er eine Nachricht empfängt, die die Kriterien für die Erstellung einer Logik-App-Instanz zum Starten des Workflows erfüllt. Sie können das Abrufintervall im Trigger über den Logik-App-Designer einrichten. Jede Abrufanforderung zählt auch dann als Ausführung, wenn keine Logik-App-Instanz erstellt wird.
* **Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich beispielsweise um Webhooktrigger.
* **Wiederholungstrigger**: Dieser Trigger erstellt eine Logik-App-Instanz basierend auf dem Wiederholungsintervall, das Sie im Trigger einrichten. Sie können beispielsweise einen Wiederholungstrigger einrichten, der alle drei Tage oder nach einem komplexeren Zeitplan ausgeführt wird.

Sie finden Triggerausführungen im Bereich „Übersicht“ Ihrer Logik-App unter dem Abschnitt „Triggerverlauf“.

### <a name="actions"></a>Aktionen
Integrierte Aktionen (z.B. Aktionen zum Aufrufen von HTTP, Azure Functions oder API Management) und Ablaufsteuerungsschritte werden als native Aktionen nutzungsbezogen abgerechnet und haben jeweils einen eigenen Typ. Aktionen, mit denen [Connectors](https://docs.microsoft.com/connectors) aufgerufen werden, haben den Typ „ApiConnection“. Connectors sind entweder als Standard- oder Unternehmensconnectors klassifiziert, und der Verbrauch wird zu den jeweiligen [Preisen][pricing] gemessen.
Alle erfolgreich und nicht erfolgreich ausgeführten Aktionen werden als Aktionsausführungen gezählt und nutzungsbezogen abgerechnet. Aktionen, die aufgrund von nicht erfüllten Bedingungen übersprungen werden, oder Aktionen, die aufgrund einer Beendigung der Logik-App vor ihrem Abschluss nicht ausgeführt werden, werden nicht als Aktionsausführungen gezählt. Deaktivierte Logik-Apps können keine neuen Instanzen initiieren, sodass keine Gebühren anfallen, während sie deaktiviert sind.

> [!NOTE]
> Nachdem Sie eine Logik-App deaktiviert haben, kann es einige Zeit dauern, bis die derzeit ausgeführten Instanzen vollständig beendet wurden.

Aktionen, die innerhalb von Schleifen ausgeführt werden, werden pro Schleifenzyklus gezählt. Eine einzelne Aktion in einer „for each“-Schleife, mit der eine Liste mit zehn Einträgen verarbeitet wird, wird beispielsweise wie folgt gezählt: Die Anzahl von Listeneinträgen (10) wird mit der Anzahl von Aktionen in der Schleife (1) multipliziert, und anschließend wird der Wert 1 für das Starten der Schleife addiert. Die Berechnung für dieses Beispiel lautet also (10 * 1) + 1, sodass sich insgesamt elf Aktionsausführungen ergeben.

### <a name="integration-account-usage"></a>Nutzung des Integrationskontos
Die verbrauchsbasierte Nutzung umfasst ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md), unter dem Sie die Features [B2B/EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) von Logic Apps ohne zusätzliche Kosten erkunden, entwickeln und testen können. Sie können eines dieser Integrationskonten pro Region verwenden und bis zu 10 Vereinbarungen und 25 Zuordnungen speichern. Sie können eine unbegrenzte Zahl von Partnern, Schemas und Zertifikaten nutzen und hochladen.

Für Logic Apps sind außerdem Basic- und Standard-Integrationskonten mit unterstützter Logic Apps-SLA verfügbar. Sie können Basic-Integrationskonten verwenden, wenn Sie entweder nur die Behandlung von Nachrichten nutzen oder als Partner für ein kleines Unternehmen fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist. Standard-Integrationskonten unterstützen komplexere B2B-Beziehungen und erhöhen die Anzahl von Entitäten, die Sie verwalten können. Weitere Informationen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="pricing"></a>Preise
Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Logic Apps][whatis]
* [Erstellen Ihrer ersten Logik-App][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

