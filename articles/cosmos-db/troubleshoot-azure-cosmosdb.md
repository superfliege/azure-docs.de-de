---
title: Verwalten von Azure Cosmos DB in Azure Storage-Explorer
description: Erfahren Sie, wie Sie Azure Cosmos DB in Azure Storage-Explorer verwalten.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: c593eb2b49d15d20a26ef735d1032d5dea45f125
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-in-storage-explorer-troubleshooting-guide-overview"></a>Azure Cosmos DB in Storage-Explorer – Leitfaden zur Problembehandlung – Übersicht

[Azure Cosmos DB in Azure Storage-Explorer](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer) ist eine eigenständige App, mit der Sie von Windows, macOS oder Linux eine Verbindung mit Azure Cosmos DB-Konten, die in Azure und Sovereign Clouds gehostet werden, herstellen können. Sie können damit Azure Cosmos DB-Entitäten verwalten, Daten bearbeiten und gespeicherte Prozeduren und Trigger zusammen mit anderen Azure-Entitäten wie Speicherblobs und Warteschlangen aktualisieren.

In diesem Leitfaden sind Lösungen für häufige Probleme von Azure Cosmos DB in Storage-Explorer zusammengefasst.

- [Probleme bei der Anmeldung](#Sign-in-issues)
  - [Selbstsigniertes Zertifikat in der Zertifikatkette](#Self-signed-certificate-in-certificate-chain)
  - [Abonnements können nicht abgerufen werden](#Unable-to-retrieve-subscriptions)
  - [Authentifizierungsseite wird nicht angezeigt](#Unable-to-see-the-authentication-page)
  - [Konto kann nicht entfernt werden](#Cannot-remove-account)
- [HTTP/HTTPS-Proxyproblem](#Http/Https-proxy-issue)
- [Problem mit dem Knoten „Development“ unter „Local and Attached“](#Development-node-under-Local-and-Attached-node-issue)
- [Fehler beim Anfügen eines Azure Cosmos DB-Kontos im Knoten „Local and Attached“](#Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error)
- [Fehler beim Erweitern des Azure Cosmos DB-Knotens](#Expand-Azure-Cosmos-DB-node-error)
- [Nächste Schritte](#Next-steps)

<h2 id="Sign-in-issues">Probleme bei der Anmeldung</h2>

Versuchen Sie vor dem Fortfahren, Ihre Anwendung neu zu starten, um zu prüfen, ob die Probleme behoben werden können.

<h2 id="Self-signed-certificate-in-certificate-chain">Selbstsigniertes Zertifikat in der Zertifikatkette</h2>

Es gibt verschiedene Gründe, warum dieser Fehler unter Umständen angezeigt wird. Die zwei häufigsten Gründe sind:

1. Sie befinden sich hinter einem „transparenten Proxy“. Dies bedeutet, dass HTTPS-Datenverkehr abgefangen (z.B. von Ihrer IT-Abteilung) und entschlüsselt und anschließend mit einem selbstsignierten Zertifikat wieder verschlüsselt wird.

2. Sie führen Software aus, z.B. Virenschutzsoftware, mit der ein selbstsigniertes Zertifikat in Ihre HTTPS-Nachrichten injiziert wird.

Wenn Storage-Explorer eines dieser „selbstsignierten Zertifikate“ erkennt, kann nicht mehr überprüft werden, ob die empfangene HTTPS-Nachricht manipuliert wurde. Falls Sie aber über eine Kopie des selbstsignierten Zertifikats verfügen, können Sie Storage-Explorer anweisen, es als vertrauenswürdig anzusehen. Wenn Sie unsicher sind, von wem das Zertifikat injiziert wird, können Sie mit den folgenden Schritten versuchen, es selbst zu finden:

1. Installieren Sie OpenSSL.
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (alle Light-Versionen sind geeignet)
     - Mac und Linux: Sollte im Betriebssystem enthalten sein
2. Führen Sie OpenSSL aus.
    - Windows: Navigieren Sie zum Installationsverzeichnis und dann zu **/bin/**, und doppelklicken Sie anschließend auf **openssl.exe**.
    - Mac und Linux: Führen Sie **openssl** über ein Terminal aus.
3. Führen Sie `s_client -showcerts -connect microsoft.com:443` aus.
4. Suchen Sie nach selbstsignierten Zertifikaten. Wenn Sie nicht sicher sind, welche Zertifikate selbstsigniert sind, sollten Sie nach Stellen suchen, an denen der Antragsteller („s:“) und der Zertifikataussteller („i:“) identisch sind.
5.  Nachdem Sie selbstsignierte Zertifikate gefunden haben, kopieren Sie den gesamten Inhalt von **-----BEGIN CERTIFICATE-----** bis **-----END CERTIFICATE-----** (einschließlich) und fügen ihn für jedes Zertifikat in eine neue CER-Datei ein.
6.  Öffnen Sie Storage-Explorer, und navigieren Sie dann zu **Bearbeiten** > **SSL-Zertifikate** > **Zertifikate importieren**. Verwenden Sie die Dateiauswahl, um die von Ihnen erstellten CER-Dateien zu suchen, auszuwählen und zu öffnen.

Falls Sie mit den oben angegebenen Schritten keine selbstsignierten Zertifikate finden können, können Sie Feedback senden, um weitere Hilfe zu erhalten.

<h2 id="Unable-to-retrieve-subscriptions">Abonnements können nicht abgerufen werden</h2>

Gehen Sie wie folgt vor, wenn Sie nach dem erfolgreichen Anmelden Ihre Abonnements nicht abrufen können:

- Melden Sie sich am [Azure-Portal](http://portal.azure.com/) an, um zu überprüfen, ob Ihr Konto Zugriff auf die Abonnements hat.
- Stellen Sie sicher, dass die Anmeldung mit der richtigen Umgebung ([Azure](http://portal.azure.com/), [Azure China](https://portal.azure.cn/), [Azure Deutschland](https://portal.microsoftazure.de/), [Azure US-Regierung](http://portal.azure.us/) oder „Benutzerdefinierte Umgebung/Azure Stack“) erfolgt ist.
- Wenn Sie sich hinter einem Proxy befinden, sollten Sie sicherstellen, dass Sie den Storage-Explorer-Proxy richtig konfiguriert haben.
- Entfernen Sie das Konto, und fügen Sie es wieder hinzu.
- Löschen Sie die folgenden Dateien aus Ihrem Basisverzeichnis (z.B. „C:\Users\ContosoUser“), und fügen Sie das Konto dann erneut hinzu:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Achten Sie beim Anmelden auf Fehlermeldungen in der Entwicklertools-Konsole (F12)

![Konsole](./media/troubleshoot-cosmosdb/console.png)

<h2 id="Unable-to-see-the-authentication-page">Authentifizierungsseite wird nicht angezeigt</h2>  

Gehen Sie wie folgt vor, wenn die Authentifizierungsseite nicht angezeigt wird:

- Je nach Verbindungsgeschwindigkeit kann es eine Weile dauern, bis die Anmeldeseite geladen wird. Warten Sie mindestens eine Minute, bevor Sie das Dialogfeld für die Authentifizierung schließen.
- Wenn Sie sich hinter einem Proxy befinden, sollten Sie sicherstellen, dass Sie den Storage-Explorer-Proxy richtig konfiguriert haben.
- Rufen Sie durch Drücken der Taste F12 die Entwicklerkonsole auf. Prüfen Sie in den Antworten in der Entwicklerkonsole, ob diese Aufschluss darüber geben, warum die Authentifizierung nicht funktioniert.

<h2 id="Cannot-remove-account">Konto kann nicht entfernt werden</h2>

Gehen Sie wie folgt vor, falls Sie ein Konto nicht entfernen können oder wenn der Link für die erneute Authentifizierung nicht funktioniert.

- Löschen Sie die folgenden Dateien aus Ihrem Basisverzeichnis, und fügen Sie das Konto dann wieder hinzu:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Löschen Sie Folgendes, wenn Sie Storage-Ressourcen mit SAS-Anfügung entfernen möchten:
  - Ordner „%AppData%/StorageExplorer“ für Windows
  - „/Users/<Ihr_Name>/Library/Application Support/StorageExplorer“ für Mac
  - „~/.config/StorageExplorer“ für Linux
  - Wenn Sie diese Dateien löschen, **müssen Sie alle Ihre Anmeldeinformationen erneut eingeben**.


<h2 id="Http/Https-proxy-issue">HTTP/HTTPS-Proxyproblem</h2>

Es ist nicht möglich, Azure Cosmos DB-Knoten in der linken Struktur aufzulisten, wenn Sie den HTTP/HTTPS-Proxy in ASE konfigurieren. Dies ist ein bekanntes Problem, das im nächsten Release behoben sein wird. Derzeit können Sie den Azure Cosmos DB-Daten-Explorer im Azure-Portal als Problemumgehung verwenden. 

<h2 id="Development-node-under-Local-and-Attached-node-issue">Problem mit dem Knoten „Development“ unter „Local and Attached“</h2>

Wenn Sie in der linken Struktur unter „Local and Attached“ (Lokal und angefügt) auf den Knoten „Development“ (Entwicklung) klicken, erfolgt keine Reaktion.  Dies ist das erwartete Verhalten. Der lokale Azure Cosmos DB-Emulator wird ab dem nächsten Release unterstützt.

![Knoten „Development“ (Entwicklung)](./media/troubleshoot-cosmosdb/development.png)

<h2 id="Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error">Fehler beim Anfügen eines Azure Cosmos DB-Kontos im Knoten „Local and Attached“</h2>

Wenn nach dem Anfügen des Azure Cosmos DB-Kontos unter dem Knoten „Local and Attached“ (Lokal und angefügt) der unten angegebene Fehler angezeigt wird, sollten Sie überprüfen, ob Sie die richtige Verbindungszeichenfolge verwenden.

![Fehler beim Anfügen von Azure Cosmos DB unter „Local and Attached“](./media/troubleshoot-cosmosdb/attached-error.png)

<h2 id="Expand-Azure-Cosmos-DB-node-error">Fehler beim Erweitern des Azure Cosmos DB-Knotens</h2>

Unter Umständen wird der folgende Fehler angezeigt, wenn Sie versuchen, in der linken Struktur die Knoten zu erweitern. 

![Fehler beim Erweitern](./media/troubleshoot-cosmosdb/expand-error.png)

Versuchen Sie es mit folgenden Lösungsvorschlägen:

- Überprüfen Sie, ob die Bereitstellung des Azure Cosmos DB-Kontos ausgeführt wird, und wiederholen Sie den Vorgang, nachdem die Erstellung des Kontos erfolgreich abgeschlossen wurde.
- Wenn sich das Konto unter dem Knoten „Quick Access“ (Schnellzugriff) oder „Local and Attached“ (Lokal und angefügt) befindet, sollten Sie überprüfen, ob das Konto gelöscht wurde. Wenn dies der Fall ist, müssen Sie den Knoten manuell entfernen.

<h2 id="Next-steps">Nächste Schritte</h2>

Falls Ihnen keine dieser Lösungen weiterhilft, können Sie eine E-Mail mit Details zum Problem an das Azure Cosmos DB Dev Tooling-Team ([cosmosdbtooling@microsoft.com](mailto:cosmosdbtooling@microsoft.com)) senden, um Informationen zur Problemlösung zu erhalten.





