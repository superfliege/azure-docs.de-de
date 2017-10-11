---
title: Sichern auf einem Exchange-Server in Azure Backup mit Azure Backup Server | Microsoft Docs
description: Informationen Sie zum Sichern von eines Exchange-Servers in Azure-Sicherung, die mithilfe von Azure Backup Server
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a>Sichern Sie einen Exchange-Server in Azure Backup mit Azure Backup Server
Dieser Artikel beschreibt, wie Microsoft Azure Backup Server (MABS) zum Sichern von Microsoft Exchange-Server in Azure konfigurieren.  

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie fortfahren, stellen Sie sicher, dass Azure Backup Server ist [installiert und vorbereitet](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS-Schutz-agent
Gehen Sie folgendermaßen vor, um den MABS-Schutz-Agent auf dem Exchange-Server zu installieren:

1. Stellen Sie sicher, dass die Firewalls richtig konfiguriert sind. Finden Sie unter [Konfigurieren von Firewallausnahmen für den Agent](https://technet.microsoft.com/library/Hh758204.aspx).
2. Installieren Sie den Agent auf dem Exchange Server, indem Sie auf **Management > Agents > Installieren** in MABS-Verwaltungskonsole. Finden Sie unter [Installieren des Schutz-Agents für MABS](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) Ausführliche Schritte.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Erstellen einer Schutzgruppe für den Exchange-server
1. Klicken Sie in der Administratorkonsole MABS auf **Schutz**, und klicken Sie dann auf **neu** auf dem Menüband zu öffnen die **Erstellen einer neuen Schutzgruppe** Assistenten.
2. Auf der **Willkommen** Bildschirm des Assistenten auf **Weiter**.
3. Auf der **schutzgruppentyp auswählen** Bildschirm **Server** , und klicken Sie auf **Weiter**.
4. Wählen Sie den Exchange Server-Datenbank, die Sie schützen möchten und klicken Sie auf **Weiter**.

   > [!NOTE]
   > Wenn Sie Exchange 2013 schützen möchten, überprüfen Sie die [Exchange 2013 Prerequisites](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    Im folgenden Beispiel wird die Exchange 2010-Datenbank ausgewählt.

    ![Auswählen von Gruppenmitgliedern](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Wählen Sie die Datenschutzmethode aus.

    Benennen Sie die Schutzgruppe aus, und wählen Sie dann beide der folgenden Optionen:

   * Ich möchte kurzfristigen Schutz per: Datenträger.
   * Ich möchte Onlineschutz.
6. Klicken Sie auf **Weiter**.
7. Wählen Sie die **"Eseutil zum Prüfen der Datenintegrität ausführen"** option, wenn Sie die Integrität der Exchange Server-Datenbanken überprüfen möchten.

    Nachdem Sie diese Option auswählen, backup konsistenzprüfung durchgeführt werden auf MABS, um den e/a-Datenverkehr zu vermeiden, die durch Ausführen von generiert wird die **"Eseutil"** Befehl auf dem Exchange-Server.

   > [!NOTE]
   > Um diese Option verwenden zu können, müssen Sie die Dateien "ESE.dll" und Eseutil.exe in das Verzeichnis "C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin" auf dem Server MAB kopieren. Andernfalls wird der folgende Fehler ausgelöst:  
   > !["Eseutil"-Fehler](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klicken Sie auf **Weiter**.
9. Wählen Sie die Datenbank für **Kopiesicherung**, und klicken Sie dann auf **Weiter**.

   > [!NOTE]
   > Wenn Sie nicht "Vollständige Sicherung" für mindestens eine DAG-Kopie der Datenbank auswählen, werden die Protokolle nicht abgeschnitten.
   >
   >
10. Konfigurieren Sie die Ziele für **kurzfristige Sicherung**, und klicken Sie dann auf **Weiter**.
11. Überprüfen Sie den verfügbaren Speicherplatz, und klicken Sie dann auf **Weiter**.
12. Wählen Sie die Uhrzeit, an dem der MAB-Server die erste Replikation erstellen, und klicken Sie dann auf **Weiter**.
13. Wählen Sie die Optionen für die konsistenzprüfung aus, und klicken Sie dann auf **Weiter**.
14. Wählen Sie die Datenbank, die Sie verwenden möchten, in Azure sichern, und klicken Sie dann auf **Weiter**. Beispiel:

    ![Online zu schützende Daten angeben](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Definieren Sie den Zeitplan für **Azure Backup**, und klicken Sie dann auf **Weiter**. Beispiel:

    ![Onlinesicherungszeitplan angeben](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Beachten Sie die Online-Wiederherstellungspunkte werden basierend auf schnelle vollständige Wiederherstellungspunkte. Aus diesem Grund müssen Sie die online-Wiederherstellungspunkts planen, nach dem die Zeit, die angegeben wird, wird für die schnelle vollständige Wiederherstellungspunkt.
    >
    >
16. Konfigurieren Sie die Aufbewahrungsrichtlinie für **Azure Backup**, und klicken Sie dann auf **Weiter**.
17. Wählen Sie eine online-Replication-Option, und klicken Sie auf **Weiter**.

    Wenn Sie eine große Datenbank verfügen, konnte es dauern, sehr lange für die erste Sicherung über das Netzwerk erstellt werden soll. Zur Vermeidung dieses Problems können Sie eine offlinesicherung zu erstellen.  

    ![Onlineaufbewahrungsrichtlinie angeben](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Bestätigen Sie die Einstellungen, und klicken Sie dann auf **Gruppenerstellungs**.
19. Klicken Sie auf **Schließen**.

## <a name="recover-the-exchange-database"></a>Wiederherstellen der Exchange-Datenbank
1. Klicken Sie zum Wiederherstellen einer Exchange-Datenbank auf **Wiederherstellung** in der Administratorkonsole MABS.
2. Suchen Sie die Exchange-Datenbank, die Sie wiederherstellen möchten.
3. Wählen Sie ein online-Wiederherstellungspunkts aus den *Wiederherstellungszeit* Dropdown-Liste.
4. Klicken Sie auf **wiederherstellen** zum Starten der **Wiederherstellungs-Assistenten**.

Für online-Wiederherstellungspunkte gibt es fünf Arten der Wiederherstellung:

* **Am ursprünglichen Exchange Server-Speicherort wiederherstellen:** werden die Daten in den ursprünglichen Exchange Server wiederhergestellt.
* **In einer anderen Datenbank auf einem Exchange-Server wiederherstellen:** werden die Daten in einer anderen Datenbank auf einem anderen Exchange-Server wiederhergestellt.
* **In einer Wiederherstellungsdatenbank wiederherstellen:** werden die Daten auf eine Exchange Wiederherstellung der Datenbank (RDB) wiederhergestellt.
* **In einen Netzwerkordner kopieren:** werden die Daten in einen Netzwerkordner wiederhergestellt.
* **Auf Band kopieren:** , wenn Sie eine Bandbibliothek oder ein eigenständiges Bandlaufwerk angefügt und auf MABS konfiguriert haben, wird der Wiederherstellungspunkt in ein freies Band kopiert werden.

    ![Wählen Sie die online-Replikation](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Nächste Schritte
* [Azure Backup – häufig gestellte Fragen](backup-azure-backup-faq.md)
