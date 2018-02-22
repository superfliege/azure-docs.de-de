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
ms.openlocfilehash: 096fdd5a6604ed8cecc931da2169194b777664d2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="logic-apps-pricing-model"></a>Preismodell für Logik-Apps

Sie können automatisierte skalierbare Integrationsworkflows in der Cloud mit Azure Logic Apps erstellen und ausführen. Hier sind die Details zur Abrechnung und zu den Preisen für Logic Apps angegeben. 

## <a name="consumption-pricing-model"></a>Modell für nutzungsbasierte Preise

Bei neu erstellten Logik-Apps zahlen Sie nur für die Ressourcen, die Sie wirklich nutzen. Für neue Logik-Apps werden ein Verbrauchsplan und ein Preismodell verwendet. Dies bedeutet, dass alle Aktionsausführungen, die von einer Logik-App-Instanz durchgeführt werden, nutzungsbezogen abgerechnet werden. Jeder Schritt einer Logik-App-Definition ist eine Aktion, die Trigger, Ablaufsteuerungsschritte und Aufrufe von integrierten Aktionen und Connectors umfasst. Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Trigger

Trigger sind spezielle Aktionen, mit denen eine Logik-App-Instanz erstellt wird, wenn ein bestimmtes Ereignis eintritt. Trigger agieren auf verschiedene Weise, wovon abhängt, wie die Logik-App nutzungsbezogen abgerechnet wird.

* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend auf Nachrichten überprüft, die die Kriterien für das Erstellen einer Logik-App-Instanz und Starten des Workflows erfüllen. Jede Abrufanforderung zählt auch dann als Ausführung und wird abgerechnet, wenn keine Logik-App-Instanz erstellt wird. Um das Abrufintervall festzulegen, richten Sie den Trigger über den Logik-App-Designer ein.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich beispielsweise um Webhooktrigger.

* **Wiederholungstrigger**: Dieser Trigger erstellt eine Logik-App-Instanz basierend auf dem Wiederholungsintervall, das Sie im Trigger einrichten. Sie können beispielsweise einen Wiederholungstrigger einrichten, der alle drei Tage oder nach einem komplexeren Zeitplan ausgeführt wird.

Sie finden Triggerausführungen im Bereich „Übersicht“ Ihrer Logik-App unter dem Abschnitt „Triggerverlauf“.

## <a name="actions"></a>Actions

Integrierte Aktionen (z.B. Aktionen zum Aufrufen von HTTP, Azure Functions oder API Management) und auch Ablaufsteuerungsschritte werden als native Aktionen, die jeweils einen eigenen Typ haben, nutzungsbezogen abgerechnet. Aktionen, mit denen [Connectors](https://docs.microsoft.com/connectors) aufgerufen werden, haben den Typ „ApiConnection“. Diese Connectors sind als Standard- oder Unternehmensconnectors klassifiziert, deren Verbrauch zu den jeweiligen [Preisen][pricing] abgerechnet wird. 

Alle erfolgreich und nicht erfolgreich ausgeführten Aktionen werden als Aktionsausführungen gezählt und nutzungsbezogen abgerechnet. Aktionen, die aufgrund von nicht erfüllten Bedingungen übersprungen werden, oder Aktionen, die aufgrund einer Beendigung der Logik-App vor ihrem Abschluss nicht ausgeführt werden, werden nicht als Aktionsausführungen gezählt. Deaktivierte Logik-Apps können keine neuen Instanzen initiieren, sodass keine Gebühren anfallen, während sie deaktiviert sind.

> [!NOTE]
> Nachdem Sie eine Logik-App deaktiviert haben, kann es einige Zeit dauern, bis die derzeit ausgeführten Instanzen vollständig beendet werden.

Aktionen, die innerhalb von Schleifen ausgeführt werden, werden pro Schleifenzyklus gezählt. Eine einzelne Aktion in einer „for each“-Schleife, mit der eine Liste mit zehn Einträgen verarbeitet wird, wird beispielsweise wie folgt gezählt: Die Anzahl von Listeneinträgen (10) wird mit der Anzahl von Aktionen in der Schleife (1) multipliziert, und anschließend wird der Wert 1 für das Starten der Schleife addiert. Die Berechnung für dieses Beispiel lautet also (10 * 1) + 1, sodass sich insgesamt elf Aktionsausführungen ergeben.

## <a name="integration-account-usage"></a>Nutzung des Integrationskontos

Die verbrauchsbasierte Nutzung umfasst ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md), unter dem Sie die Features [B2B/EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) von Logic Apps ohne zusätzliche Kosten erkunden, entwickeln und testen können. Sie können eines dieser Integrationskonten pro Region verwenden und bis zu 10 Vereinbarungen und 25 Zuordnungen speichern. Sie können eine unbegrenzte Zahl von Partnern, Schemas und Zertifikaten nutzen und hochladen.

Für Logic Apps sind außerdem Basic- und Standard-Integrationskonten mit unterstützter Logic Apps-SLA verfügbar. Sie können Basic-Integrationskonten verwenden, wenn Sie entweder nur die Behandlung von Nachrichten nutzen oder als Partner für ein kleines Unternehmen fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist. Standard-Integrationskonten unterstützen komplexere B2B-Beziehungen und erhöhen die Anzahl von Entitäten, die Sie verwalten können. Weitere Informationen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher über Logic Apps.][whatis]
* [Erstellen Ihrer ersten Logik-App][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

