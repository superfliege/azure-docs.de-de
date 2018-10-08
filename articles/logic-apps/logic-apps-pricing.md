---
title: Preise und Abrechnung – Azure Logic Apps | Microsoft-Dokumentation
description: Erfahren Sie, wie Preisdetails und Abrechnung für Azure Logic Apps funktionieren.
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 5f9147035c07bbe4fb3f38b74025015e70dd87b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159559"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Preismodell für Azure Logic Apps

Sie können automatisierte skalierbare Integrationsworkflows in der Cloud mit Azure Logic Apps erstellen und ausführen. Hier sind die Details zur Abrechnung und zu den Preisen für Logic Apps angegeben. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modell für nutzungsbasierte Preise

Bei neuen Logik-Apps, die Sie mit dem öffentlichen oder „globalen“ Logic Apps-Dienst erstellen, bezahlen Sie nur für Ihre tatsächliche Nutzung. Diese Logik-Apps verwenden einen nutzungsbezogenen Plan und ein Preismodell. Dies bedeutet, dass alle Aktionsausführungen, die von einer Logik-App durchgeführt werden, nutzungsbezogen abgerechnet werden. Jeder Schritt einer Logik-App-Definition ist eine Aktion, die Trigger, Ablaufsteuerungsschritte und Aufrufe von integrierten Aktionen und Connectors umfasst. Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Feststehendes Preismodell

> [!NOTE]
> Die Integrationsdienstumgebung befindet sich in der *privaten Vorschau*. Um den Zugriff anzufordern, [erstellen Sie Ihre Beitrittsanforderung hier](https://aka.ms/iseprivatepreview).

Für neue Logik-Apps, die Sie mit einer [*Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen, d.h. einer privaten isolierten Logic Apps-Instanz, die dedizierte Ressourcen verwendet, zahlen Sie eine monatliche Grundgebühr für integrierte Aktionen und Standardconnectors mit der ISE-Bezeichnung. Ihre ISE umfasst einen kostenlosen Unternehmensconnector, während zusätzliche Unternehmensconnectors basierend auf dem Verbrauchspreis des Unternehmens abgerechnet werden. Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Trigger

Trigger sind spezielle Aktionen, mit denen eine Logik-App-Instanz erstellt wird, wenn ein bestimmtes Ereignis eintritt. Trigger agieren auf verschiedene Weise, wovon abhängt, wie die Logik-App nutzungsbezogen abgerechnet wird.

* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend auf Nachrichten überprüft, die die Kriterien für das Erstellen einer Logik-App-Instanz und Starten des Workflows erfüllen. Jede Abrufanforderung zählt auch dann als Ausführung und wird abgerechnet, wenn keine Logik-App-Instanz erstellt wird. Um das Abrufintervall festzulegen, richten Sie den Trigger über den Logik-App-Designer ein.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich beispielsweise um Webhooktrigger.

* **Wiederholungstrigger**: Dieser Trigger erstellt eine Logik-App-Instanz basierend auf dem Wiederholungsintervall, das Sie im Trigger einrichten. Sie können beispielsweise einen Wiederholungstrigger einrichten, der alle drei Tage oder nach einem komplexeren Zeitplan ausgeführt wird.

Sie finden Triggerausführungen im Bereich „Übersicht“ Ihrer Logik-App unter dem Abschnitt „Triggerverlauf“.

## <a name="actions"></a>Aktionen

Integrierte Aktionen (z.B. Aktionen zum Aufrufen von HTTP, Azure Functions oder API Management) und auch Ablaufsteuerungsschritte werden als native Aktionen, die jeweils einen eigenen Typ haben, nutzungsbezogen abgerechnet. Aktionen, mit denen [Connectors](https://docs.microsoft.com/connectors) aufgerufen werden, haben den Typ „ApiConnection“. Diese Connectors sind als Standard- oder Unternehmensconnectors klassifiziert, deren Verbrauch zu den jeweiligen [Preisen][pricing] abgerechnet wird. Unternehmensconnectors in der *Vorschau* werden als Standardconnectors in Rechnung gestellt.

Alle erfolgreich und nicht erfolgreich ausgeführten Aktionen werden als Aktionsausführungen gezählt und nutzungsbezogen abgerechnet. Aktionen, die aufgrund von nicht erfüllten Bedingungen übersprungen werden, oder Aktionen, die aufgrund einer Beendigung der Logik-App vor ihrem Abschluss nicht ausgeführt werden, werden nicht als Aktionsausführungen gezählt. Deaktivierte Logik-Apps können keine neuen Instanzen initiieren, sodass keine Gebühren anfallen, während sie deaktiviert sind.

> [!NOTE]
> Nachdem Sie eine Logik-App deaktiviert haben, kann es einige Zeit dauern, bis die derzeit ausgeführten Instanzen vollständig beendet werden.

Aktionen, die innerhalb von Schleifen ausgeführt werden, werden pro Schleifenzyklus gezählt. Eine einzelne Aktion in einer „for each“-Schleife, mit der eine Liste mit zehn Einträgen verarbeitet wird, wird beispielsweise wie folgt gezählt: Die Anzahl von Listeneinträgen (10) wird mit der Anzahl von Aktionen in der Schleife (1) multipliziert, und anschließend wird der Wert 1 für das Starten der Schleife addiert. Die Berechnung für dieses Beispiel lautet also (10 * 1) + 1, sodass sich insgesamt elf Aktionsausführungen ergeben.

## <a name="integration-account-usage"></a>Nutzung des Integrationskontos

Die verbrauchsbasierte Nutzung umfasst ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md), unter dem Sie die Features [B2B/EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) in Logic Apps ohne zusätzliche Kosten erkunden, entwickeln und testen können. Sie können jeweils ein Integrationskonto pro Region nutzen und eine bestimmte [Anzahl von Artefakten](../logic-apps/logic-apps-limits-and-config.md) speichern – beispielsweise EDI-Handelspartner und -Vereinbarungen, Zuordnungen, Schemas, Assemblys, Zertifikate und Batchkonfigurationen.

Für Logic Apps sind außerdem Basic- und Standard-Integrationskonten mit unterstützter Logic Apps-SLA verfügbar. Sie können Basic-Integrationskonten verwenden, wenn Sie entweder nur die Behandlung von Nachrichten nutzen oder als Partner für ein kleines Unternehmen fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist. Standard-Integrationskonten unterstützen komplexere B2B-Beziehungen und erhöhen die Anzahl von Entitäten, die Sie verwalten können. Weitere Informationen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher über Logic Apps.][whatis]
* [Erstellen Ihrer ersten Logik-App][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

