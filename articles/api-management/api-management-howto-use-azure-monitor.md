---
title: "Überwachen von veröffentlichten APIs in Azure API Management | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie Ihre API in API Management überwachen."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Überwachen von veröffentlichten APIs

Azure Monitor ist ein Azure-Dienst, mit dem Sie all Ihre Azure-Ressourcen von einer einzigen Quelle aus verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus Azure-Ressourcen wie API Management visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API 
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Im folgenden Video wird die Überwachung von API Management mithilfe von Azure Monitor veranschaulicht. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie das folgende Schnellstarttutorial: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Import and publish your first API](import-and-publish.md) (Importieren und Veröffentlichen Ihrer ersten API).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Anzeigen von Aktivitätsprotokollen

Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre API Management-Dienste ausgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für Ihre API Management-Dienste durchgeführt wurden. 

> [!NOTE]
> Aktivitätsprotokolle enthalten keine Lesevorgänge (GET) oder Vorgänge, die im klassischen Herausgeberportal oder mithilfe der ursprünglichen Management-APIs durchgeführt wurden.

Sie können in Ihrem API Management-Dienst auf Aktivitätsprotokolle oder in Azure Monitor auf Protokolle all Ihrer Azure-Ressourcen zugreifen. 

So zeigen Sie Aktivitätsprotokolle an:

1. Klicken Sie in Ihrer **API Management**-Instanz auf **Aktivitätsprotokoll**.

## <a name="view-diagnostic-logs"></a>Anzeigen von Diagnoseprotokollen

Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

So greifen Sie auf Diagnoseprotokolle zu:

1. Klicken Sie in Ihrer **API Management**-Instanz auf **Diagnoseprotokoll**.

## <a name="view-metrics-of-your-apis"></a>Anzeigen von Metriken Ihrer API

Von API Management werden jede Minute Metriken ausgegeben, sodass Sie einen Überblick über den Zustand und die Integrität Ihrer APIs nahezu in Echtzeit erhalten. Hier sehen Sie eine Übersicht über einige der verfügbaren Metriken:

* Kapazität (Vorschau): Unterstützt Sie beim Treffen von Entscheidungen in Bezug auf Upgrades/Downgrades Ihrer APIM-Dienste. Die Metrik wird minütlich ausgegeben und spiegelt die Gatewaykapazität zum Zeitpunkt der Meldung wider. Der Wert der Metrik kann zwischen 0 und 100 liegen und wird basierend auf Gatewayressourcen wie CPU und Speicherauslastung berechnet.
* Gatewayanforderungen gesamt: Anzahl der API-Anforderungen innerhalb des Zeitraums. 
* Erfolgreiche Gatewayanforderungen: Anzahl der API-Anforderungen, die erfolgreiche HTTP-Antwortcodes einschließlich 304, 307 und alle unter 301 (z.B. 200) erhalten haben. 
* Fehlerhafte Gatewayanforderungen: Anzahl der API-Anforderungen, die fehlerhafte HTTP-Antwortcodes einschließlich 400 und alle über 500 erhalten haben.
* Nicht autorisierte Gatewayanforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes einschließlich 401, 403 und 429 erhalten haben. 
* Andere Gateway-Anforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes erhalten haben, die keiner der oben genannten Kategorien angehören (z.B. 418).

So greifen Sie auf Metriken zu:

1. Klicken Sie im Menü am unteren Seitenrand auf **Metriken**.
2. Wählen Sie im Dropdownmenü die gewünschten Metriken aus (Sie können mehrere Metriken hinzufügen). 
    
    Wählen Sie in der Liste der verfügbaren Metriken beispielsweise **Gatewayanforderungen gesamt** und **Fehlerhafte Gatewayanforderungen** aus.
3. Im Diagramm ist die Gesamtanzahl von API-Aufrufen gezeigt. Es enthält außerdem die Anzahl fehlerhafter API-Aufrufe. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Einrichten einer Warnungsregel für nicht autorisierte Anforderungen

Sie können die Konfiguration so durchführen, dass Warnungen basierend auf Metriken und Aktivitätsprotokollen empfangen werden. Mit Azure Monitor können Sie eine Warnung so konfigurieren, dass Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden einer E-Mail-Benachrichtigung
* Aufrufen eines Webhooks
* Aufrufen einer Azure-Logik-App

So konfigurieren Sie Warnungen:

1. Klicken Sie auf der Menüleiste am unteren Seitenrand auf **Warnungsregeln**.
2. Wählen Sie **Metrikwarnung hinzufügen** aus.
3. Geben Sie unter **Name** einen Namen für die Warnung ein.
4. Wählen Sie als zu überwachende Metrik **Nicht autorisierte Gatewayanforderungen** aus.
5. Wählen Sie **Email owners, contributors, and readers** (E-Mail an Besitzer, Mitwirkende und Leser senden) aus.
6. Klicken Sie auf **OK**.
7. Versuchen Sie, die Konferenz-API ohne API-Schlüssel aufzurufen. Als Besitzer dieses API Management-Diensts erhalten Sie eine E-Mail-Benachrichtigung. 

    > [!TIP]
    > Die Warnungsregel kann bei Auslösung auch einen Webhook oder eine Azure-Logik-App aufrufen.

    ![set-up-alert](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API 
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management](api-management-howto-api-inspector.md)