---
title: "Anweisungen für eine mehrinstanzenfähige SaaS-Beispiel-App in der SQL-Datenbank – Wingtip | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anweisungen zum Installieren und Ausführen der mehrinstanzenfähigen SaaS-Beispielanwendung, die Azure SQL-Datenbank verwendet – in diesem Fall Wingtip."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: genemi
ms.openlocfilehash: 4c90d70bb3b043ef81a224f0f69107eaa6eb0547
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="guidance-and-tips-for-azure-sql-database-multi-tenant-saas-app-example"></a>Anweisungen und Tipps für eine mehrinstanzenfähige SaaS-Beispiel-App in Azure SQL-Datenbank


## <a name="download-and-unblock-the-wingtip-tickets-saas-database-per-tenant-scripts"></a>Herunterladen und Entsperren der Skripts für die SaaS-App Wingtip Tickets mit einer Datenbank pro Mandant

Ausführbare Inhalte (Skripts, DLLs) können durch Windows blockiert werden, wenn ZIP-Dateien aus einer externen Quelle heruntergeladen und extrahiert werden. Führen Sie bei der Extraktion der Skripts aus einer ZIP-Datei ***die nachfolgenden Schritte durch, um die Blockierung der ZIP-Datei vor der Extraktion aufzuheben***. Hierdurch wird sichergestellt, dass die Skripts ausgeführt werden dürfen.

1. Navigieren Sie zum [GitHub-Repository für die SaaS-App Wingtip Tickets mit einer Datenbank pro Mandant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
2. Klicken Sie auf **Klonen oder herunterladen**.
3. Klicken Sie auf **ZIP herunterladen**, und speichern Sie die Datei.
4. Klicken Sie mit der rechten Maustaste auf die Datei **WingtipTicketsSaaS-DbPerTenant-master.zip**, und wählen Sie **Eigenschaften** aus.
5. Wählen Sie auf der Registerkarte **Allgemein** die Option **Blockierung aufheben**.
6. Klicken Sie auf **OK**.
7. Extrahieren Sie die Dateien.

Skripts befinden sich im Ordner *..\\Learning Modules* (Lernmodule).


## <a name="working-with-the-wingtip-tickets-saas-database-per-tenant-powershell-scripts"></a>Verwenden der PowerShell-Skripts für die SaaS-App Wingtip Tickets mit einer Datenbank pro Mandant

Um das Beispiel optimal nutzen zu können, müssen Sie sich mit den bereitgestellten Skripts auseinandersetzen. Verwenden Sie Haltepunkte, und gehen Sie die Skripts durch, indem Sie sich die Einzelheiten bezüglich der Implementierung der unterschiedlichen SaaS-Muster ansehen. Um mühelos die bereitgestellten Skripts und Module durchzugehen und auf diese Weise bestmöglich zu verstehen, wird die Verwendung der [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise) empfohlen.

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualisieren der Konfigurationsdatei für Ihre Bereitstellung

Bearbeiten Sie die Datei **UserConfig.psm1** mit dem Ressourcengruppen- und Benutzerwert, den Sie bei der Bereitstellung festlegen:

1. Öffnen Sie die *PowerShell ISE*, und laden Sie ...\\Learning Modules\\*UserConfig.psm1*. 
2. Aktualisieren Sie *ResourceGroupName* und *Name* mit den jeweiligen Werten für Ihre Bereitstellung (nur in den Zeilen 10 und 11).
3. Speichern Sie die Änderungen.

Indem Sie diese Werte hier festlegen, wird es Ihnen erspart, diese bereitstellungsspezifischen Werte in jedem Skript aktualisieren zu müssen.

### <a name="execute-scripts-by-pressing-f5"></a>Ausführen von Skripts durch Drücken von F5

Zur Navigation in Ordnern verwenden die verschiedenen Skripts *$PSScriptRoot*. *$PSScriptRoot* wird nur ausgewertet, wenn die Skripts durch Drücken der Taste **F5** ausgeführt werden.  Das Markieren und Ausführen einer Auswahl (**F8**) kann zu Fehlern führen. Drücken Sie daher zum Ausführen von Skripts **F5**.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Schrittweises Durcharbeiten der Skripts zum Überprüfen der Implementierung

Die beste Möglichkeit, die Skripts zu verstehen, besteht darin, die jeweiligen Funktionen durchzugehen. Sehen Sie sich die enthaltenen **Demo-**-Skripts an, die einen einfachen allgemeinen Workflow darstellen. Die **Demo -**-Skripts veranschaulichen die erforderlichen Schritte zur Durchführung der einzelnen Aufgaben. Legen Sie daher die Haltepunkte fest, und führen Sie einen Drilldown auf die einzelnen Aufrufe durch, um Implementierungsdetails für die verschiedenen SaaS-Muster anzuzeigen.

Tipps zum Prüfen und schrittweisen Ausführen der PowerShell-Skripts:

- Öffnen Sie die **Demo-**-Skripts in der PowerShell ISE.
- Drücken Sie zum Ausführen oder Fortfahren die Taste **F5**. (Es wird davon abgeraten, **F8** zu drücken, da *$PSScriptRoot* beim Ausführen einer Auswahl aus einem Skript nicht ausgewertet wird.)
- Platzieren Sie Haltepunkte, indem Sie auf eine Zeile klicken bzw. diese auswählen und **F9** drücken.
- Überspringen Sie eine Funktion oder einen Skriptaufruf durch Drücken von **F10**.
- Führen Sie eine Funktion oder einen Skriptaufruf als Einzelschritt aus, indem Sie **F11** drücken.
- Führen Sie die aktuelle Funktion bzw. den aktuellen Skriptaufruf bis zum Rücksprung aus, indem Sie **UMSCHALT+F11** drücken.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Untersuchen des Datenbankschemas und Ausführen von SQL-Abfragen mit SSMS

Verwenden Sie [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), um eine Verbindung mit den Anwendungsservern und -datenbanken herzustellen und diese zu durchsuchen.

Die Bereitstellung enthält zunächst zwei SQL-Datenbankserver, mit denen eine Verbindung mit den Servern *tenants1-dpt-&lt;User&gt;* und *catalog-dpt-&lt;User&gt;* hergestellt werden kann. Um sicherzustellen, dass erfolgreich eine Verbindung mit der Demo hergestellt wird, wird beiden Servern eine [Firewallregel](sql-database-firewall-configure.md) zugewiesen, die alle IP-Adressen zulässt.


1. Öffnen Sie *SSMS*, und stellen Sie eine Verbindung mit dem Server *tenants1-dpt-&lt;User&gt;.database.windows.net* her.
2. Klicken Sie auf **Verbinden** > **Datenbankmodul...**:

   ![Katalogserver](media/saas-dbpertenant-wingtip-app-guidance-tips/connect.png)

3. Demo-Anmeldeinformationen: Benutzername = *developer*, Kennwort = *P@ssword1*

   ![connection](media/saas-dbpertenant-wingtip-app-guidance-tips/tenants1-connect.png)

4. Wiederholen Sie die Schritte 2 und 3, und stellen Sie eine Verbindung mit dem Server *catalog-dpt-&lt;User&gt;.database.windows.net* her.


Nachdem die Verbindung erfolgreich hergestellt wurde, werden beide Server angezeigt. Ihre Liste der Datenbanken kann variieren, je nachdem, welche Mandanten Sie bereitgestellt haben.

![Objekt-Explorer](media/saas-dbpertenant-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen der SaaS-Anwendung Wingtips Tickets mit einer Datenbank pro Mandant](saas-dbpertenant-get-started-deploy.md)

