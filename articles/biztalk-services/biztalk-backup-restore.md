---
title: Erstellen und Wiederherstellen eine Sicherung in BizTalk Services | Microsoft Docs
description: "BizTalk Services umfasst Funktionen für das Sichern und Wiederherstellen. Erfahren Sie, wie Sie eine Sicherung erstellen und wiederherstellen, und wie Sie festlegen, was gesichert werden soll. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 45365092f5bcd1a8d309c10404a7437c494a8967
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Sichern und Wiederherstellen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services bieten eine Sicherungs- und Wiederherstellungsfunktion. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Hybridverbindungen werden editionsunabhängig NICHT gesichert. Sie müssen die Hybridverbindungen neu erstellen.


## <a name="before-you-begin"></a>Voraussetzungen
* Sichern und Wiederherstellen ist unter Umständen nicht für alle Editionen verfügbar. Siehe [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md).
* Gesicherte Inhalte können im selben BizTalk Service oder in einem neuen BizTalk Service wiederhergestellt werden. Wenn Sie den BizTalk Service unter demselben Namen wiederherstellen möchten, muss der vorhandene BizTalk Service gelöscht werden und der Name muss verfügbar sein. Nach dem Löschen eines BizTalk Service kann es länger dauern als erwartet, bis der gleiche Name verfügbar ist. Wenn Sie nicht warten können, bis der gleiche Name verfügbar ist, dann stellen Sie in einem neuen BizTalk Service wieder her.
* BizTalk Services können in derselben Edition oder in einer höheren Edition wiederhergestellt werden. Die Wiederherstellung von BizTalk Services in einer früheren Edition als derjenigen, von der die Sicherungskopie erstellt wurde, wird nicht unterstützt.
  
    Eine Sicherungskopie der Basic Edition kann z. B. auf die Premium Edition wiederhergestellt werden. Aber eine Sicherungskopie der Premium Edition kann nicht auf die Standard Edition wiederhergestellt werden.
* Die EDI-Kontrollnummern werden gesichert, um ihre Kontinuität aufrechtzuerhalten. Wenn Nachrichten nach der letzten Sicherung verarbeitet werden, kann es bei der Wiederherstellung dieses Sicherungsinhalts zu doppelten Kontrollnummern kommen.
* Wenn in einem Batch aktive Nachrichten vorliegen, verarbeiten Sie den Batch, **bevor** Sie eine Sicherung durchführen. Beim Erstellen (bedarfsgesteuerter oder geplanter) Sicherungen werden Nachrichten in Batches niemals gespeichert. 
  
    **Wenn eine Sicherung mit aktiven Nachrichten in einem Batch durchgeführt wird, werden diese Nachrichten nicht gesichert und gehen deshalb verloren.**
* Optional: Beenden Sie alle Verwaltungsvorgänge im BizTalk Services-Portal.

