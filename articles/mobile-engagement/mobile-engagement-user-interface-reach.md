---
title: "Azure Mobile Engagement-Benutzeroberfläche – Reichweite"
description: Erfahren Sie, wie mit Pushbenachrichtigungen mithilfe von Azure Mobile Engagement eine Verbindung mit dem Benutzer der Anwendung
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ce30456e41ff1a2f4824bcb64246ee115fdd1ef7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Wie Sie eine Verbindung mit dem Benutzer der Anwendung mit Pushbenachrichtigungen
Dieser Artikel beschreibt die **erreichen** auf der Registerkarte die **Mobile Engagement** Portal. Verwenden Sie die **Mobile Engagement** Portal überwachen und Verwalten von mobilen apps. Beachten Sie, dass Starten über das Portal Sie zuerst erstellen Sie müssen eine **Azure Mobile Engagement** Konto. Weitere Informationen finden Sie unter [erstellen Sie ein Azure Mobile Engagement-Konto](mobile-engagement-create.md).

Die Reach-Abschnitt der Benutzeroberfläche ist das Push-Kampagnen-Verwaltungstool erstellen/bearbeiten/aktivieren/Ende/überwachen können und Abrufen von Statistiken für die Benachrichtigung pushkampagnen und Funktionen, die auch über die Reach-API (und einige Elemente der niedrigen Ebene Push-API) zugegriffen werden können. Beachten Sie, ob Sie die APIs oder die Benutzeroberfläche verwenden, müssen Sie die Integration von Azure Mobile Engagement und Reichweite in Ihre Anwendung für jede Plattform mit dem SDK vor der Verwendung Kampagnen zu erreichen.

> [!NOTE]
> Viele Bereiche von den **Mobile Engagement** Portal UI enthalten die **Hilfe anzeigen** Schaltfläche. Drücken Sie diese Schaltfläche, um weitere Kontextinformationen für einen Abschnitt zu erhalten.
> 
> 

## <a name="four-types-of-push-notifications"></a>Vier Arten von Pushbenachrichtigungen
1. Ankündigungen - können Sie Werbung-Nachrichten an Benutzer senden, die sie an eine andere Position innerhalb der app umleiten, sie zu einer Webseite oder einem Speicher außerhalb der app zu senden. 
2. Abrufe - können Sie Informationen von Endbenutzern zu sammeln, indem sie Fragen stellen.
3. Daten-Push - können Sie eine binäre oder base64-Datendatei zu senden. Die Informationen in einem datenpush wird für Ihre Anwendung so ändern Sie Ihre aktuellen benutzererfahrung in Ihrer app gesendet. Die Anwendung muss auch die Daten in einem datenpush verarbeitet werden.

## <a name="campaign-details"></a>Details der Kampagne
Sie bearbeiten, Klonen, löschen oder Aktivieren von Kampagnen, die noch nicht aktiviert haben, indem Sie deren Namen mit der Maus, oder Sie klicken können, um sie zu öffnen. Klonvorgänge können Kampagnen, die bereits aktiviert wurde, indem Sie deren Namen mit der Maus, oder Sie klicken können, um sie zu öffnen. Eine Kampagne jedoch kann nicht ändern, nachdem er aktiviert wurde.

![Reach1][18]

## <a name="reach-feedback"></a>Erreichen von Feedback
Klicken Sie auf **Statistiken** um die Details einer Reach-Kampagne anzuzeigen. Die **einfache** Ansicht bietet eine visuelle Darstellung in Form einer Spalte Balkendiagramm zu Was ist geschehen, nachdem eine Kampagne aktiviert wurde. Die **erweitert** Ansicht bietet eine detailliertere Informationen zu den pushkampagne. Diese Informationen werden nicht verfügbar, wenn Sie einer Testkampagne d. h. einen Push gesendet, um ein Testgerät senden. Hier ist, wie diese Details interpretiert werden sollte:

1. **Wird per Push übertragen** -Hiermit wird die Anzahl der Nachrichten, die per Push auf Geräte übertragen. Diese Anzahl hängt von der Zielgruppe, die Sie beim Erstellen der pushkampagne angegeben. Wenn Sie keine Zielgruppe angeben, wird dann diese Push für alle registrierten Geräte gesendet werden. Wie alle anderen Push-Dienste wir nicht direkt auf den Geräten Pushbenachrichtigungen jedoch sie stattdessen auf die jeweilige Plattform weiterleiten bestimmte Push Notification Services (PNS - APNS/GCM/WNS), damit die Benachrichtigungen an Geräte übermittelt werden kann. 
2. **Übermittelt** – Dies gibt die Anzahl der Nachrichten, die erfolgreich vom PNS auf dem Gerät übermittelt und bestätigt wurden, als von Mobile Engagement SDK empfangen. 
   
   *Gründe für das übermittelte Anzahl kleiner als die Anzahl der mithilfe von Push wird:*
   
   1. Wenn der Benutzer hat die app vom Gerät deinstalliert, aber das PNS Informationen zu dem Zeitpunkt, den wir den Push an das PNS senden, weiß nicht, wird die Nachricht verworfen.
   2. Wenn das Gerät die app hat, aber die Geräte selbst für längere Zeit offline waren, kann das PNS die Nachricht an das Gerät übermitteln. 
   3. Wenn das Gerät die Nachricht zugestellt abrufen, aber das Mobile Engagement SDK in der app den Inhalt der Nachricht erkennt, legt sie diese Nachricht. Dies kann passieren, wenn die Anpassung der in der app-Benachrichtigung generiert eine Ausnahme, die wir catch im SDK und Drop der Nachricht. Dies kann auch auftreten, wenn die app auf dem Gerät verwendet eine Version des Mobile Engagement SDK also nicht verstehen können, die neuere Version der pushnachricht, die von der Plattform gesendet, aber dies ist nur bei die app aktualisiert wurde, nachdem die Benachrichtigung, aus der Service-Plattform verteilt wurde. Die **erweitert** Registerkarte erfahren, wie viele Nachrichten gelöscht wurden. 
   4. Auf iOS-Geräten werden Nachrichten manchmal nicht übermittelt abrufen, wenn das Gerät entweder auf niedriger Akkukapazität ist oder wenn die app viel Energie verbraucht, bei der Verarbeitung von remote-Benachrichtigungen. Dies ist eine Einschränkung des iOS-Geräte.   
