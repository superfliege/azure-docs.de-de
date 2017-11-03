---
title: "Integrieren von Azure Stack-Sicherheitsfunktionen in Datencenter-Sicherheitslösungen"
description: "Erfahren Sie, wie Sie Azure Stack-Sicherheitsfunktionen in Ihre Datencenter-Sicherheitslösungen integrieren."
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/17/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: eb7c651362838d44d6558e080e6130b4a8041d1e
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---security"></a>Integrieren von Azure Stack-Sicherheitsfunktionen in Datencenter-Sicherheitslösungen

*Gilt für: Integrierte Azure Stack-Systeme*

Azure Stack wurde unter Berücksichtigung von Sicherheitsaspekten entworfen und entwickelt. Aus diesem Grund ist Azure Stack ein in sich geschlossenes System, in dem die Installation eines Softwaresicherheitsagents nicht unterstützt wird.

Dieser Artikel hilft Ihnen beim Integrieren von Azure Stack-Sicherheitsfunktionen in Sicherheitslösungen, die bereits in Ihrem Datencenter bereitgestellt sind.

## <a name="security-logs"></a>Sicherheitsprotokolle

Azure Stack erfasst alle zwei Minuten Betriebssystem- und Sicherheitsereignisse für Infrastrukturrollen und Knoten von Skalierungseinheiten. Protokolle werden in Blobcontainern in einem Speicherkonto gespeichert.

Für jede Infrastrukturrolle ist ein Speicherkonto vorhanden, und für alle üblichen Betriebssystemereignisse steht ein allgemeines Speicherkonto zur Verfügung.

Der Integritätsressourcenanbieter kann über das REST-Protokoll aufgerufen werden und ruft so die URL für den Blobcontainer ab. Sicherheitslösungen von Drittanbietern können auf die API und die Speicherkonten zurückgreifen, um Ereignisse abzurufen, die verarbeitet werden sollen.

### <a name="use-azure-storage-explorer-to-view-events"></a>Verwenden des Azure Storage-Explorers zum Anzeigen von Ereignissen

Sie können von Azure Stack erfasste Ereignisse mithilfe des Tools Azure Storage-Explorer abrufen. Dieses steht unter [http://storageexplorer.com](http://storageexplorer.com) zum Download bereit.

Die folgende Vorgehensweise ist ein Beispiel, das Sie zum Konfigurieren des Azure Storage-Explorers für Azure Stack verwenden können:

1. Melden Sie sich beim Azure Stack-Administratorportal als Operator an.
2. Suchen Sie unter **Speicherkonten** nach dem Eintrag **frphealthaccount**. Das Konto **frphealthaccount** ist das allgemeine Speicherkonto, mit dem alle Betriebssystemereignisse gespeichert werden.

   ![Speicherkonten](media/azure-stack-integrate-security/storage-accounts.png)

3. Wählen Sie **frphealthaccount** aus, und klicken Sie anschließend auf **Zugriffsschlüssel**.

   ![Zugriffsschlüssel](media/azure-stack-integrate-security/access-keys.png)

4. Kopieren Sie den Zugriffsschlüssel in die Zwischenablage.
5. Öffnen Sie den Azure-Speicher-Explorer.
6. Wählen Sie im Menü **Bearbeiten** **Target Azure Stack** (Azure Stack als Ziel verwenden) aus.
7. Wählen Sie **Konto hinzufügen** aus, und aktivieren Sie anschließend das Kontrollkästchen **Use a storage account name and key** (Einen Speicherkontonamen und -schlüssel verwenden).

   ![Herstellen einer Verbindung mit Azure Storage](media/azure-stack-integrate-security/connect-storage.png)

8. Klicken Sie auf **Weiter**.

9. Führen Sie auf der Seite **Externen Speicher anfügen** folgende Schritte aus:

   a. Geben Sie als Kontonamen **frphealthaccount** ein.

   b. Fügen Sie den Zugriffsschlüssel für das Speicherkonto ein.

   c. Aktivieren Sie unter **Domäne der Speicherendpunkte** das Kontrollkästchen **Other (specify below)** (Weitere... (können unten angegeben werden)), und geben Sie den Speicherendpunkt in der Form **[Region].[Domänenname]** an.

   d. Aktivieren Sie das Kontrollkästchen **Use HTTP** (HTTP verwenden).

   ![Anfügen von externem Speicher](media/azure-stack-integrate-security/attach-storage.png)

10. Klicken Sie auf **Weiter**, überprüfen Sie die Zusammenfassung, und klicken Sie anschließend auf **Finish** (Fertig stellen), um den Assistenten zu beenden.
11. Sie können nun die einzelnen Blobcontainer durchsuchen und die Ereignisse herunterladen.

   ![Durchsuchen von Blobcontainern](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Verwenden von Programmiersprachen zum Zugreifen auf Ereignisse

Sie können verschiedene Programmiersprachen verwenden, um auf ein Speicherkonto zuzugreifen. In der folgenden Dokumentation finden Sie Beispiele für die von Ihnen verwendete Programmiersprache:

[https://azure.microsoft.com/resources/samples/?term=storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Überwachen von Gerätezugriffen

Alle physischen Geräte in Azure Stack unterstützen TACACS oder RADIUS. Hierdurch kann u.a. auf den Baseboard-Verwaltungscontroller (BMC) und auf Netzwerkswitches zugegriffen werden.

RADIUS und TACACS gehören nicht zum Leistungsumfang von Azure Stack-Lösungen. Die Lösungen wurden jedoch dahingehend überprüft, dass eine Verwendung mit bereits am Markt verfügbaren RADIUS- und TACACS-Lösungen unterstützt wird.

Für RADIUS (nicht jedoch für TACACS) wurde die Verwendung von MSCHAPv2 überprüft. Hierbei handelt es sich um die sicherste Implementierung mithilfe von RADIUS.
Wenden Sie sich zum Aktivieren von TACAS oder RADIUS auf Geräten in Ihrer Azure Stack-Lösung an Ihren OEM-Hardwarehersteller.

## <a name="syslog"></a>syslog

Alle physischen Geräte in Azure Stack können Syslog-Nachrichten senden. Ein Syslog-Server gehört nicht zum Leistungsumfang von Azure Stack-Lösungen. Die Lösungen wurden jedoch dahingehend überprüft, dass das Senden von Nachrichten an bereits am Markt verfügbare Syslog-Lösungen unterstützt wird.

Die Syslog-Zieladresse ist ein optionaler Parameter, der für die Bereitstellung erfasst wird. Er kann jedoch auch nach der Bereitstellung hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

[Azure Stack datacenter integration - publish endpoints (Integration des Azure Stack-Datencenters – Veröffentlichen von Endpunkten)](azure-stack-integrate-endpoints.md)
