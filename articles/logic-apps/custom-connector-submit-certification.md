---
title: "Zertifizieren benutzerdefinierter Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: "Machen Sie Connectors für alle Benutzer in Azure Logic Apps, Microsoft Flow und Microsoft PowerApps verfügbar."
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9bcb138c181df0c12548cc5dc721542fd9c9ba68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="submit-your-connectors-for-microsoft-certification"></a>Einreichen von Connectors für die Microsoft-Zertifizierung

Um benutzerdefinierte Connectors für alle Benutzer in Azure Logic Apps, Microsoft Flow und Microsoft PowerApps öffentlich verfügbar zu machen, müssen Sie Ihren Connector zur Überprüfung, Validierung und Genehmigung zur Veröffentlichung an Microsoft senden. 

## <a name="certification-criteria"></a>Zertifizierungskriterien

| Funktion | Details | Erforderlich oder empfohlen |
|------------|---------|-------------------------|
| Software-as-a-Service-App (SaaS) | Erfüllt die Anforderungen eines Benutzerszenarios, das gut für Logic Apps, Flow und PowerApps geeignet ist. | Erforderlich |
| Authentifizierungstyp | Ihre API muss die OAuth2-, API-Schlüssel- oder Standardauthentifizierung unterstützen. | Erforderlich | 
| Support | Sie müssen einen Supportansprechpartner angeben, den Kunden um Hilfe bitten können. | Erforderlich | 
| Verfügbarkeit und Betriebszeit | Ihre App hat eine Betriebszeit von mindestens 99,9%. | Empfohlen | 
|||| 

Wenn Sie außerdem kein Microsoft-Partner oder unabhängiger Softwareanbieter (Independent Software Vendor, ISV) sind und einen Connector zertifizieren und öffentlich freigeben möchten, müssen Sie entweder den zugrunde liegenden Dienst besitzen oder ausdrückliche Rechte zur Nutzung der API vorweisen.

## <a name="validation-phases"></a>Validierungsphasen

Microsoft verwendet diese Validierungsphasen zur Bewertung Ihres Connectors:

| Validierungsphase | Beschreibung | 
| ----- | ----------- |
| Funktionalität | Microsoft testet Ihren Connector mit Logic Apps, Flow und PowerApps auf diese Fehler: <p>Ungültige OpenAPI (Swagger) oder JSON-Fehler, die im Abschnitt "Definition" des Assistenten für benutzerdefinierte Connectors angezeigt werden <p>Laufzeit- und Schemavalidierungsfehler, die im Abschnitt "Test" des Assistenten für benutzerdefinierte Connectors angezeigt werden | 
| Inhalt | Microsoft prüft, ob Ihr Connector für jede Entität Anzeigenamen und Beschreibungen verwendet. Stellen Sie sicher, dass jeder Vorgang, jeder Eingabeparameter und jedes Antwortattribut in der Swagger-Datei Ihres Connectors folgende Elemente enthält: <p>- [Zusammenfassung](../logic-apps/custom-connector-openapi-extensions.md#summary) <br>- [Beschreibung](../logic-apps/custom-connector-openapi-extensions.md#description) </br>- [Informationen zur Sichtbarkeit](../logic-apps/custom-connector-openapi-extensions.md#visibility) | 
||| 

## <a name="nominate-and-submit-your-connector-to-microsoft-for-certification"></a>Vorschlagen und Einreichen des Connectors zur Zertifizierung bei Microsoft

Führen Sie zum Beantragen der Zertifizierung die folgenden Schritte aus:

1. **Vorschlagen**

   1. [Reichen Sie Ihren Vorschlag ein](https://go.microsoft.com/fwlink/?linkid=848754).

      1. Wählen Sie unten auf der Seite **Kontakt** aus.
      2. Füllen Sie das Formular aus, und wählen Sie **Fragen zum ISV-Connector-Programm und zum Co-Marketing** aus.
      3. Klicken Sie auf **Übermitteln**.

   2. Unterzeichnen Sie die gegenseitige Vertraulichkeits- und Partnervereinbarung, die Sie erhalten. 

      Microsoft verlangt diese unterschriebenen Verträge, bevor es weitergeht. 
      Wir prüfen anschließend, ob Ihr Connector die Zertifizierungskriterien erfüllt. 
   
   3. Sobald Ihr Connector freigegeben ist, erhalten Sie von Microsoft Anweisungen für das Onboarding.
    
2. **Überprüfung**

   1. Senden Sie diese Informationen zur Überprüfung an den Ansprechpartner für Ihren Vorschlag:

      * Die OpenAPI-Datei, die Ihre API beschreibt
      * Die Symboldatei (PNG oder JPG), die Ihren Connector darstellt 
      
        Ihr Logo muss eine Größe von ca. 160 Pixeln innerhalb eines Quadrats mit 230 Pixeln haben. 
        Ein weißes Logo auf farbigem Hintergrund wird bevorzugt.
      
      * Die Brandingfarbe Ihres Symbols im Hexadezimalformat, die dem farbigen Hintergrund in der Symboldatei entsprechen muss

      * Ein Testkonto für die Validierung
      * Ein Supportansprechpartner

   2. Sollten wir weitere Informationen benötigen, werden wir Sie kontaktieren.

3. **Veröffentlichen**

    Nachdem wir die Funktionalität und den Inhalt Ihres Connectors validiert haben, stellen wir den Connector in allen Produkten und Regionen bereit.
    
    Standardmäßig werden alle Connectors als „Premium“-Connectors veröffentlicht. 
    Wenn Sie Ihre App mit Azure erstellt haben, können Sie beantragen, dass Ihr Connector als „Standard“-Connector für alle Benutzer mit Office 365 Enterprise-Plänen registriert wird. 
    Weitere Details erhalten Sie vom Ansprechpartner für Ihren Vorschlag.

## <a name="next-steps"></a>Nächste Schritte

* [Häufig gestellte Fragen zu benutzerdefinierten Connectors](../logic-apps/custom-connector-faq.md)
* [Übersicht über benutzerdefinierte Connectors](../logic-apps/custom-connector-overview.md)