3. **Angezeigten** -gibt die Anzahl von Nachrichten, die erfolgreich an dem app-Benutzer auf dem Gerät in Form einer Benachrichtigung zum Systemstatus: Push/Out-des-app in die mitteilungszentrale oder eine in-app-Benachrichtigung innerhalb der mobilen Anwendung angezeigt werden.  Die **erweitert** Registerkarte informiert Sie darüber, wie viele systembenachrichtigungen wurden und wie viele in app-Benachrichtigungen. 
   
   *Gründe für das angezeigte Anzahl kleiner als übermittelte Count (wartet auf angezeigt werden)*
   
   1. Wenn die Benachrichtigung Kampagne ein Enddatum enthielt ist es möglich, dass die Benachrichtigung übermittelt wurde, aber wenn die Zeit, die zu öffnen, und zeigen Sie es dem Benutzer der app geliefert wurde, wurde bereits abgelaufen sie nie angezeigt wurde.   
   2. Wenn die Benachrichtigung eine Benachrichtigung in der Anwendung befindet, wird die Benachrichtigung nur angezeigt wird, wenn der Benutzer der app die app wird geöffnet. In Fällen, in denen die app-Benutzer die app noch nicht geöffnet, werden, meldet das SDK, dass die Benachrichtigung übermittelt wurde, aber noch nicht angezeigt wird, bis die Anwendung geöffnet wird. 
   3. Wenn die Benachrichtigung eine Benachrichtigung in der Anwendung wird und konfiguriert, die um auf eine bestimmte Aktivität/Bildschirm angezeigt werden, und klicken Sie dann auch die Benachrichtigung als gemeldet werden übermittelt, aber noch nicht übermittelt bis öffnet der Benutzer die app auf einem bestimmten Bildschirm an. 
4. **Benutzerinteraktionen** -gibt die Anzahl von Nachrichten, die mit der app-Benutzer interagiert hat und enthält Nachrichten, die entweder ausgelöst oder die beendet werden. 
   
   * *Der app-Benutzer können die Aktion einer Benachrichtigung in einer der folgenden Methoden:*
     
     1. Wenn die Benachrichtigung ist ein System/Out-des-app-Benachrichtigung oder eine Benachrichtigung in der Anwendung, die nur für den Benachrichtigung gesendet, und klicken Sie dann auf die Benachrichtigung der Benutzer der app klickt.
     2. Wenn die Benachrichtigung eine Benachrichtigung in der Anwendung mit einer Text- oder Webansicht oder Abrufe ist klickt der Benutzer app auf die Aktionsschaltfläche in der Benachrichtigung auf.
     3. Wenn die Benachrichtigung eine Benachrichtigung in der Anwendung mit einer Web-Ansicht ist dann app klickt der Benutzer auf in einer URL in der Webansicht [nur Android]
   * *Der app-Benutzer kann eine Benachrichtigung in einer der folgenden Methoden beenden:*
     
     1. Klicken Sie direkt auf die Schaltfläche "Schließen", auf die Benachrichtigung. 
     2. Ein Lesegerät Weg, oder löschen die Benachrichtigung. 
     3. In app-Benachrichtigungen mit Text/Webinhalte bzw. bei Umfragen werden in der Regel für den Benutzer der app in einem zweistufigen Prozess angezeigt. Sehen sie zuerst eine Benachrichtigung, und wenn sie darauf klicken, sehen sie den nachfolgenden Text/Web/Abruf-Inhalt. Der app-Benutzer kann eine Benachrichtigung in einem der folgenden Schritte aus beenden, und die Details in der erweiterten Sicht erfasst diese. 
5. **Aktiviert** -gibt die Anzahl von Nachrichten, die explizit von der app-Benutzer aktiviert wurden. Dies ist die interessantesten Anzahl, wie dieser Wert ermöglicht, wie viele Benutzer der app durch die Nachricht interessiert waren, die Sie in der Benachrichtigung hindurch. 

> [!NOTE]
> Für iOS und Windows öffnen Sie die Plattformen, wenn der Benutzer die app hat die Kampagne wurde eine Kampagne "Jederzeit", und es ist möglich, dass sowohl aus der app und app-Benachrichtigungen gleichzeitig angezeigt werden. Dies verursacht möglicherweise höher als die übermittelte angezeigte Anzahl. Wenn die Interaktion des Benutzers oder Aktionen die Benachrichtigung, und auch die Anzahl der Benutzer Aktivitäten/Actioned möglicherweise größer als übermittelt. 
> 
> 

![Reach2][19]

## <a name="see-also"></a>Weitere Informationen:
* [Konzepte][Link 6]
* [Problembehandlung bei Service Handbuch][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

