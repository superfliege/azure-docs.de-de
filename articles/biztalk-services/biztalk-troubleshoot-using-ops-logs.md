---
title: "Problembehandlung bei BizTalk-Dienste über Vorgangsprotokolle | Microsoft Docs"
description: "Problembehandlung bei BizTalk-Dienste über Vorgangsprotokolle. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c0c83361f94ffd9c30d7fcc551ff4b85ad7d6fa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Problembehandlung bei der Verwendung von vorgangsprotokollen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-the-operation-logs"></a>Was sind die Vorgangsprotokolle
Vorgangsprotokolle ist eine Verwaltungsdienste Funktion im klassischen Azure-Portal, das Sie Verlaufsprotokolle der durchgeführten Operationen für die Azure-Dienste, einschließlich BizTalk-Dienste anzeigen kann. Dadurch können Sie historische Daten, die im Zusammenhang mit Management-Vorgänge in Ihrem BizTalk Service-Abonnement bis 180 Tage anzeigen.

> [!NOTE]
> Diese Funktion zeichnet nur Protokolle für Verwaltungsvorgänge an BizTalk-Dienste, z. B. wenn der Dienst gestartet wurde, bis usw. gesichert. Solche Vorgänge werden nachverfolgt, unabhängig davon, ob sie über das klassische Azure-Portal oder mithilfe ausgeführt werden der [BizTalk-Dienst-REST-APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx). Eine vollständige Liste der Vorgänge, die mithilfe der Verwaltungsdienste nachverfolgt werden, finden Sie unter [Vorgänge nachverfolgten mithilfe von Azure Verwaltungsdienste](#bizops).<br/><br/>
> Dadurch wird die Protokolle für Aktivitäten im Zusammenhang mit der BizTalk Service-Laufzeit (z. B. die Nachricht von Brücken usw. verarbeitet.) nicht erfasst. Verwenden Sie zum Anzeigen dieser Protokolle die nachverfolgungsansicht die BizTalk Services-Portal ein. Weitere Informationen finden Sie unter [Nachrichten nachverfolgen](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Anzeigen von BizTalk über Vorgangsprotokolle Services
1. Wählen Sie im klassischen Azure-Portal, **Verwaltungsdienste**, und wählen Sie dann die **Vorgangsprotokolle** Registerkarte.
2. Sie können die Protokolle, die basierend auf verschiedenen Parametern wie Abonnement, Datumsbereich, Diensttyp (z. B. BizTalk-Dienste), Dienstnamen oder Status des Vorgangs ("erfolgreich", "Fehler") filtern.
3. Wählen Sie das Häkchen, um die gefilterte Liste anzuzeigen. Die folgende Abbildung zeigt die Aktivitäten im Zusammenhang mit Testbiztalkservice: ![Vorgangsprotokolle anzeigen][ViewLogs] 
4. Um mehr über einen bestimmten Vorgang anzuzeigen, wählen Sie die Zeile, und klicken Sie auf **Details** auf der Taskleiste im unteren Bereich.

## <a name="bizops"></a>Vorgänge, die mithilfe von Azure Verwaltungsdienste nachverfolgt
Die folgende Tabelle enthält die Vorgänge, die mithilfe der Azure-Verwaltungsdienste nachverfolgt werden:

| Vorgangsname | Aufgabe |
| --- | --- |
| CreateBizTalkService |Vorgang zum Erstellen einer neuen BizTalk Service |
| DeleteBizTalkService |Vorgang zum Löschen eines BizTalk Services |
| RestartBizTalkService |Vorgang um einen BizTalk Service neu zu starten. |
| StartBizTalkService |Vorgang zum Starten eines BizTalk Services |
| StopBizTalkService |Vorgang zum Beenden eines BizTalk Services |
| DisableBizTalkService |Vorgang zum Deaktivieren eines BizTalk Services |
| EnableBizTalkService |Vorgang um einen BizTalk Service zu aktivieren. |
| BackupBizTalkService |Vorgang zum Sichern eines BizTalk Services |
| RestoreBizTalkService |Vorgang um einen BizTalk Service aus einer angegebenen Sicherung wiederherstellen |
| SuspendBizTalkService |Vorgang zum Anhalten eines BizTalk Services |
| ResumeBizTalkService |Vorgang zum Fortsetzen eines BizTalk Services |
| ScaleBizTalkService |Vorgang um einen BizTalk Service vertikal oder horizontal skalieren |
| ConfigUpdateBizTalkService |Vorgang zum Aktualisieren der Konfiguration des BizTalk Service |
| ServiceUpdateBizTalkService |Vorgang zum upgrade oder Downgrade für einen BizTalk Service auf eine andere version |
| PurgeBackupBizTalkService |Vorgang zum Löschen von Sicherungen des BizTalk Service außerhalb der Beibehaltungsdauer |

## <a name="see-also"></a>Siehe auch
* [Sichern von BizTalk Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Wiederherstellen Sie BizTalk Service aus einer Sicherung.](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard und Premium Edition-Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Bereitstellung Klassisches mit Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Dienststatusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Registerkarten "Dashboard, Monitor und Skala"](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Einschränkung](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Ausstellername und Ausstellerschlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Gewusst wie: Starten Sie mithilfe der Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

