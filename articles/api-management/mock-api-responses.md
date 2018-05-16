---
title: Simulieren von API-Antworten mit dem Azure-Portal | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie API Management (APIM) verwenden, um eine Richtlinie für eine API festzulegen, damit diese eine simulierte Antwort zurückgibt. Diese Methode ermöglicht Entwicklern das Fortsetzen von Implementierung und Tests der API Management-Instanz, wenn das Back-End nicht verfügbar ist und keine echten Antworten senden kann.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4383ce3788f6fade5299d69ef99b80221c58d9e7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="mock-api-responses"></a>Simulieren von API-Antworten

Back-End-APIs können in eine API-APIM importiert oder manuell erstellt und verwaltet werden. Die Schritte in diesem Tutorial veranschaulichen das Verwenden von APIM zum Erstellen einer leeren API, die manuell verwaltet wird. Im Tutorial wird gezeigt, wie Sie eine Richtlinie für eine API festlegen, damit diese eine simulierte Antwort zurückgibt. Diese Methode ermöglicht Entwicklern das Fortsetzen von Implementierung und Tests der APIM-Instanz, selbst wenn das Back-End nicht verfügbar ist und keine echten Antworten senden kann. Die Möglichkeit zum Simulieren von Antworten kann in verschiedenen Szenarien nützlich sein:

+ Die API-Facade wird entworfen, bevor das Back-End implementiert wird. Oder das Back-End wird parallel entwickelt.
+ Das Back-End ist vorübergehend nicht betriebsbereit, oder es kann nicht skaliert werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Test-API 
> * Hinzufügen eines Vorgangs zur Test-API
> * Aktivieren der Antwortsimulation
> * Testen der simulierten API

![Simulierte Antwort auf einen Vorgang](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).

## <a name="create-a-test-api"></a>Erstellen einer Test-API 

In diesem Abschnitt erfahren Sie, wie Sie eine leere API ohne Back-End erstellen. Außerdem wird gezeigt, wie Sie der API einen Vorgang hinzufügen. Das Aufrufen des Vorgangs nach Abschluss der Schritte in diesem Abschnitt führt zu einem Fehler. Sie erhalten keine Fehler mehr, nachdem die Schritte im Abschnitt „Aktivieren der Antwortsimulation“ abgeschlossen wurden.

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie im linken Menü die Option **+ API hinzufügen** aus.
3. Wählen Sie in der Liste **Leere API** aus.
4. Geben Sie „*Test API*“ als **Anzeigename** ein.
5. Geben Sie „*Unlimited*“ für **Produkte** ein.
6. Klicken Sie auf **Erstellen**.

## <a name="add-an-operation-to-the-test-api"></a>Hinzufügen eines Vorgangs zur Test-API

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben.
2. Klicken Sie auf **+ Vorgang hinzufügen**.

    ![Simulierte Antwort auf einen Vorgang](./media/mock-api-responses/mock-api-responses02.png)

    |Einstellung|Wert|BESCHREIBUNG|
    |---|---|---|
    |**URL** (HTTP-Verb)|GET|Sie können eines der vordefinierten HTTP-Verben auswählen.|
    |**URL** |*/test*|Ein URL-Pfad für die API. |
    |**Anzeigename**|*Test call*|Dieser Name wird im **Entwicklerportal** angezeigt.|
    |**Beschreibung**||Geben Sie eine Beschreibung für den Vorgang an, die als Dokumentation für Entwickler dient, die diese API im **Entwicklerportal** verwenden.|
    |Registerkarte **Abfrage**||Sie können Abfrageparameter hinzufügen. Neben einem Namen und einer Beschreibung können Sie Werte angeben, die diesem Parameter zugewiesen werden können. Einer der Werte kann als Standard markiert werden (optional).|
    |Registerkarte **Anforderung**||Sie können Inhaltstypen für die Anforderung, Beispiele und Schemas definieren. |
    |Registerkarte **Antwort**|Sehen Sie sich Schritte in dieser Tabelle an.|Definieren Sie Statuscodes, Inhaltstypen, Beispiele und Schemas für die Antwort.|

3. Wählen Sie die Registerkarte **Antwort** unter den Feldern „URL“, „Anzeigename“ und „Beschreibung“ aus.
4. Klicken Sie auf **+ Antwort hinzufügen**.
5. Wählen Sie **200 OK** in der Liste aus.
6. Wählen Sie unter der Überschrift **Darstellungen** auf der rechten Seite **+ Darstellung hinzufügen** aus.
7. Geben Sie „*application/json*“ in das Suchfeld ein, und wählen Sie den Inhaltstyp **application/json** aus.
8. Geben Sie im Testfeld **Beispiel** die Zeichenfolge „*{ 'sampleField' : 'test' }*“ ein.
9. Wählen Sie **Speichern**aus.

## <a name="enable-response-mocking"></a>Aktivieren der Antwortsimulation

1. Wählen Sie die API aus, die Sie im Schritt „Erstellen einer Test-API“ erstellt haben.
2. Wählen Sie den Testvorgang aus, den Sie hinzugefügt haben.
2. Klicken Sie im Fenster auf der rechten Seite auf die Registerkarte **Entwurf**.
3. Klicken Sie im Fenster **Eingehende Verarbeitung** auf das Stiftsymbol.
4. Wählen Sie auf der Registerkarte **Simulation** für **Simulationsverhalten** die Option **Statische Antworten** aus.
5. Geben Sie im Textfeld **API Management returns the following response:** (API Management gibt folgende Antwort zurück) die Zeichenfolge **200 OK, application/json** ein. Diese Auswahl gibt an, dass Ihre API das Antwortbeispiel zurückgeben soll, das Sie im vorherigen Abschnitt definiert haben.
6. Wählen Sie **Speichern**aus.

## <a name="test-the-mocked-api"></a>Testen der simulierten API

1. Wählen Sie die API aus, die Sie im Schritt „Erstellen einer Test-API“ erstellt haben.
2. Öffnen Sie die Registerkarte **Testen**.
3. Vergewissern Sie sich, dass die **Testaufruf**-API ausgewählt ist.

    > [!TIP]
    > Ein gelber Balken mit dem Text **Die Antwortsimulation ist aktiviert** gibt an, dass die von API Management zurückgegebenen Antworten einer Simulationsrichtlinie folgen und keine tatsächliche Back-End-Antwort darstellen.

3. Wählen Sie **Senden** aus, um einen Testaufruf durchzuführen.
4. In **HTTP-Antwort** wird der JSON-Code angezeigt, der als Beispiel im ersten Abschnitt des Tutorials angegeben wurde.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Test-API
> * Hinzufügen eines Vorgangs zur Test-API
> * Aktivieren der Antwortsimulation
> * Testen der simulierten API

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Transformieren und Schützen einer veröffentlichten API](transform-api.md)
