---
title: Includedatei
description: Includedatei
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 5169432af65a046465c9d1ced349687d6fdc8bb7
ms.sourcegitcommit: caebf2bb2fc6574aeee1b46d694a61f8b9243198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2018
ms.locfileid: "35394025"
---
Wenn Python beim Starten Ihrer Anwendung einen Fehler erkennt, wird nur eine einfache Seite mit einer Fehlermeldung zurückgegeben (z.B. „Die Seite kann aufgrund eines internen Serverfehlers nicht angezeigt werden.“).

So erfassen Sie Python-Anwendungsfehler

1. Wählen Sie im Azure-Portal in Ihrer Web-App **Einstellungen** aus.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Anwendungseinstellungen**.
3. Geben Sie unter **App-Einstellungen** das folgende Schlüssel/Wert-Paar ein:
    * Schlüssel: WSGI_LOG
    * Wert: D:\home\site\wwwroot\logs.txt (geben Sie den Dateinamen Ihrer Wahl ein)

Fehler sollten jetzt im Ordner „wwwroot“ in der Datei „logs.txt“ angezeigt werden.