## <a name="create-a-backup"></a>Erstellen einer Sicherung
Eine Sicherungskopie kann jederzeit erstellt werden und ist komplett benutzergesteuert. Verwenden Sie zum Erstellen einer Sicherung die [REST-API für die Verwaltung von BizTalk Services in Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Restore 
Verwenden Sie zum Wiederherstellen einer Sicherung die [REST-API für die Verwaltung von BizTalk Services in Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>Nach dem Wiederherstellen einer Sicherungskopie
BizTalk Services werden immer im Status **Angehalten** wiederhergestellt. In diesem Status können Sie vor Inbetriebnahme der neuen Umgebung beliebige Konfigurationsänderungen vornehmen. Beispiele:

* Wenn Sie BizTalk Service-Anwendungen mithilfe des Azure BizTalk Services SDK erstellt haben, müssen Sie eventuell die ACS-Anmeldeinformationen (Access Control Service, Zugriffssteuerungsdienst) in diesen Anwendungen aktualisieren, damit diese in der wiederhergestellten Umgebung funktionieren.
* Sie stellen einen BizTalk Service wieder her, um eine vorhandene BizTalk Service-Umgebung zu replizieren. Wenn Sie in einem solchen Fall im ursprünglichen BizTalk Services-Portal Vereinbarungen konfiguriert haben, die FTP-Quellordner verwenden, müssen Sie unter Umständen die Vereinbarungen in der wiederhergestellten Umgebung auf die Verwendung eines anderen FTP-Quellordners aktualisieren. Ansonsten versuchen möglicherweise zwei unterschiedliche Vereinbarungen, dieselbe Nachricht abzurufen.
* Wenn Sie eine Wiederherstellung durchgeführt haben, um mehrere BizTalk Service-Umgebungen laufen zu lassen, stellen Sie sicher, dass in den Visual Studio-Anwendungen, PowerShell-Cmdlets, REST-APIs oder Trading Partner Management OM-APIs die richtige Umgebung als Ziel angegeben ist.
* Es empfiehlt sich auch, automatisierte Sicherungen der wiederhergestellten BizTalk Service-Umgebung zu konfigurieren.

## <a name="what-gets-backed-up"></a>Was wird gesichert?
Wenn eine Sicherung erstellt wird, werden die folgenden Elemente gesichert:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Gesicherte Elemente</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Konfiguration und Laufzeit</td> 
<td>
<ul>
<li>Partner- und Profildetails</li>
<li>Partnerverträge</li>
<li>Bereitgestellte benutzerdefinierte Assemblies</li>
<li>Bereitgestellte Brücken</li>
<li>Zertifikate</li>
<li>Bereitgestellte Transformationen</li>
<li>Pipelines</li>
<li>Im BizTalk Services-Portal erstellte und gespeicherte Vorlagen</li>
<li>X12 ST01- und GS01-Zuordnungen</li>
<li>Kontrollnummern (EDI)</li>
<li>AS2-Nachrichten-MIC-Werte</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk Services</strong></td>
</tr> 
<tr>
<td>SSL-Zertifikat</td> 
<td>
<ul>
<li>SSL-Zertifikatdaten</li>
<li>SSL-Zertifikatkennwort</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk Service-Einstellungen</td> 
<td>
<ul>
<li>Skalierungseinheitenzählung</li>
<li>Edition</li>
<li>Produktversion</li>
<li>Region/Rechenzentrum</li>
<li>Access Control Service (ACS)-Namespace und -Schlüssel</li>
<li>Nachverfolgungsdatenbank-Verbindungszeichenfolgen</li>
<li>Archivierung von Speicherkonto-Verbindungszeichenfolgen</li>
<li>Überwachung von Speicherkonto-Verbindungszeichenfolgen</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Zusätzliche Elemente</strong></td>
</tr> 
<tr>
<td>Nachverfolgungsdatenbank</td> 
<td>Bei der Erstellung des BizTalk Service werden die Details zur Nachverfolgungsdatenbank, darunter der Azure-SQL-Datenbankserver und der Name der Nachverfolgungsdatenbank, eingegeben. Die Nachverfolgungsdatenbank wird nicht automatisch gesichert.
<br/><br/>
<strong>Wichtig</strong><br/>
Wenn die Nachverfolgungsdatenbank gelöscht wird und wiederhergestellt werden soll, muss bereits eine vorherige Sicherung existieren. Wenn keine Sicherungskopie existiert, können die Nachverfolgungsdatenbank und ihre Daten nicht wiederhergestellt werden. Erstellen Sie in diesem Fall eine neue Nachverfolgungsdatenbank mit demselben Datenbanknamen. Georeplikation wird empfohlen.</td>
</tr> 
</table>

## <a name="next"></a>Next (Weiter)
Informationen zum Erstellen von Azure BizTalk Services finden Sie unter [Konfigurieren von BizTalk Services im Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Wenn Sie mit dem Erstellen von Anwendungen beginnen möchten, wechseln Sie zu [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Weitere Informationen
* [Sichern von BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Wiederherstellen von BizTalk Services aus einer Sicherung](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Editionsübersicht](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [Konfigurieren von BizTalk Services im Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Bereitstellungsstatusübersicht](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Registerkarten "Dashboard", "Überwachen" und "Skalieren"](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Drosselung](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Name und Schlüssel des Ausstellers](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Wie verwende ich das Azure BizTalk Services SDK?](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

