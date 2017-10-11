---
title: Erste Schritte mit Azure Scheduler im Azure-Portal | Microsoft Docs
description: Erste Schritte mit Azure Scheduler im Azure-portal
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 3861ee121ed1c4d086ea81640e84d924d7d17ea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Erste Schritte mit Azure Scheduler im Azure-portal
Es ist einfach, geplante Aufträge in Azure Scheduler zu erstellen. In diesem Lernprogramm erfahren Sie, wie Sie beim Erstellen eines Auftrags. Außerdem erfahren Sie, Überwachung und Verwaltung des Planers-Funktionen.

## <a name="create-a-job"></a>Erstellen eines Auftrags
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/).  
2. Klicken Sie auf **+ neu** > Typ *Planer* in das Suchfeld > Wählen Sie **Planer** in Ergebnissen > Klicken Sie auf **erstellen**.
   
    ![][marketplace-create]
3. Erstellen Sie einen Auftrag, der mit einer GET-Anforderung http://www.microsoft.com/ einfach zu erreichen. In der **Zeitplanungsauftrag** Bildschirm, geben Sie die folgenden Informationen:
   
   1. **Name:**`getmicrosoft`  
   2. **Abonnement:** Ihr Azure-Abonnement   
   3. **Auftragssammlung:** wählen Sie eine bereits vorhandene auftragssammlung aus, oder klicken Sie auf **neu erstellen** > Geben Sie einen Namen.
4. Im nächsten Schritt in **Aktionseinstellungen**, definieren Sie die folgenden Werte:
   
   1. **Aktionstyp:**` HTTP`  
   2. **Methode:**`GET`  
   3. **URL:**` http://www.microsoft.com`  
      
      ![][action-settings]
5. Abschließend sehen wir einen Zeitplan definieren. Der Auftrag als Einmaliger Auftrag definiert werden, aber wir wählen Sie eine Wiederholung des Zeitplans:
   
   1. **Wiederholung**:`Recurring`
   2. **Starten Sie**: des heutigen Datums
   3. **Wiederholen alle**:`12 Hours`
   4. **Beenden von**: zwei Tage nach dem heutigen Datum dem Datum  
      
      ![][recurrence-schedule]
6. Klicken Sie auf **erstellen**

## <a name="manage-and-monitor-jobs"></a>Aufträge verwalten und überwachen
Nachdem ein Auftrag erstellt wurde, wird es in Azure Haupt-Dashboard angezeigt. Klicken Sie auf den Auftrag und ein neues Fenster wird geöffnet, mit den folgenden Registerkarten:

1. Eigenschaften  
2. Aktionseinstellungen  
3. Zeitplan  
4. Verlauf
5. Benutzer
   
   ![][job-overview]

### <a name="properties"></a>Eigenschaften
Diese schreibgeschützte Eigenschaften beschrieben, die Metadaten für den Scheduler-Auftrag.

   ![][job-properties]

### <a name="action-settings"></a>Aktionseinstellungen
Durch Klicken auf einen Auftrag in der **Aufträge** Bildschirm können Sie den entsprechenden Auftrag konfigurieren. Dadurch können Sie erweiterte Einstellungen konfigurieren, wenn Sie sie in konfigurieren, haben nicht die schnellerstellungs-Assistent.

Alle Aktionstyp "speicherwarteschlangenauftrag" können Sie die wiederholungsrichtlinie und die Fehleraktion ändern.

Für HTTP und HTTPS Auftrag Aktionstyp "speicherwarteschlangenauftrag" können Sie die Methode, die zulässigen HTTP-Verb ändern. Sie können auch hinzufügen, löschen Sie oder ändern Sie die Header und standardauthentifizierungsinformationen.

Speicher Aktionstyp "speicherwarteschlangenauftrag" können Sie das Speicherkonto, Warteschlangenname, SAS-Token und Text ändern.

Servicebus Aktionstyp "speicherwarteschlangenauftrag" können Sie den Namespace, Thema/Warteschlangenpfad Authentifizierungseinstellungen, Transporttyp, Nachrichteneigenschaften und Nachrichtentext ändern.

   ![][job-action-settings]

### <a name="schedule"></a>Zeitplan
Dies können Sie den Zeitplan neu konfigurieren, wenn möchten Sie den Zeitplan zu ändern, Sie erstellt, in haben, der schnellerstellungs-Assistent.

Dies ist eine Möglichkeit zum Erstellen [komplexe Zeitpläne und erweiterte Wiederholungsrate in Ihrem Auftrag](scheduler-advanced-complexity.md)

Sie können ändern, das Startdatum und die Uhrzeit, Wiederholungszeitplan sowie das Enddatum Datum und Uhrzeit (falls der Auftrag wiederholt wird.)

   ![][job-schedule]

### <a name="history"></a>Verlauf
Die **Verlauf** Registerkarte werden ausgewählte Metriken zu jeder auftragsausführung im System für den ausgewählten Auftrag angezeigt. Diese Metriken geben in Echtzeit Statuswerte zu Ihrer Scheduler:

1. Status  
2. Details  
3. Wiederholungsversuche
4. Vorkommen: 1., 2., 3. usw..
5. Startzeit der Ausführung  
6. Endzeit der Ausführung
   
   ![][job-history]

Klicken Sie auf eine Ausführung anzeigen seiner **Verlaufsdetails**, z. B. die gesamte Antwort für jede Ausführung. Dieses Dialogfeld können Sie die Antwort in die Zwischenablage zu kopieren.

   ![][job-history-details]

### <a name="users"></a>Benutzer
(Azure Role-Based Access Control, RBAC) ermöglicht die differenzierte zugriffsverwaltung für Azure Scheduler. Informationen zum Verwenden der Registerkarte "Benutzer", finden Sie unter [Ressourcenzugriffs Access Control](../active-directory/role-based-access-control-configure.md)

## <a name="see-also"></a>Siehe auch
 [Was ist Scheduler?](scheduler-intro.md)

 [Scheduler-Konzepte,-Terminologie und-Entitätshierarchie](scheduler-concepts-terms.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Gewusst wie: Erstellen von komplexen Zeitpläne und erweiterte Wiederholung mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Scheduler-REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Scheduler Referenz zu PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Scheduler – hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Scheduler – Einschränkungen, Standardwerte und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung der zeitplanung](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
