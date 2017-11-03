---
title: Computergruppen in Log Analytics-Protokollsuchen | Microsoft Docs
description: "Mit Computergruppen in Log Analytics können Sie Protokollsuchvorgänge auf eine bestimmte Gruppe von Computern eingrenzen.  In diesem Artikel werden die verschiedenen Methoden beschrieben, mit denen Sie Computergruppen erstellen, sowie wie sie diese in einer Suche verwenden."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: bwren
ms.openlocfilehash: f27f038e0507270c0bfe200cb8c86622ebac5372
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergruppen in Log Analytics-Protokollsuchen

Mit Computergruppen in Log Analytics können Sie [Protokollsuchvorgänge](log-analytics-log-search-new.md) auf eine bestimmte Gruppe von Computern eingrenzen.  Die einzelnen Gruppen werden über eine von Ihnen definierte Abfrage mit Computern aufgefüllt oder indem Sie Gruppen aus verschiedenen Quellen importieren.  Wenn die Gruppe in eine Protokollsuche eingeschlossen wird, sind die Ergebnisse auf Datensätze beschränkt, die Computern in der Gruppe entsprechen.

## <a name="creating-a-computer-group"></a>Erstellen einer Computergruppe
Sie können eine Computergruppe in Log Analytics mithilfe einer der Methoden in der folgenden Tabelle erstellen.  Einzelheiten zu den einzelnen Methoden finden Sie in den Abschnitten unten. 

| Methode | Beschreibung |
|:--- |:--- |
| Protokollsuche |Erstellen Sie eine Protokollsuche, die eine Liste mit Computern zurückgibt. |
| Protokollsuch-API |Verwenden Sie die Protokollsuch-API, um programmgesteuert eine Computergruppe basierend auf den Ergebnissen einer Protokollsuche zu erstellen. |
| Active Directory |Scannen Sie automatisch die Gruppenmitgliedschaft aller Agent-Computer, die Mitglieder einer Active Directory-Domäne sind, und erstellen Sie für die einzelnen Sicherheitsgruppen jeweils eine Gruppe in Log Analytics. |
| Konfigurations-Manager | Importieren Sie Sammlungen aus System Center Configuration Manager, und erstellen Sie für jede eine Gruppe in Log Analytics. |
| Windows Server Update Services |Scannen Sie automatisch WSUS-Server oder -Clients auf Zielgruppen, und erstellen Sie jeweils eine Gruppe in Log Analytics. |

### <a name="log-search"></a>Protokollsuche
Computergruppen, die durch eine Protokollsuche erstellt wurden, enthalten alle Computer, die bei der von Ihnen definierten Abfrage zurückgegeben wurden.  Diese Abfrage wird jedes Mal ausgeführt, wenn die Computergruppe verwendet wird. Daher werden sämtliche Änderungen seit der Erstellung der Gruppe berücksichtigt.  

Sie können für eine Computergruppe eine beliebige Abfrage verwenden. Diese muss allerdings unter Verwendung von `distinct Computer` eine eindeutige Gruppe von Computern zurückgeben.  Das folgende Beispiel enthält eine typische Suche, die Sie für eine Computergruppe verwendet können.

    Heartbeat | where Computer contains "srv" | distinct Computer

Die folgende Tabelle beschreibt die Eigenschaften, durch die eine Computergruppe definiert wird.

| Eigenschaft | Beschreibung |
|:---|:---|
| Anzeigename   | Name der Suche, die im Portal angezeigt werden soll |
| Category (Kategorie)       | Kategorie zum Organisieren von Suchvorgängen im Portal |
| Abfrage          | die Abfrage für die Computergruppe |
| Funktionsalias | ein eindeutiger Alias, mit dem die Computergruppe bei einer Abfrage identifiziert wird |

Gehen Sie zum Erstellen einer Computergruppe aus einer Protokollsuche im Azure-Portal wie folgt vor:

2. Öffnen Sie die **Protokollsuche**, und klicken Sie anschließend auf **gespeicherte Suchvorgänge** im oberen Bereich der Anzeige.
3. Klicken Sie auf **Hinzufügen**, und geben Sie Werte für jede Eigenschaft der Computergruppe an.
4. Wählen Sie **Diese Abfrage als Computergruppe speichern** aus, und klicken Sie auf **OK**.


