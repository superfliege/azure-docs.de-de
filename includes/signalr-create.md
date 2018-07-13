---
title: Includedatei
description: Includedatei
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: c26e2de2c492c51c35527979ef0450ed7c2de499
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100826"
---
1. Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an, um eine neue Azure SignalR Service-Ressource zu erstellen. Klicken Sie oben links auf der Seite auf **+ Create a resource** (+ Ressource erstellen). Geben Sie im Textfeld **Marketplace durchsuchen** **SignalR Service** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie in den Ergebnissen auf **SignalR Service** und dann auf **Erstellen**.

3. Fügen Sie auf der neuen **SignalR**-Seite „Einstellungen“ die folgenden Einstellungen für die neue SignalR-Ressource hinzu:

    | Name | Empfohlener Wert | Beschreibung |
    | ---- | ----------------- | ----------- |
    | Ressourcenname | *testsignalr* | Geben Sie für die SignalR-Ressource einen eindeutigen Ressourcennamen ein. Der Name muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und das Zeichen `-` enthalten. Der Name darf weder mit dem Zeichen `-` beginnen oder enden, noch mehrere aufeinanderfolgende Zeichen vom Typ `-` enthalten.|
    | Abonnement | Auswählen Ihres Abonnements |  Wählen Sie das Azure-Abonnement aus, das Sie zum Testen von SignalR verwenden möchten. Falls das Konto nur über ein einzelnes Abonnement verfügt, wird automatisch dieses Abonnement ausgewählt, und die Dropdownliste **Abonnement** wird nicht angezeigt.|
    | Ressourcengruppe | Erstellen einer neue Ressourcengruppe namens *SignalRTestResources*| Wählen Sie eine Ressourcengruppe für Ihre SignalR-Ressource aus, oder erstellen Sie eine. Diese Gruppe ist beim Organisieren mehrerer Ressourcen hilfreich, die Sie möglicherweise zur gleichen Zeit löschen möchten, indem Sie Ressourcengruppe löschen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-overview.md). |
    | Standort | *USA, Osten* | Verwenden Sie **Standort**, um den geografischen Standort anzugeben, an dem Ihre SignalR-Ressource gehostet wird. Für die beste Leistung wird empfohlen, dass Sie die Ressource in derselben Region wie andere Komponenten Ihrer Anwendung erstellen. |
    | Tarif | *Free* | Aktuell sind die Optionen **Free** und **Standard** verfügbar. |
    | An Dashboard anheften | ✔ | Aktivieren Sie dieses Kontrollkästchen, um die Ressource an Ihr Dashboard anzuheften, damit sie einfacher zu finden ist. |

4. Klicken Sie auf **Create** (Erstellen). Die Bereitstellung kann einige Minuten dauern.

5. Sobald die Bereitstellung abgeschlossen ist, klicken Sie unter **SETTINGS** (EINSTELLUNGEN) auf **Keys** (Schlüssel). Kopieren Sie die Primärschlüssel-Verbindungszeichenfolge. Sie werden sie später zum Konfigurieren Ihrer App verwenden, um die Azure SignalR Service-Ressource zu verwenden.

    Die Verbindungszeichenfolge weist das folgende Format auf:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
