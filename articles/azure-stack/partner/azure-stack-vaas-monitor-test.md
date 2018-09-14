---
title: Überwachen eines Tests mit Azure Stack Validation-as-a-Service | Microsoft-Dokumentation
description: Überwachen eines Tests mit Azure Stack Validation-as-a-Service
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 2dc4d3f2855864ff80648b5b9635ff28c0dacbb7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163328"
---
# <a name="monitor-a-test-with-azure-stack-validation-as-a-service"></a>Überwachen eines Tests mit Azure Stack Validation-as-a-Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Die Ausführung eines Tests kann auf der Seite **Vorgänge** für aktive oder abgeschlossene Testsammlungen überwacht werden. Auf dieser Seite sind Details zum Status des Tests und seiner Vorgänge aufgeführt.

## <a name="monitor-a-test"></a>Überwachen eines Tests

1. Wählen Sie eine Lösung aus.

2. Klicken Sie auf einer beliebigen Workflowkachel auf **Verwalten**.

3. Klicken Sie auf einen Workflow, um die zugehörige Seite mit der Testzusammenfassung zu öffnen.

4. Erweitern Sie das Kontextmenü **[...]** für eine beliebige Testsammlungsinstanz.

5. Klicken Sie auf **View Operations** (Vorgänge anzeigen).

![Alt text](media\image4.png)

Für abgeschlossene Tests können Protokolle von der Seite mit der Testzusammenfassung heruntergeladen werden. Klicken Sie dazu im Kontextmenü **[...]** eines Tests auf **Protokolle herunterladen**. Azure Stack-Partner können bei fehlerhaften Tests mithilfe dieser Protokolle Probleme debuggen.

## <a name="open-the-test-pass-summary"></a>Öffnen der Zusammenfassung des Testdurchlaufs

1. Öffnen Sie das Portal. 
2. Wählen Sie den Namen einer vorhandenen Lösung aus, die zuvor ausgeführte oder geplante Tests enthält.

    ![Verwalten von Testdurchläufen](media/managetestpasses.png)

3. Klicken Sie im Bereich **Test Passes** (Testdurchläufe) auf **Verwalten**.
4. Wählen Sie den Testdurchlauf aus, um die Zusammenfassung des Testdurchlaufs zu öffnen. Sie können den Testnamen, das Erstellungs- und Ausführungsdatum, die Testdauer und das Ergebnis (erfolgreich oder fehlerhaft) überprüfen.
5. Klicken Sie auf [ **. .  .** ].

### <a name="test-pass-summary"></a>Zusammenfassung des Testdurchlaufs

| Column | BESCHREIBUNG |
| --- | --- |
| Testname | Der Name des Tests. Verweist auf die Validierungsnummer. |
| Erstellt | Zeitpunkt, zu der der Testdurchlauf erstellt wurde |
| Gestartet | Zeitpunkt, zu der der Testdurchlauf ausgeführt wurde |
| Duration | Die Ausführungsdauer des Testdurchlaufs |
| Status | Das Ergebnis (erfolgreich oder fehlerhaft) des Testdurchlaufs |
| Agent-Name | Der vollqualifizierte Domänenname des Agents |
| Vorgänge gesamt | Die Gesamtanzahl von Vorgängen, die im Testdurchlauf versucht wurden |
| Passed operations (Bestandene Vorgänge) | Die Anzahl von Vorgängen, die im Testdurchlauf bestanden haben |
|  Fehlerhafte Vorgänge | Die Anzahl von Vorgängen, bei denen Fehler aufgetreten sind |

### <a name="group-columns-in-the-test-pass-summary"></a>Gruppieren der Spalten in der Zusammenfassung des Testdurchlaufs

Sie können eine Spalte auswählen und in die Kopfzeile ziehen, um eine Gruppe für den Spaltenwert zu erstellen.

## <a name="reschedule-a-test"></a>Neuplanen eines Tests

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Klicken Sie auf **Reschedule** (Neu planen), um den Testdurchlauf neu zu planen.
3. Geben Sie das Kennwort des Cloudadministrators für Ihre Azure Stack-Instanz ein.
4. Geben Sie die Diagnosespeicher-Zeichenfolge ein, die Sie beim Einrichten Ihres Kontos definiert haben.
5. Planen Sie den Test neu.

## <a name="cancel-a-test"></a>Abbrechen eines Tests

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Wählen Sie **Abbrechen** aus.

## <a name="get-test-information"></a>Abrufen von Testinformationen

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Klicken Sie auf **View information** (Informationen anzeigen), um den Testdurchlauf neu zu planen.

**Testinformationen**

| NAME | BESCHREIBUNG |
| -- | -- |
| Testname | Der Name des Tests, z.B. „OEM Update on Azure Stack 1806 RC Validation“ |
| Testversion | Die Version des Tests, z.B. 5.1.4.0 |
| Herausgeber | Der Herausgeber des Tests, etwa Microsoft |
| Category (Kategorie) | Die Kategorie des Tests, z.B. **Functional** (Funktion) oder **Reliability** (Zuverlässigkeit) |
| Target services (Zieldienste) | Die getesteten Dienste, z.B. VirtualMachines |
| BESCHREIBUNG | Die Beschreibung des Tests |
| Estimated duration (minutes) (Geschätzte Dauer (Minuten)) | Die Dauer der Testausführung in Minuten |
| Links | Ein Link zur GitHub-Problemverfolgung |

## <a name="get-test-parameters"></a>Abrufen der Testparameter

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Klicken Sie auf **View parameters** (Parameter anzeigen), um den Testdurchlauf neu zu planen.

**Parameter**

| NAME | BESCHREIBUNG |
| -- | -- |
| Testname | Der Name des Tests, z.B. „oemupdate1806test“ |
| Testversion | Die Version des Tests, z.B. 5.1.4.0 |
| Test instance ID (Testinstanz-ID) | Eine GUID zum Identifizieren der spezifischen Instanz des Tests, z.B. „20b20645-b400-4f0d-bf6f-1264d866ada9“ |
| cloudAdminUser | Der Name des Kontos, das als Cloudadministrator verwendet wird, etwa **cloudadmin** |
| DiagnosticsContainerName | Die ID des Diagnosecontainers, z.B. „04dd3815-5f35-4158-92ea-698027693080“ |

## <a name="get-test-operations"></a>Abrufen von Testvorgängen

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Klicken Sie auf **View operations** (Vorgänge anzeigen), um den Testdurchlauf neu zu planen. Der Bereich mit der Zusammenfassung der Vorgänge wird geöffnet.

## <a name="get-test-logs"></a>Abrufen von Testprotokollen

1. [Öffnen Sie die Zusammenfassung des Testdurchlaufs.](#open-the-test-pass-summary)
2. Klicken Sie auf **Protokolle herunterladen**, um den Testdurchlauf neu zu planen.  
    Eine ZIP-Datei namens „ReleaseYYYY-MM-DD.zip“, die die Protokolle enthält, wird heruntergeladen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
