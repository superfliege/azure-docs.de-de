---
title: Verwenden von API-Aufrufen in einer Unterhaltungslernanwendung – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie API-Aufrufe in einer Unterhaltungslernanwendung verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ec752cbadfac7a47e08ed7b0ffe8bb475969fac5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376275"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-application"></a>Hinzufügen von API-Aufrufen zu einer Unterhaltungslernanwendung

Dieses Tutorial zeigt, wie Sie Ihrer Anwendung API-Aufrufe hinzufügen. API-Aufrufe sind Funktionen, die Sie in Ihrem Bot definieren und erstellen und die vom Unterhaltungslernmodul aufgerufen werden können.

## <a name="requirements"></a>Anforderungen
Für dieses Tutorial muss der tutorialAPICalls.ts-Bot ausgeführt werden.

    npm run tutorial-api-calls

## <a name="details"></a>Details

- Mithilfe von API-Aufrufen können Entitäten gelesen und bearbeitet werden.
- API-Aufrufe haben Zugriff auf das Speicher-Manager-Objekt.
- API-Aufrufe können auch Argumente annehmen – dadurch kann der gleiche API-Aufruf zu verschiedenen Zwecken dienen.

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste der Webbenutzeroberfläche auf „Tutorial-12-APICalls“. 

### <a name="entities"></a>Entitäten

Wir haben in der Anwendung eine Entität mit dem Namen „number“ (Zahl) definiert.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API-Aufrufe
Der Code für die API-Aufrufe ist in dieser Datei definiert: „C:\<Installationspfad\>\src\demos\tutorialAPICalls.ts“.

![](../media/tutorial12_apicalls.PNG)

- Der erste API-Rückruf ist „RandomGreeting“. Er gibt eine zufällige Grußformel zurück, die in der Variable „greeting“ (Grußformel) definiert ist.
- Der Multiply-API-Rückruf: Er multipliziert zwei vom Benutzer angegebene Zahlen. Anschließend gibt er das Ergebnis der Multiplikation der zwei Zahlen zurück. Dies zeigt, dass API-Rückrufe Eingaben annehmen können. Beachten Sie, dass der Speichermanager das erste Argument ist. 
- Der ClearEntities-API-Rückruf: Löscht die number-Entität, um dem Benutzer die Eingabe der nächsten Zahl zu ermöglichen. Dies veranschaulicht, wie Entitäten mithilfe von API-Aufrufen bearbeitet werden können.

### <a name="actions"></a>Aktionen

Wir haben vier Aktionen erstellt. 

![](../media/tutorial12_actions.PNG)

- Über ‚Welche Zahl möchten Sie mit 12 multiplizieren?‘ hinaus, das eine kommunikative Aktion darstellt, gibt es drei verschiedene API-Aufrufe, die die typischen Muster von API-Aufrufen veranschaulichen.

- „RandomGreeting“ ist eine Aktion ohne Wartestatus. Um sie einzurichten, haben wir im Dialogfeld „Create Action“ (Aktion erstellen) den Aktionstyp API_LOCAL und anschließend „RandomGreeting“ ausgewählt. 

![](../media/tutorial12_setupapicall.PNG)

Die Aktualisieren-Schaltfläche neben der API wird in dem Fall verwendet, dass der Bot angehalten und Änderungen an den APIs vorgenommen werden. In diesem Fall würden durch Klicken auf „Aktualisieren“ die letzten Änderungen abgerufen.

So haben wir die Multiply-Aktion erstellt: Nach dem Auswählen von API_Local und API haben wir eine Entität ($number) für den ersten Eingabewert (num1string) und einen Wert (12) für den zweiten Eingabewert (num2string) eingegeben. Dadurch wird eine Dereferenzierungsebene zwischen dem Bot und den API-Aufrufen bereitgestellt, sodass der gleiche Rückruf mehreren Aktionen im System zugeordnet werden kann, die sich in der Zuweisung der Aktionen unterscheiden.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Trainingsdialog

Im Folgenden befassen wir uns mit einem Trainingsdialog.

1. Klicken Sie auf „Train Dialogs“ (Trainingsdialoge) und dann auf „New Train Dialog“ (Neuer Trainingsdialog).
1. Geben Sie „Hi“ ein.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Klicken Sie, um „RandomGreeting“ auszuwählen. Dadurch wird der Random-Greeting-API-Aufruf ausgeführt.
3. Klicken Sie, um „What number to do you want to multiply by 12?“ (Welche Zahl möchten Sie mit 12 multiplizieren?) auszuwählen.
4. Geben Sie „8“ ein. Klicken Sie dann auf „Score Actions“ (Bewertungsaktionen).
4. Wählen Sie „Multiply $number 12“ aus. Beachten Sie das Ergebnis der Multiplikation.
5. Wählen Sie „Clear Entities“ (Entitäten löschen) aus.
    - Beachten Sie, dass der Wert der Zahlentität gelöscht wurde.
3. Klicken Sie, um „What number to do you want to multiply by 12?“ (Welche Zahl möchten Sie mit 12 multiplizieren?) auszuwählen.
4. Klicken Sie auf „Test abgeschlossen“.

![](../media/tutorial12_dialog.PNG)

Sie haben nun erfahren, wie API-Rückrufe registriert werden, eine gängige Muster gesehen und das Definieren von Argumenten sowie das Zuordnen von Werten und Entitäten in ihnen kennengelernt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Karten – Teil 1](./13-cards-1.md)