Gehen Sie zum Erstellen einer Computergruppe aus einer Protokollsuche im OMS-Portal wie folgt vor:

1. Öffnen Sie die **Protokollsuche**, und erstellen Sie die Protokollsuche für die Computergruppe.  
2. Klicken Sie oben auf der Seite auf die Schaltfläche **Speichern**.
3. Wählen Sie für **Speichern Sie diese Abfrage als Computergruppe** die Option **Ja** aus.
5. Geben Sie Werte für jede Eigenschaft der Computergruppe an. 


>[!NOTE]
> Wenn Sie für Ihren Arbeitsbereich die [Legacyversion der Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) verwenden, gehen Sie zum Erstellen einer Computergruppe genauso vor, nutzen aber die Syntax der Legacysprache.


### <a name="log-search-api"></a>Protokollsuch-API
Computergruppen, die mit der Protokoll-API erstellt wurden, sind mit denen, die mit einer Protokollsuche erstellt wurden, identisch.  Weitere Informationen zum Erstellen einer Computergruppe mithilfe der Protokollsuch-API finden Sie unter [Computergruppen in der Log Analytics-REST-API für die Protokollsuche](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Wenn Sie Log Analytics für das Importieren von Active Directory-Gruppenmitgliedschaften konfigurieren, werden dabei die Gruppenmitgliedschaften von in eine Domäne eingebundenen Computern mit dem OMS-Agent analysiert.  Für jede Sicherheitsgruppe in Active Directory wird in Log Analytics eine Computergruppe erstellt, und jeder Computer wird der Computergruppe hinzugefügt, die den Sicherheitsgruppen entspricht, in denen er Mitglied ist.  Diese Mitgliedschaft wird kontinuierlich alle 4 Stunden aktualisiert.  

Sie können Log Analytics zum Importieren von Active Directory-Sicherheitsgruppen in den **Erweiterten Einstellungen** von Log Analytics im Azure-Portal konfigurieren.  Wählen Sie hierzu **Computergruppen**, **Active Directory** und anschließend **Active Directory-Gruppenmitgliedschaften von Computern importieren** aus.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wenn Gruppen importiert wurden, werden im Menü die Anzahl der Computer, für die eine Gruppenmitgliedschaft erkannt wurde, und die Anzahl der importierten Gruppen aufgeführt.  Klicken Sie auf einen dieser Links, um die **ComputerGroup**-Datensätze mit diesen Informationen zurückzugeben.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Wenn Sie Log Analytics für das Importieren von WSUS-Gruppenmitgliedschaften konfigurieren, werden dabei die Zielgruppenmitgliedschaften aller Computer mit dem OMS-Agent analysiert.  Bei Verwendung clientseitiger Zielgruppen wird die Gruppenmitgliedschaft aller Computer, die mit OMS verbunden und Teil einer WSUS-Zielgruppe sind, in Log Analytics importiert. Bei Verwendung serverseitiger Zielgruppen sollte der OMS-Agent auf dem WSUS-Server installiert sein, damit die Informationen zur Gruppenmitgliedschaft in OMS importiert werden können.  Diese Mitgliedschaft wird kontinuierlich alle 4 Stunden aktualisiert. 

Sie können Log Analytics zum Importieren von WSUS-Gruppen in den **Erweiterten Einstellungen** von Log Analytics im Azure-Portal konfigurieren.  Wählen Sie hierzu **Computergruppen**, **WSUS** und anschließend **WSUS-Gruppenmitgliedschaften importieren** aus.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Wenn Gruppen importiert wurden, werden im Menü die Anzahl der Computer, für die eine Gruppenmitgliedschaft erkannt wurde, und die Anzahl der importierten Gruppen aufgeführt.  Klicken Sie auf einen dieser Links, um die **ComputerGroup**-Datensätze mit diesen Informationen zurückzugeben.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Wenn Sie Log Analytics konfigurieren, um Configuration Manager-Sammlungsmitgliedschaften zu importieren, wird eine Computergruppe für jede Sammlung erstellt.  Die Informationen zu Sammlungsmitgliedschaften werden alle drei Stunden abgerufen, um die Computergruppen zu aktualisieren. 

Bevor Sie Configuration Manager-Sammlungen importieren können, müssen Sie [zwischen Configuration Manager und Log Analytics eine Verbindung herstellen](log-analytics-sccm.md).  Anschließend können Sie den Import in den **Erweiterten Einstellungen** von Log Analytics im Azure-Portal konfigurieren.  Wählen Sie hierzu **Computergruppen**, **SCCM** und anschließend **Configuration Manager-Sammlungsmitgliedschaften importieren** aus.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Wenn Sammlungen importiert wurden, werden im Menü die Anzahl der Computer, für die eine Gruppenmitgliedschaft erkannt wurde, und die Anzahl der importierten Gruppen aufgeführt.  Klicken Sie auf einen dieser Links, um die **ComputerGroup**-Datensätze mit diesen Informationen zurückzugeben.

## <a name="managing-computer-groups"></a>Verwalten von Computergruppen
Sie können Computergruppen, die aus einer Protokollsuche oder der über die API für die Protokollsuche erstellt wurden, in den **Erweiterten Einstellungen** von Log Analytics im Azure-Portal aufrufen.  Wählen Sie hierzu **Computergruppen** und anschließend **Gespeicherte Gruppen** aus.  

Klicken Sie auf das **x** in der Spalte **Entfernen**, um die Computergruppe zu löschen.  Klicken Sie auf das Symbol **Mitglieder anzeigen** für eine Gruppe, um die Protokollsuche für die Gruppe ausführen, mit der die Elemente zurückgegeben werden.  Sie können eine Computergruppe nicht ändern, sondern nur löschen und dann mit den geänderten Einstellungen erneut erstellen.

![Gespeicherte Computergruppen](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Verwenden einer Computergruppe in einer Protokollsuche
Sie können eine Computergruppe in einer Abfrage verwenden, indem Sie den zugehörigen Alias als Funktion behandeln. Üblicherweise nutzen Sie dabei die folgende Syntax:

  `Table | where Computer in (ComputerGroup)`

Mit dem folgenden Codebeispiel geben Sie ausschließlich UpdateSummary-Datensätze von Computern der Computergruppe „mycomputergroup“ zurück.
 
  `UpdateSummary | where Computer in (mycomputergroup)`

>[!NOTE]
> Wenn Sie für Ihren Arbeitsbereich die [Legacyversion der Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) verwenden, nutzen Sie die folgende Syntax, um in einer Protokollsuche auf eine Computergruppe zu verweisen:  Die Angabe der **Kategorie** ist optional und nur erforderlich, wenn Sie über mehrere Computergruppen mit demselben Namen in verschiedenen Kategorien verfügen. 
>
>    `$ComputerGroups[Category: Name]`
>
>Computergruppen werden häufig mit der **IN**-Klausel in der Protokollsuche verwendet, wie im folgenden Beispiel gezeigt:
>
>    `Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]`



## <a name="computer-group-records"></a>Computergruppen-Datensätze
Im OMS-Repositorys wird für jede Mitgliedschaft in einer Computergruppe, die aus Active Directory oder WSUS erstellt wurde, ein Datensatz erstellt.  Diese Datensätze sind vom Typ **ComputerGroup** und weisen die in der folgenden Tabelle aufgeführten Eigenschaften auf.  Es werden keine Datensätze für Computergruppen basierend auf der Protokollsuche erstellt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computer |Der Name des Mitgliedscomputers |
| Group |Der Anzeigename der Gruppe |
| GroupFullName |Der vollständige Pfad zur Gruppe, einschließlich Quelle und Quellname |
| GroupSource |Die Quelle, aus der die Gruppe zusammengestellt wurde <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Der Name der Quelle, aus der die Gruppe zusammengestellt wurde.  Für Active Directory ist dies der Domänenname. |
| ManagementGroupName |Name der Verwaltungsgruppe für SCOM-Agents.  Bei anderen Agents lautet dieser „AOI-\<Arbeitsbereich-ID\>“. |
| TimeGenerated |Das Datum und die Uhrzeit der Erstellung oder letzten Aktualisierung der Computergruppe |

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  

